Model: Default (GPT-3.5)

> 1 / 1
> 
> Saya tidak tahu cara membuat kode, tetapi saya ingin memahami cara kerjanya. Jelaskan kode berikut ini kepada saya dengan cara yang dapat dimengerti oleh orang non-teknis. Selalu gunakan Markdown dengan format yang bagus agar lebih mudah diikuti. Aturlah berdasarkan bagian dengan tajuk. Sertakan referensi ke kode sebagai blok kode penurunan harga di setiap bagian. Kode: #!/usr/bin/env python3 import os import openai import pinecone import time import sys from collections import deque from typing import Dict, List from dotenv import load\_dotenv import os #Set Variables load\_dotenv() # Set API Keys OPENAI\_API\_KEY = os.getenv("OPENAI\_API\_KEY", "") assert OPENAI\_API\_KEY, "OPENAI\_API\_KEY environment variable is missing from .env" # Use GPT-3 model USE\_GPT4 = False if USE\_GPT4: print("\\033\[91m\\033\[1m"+"\\n\*\*\*\*\*USING GPT-4. POTENTIALLY EXPENSIVE. MONITOR YOUR COSTS\*\*\*\*\*"+"\\033\[0m\\033\[0m") PINECONE\_API\_KEY = os.getenv("PINECONE\_API\_KEY", "") assert PINECONE\_API\_KEY, "PINECONE\_API\_KEY environment variable is missing from .env" PINECONE\_ENVIRONMENT = os.getenv("PINECONE\_ENVIRONMENT", "us-east1-gcp") assert PINECONE\_ENVIRONMENT, "PINECONE\_ENVIRONMENT environment variable is missing from .env" # Table config YOUR\_TABLE\_NAME = os.getenv("TABLE\_NAME", "") assert YOUR\_TABLE\_NAME, "TABLE\_NAME environment variable is missing from .env" # Project config OBJECTIVE = sys.argv\[1\] if len(sys.argv) > 1 else os.getenv("OBJECTIVE", "") assert OBJECTIVE, "OBJECTIVE environment variable is missing from .env" YOUR\_FIRST\_TASK = os.getenv("FIRST\_TASK", "") assert YOUR\_FIRST\_TASK, "FIRST\_TASK environment variable is missing from .env" #Print OBJECTIVE print("\\033\[96m\\033\[1m"+"\\n\*\*\*\*\*OBJECTIVE\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(OBJECTIVE) # Configure OpenAI and Pinecone openai.api\_key = OPENAI\_API\_KEY pinecone.init(api\_key=PINECONE\_API\_KEY, environment=PINECONE\_ENVIRONMENT) # Create Pinecone index table\_name = YOUR\_TABLE\_NAME dimension = 1536 metric = "cosine" pod\_type = "p1" if table\_name not in pinecone.list\_indexes(): pinecone.create\_index(table\_name, dimension=dimension, metric=metric, pod\_type=pod\_type) # Connect to the index index = pinecone.Index(table\_name) # Task list task\_list = deque(\[\]) def add\_task(task: Dict): task\_list.append(task) def get\_ada\_embedding(text): text = text.replace("\\n", " ") return openai.Embedding.create(input=\[text\], model="text-embedding-ada-002")\["data"\]\[0\]\["embedding"\] def openai\_call(prompt: str, use\_gpt4: bool = False, temperature: float = 0.5, max\_tokens: int = 100): if not use\_gpt4: #Call GPT-3 DaVinci model response = openai.Completion.create( engine='text-davinci-003', prompt=prompt, temperature=temperature, max\_tokens=max\_tokens, top\_p=1, frequency\_penalty=0, presence\_penalty=0 ) return response.choices\[0\].text.strip() else: #Call GPT-4 chat model messages=\[{"role": "user", "content": prompt}\] response = openai.ChatCompletion.create( model="gpt-4", messages = messages, temperature=temperature, max\_tokens=max\_tokens, n=1, stop=None, ) return response.choices\[0\].message.content.strip() def task\_creation\_agent(objective: str, result: Dict, task\_description: str, task\_list: List\[str\], gpt\_version: str = 'gpt-3'): prompt = f"You are an task creation AI that uses the result of an execution agent to create new tasks with the following objective: {objective}, The last completed task has the result: {result}. This result was based on this task description: {task\_description}. These are incomplete tasks: {', '.join(task\_list)}. Based on the result, create new tasks to be completed by the AI system that do not overlap with incomplete tasks. Return the tasks as an array." response = openai\_call(prompt, USE\_GPT4) new\_tasks = response.split('\\n') return \[{"task\_name": task\_name} for task\_name in new\_tasks\] def prioritization\_agent(this\_task\_id:int, gpt\_version: str = 'gpt-3'): global task\_list task\_names = \[t\["task\_name"\] for t in task\_list\] next\_task\_id = int(this\_task\_id)+1 prompt = f"""You are an task prioritization AI tasked with cleaning the formatting of and reprioritizing the following tasks: {task\_names}. Consider the ultimate objective of your team:{OBJECTIVE}. Do not remove any tasks. Return the result as a numbered list, like: #. First task #. Second task Start the task list with number {next\_task\_id}.""" response = openai\_call(prompt, USE\_GPT4) new\_tasks = response.split('\\n') task\_list = deque() for task\_string in new\_tasks: task\_parts = task\_string.strip().split(".", 1) if len(task\_parts) == 2: task\_id = task\_parts\[0\].strip() task\_name = task\_parts\[1\].strip() task\_list.append({"task\_id": task\_id, "task\_name": task\_name}) def execution\_agent(objective:str,task: str, gpt\_version: str = 'gpt-3') -> str: #context = context\_agent(index="quickstart", query="my\_search\_query", n=5) context=context\_agent(index=YOUR\_TABLE\_NAME, query=objective, n=5) #print("\\n\*\*\*\*\*\*\*RELEVANT CONTEXT\*\*\*\*\*\*\\n") #print(context) prompt =f"You are an AI who performs one task based on the following objective: {objective}.\\nTake into account these previously completed tasks: {context}\\nYour task: {task}\\nResponse:" return openai\_call(prompt, USE\_GPT4, 0.7, 2000) def context\_agent(query: str, index: str, n: int): query\_embedding = get\_ada\_embedding(query) index = pinecone.Index(index\_name=index) results = index.query(query\_embedding, top\_k=n, include\_metadata=True) #print("\*\*\*\*\* RESULTS \*\*\*\*\*") #print(results) sorted\_results = sorted(results.matches, key=lambda x: x.score, reverse=True) return \[(str(item.metadata\['task'\])) for item in sorted\_results\] # Add the first task first\_task = { "task\_id": 1, "task\_name": YOUR\_FIRST\_TASK } add\_task(first\_task) # Main loop task\_id\_counter = 1 while True: if task\_list: # Print the task list print("\\033\[95m\\033\[1m"+"\\n\*\*\*\*\*TASK LIST\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") for t in task\_list: print(str(t\['task\_id'\])+": "+t\['task\_name'\]) # Step 1: Pull the first task task = task\_list.popleft() print("\\033\[92m\\033\[1m"+"\\n\*\*\*\*\*NEXT TASK\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(str(task\['task\_id'\])+": "+task\['task\_name'\]) # Send to execution function to complete the task based on the context result = execution\_agent(OBJECTIVE,task\["task\_name"\]) this\_task\_id = int(task\["task\_id"\]) print("\\033\[93m\\033\[1m"+"\\n\*\*\*\*\*TASK RESULT\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(result) # Step 2: Enrich result and store in Pinecone enriched\_result = {'data': result} # This is where you should enrich the result if needed result\_id = f"result\_{task\['task\_id'\]}" vector = enriched\_result\['data'\] # extract the actual result from the dictionary index.upsert(\[(result\_id, get\_ada\_embedding(vector),{"task":task\['task\_name'\],"result":result})\]) # Step 3: Create new tasks and reprioritize task list new\_tasks = task\_creation\_agent(OBJECTIVE,enriched\_result, task\["task\_name"\], \[t\["task\_name"\] for t in task\_list\]) for new\_task in new\_tasks: task\_id\_counter += 1 new\_task.update({"task\_id": task\_id\_counter}) add\_task(new\_task) prioritization\_agent(this\_task\_id) time.sleep(1) # Sleep before checking the task list again

