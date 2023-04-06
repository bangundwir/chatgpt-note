Model: GPT-4

> 1 / 1
> 
> Saya tidak tahu cara membuat kode, tetapi saya ingin memahami cara kerjanya. Jelaskan kode berikut ini kepada saya dengan cara yang dapat dimengerti oleh orang non-teknis. Selalu gunakan Markdown dengan format yang bagus agar lebih mudah diikuti. Aturlah berdasarkan bagian dengan tajuk. Sertakan referensi ke kode sebagai blok kode penurunan harga di setiap bagian. Kode: #!/usr/bin/env python3 import os import openai import pinecone import time import sys from collections import deque from typing import Dict, List from dotenv import load\_dotenv import os #Set Variables load\_dotenv() # Set API Keys OPENAI\_API\_KEY = os.getenv("OPENAI\_API\_KEY", "") assert OPENAI\_API\_KEY, "OPENAI\_API\_KEY environment variable is missing from .env" # Use GPT-3 model USE\_GPT4 = False if USE\_GPT4: print("\\033\[91m\\033\[1m"+"\\n\*\*\*\*\*USING GPT-4. POTENTIALLY EXPENSIVE. MONITOR YOUR COSTS\*\*\*\*\*"+"\\033\[0m\\033\[0m") PINECONE\_API\_KEY = os.getenv("PINECONE\_API\_KEY", "") assert PINECONE\_API\_KEY, "PINECONE\_API\_KEY environment variable is missing from .env" PINECONE\_ENVIRONMENT = os.getenv("PINECONE\_ENVIRONMENT", "us-east1-gcp") assert PINECONE\_ENVIRONMENT, "PINECONE\_ENVIRONMENT environment variable is missing from .env" # Table config YOUR\_TABLE\_NAME = os.getenv("TABLE\_NAME", "") assert YOUR\_TABLE\_NAME, "TABLE\_NAME environment variable is missing from .env" # Project config OBJECTIVE = sys.argv\[1\] if len(sys.argv) > 1 else os.getenv("OBJECTIVE", "") assert OBJECTIVE, "OBJECTIVE environment variable is missing from .env" YOUR\_FIRST\_TASK = os.getenv("FIRST\_TASK", "") assert YOUR\_FIRST\_TASK, "FIRST\_TASK environment variable is missing from .env" #Print OBJECTIVE print("\\033\[96m\\033\[1m"+"\\n\*\*\*\*\*OBJECTIVE\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(OBJECTIVE) # Configure OpenAI and Pinecone openai.api\_key = OPENAI\_API\_KEY pinecone.init(api\_key=PINECONE\_API\_KEY, environment=PINECONE\_ENVIRONMENT) # Create Pinecone index table\_name = YOUR\_TABLE\_NAME dimension = 1536 metric = "cosine" pod\_type = "p1" if table\_name not in pinecone.list\_indexes(): pinecone.create\_index(table\_name, dimension=dimension, metric=metric, pod\_type=pod\_type) # Connect to the index index = pinecone.Index(table\_name) # Task list task\_list = deque(\[\]) def add\_task(task: Dict): task\_list.append(task) def get\_ada\_embedding(text): text = text.replace("\\n", " ") return openai.Embedding.create(input=\[text\], model="text-embedding-ada-002")\["data"\]\[0\]\["embedding"\] def openai\_call(prompt: str, use\_gpt4: bool = False, temperature: float = 0.5, max\_tokens: int = 100): if not use\_gpt4: #Call GPT-3 DaVinci model response = openai.Completion.create( engine='text-davinci-003', prompt=prompt, temperature=temperature, max\_tokens=max\_tokens, top\_p=1, frequency\_penalty=0, presence\_penalty=0 ) return response.choices\[0\].text.strip() else: #Call GPT-4 chat model messages=\[{"role": "user", "content": prompt}\] response = openai.ChatCompletion.create( model="gpt-4", messages = messages, temperature=temperature, max\_tokens=max\_tokens, n=1, stop=None, ) return response.choices\[0\].message.content.strip() def task\_creation\_agent(objective: str, result: Dict, task\_description: str, task\_list: List\[str\], gpt\_version: str = 'gpt-3'): prompt = f"You are an task creation AI that uses the result of an execution agent to create new tasks with the following objective: {objective}, The last completed task has the result: {result}. This result was based on this task description: {task\_description}. These are incomplete tasks: {', '.join(task\_list)}. Based on the result, create new tasks to be completed by the AI system that do not overlap with incomplete tasks. Return the tasks as an array." response = openai\_call(prompt, USE\_GPT4) new\_tasks = response.split('\\n') return \[{"task\_name": task\_name} for task\_name in new\_tasks\] def prioritization\_agent(this\_task\_id:int, gpt\_version: str = 'gpt-3'): global task\_list task\_names = \[t\["task\_name"\] for t in task\_list\] next\_task\_id = int(this\_task\_id)+1 prompt = f"""You are an task prioritization AI tasked with cleaning the formatting of and reprioritizing the following tasks: {task\_names}. Consider the ultimate objective of your team:{OBJECTIVE}. Do not remove any tasks. Return the result as a numbered list, like: #. First task #. Second task Start the task list with number {next\_task\_id}.""" response = openai\_call(prompt, USE\_GPT4) new\_tasks = response.split('\\n') task\_list = deque() for task\_string in new\_tasks: task\_parts = task\_string.strip().split(".", 1) if len(task\_parts) == 2: task\_id = task\_parts\[0\].strip() task\_name = task\_parts\[1\].strip() task\_list.append({"task\_id": task\_id, "task\_name": task\_name}) def execution\_agent(objective:str,task: str, gpt\_version: str = 'gpt-3') -> str: #context = context\_agent(index="quickstart", query="my\_search\_query", n=5) context=context\_agent(index=YOUR\_TABLE\_NAME, query=objective, n=5) #print("\\n\*\*\*\*\*\*\*RELEVANT CONTEXT\*\*\*\*\*\*\\n") #print(context) prompt =f"You are an AI who performs one task based on the following objective: {objective}.\\nTake into account these previously completed tasks: {context}\\nYour task: {task}\\nResponse:" return openai\_call(prompt, USE\_GPT4, 0.7, 2000) def context\_agent(query: str, index: str, n: int): query\_embedding = get\_ada\_embedding(query) index = pinecone.Index(index\_name=index) results = index.query(query\_embedding, top\_k=n, include\_metadata=True) #print("\*\*\*\*\* RESULTS \*\*\*\*\*") #print(results) sorted\_results = sorted(results.matches, key=lambda x: x.score, reverse=True) return \[(str(item.metadata\['task'\])) for item in sorted\_results\] # Add the first task first\_task = { "task\_id": 1, "task\_name": YOUR\_FIRST\_TASK } add\_task(first\_task) # Main loop task\_id\_counter = 1 while True: if task\_list: # Print the task list print("\\033\[95m\\033\[1m"+"\\n\*\*\*\*\*TASK LIST\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") for t in task\_list: print(str(t\['task\_id'\])+": "+t\['task\_name'\]) # Step 1: Pull the first task task = task\_list.popleft() print("\\033\[92m\\033\[1m"+"\\n\*\*\*\*\*NEXT TASK\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(str(task\['task\_id'\])+": "+task\['task\_name'\]) # Send to execution function to complete the task based on the context result = execution\_agent(OBJECTIVE,task\["task\_name"\]) this\_task\_id = int(task\["task\_id"\]) print("\\033\[93m\\033\[1m"+"\\n\*\*\*\*\*TASK RESULT\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(result) # Step 2: Enrich result and store in Pinecone enriched\_result = {'data': result} # This is where you should enrich the result if needed result\_id = f"result\_{task\['task\_id'\]}" vector = enriched\_result\['data'\] # extract the actual result from the dictionary index.upsert(\[(result\_id, get\_ada\_embedding(vector),{"task":task\['task\_name'\],"result":result})\]) # Step 3: Create new tasks and reprioritize task list new\_tasks = task\_creation\_agent(OBJECTIVE,enriched\_result, task\["task\_name"\], \[t\["task\_name"\] for t in task\_list\]) for new\_task in new\_tasks: task\_id\_counter += 1 new\_task.update({"task\_id": task\_id\_counter}) add\_task(new\_task) prioritization\_agent(this\_task\_id) time.sleep(1) # Sleep before checking the task list again

