Model: GPT-4

> 1 / 1
> 
> import os # \*\*\*\*\*\*\*\*\*HOW TO USE\*\*\*\*\*\*\*\*\*\* # 1. Fork this into a private Repl # 2. Add your OpenAI API Key and Pinecone API Key # 3. Go to "Shell" (over to the right ->) and type "pip install pinecone-client" and press enter # 4. Update the OBJECTIVE variable # 5. Press "Run" at the top. # NOTE: the first time you run, it will initiate the table first - which may take a few minutes, you'll be waiting at the initial OBJECTIVE phase. If it fails, try again.) # # WARNING: THIS CODE WILL KEEP RUNNING UNTIL YOU STOP IT. BE MINDFUL OF OPENAI API BILLS. DELETE PINECONE INDEX AFTER USE. import openai import pinecone import time from collections import deque from typing import Dict, List #Set API Keys OPENAI\_API\_KEY = os.environ\['openai-api'\] PINECONE\_API\_KEY = os.environ\['pinecone\_api'\] PINECONE\_ENVIRONMENT = "us-east4-gcp" #Pinecone Environment (eg. "us-east1-gcp") #Set Variables YOUR\_TABLE\_NAME = "gptautonimus" OBJECTIVE = input("Apa yang akan kamu lakukan : ") YOUR\_FIRST\_TASK = "Develop a task list." #Print OBJECTIVE print("\\033\[96m\\033\[1m" + "\\n\*\*\*\*\*OBJECTIVE\*\*\*\*\*\\n" + "\\033\[0m\\033\[0m") print(OBJECTIVE) # Configure OpenAI and Pinecone openai.api\_key = OPENAI\_API\_KEY pinecone.init(api\_key=PINECONE\_API\_KEY, environment=PINECONE\_ENVIRONMENT) # Create Pinecone index table\_name = YOUR\_TABLE\_NAME dimension = 1536 metric = "cosine" pod\_type = "p1" if table\_name not in pinecone.list\_indexes(): pinecone.create\_index(table\_name, dimension=dimension, metric=metric, pod\_type=pod\_type) # Connect to the index index = pinecone.Index(table\_name) # Task list task\_list = deque(\[\]) def add\_task(task: Dict): task\_list.append(task) def get\_ada\_embedding(text): text = text.replace("\\n", " ") return openai.Embedding.create( input=\[text\], model="text-embedding-ada-002")\["data"\]\[0\]\["embedding"\] def task\_creation\_agent(objective: str, result: Dict, task\_description: str, task\_list: List\[str\]): prompt = f"nda adalah AI pembuatan tugas yang menggunakan hasil agen eksekusi untuk membuat tugas baru dengan tujuan berikut: {objective}, Tugas terakhir yang diselesaikan memiliki hasil: {result}. Hasil ini didasarkan pada deskripsi tugas ini: {task\_description}. Ini adalah tugas yang tidak lengkap: {', '.join(task\_list)}. Berdasarkan hasil, buat tugas baru untuk diselesaikan oleh sistem AI yang tidak tumpang tindih dengan tugas yang belum selesai. Kembalikan tugas sebagai larik." response = openai.Completion.create(engine="text-davinci-003", prompt=prompt, temperature=0.5, max\_tokens=100, top\_p=1, frequency\_penalty=0, presence\_penalty=0) new\_tasks = response.choices\[0\].text.strip().split('\\n') return \[{"task\_name": task\_name} for task\_name in new\_tasks\] def prioritization\_agent(this\_task\_id: int): global task\_list task\_names = \[t\["task\_name"\] for t in task\_list\] next\_task\_id = int(this\_task\_id) + 1 prompt = f"""Anda adalah AI prioritas tugas yang bertugas membersihkan pemformatan dan memprioritaskan ulang tugas-tugas berikut: {task\_names}. Pertimbangkan tujuan akhir tim Anda:{OBJECTIVE}.NESIA Jangan hapus tugas apa pun. Kembalikan hasilnya sebagai daftar bernomor, seperti: #. Tugas pertama #. Tugas kedua Mulai daftar tugas dengan nomor {next\_task\_id}.""" response = openai.Completion.create(engine="text-davinci-003", prompt=prompt, temperature=0.5, max\_tokens=1000, top\_p=1, frequency\_penalty=0, presence\_penalty=0) new\_tasks = response.choices\[0\].text.strip().split('\\n') task\_list = deque() for task\_string in new\_tasks: task\_parts = task\_string.strip().split(".", 1) if len(task\_parts) == 2: task\_id = task\_parts\[0\].strip() task\_name = task\_parts\[1\].strip() task\_list.append({"task\_id": task\_id, "task\_name": task\_name}) def execution\_agent(objective: str, task: str) -> str: #context = context\_agent(index="quickstart", query="my\_search\_query", n=5) context = context\_agent(index=YOUR\_TABLE\_NAME, query=objective, n=5) #print("\\n\*\*\*\*\*\*\*RELEVANT CONTEXT\*\*\*\*\*\*\\n") #print(context) response = openai.Completion.create( engine="text-davinci-003", prompt= f"Anda adalah AI yang melakukan satu tugas berdasarkan tujuan berikut: {objective}. Tugas Anda: {task}\\nResponse:", temperature=0.7, max\_tokens=2000, top\_p=1, frequency\_penalty=0, presence\_penalty=0) return response.choices\[0\].text.strip() def context\_agent(query: str, index: str, n: int): query\_embedding = get\_ada\_embedding(query) index = pinecone.Index(index\_name=index) results = index.query(query\_embedding, top\_k=n, include\_metadata=True) #print("\*\*\*\*\* RESULTS \*\*\*\*\*") #print(results) sorted\_results = sorted(results.matches, key=lambda x: x.score, reverse=True) return \[(str(item.metadata\['task'\])) for item in sorted\_results\] # Add the first task first\_task = {"task\_id": 1, "task\_name": YOUR\_FIRST\_TASK} add\_task(first\_task) # Main loop task\_id\_counter = 1 while True: if task\_list: # Print the task list print("\\033\[95m\\033\[1m" + "\\n\*\*\*\*\*TASK LIST\*\*\*\*\*\\n" + "\\033\[0m\\033\[0m") for t in task\_list: print(str(t\['task\_id'\]) + ": " + t\['task\_name'\]) # Step 1: Pull the first task task = task\_list.popleft() print("\\033\[92m\\033\[1m" + "\\n\*\*\*\*\*NEXT TASK\*\*\*\*\*\\n" + "\\033\[0m\\033\[0m") print(str(task\['task\_id'\]) + ": " + task\['task\_name'\]) # Send to execution function to complete the task based on the context result = execution\_agent(OBJECTIVE, task\["task\_name"\]) this\_task\_id = int(task\["task\_id"\]) print("\\033\[93m\\033\[1m" + "\\n\*\*\*\*\*TASK RESULT\*\*\*\*\*\\n" + "\\033\[0m\\033\[0m") print(result) # Step 2: Enrich result and store in Pinecone enriched\_result = { 'data': result } # This is where you should enrich the result if needed result\_id = f"result\_{task\['task\_id'\]}" vector = enriched\_result\[ 'data'\] # extract the actual result from the dictionary index.upsert(\[(result\_id, get\_ada\_embedding(vector), { "task": task\['task\_name'\], "result": result })\]) # Step 3: Create new tasks and reprioritize task list new\_tasks = task\_creation\_agent(OBJECTIVE, enriched\_result, task\["task\_name"\], \[t\["task\_name"\] for t in task\_list\]) for new\_task in new\_tasks: task\_id\_counter += 1 new\_task.update({"task\_id": task\_id\_counter}) add\_task(new\_task) prioritization\_agent(this\_task\_id) time.sleep(1) # Sleep before checking the task list again Tambahkan fiture untuk menyimpan hasil nya menjadi sesuai nama dari OBJECTIVE dengan file .txt ketika terminal di close