1 / 1

Pendahuluan
-----------

Kode yang disediakan adalah program Python yang terdiri dari beberapa baris perintah. Program ini mengotomatiskan penciptaan, penyelesaian, dan prioritisasi tugas berdasarkan tujuan tertentu. Untuk menjalankan program ini, beberapa API Keys dibutuhkan, seperti OpenAI API Key dan Pinecone API Key. Penggunaan kunci ini memungkinkan program untuk memanfaatkan layanan dari OpenAI dan Pinecone.

Import Libraries
----------------

python

```python
import os
import openai
import pinecone
import time
import sys
from collections import deque
from typing import Dict, List
from dotenv import load_dotenv
```

Baris kode ini mengimpor beberapa pustaka Python yang diperlukan untuk menjalankan program ini. Berikut adalah daftar pustaka dan fungsinya:

*   `os` digunakan untuk berinteraksi dengan sistem operasi, termasuk membaca variabel lingkungan.
*   `openai` digunakan untuk berkomunikasi dengan OpenAI API dan menggunakan model bahasa pemrograman GPT-3 dan GPT-4.
*   `pinecone` digunakan untuk mengakses layanan indeks vektor Pinecone.
*   `time` digunakan untuk menambahkan jeda pada program.
*   `sys` digunakan untuk mengakses argumen baris perintah.
*   `deque` digunakan untuk menerapkan struktur data antrian.
*   `typing` digunakan untuk memberikan informasi tipe data.
*   `load_dotenv` digunakan untuk membaca variabel lingkungan dari file `.env`.