1 / 1

Kode ini adalah program Python yang menggunakan API OpenAI dan Pinecone untuk membuat, mengeksekusi, dan mengurutkan tugas berdasarkan hasil yang dihasilkan oleh model AI OpenAI seperti GPT-3 atau GPT-4. Berikut ini adalah penjelasan bagian kode yang paling penting.

Impor Modul
-----------

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
import os
```

Kode di atas mengimpor modul yang diperlukan untuk menjalankan program ini, seperti OpenAI dan Pinecone untuk menggunakan API mereka.

Atur Variabel
-------------

python

```python
load_dotenv()

# Set API Keys
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY", "")
PINECONE_API_KEY = os.getenv("PINECONE_API_KEY", "")

# Use GPT-3 model
USE_GPT4 = False
```

Bagian ini mengatur variabel penting seperti kunci API untuk OpenAI dan Pinecone, dan menentukan apakah menggunakan model GPT-4 atau GPT-3 (default).

Inisialisasi OpenAI dan Pinecone
--------------------------------

python

```python
openai.api_key = OPENAI_API_KEY
pinecone.init(api_key=PINECONE_API_KEY, environment=PINECONE_ENVIRONMENT)
```

Bagian ini menginisialisasi koneksi dengan API OpenAI dan Pinecone menggunakan kunci API yang didefinisikan sebelumnya.

Fungsi Utama
------------

Ada beberapa fungsi utama dalam kode ini:

1.  `add_task()`: Menambahkan tugas ke dalam antrian tugas.
2.  `get_ada_embedding()`: Menggunakan model OpenAI untuk mendapatkan embedding dari teks.
3.  `openai_call()`: Memanggil model OpenAI GPT-3 atau GPT-4 dan mengembalikan respons teks.
4.  `task_creation_agent()`: Membuat tugas baru berdasarkan hasil dari tugas yang baru saja diselesaikan.
5.  `prioritization_agent()`: Mengurutkan tugas yang ada berdasarkan prioritas.
6.  `execution_agent()`: Menjalankan tugas berdasarkan konteks yang ada.
7.  `context_agent()`: Mencari tugas yang relevan yang telah selesai sebelumnya.

Proses Utama
------------

Bagian ini menggambarkan proses utama yang mencakup pengaturan tugas awal, menjalankan tugas, dan mengurutkan tugas.

1.  Menambahkan tugas pertama.
2.  Melakukan looping selama tugas ada dalam antrian. a. Mengambil dan menampilkan tugas pertama. b. Menjalankan tugas dengan menggunakan `execution_agent()`. c. Menyimpan hasil tugas ke Pinecone. d. Membuat tugas baru dan menambahkannya ke antrian. e. Mengurutkan antrian tugas dengan menggunakan `prioritization_agent()`.

Kode ini akan berjalan terus menerus sampai tidak ada tugas yang tersisa dalam antrian.

> 1 / 1
> 
> lanjutkan

1 / 1

Sekarang, kita akan membahas lebih lanjut mengenai beberapa fungsi yang ada dalam kode dan bagaimana mereka saling terkait.

### Fungsi `add_task()`

python

```python
def add_task(task: Dict):
    task_list.append(task)