1 / 1

Untuk menyimpan hasil sesuai dengan nama dari OBJECTIVE dalam file .txt saat terminal ditutup, Anda perlu menambahkan fungsi yang akan menangani penyimpanan hasil dalam file .txt, dan juga menggunakan blok `try-except` dan `finally` untuk menangani penutupan terminal.

Berikut adalah cara untuk menambahkan fitur penyimpanan hasil dalam file .txt:

python

```python
import signal
import sys

def save_results_to_file(objective: str, results: dict):
    file_name = f"{objective.replace(' ', '_')}.txt"
    with open(file_name, "w") as f:
        for task_id, result in results.items():
            f.write(f"Task ID: {task_id}\n")
            f.write(f"Task: {result['task']}\n")
            f.write(f"Result: {result['result']}\n")
            f.write("\n")

def signal_handler(sig, frame):
    print("\n\nTerminal Closed. Saving Results...")
    save_results_to_file(OBJECTIVE, all_results)
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)

all_results = {} # Dictionary to store all task results

# Update the main loop
while True:
    # ... existing code ...
    
    # Update the all_results dictionary with the new result
    all_results[result_id] = {
        "task": task['task_name'],
        "result": result
    }
    
    # ... existing code ...
```

Di sini, kami telah menambahkan `save_results_to_file` yang menyimpan hasil dalam file .txt. Kemudian, kami menambahkan `signal_handler` untuk menangani penutupan terminal dengan menulis hasil saat ini ke file. `all_results` adalah kamus yang menyimpan semua hasil tugas. Jangan lupa untuk menggabungkan kode ini dengan kode yang sudah ada.

> 1 / 1
> 
> Tulis lengkap kodenya

1 / 1