Variabel Lingkungan
-------------------

python

```python
#Set Variables
load_dotenv()

# Set API Keys
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY", "")
assert OPENAI_API_KEY, "OPENAI_API_KEY environment variable is missing from .env"

# Use GPT-3 model
USE_GPT4 = False
if USE_GPT4:
    print("\033[91m\033[1m"+"\n*****USING GPT-4. POTENTIALLY EXPENSIVE. MONITOR YOUR COSTS*****"+"\033[0m\033[0m")

PINECONE_API_KEY = os.getenv("PINECONE_API_KEY", "")
assert PINECONE_API_KEY, "PINECONE_API_KEY environment variable is missing from .env"

PINECONE_ENVIRONMENT = os.getenv("PINECONE_ENVIRONMENT", "us-east1-gcp")
assert PINECONE_ENVIRONMENT, "PINECONE_ENVIRONMENT environment variable is missing from .env"

# Table config
YOUR_TABLE_NAME = os.getenv("TABLE_NAME", "")
assert YOUR_TABLE_NAME, "TABLE_NAME environment variable is missing from .env"

# Project config
OBJECTIVE = sys.argv[1] if len(sys.argv) > 1 else os.getenv("OBJECTIVE", "")
assert OBJECTIVE, "OBJECTIVE environment variable is missing from .env"

YOUR_FIRST_TASK = os.getenv("FIRST_TASK", "")
assert YOUR_FIRST_TASK, "FIRST_TASK environment variable is missing from .env"
```

Baris kode ini memuat variabel lingkungan yang dibaca dari file `.env`. Berikut adalah penjelasan dari masing-masing variabel:

*   `OPENAI_API_KEY`: Kunci API OpenAI.
*   `USE_GPT4`: Variabel boolean yang menentukan apakah model bahasa pemrograman GPT-4 digunakan.
*   `PINECONE_API_KEY`: Kunci

> 1 / 1
> 
> lanjutkan

1 / 1

API Pinecone.

*   `PINECONE_ENVIRONMENT`: Variabel yang menentukan lingkungan Pinecone yang digunakan.
*   `YOUR_TABLE_NAME`: Nama tabel Pinecone yang digunakan untuk menyimpan hasil tugas.
*   `OBJECTIVE`: Tujuan utama dari program yang dibaca dari argumen baris perintah atau variabel lingkungan `.env`.
*   `YOUR_FIRST_TASK`: Nama tugas pertama yang dibuat saat program dijalankan.

Konfigurasi OpenAI dan Pinecone
-------------------------------

python

```python
# Configure OpenAI and Pinecone
openai.api_key = OPENAI_API_KEY
pinecone.init(api_key=PINECONE_API_KEY, environment=PINECONE_ENVIRONMENT)
```

Baris kode ini mengonfigurasi OpenAI dan Pinecone menggunakan kunci API yang telah dibaca dari variabel lingkungan.

Membuat Indeks Pinecone
-----------------------

python