```

Fungsi ini menerima tugas dalam bentuk kamus dan menambahkannya ke antrian tugas (`task_list`).

### Fungsi `get_ada_embedding()`

python

```python
def get_ada_embedding(text):
    text = text.replace("\n", " ")
    return openai.Embedding.create(input=[text], model="text-embedding-ada-002")["data"][0]["embedding"]
```

Fungsi ini menggunakan model OpenAI (`text-embedding-ada-002`) untuk menghasilkan vektor embedding dari teks yang diberikan. Vektor ini nantinya digunakan untuk menyimpan dan mencari konteks yang relevan dalam Pinecone.

### Fungsi `openai_call()`

python

```python
def openai_call(prompt: str, use_gpt4: bool = False, temperature: float = 0.5, max_tokens: int = 100):
    #...
```

Fungsi ini mengambil teks prompt dan mengirimkannya ke model GPT-3 atau GPT-4, lalu mengembalikan teks respons yang dihasilkan oleh model.

### Fungsi `task_creation_agent()`

python

```python
def task_creation_agent(objective: str, result: Dict, task_description: str, task_list: List[str], gpt_version: str = 'gpt-3'):
    #...
```

Fungsi ini meminta model AI untuk membuat tugas baru berdasarkan hasil tugas sebelumnya dan daftar tugas yang belum selesai. Tugas baru yang dihasilkan kemudian ditambahkan ke antrian.

### Fungsi `prioritization_agent()`

python

```python
def prioritization_agent(this_task_id:int, gpt_version: str = 'gpt-3'):
    #...