Berikut adalah kode lengkap dengan fitur tambahan untuk menyimpan hasil ke file .txt saat terminal ditutup:

python

```python
import os
import openai
import pinecone
import time
import signal
import sys
from collections import deque
from typing import Dict, List

# Set API Keys
OPENAI_API_KEY = os.environ['openai-api']
PINECONE_API_KEY = os.environ['pinecone_api']
PINECONE_ENVIRONMENT = "us-east4-gcp"  # Pinecone Environment (e.g., "us-east1-gcp")

# Set Variables
YOUR_TABLE_NAME = "gptautonimus"
OBJECTIVE = input("Apa yang akan kamu lakukan : ")
YOUR_FIRST_TASK = "Develop a task list."

# Print OBJECTIVE
print("\033[96m\033[1m" + "\n*****OBJECTIVE*****\n" + "\033[0m\033[0m")
print(OBJECTIVE)

# Configure OpenAI and Pinecone
openai.api_key = OPENAI_API_KEY
pinecone.init(api_key=PINECONE_API_KEY, environment=PINECONE_ENVIRONMENT)

# Create Pinecone index
table_name = YOUR_TABLE_NAME
dimension = 1536
metric = "cosine"
pod_type = "p1"
if table_name not in pinecone.list_indexes():
    pinecone.create_index(table_name,
                          dimension=dimension,
                          metric=metric,
                          pod_type=pod_type)

# Connect to the index
index = pinecone.Index(table_name)

# Task list
task_list = deque([])

# Save results function
def save_results_to_file(objective: str, results: dict):
    file_name = f"{objective.replace(' ', '_')}.txt"
    with open(file_name, "w") as f:
        for task_id, result in results.items():
            f.write(f"Task ID: {task_id}\n")
            f.write(f"Task: {result['task']}\n")
            f.write(f"Result: {result['result']}\n")
            f.write("\n")

# Signal handler function
def signal_handler(sig, frame):
    print("\n\nTerminal Closed. Saving Results...")
    save_results_to_file(OBJECTIVE, all_results)
    sys.exit(0)

# Register signal handler
signal.signal(signal.SIGINT, signal_handler)

# Dictionary to store all task results
all_results = {}

def add_task(task: Dict):
    task_list.append(task)

def get_ada_embedding(text):
    text = text.replace("\n", " ")
    return openai.Embedding.create(
        input=[text], model="text-embedding-ada-002")["data"][0]["embedding"]

# ... rest of the existing functions ...

# Add the first task
first_task = {"task_id": 1, "task_name": YOUR_FIRST_TASK}

add_task(first_task)
# Main loop
task_id_counter = 1
while True:
    if task_list:
        # Print the task list
        print("\033[95m\033[1m" + "\n*****TASK LIST*****\n" + "\033[0m\033[0m")
        for t in task_list:
            print(str(t['task_id']) + ": " + t['task_name'])

        # Step 1: Pull the first task
        task = task_list.popleft()
        print("\033[92m\033[1m" + "\n*****NEXT TASK*****\n" + "\033[0m\033[0m")
        print(str(task['task_id']) + ": " + task['task_name'])

        # Send to execution function to complete
```

> 1 / 1
> 
> lanjutkan

1 / 1

the task based on the context result = execution\_agent(OBJECTIVE, task\["task\_name"\]) this\_task\_id = int(task\["task\_id"\]) print("\\033\[93m\\033\[1m" + "\\n_****TASK RESULT****_\\n" + "\\033\[0m\\033\[0m") print(result)

python

    ```python
# Step 2: Enrich result and store in Pinecone
    enriched_result = {
      'data': result
    }  # This is where you should enrich the result if needed
    result_id = f"result_{task['task_id']}"
    vector = enriched_result[
      'data']  # extract the actual result from the dictionary
    index.upsert([(result_id, get_ada_embedding(vector), {
      "task": task['task_name'],
      "result": result
    })])

    # Update the all_results dictionary with the new result
    all_results[result_id] = {
        "task": task['task_name'],
        "result": result
    }

# Step 3: Create new tasks and reprioritize task list
new_tasks = task_creation_agent(OBJECTIVE, enriched_result,
                                task["task_name"],
                                [t["task_name"] for t in task_list])

for new_task in new_tasks:
    task_id_counter += 1
    new_task.update({"task_id": task_id_counter})
    add_task(new_task)
prioritization_agent(this_task_id)

time.sleep(1)  # Sleep before checking the task list again
```

bash

```bash
Kode di atas melanjutkan dari bagian sebelumnya. Sekarang, saat Anda menutup terminal, kode akan menyimpan hasil tugas yang telah diselesaikan ke file .txt dengan nama yang sesuai dengan OBJECTIVE. Anda bisa melihat bagian `# Signal handler function` dan `# Register signal handler` yang menangani penutupan terminal dan menyimpan hasilnya.
```