```python
# Create Pinecone index
table_name = YOUR_TABLE_NAME
dimension = 1536
metric = "cosine"
pod_type = "p1"
if table_name not in pinecone.list_indexes():
    pinecone.create_index(table_name, dimension=dimension, metric=metric, pod_type=pod_type)
```

Baris kode ini membuat indeks Pinecone dengan nama `YOUR_TABLE_NAME` dan dimensi 1536. Indeks ini akan digunakan untuk menyimpan hasil tugas dalam bentuk vektor. Jika indeks ini belum ada, maka indeks baru akan dibuat dengan menggunakan metric `cosine` dan tipe pod `p1`.

Fungsi `add_task`
-----------------

python

```python
# Task list
task_list = deque([])

def add_task(task: Dict):
    task_list.append(task)
```

Baris kode ini mendefinisikan sebuah antrian tugas yang didefinisikan sebagai `task_list`. Fungsi `add_task` digunakan untuk menambahkan tugas ke dalam antrian.

Fungsi `get_ada_embedding`
--------------------------

python

```python
def get_ada_embedding(text):
    text = text.replace("\n", " ")
    return openai.Embedding.create(input=[text], model="text-embedding-ada-002")["data"][0]["embedding"]
```

Fungsi ini mengembalikan vektor embedding dari model bahasa pemrograman GPT-3 untuk teks yang diberikan sebagai argumen.

Fungsi `openai_call`
--------------------

python

```python
def openai_call(prompt: str, use_gpt4: bool = False, temperature: float = 0.5, max_tokens: int = 100):
    if not use_gpt4:
        #Call GPT-3 DaVinci model
        response = openai.Completion.create(
            engine='text-davinci-003',
            prompt=prompt,
            temperature=temperature,
            max_tokens=max_tokens,
            top_p=1,
            frequency_penalty=0,
            presence_penalty=0
        )
        return response.choices[0].text.strip()
    else:
        #Call GPT-4 chat model
        messages=[{"role": "user", "content": prompt}]
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages = messages,
            temperature=temperature,
            max_tokens=max_tokens,
            n=1,
            stop=None,
        )
        return response.choices[0].message.content.strip()
```

Fungsi ini memanggil model bahasa pemrograman GPT-3 atau GPT-4 dari OpenAI dan mengembalikan teks yang dihasilkan berdasarkan input prompt.

> 1 / 1
> 
> lanjutkan

1 / 1

Fungsi `task_creation_agent`
----------------------------

python

```python
def task_creation_agent(objective: str, result: Dict, task_description: str, task_list: List[str], gpt_version: str = 'gpt-3'):
    prompt = f"You are an task creation AI that uses the result of an execution agent to create new tasks with the following objective: {objective}, The last completed task has the result: {result}. This result was based on this task description: {task_description}. These are incomplete tasks: {', '.join(task_list)}. Based on the result, create new tasks to be completed by the AI system that do not overlap with incomplete tasks. Return the tasks as an array."
    response = openai_call(prompt, USE_GPT4)
    new_tasks = response.split('\n')
    return [{"task_name": task_name} for task_name in new_tasks]
```

Fungsi ini membentuk prompt dan kemudian memanggil `openai_call` untuk memanggil model bahasa pemrograman GPT-3 atau GPT-4 dari OpenAI dan meminta rekomendasi untuk tugas baru berdasarkan hasil dari eksekusi tugas sebelumnya. Fungsi ini kemudian mengembalikan daftar tugas baru dalam bentuk array.

Fungsi `prioritization_agent`
-----------------------------

python

```python
def prioritization_agent(this_task_id:int, gpt_version: str = 'gpt-3'):
    global task_list
    task_names = [t["task_name"] for t in task_list]
    next_task_id = int(this_task_id)+1
    prompt = f"""You are an task prioritization AI tasked with cleaning the formatting of and reprioritizing the following tasks: {task_names}. Consider the ultimate objective of your team:{OBJECTIVE}. Do not remove any tasks. Return the result as a numbered list, like:
    #. First task
    #. Second task
    Start the task list with number {next_task_id}."""
    response = openai_call(prompt, USE_GPT4)
    new_tasks = response.split('\n')
    task_list = deque()
    for task_string in new_tasks:
        task_parts = task_string.strip().split(".", 1)
        if len(task_parts) == 2:
            task_id = task_parts[0].strip()
            task_name = task_parts[1].strip()
            task_list.append({"task_id": task_id, "task_name": task_name})
```