```

Fungsi ini menggunakan model AI untuk mengurutkan tugas dalam antrian berdasarkan prioritas. Tugas dengan prioritas lebih tinggi akan dikerjakan terlebih dahulu.

### Fungsi `execution_agent()`

python

```python
def execution_agent(objective:str,task: str, gpt_version: str = 'gpt-3') -> str:
    #...
```

Fungsi ini menjalankan tugas yang diberikan berdasarkan konteks yang relevan. Model AI menyelesaikan tugas ini dan mengembalikan hasilnya.

### Fungsi `context_agent()`

python

```python
def context_agent(query: str, index: str, n: int):
    #...
```

Fungsi ini mencari n tugas yang relevan dengan query yang diberikan di dalam indeks Pinecone. Hasil pencarian ini digunakan sebagai konteks untuk menjalankan tugas berikutnya.

Kode ini secara keseluruhan menggambarkan bagaimana sistem AI ini secara otomatis menciptakan, mengurutkan, dan menyelesaikan tugas dengan menggunakan model OpenAI seperti GPT-3 atau GPT-4. Pinecone digunakan untuk menyimpan hasil tugas dan mencari konteks yang relevan untuk menjalankan tugas berikutnya. Sistem ini akan terus berjalan sampai tidak ada tugas yang tersisa dalam antrian.

> 1 / 1
> 
> lanjutkan

1 / 1

Terakhir, kita akan membahas bagian utama dari kode yang menggabungkan semua fungsi yang telah dijelaskan sebelumnya.

### Loop Utama

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
    #...
```

Di bagian ini, tugas pertama ditambahkan ke antrian, dan sebuah loop utama dimulai. Loop ini akan terus berjalan hingga tidak ada tugas yang tersisa dalam antrian. Dalam setiap iterasi loop, kode melakukan hal berikut:

1.  Menampilkan daftar tugas saat ini.
2.  Mengambil tugas berikutnya dari antrian.
3.  Menjalankan fungsi `execution_agent()` untuk menyelesaikan tugas berdasarkan konteks yang relevan. Hasil tugas kemudian ditampilkan.
4.  Menyimpan hasil tugas yang diperkaya ke dalam indeks Pinecone.
5.  Membuat tugas baru dengan menggunakan fungsi `task_creation_agent()` berdasarkan hasil tugas sebelumnya dan daftar tugas yang belum selesai.
6.  Mengurutkan tugas yang ada dalam antrian berdasarkan prioritas dengan menggunakan fungsi `prioritization_agent()`.

Dengan demikian, kode ini menciptakan sistem yang secara otomatis menciptakan, mengurutkan, dan menyelesaikan tugas dengan bantuan model OpenAI dan Pinecone sebagai penyimpanan konteks yang relevan. Sistem ini akan terus berjalan hingga tidak ada tugas yang tersisa dalam antrian.

Sekarang Anda telah memahami bagaimana kode ini bekerja dan bagaimana masing-masing komponen saling terkait dalam sistem AI yang mengotomatiskan penciptaan, pengurutan, dan penyelesaian tugas.