Fungsi ini mengambil tugas dari `task_list`, membersihkan format dan memberi prioritas pada daftar tugas yang tersisa. Kemudian, ia mengembalikan tugas yang sudah diberi nomor dalam format yang diinginkan. Fungsi ini kemudian menyimpan daftar tugas yang baru dan mengganti daftar tugas yang lama dengan yang baru.

Fungsi `execution_agent`
------------------------

python

```python
def execution_agent(objective:str,task: str, gpt_version: str = 'gpt-3') -> str:
    #context = context_agent(index="quickstart", query="my_search_query", n=5)
    context=context_agent(index=YOUR_TABLE_NAME, query=objective, n=5)
    #print("\n*******RELEVANT CONTEXT******\n")
    #print(context)
    prompt =f"You are an AI who performs one task based on the following objective: {objective}.\nTake into account these previously completed tasks: {context}\nYour task: {task}\nResponse:"
    return openai_call(prompt, USE_GPT4, 0.7,
``` 

> 1 / 1
> 
> lanjutkan

1 / 1

Fungsi ini melakukan tugas yang diberikan pada daftar tugas. Ia terlebih dahulu memanggil fungsi `context_agent` untuk mendapatkan konteks dari tugas yang sedang dilakukan. Kemudian, ia membentuk prompt dan memanggil `openai_call` untuk memanggil model bahasa pemrograman GPT-3 atau GPT-4 dari OpenAI untuk menyelesaikan tugas tersebut. Fungsi ini mengembalikan hasil dari model bahasa pemrograman yang dipanggil.

Fungsi `context_agent`
----------------------

python

```python
def context_agent(query: str, index: str, n: int):
    query_embedding = get_ada_embedding(query)
    index = pinecone.Index(index_name=index)
    results = index.query(query_embedding, top_k=n,
    include_metadata=True)
    #print("***** RESULTS *****")
    #print(results)
    sorted_results = sorted(results.matches, key=lambda x: x.score, reverse=True)    
    return [(str(item.metadata['task'])) for item in sorted_results]
```

Fungsi ini memanggil Pinecone index dan melakukan pencarian dengan menggunakan `query_embedding` untuk mengembalikan hasil yang sesuai dengan query. Fungsi ini mengembalikan daftar hasil dengan metadata yang terdapat pada index yang digunakan.

Loop Utama
----------

python

```python
# Add the first task
first_task = {
    "task_id": 1,
    "task_name": YOUR_FIRST_TASK
}

add_task(first_task)
# Main loop
task_id_counter = 1
while True:
    if task_list:
        # Print the task list
        print("\033[95m\033[1m"+"\n*****TASK LIST*****\n"+"\033[0m\033[0m")
        for t in task_list:
            print(str(t['task_id'])+": "+t['task_name'])

        # Step 1: Pull the first task
        task = task_list.popleft()
        print("\033[92m\033[1m"+"\n*****NEXT TASK*****\n"+"\033[0m\033[0m")
        print(str(task['task_id'])+": "+task['task_name'])

        # Send to execution function to complete the task based on the context
        result = execution_agent(OBJECTIVE,task["task_name"])
        this_task_id = int(task["task_id"])
        print("\033[93m\033[1m"+"\n*****TASK RESULT*****\n"+"\033[0m\033[0m")
        print(result)

        # Step 2: Enrich result and store in Pinecone
        enriched_result = {'data': result}  # This is where you should enrich the result if needed
        result_id = f"result_{task['task_id']}"
        vector = enriched_result['data']  # extract the actual result from the dictionary
        index.upsert([(result_id, get_ada_embedding(vector),{"task":task['task_name'],"result":result})])

    # Step 3: Create new tasks and reprioritize task list
    new_tasks = task_creation_agent(OBJECTIVE,enriched_result, task["task_name"], [t["task_name"] for t in task_list])

    for new_task in new_tasks:
        task_id_counter += 1
        new_task.update({"task_id": task_id_counter})
        add_task(new_task)
    prioritization_agent(this_task_id)

    time.sleep(1)  # Sleep before checking the task list again
```

Loop utama terdiri dari tiga tah

> 1 / 1
> 
> lanjutkan

1 / 1

1.  Mengambil tugas dari depan daftar `task_list`.
2.  Menyelesaikan tugas dengan memanggil fungsi `execution_agent` dan memperbarui Pinecone index dengan hasil yang diperoleh dari tugas.
3.  Membuat daftar tugas baru menggunakan fungsi `task_creation_agent`, menambahkannya ke daftar `task_list`, dan memprioritaskan daftar tugas dengan memanggil fungsi `prioritization_agent`.

Jika daftar `task_list` kosong, program akan tidur selama 1 detik sebelum memeriksa daftar tugas lagi.

Dalam loop utama, terdapat juga kode yang memanggil fungsi `add_task` dan menambahkan tugas pertama ke dalam daftar `task_list`. Selain itu, terdapat variabel `task_id_counter` yang digunakan untuk memberikan ID pada tugas yang baru dibuat. Variabel ini akan diinkrementasi setiap kali tugas baru ditambahkan.

> 1 / 1
> 
> lanjutkan

1 / 1

Selain itu, terdapat juga perintah untuk mencetak informasi mengenai daftar tugas, tugas berikutnya, dan hasil dari tugas yang telah diselesaikan.

python

        ```python
# Print the task list
        print("\033[95m\033[1m"+"\n*****TASK LIST*****\n"+"\033[0m\033[0m")
        for t in task_list:
            print(str(t['task_id'])+": "+t['task_name'])

        # Step 1: Pull the first task
        task = task_list.popleft()
        print("\033[92m\033[1m"+"\n*****NEXT TASK*****\n"+"\033[0m\033[0m")
        print(str(task['task_id'])+": "+task['task_name'])

        # Send to execution function to complete the task based on the context
        result = execution_agent(OBJECTIVE,task["task_name"])
        this_task_id = int(task["task_id"])
        print("\033[93m\033[1m"+"\n*****TASK RESULT*****\n"+"\033[0m\033[0m")
        print(result)
```

Kode ini mencetak tiga informasi yang berkaitan dengan tugas, yaitu daftar tugas, tugas berikutnya, dan hasil dari tugas yang selesai. Cetakannya sudah diformat dengan kode escape ANSI untuk mengubah warna dan gaya font pada teks yang dicetak.

Selain itu, terdapat juga kode yang memanggil fungsi `get_ada_embedding`, `openai_call`, dan `pinecone.Index`. Fungsi `get_ada_embedding` digunakan untuk mengembalikan vektor embedding yang mewakili teks yang diberikan. Fungsi `openai_call` digunakan untuk memanggil model bahasa pemrograman GPT-3 atau GPT-4 dari OpenAI. Sedangkan, fungsi `pinecone.Index` digunakan untuk mengakses Pinecone index dan melakukan operasi pencarian dan penyimpanan.

> 1 / 1
> 
> lanjutkan

1 / 1

Kode ini juga menggunakan beberapa library Python, seperti `os`, `openai`, `pinecone`, `time`, `sys`, `collections`, dan `dotenv`.

`os` digunakan untuk mengambil nilai dari variabel lingkungan (environment variable), yang disimpan di file `.env`. `openai` digunakan untuk mengakses API dari OpenAI dan memanggil model bahasa pemrograman GPT-3 atau GPT-4. `pinecone` digunakan untuk mengakses layanan index dari Pinecone dan melakukan operasi pencarian dan penyimpanan. `time` digunakan untuk memberi jeda waktu dalam loop utama, dan `sys` digunakan untuk mengambil argumen dari baris perintah. `collections` digunakan untuk mengimpor tipe data `deque`, yang digunakan untuk mengimplementasikan daftar tugas. `dotenv` digunakan untuk mengambil nilai dari variabel lingkungan dari file `.env`.

Secara keseluruhan, kode ini adalah implementasi dari sebuah sistem berbasis AI yang menggunakan GPT-3 atau GPT-4 dari OpenAI dan Pinecone index untuk mengotomatiskan pembuatan dan pelaksanaan tugas. Sistem ini menerima deskripsi tugas dan tujuan, dan kemudian menciptakan dan menyelesaikan tugas secara otomatis, berdasarkan pada konteks yang relevan dari Pinecone index. Sistem ini juga memprioritaskan dan memperbarui daftar tugas secara otomatis.