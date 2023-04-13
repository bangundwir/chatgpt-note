Model: GPT-4

> 1 / 1
> 
> Saya tidak tahu cara membuat kode, tetapi saya ingin memahami cara kerjanya. Jelaskan kode berikut ini kepada saya dengan cara yang dapat dimengerti oleh orang non-teknis. Selalu gunakan Markdown dengan format yang bagus agar lebih mudah diikuti. Aturlah berdasarkan bagian dengan tajuk. Sertakan referensi ke kode sebagai blok kode penurunan harga di setiap bagian. Kode: import json import random import commands as cmd from memory import PineconeMemory import data import chat from colorama import Fore, Style from spinner import Spinner import time import speak from enum import Enum, auto import sys from config import Config from json\_parser import fix\_and\_parse\_json from ai\_config import AIConfig import traceback import yaml import argparse def print\_to\_console( title, title\_color, content, speak\_text=False, min\_typing\_speed=0.05, max\_typing\_speed=0.01): global cfg if speak\_text and cfg.speak\_mode: speak.say\_text(f"{title}. {content}") print(title\_color + title + " " + Style.RESET\_ALL, end="") if content: if isinstance(content, list): content = " ".join(content) words = content.split() for i, word in enumerate(words): print(word, end="", flush=True) if i < len(words) - 1: print(" ", end="", flush=True) typing\_speed = random.uniform(min\_typing\_speed, max\_typing\_speed) time.sleep(typing\_speed) # type faster after each word min\_typing\_speed = min\_typing\_speed \* 0.95 max\_typing\_speed = max\_typing\_speed \* 0.95 print() def print\_assistant\_thoughts(assistant\_reply): global ai\_name global cfg try: # Parse and print Assistant response assistant\_reply\_json = fix\_and\_parse\_json(assistant\_reply) # Check if assistant\_reply\_json is a string and attempt to parse it into a JSON object if isinstance(assistant\_reply\_json, str): try: assistant\_reply\_json = json.loads(assistant\_reply\_json) except json.JSONDecodeError as e: print\_to\_console("Error: Invalid JSON\\n", Fore.RED, assistant\_reply) assistant\_reply\_json = {} assistant\_thoughts\_reasoning = None assistant\_thoughts\_plan = None assistant\_thoughts\_speak = None assistant\_thoughts\_criticism = None assistant\_thoughts = assistant\_reply\_json.get("thoughts", {}) assistant\_thoughts\_text = assistant\_thoughts.get("text") if assistant\_thoughts: assistant\_thoughts\_reasoning = assistant\_thoughts.get("reasoning") assistant\_thoughts\_plan = assistant\_thoughts.get("plan") assistant\_thoughts\_criticism = assistant\_thoughts.get("criticism") assistant\_thoughts\_speak = assistant\_thoughts.get("speak") print\_to\_console(f"{ai\_name.upper()} THOUGHTS:", Fore.YELLOW, assistant\_thoughts\_text) print\_to\_console("REASONING:", Fore.YELLOW, assistant\_thoughts\_reasoning) if assistant\_thoughts\_plan: print\_to\_console("PLAN:", Fore.YELLOW, "") # If it's a list, join it into a string if isinstance(assistant\_thoughts\_plan, list): assistant\_thoughts\_plan = "\\n".join(assistant\_thoughts\_plan) elif isinstance(assistant\_thoughts\_plan, dict): assistant\_thoughts\_plan = str(assistant\_thoughts\_plan) # Split the input\_string using the newline character and dashes lines = assistant\_thoughts\_plan.split('\\n') for line in lines: line = line.lstrip("- ") print\_to\_console("- ", Fore.GREEN, line.strip()) print\_to\_console("CRITICISM:", Fore.YELLOW, assistant\_thoughts\_criticism) # Speak the assistant's thoughts if cfg.speak\_mode and assistant\_thoughts\_speak: speak.say\_text(assistant\_thoughts\_speak) except json.decoder.JSONDecodeError: print\_to\_console("Error: Invalid JSON\\n", Fore.RED, assistant\_reply) # All other errors, return "Error: + error message" except Exception as e: call\_stack = traceback.format\_exc() print\_to\_console("Error: \\n", Fore.RED, call\_stack) def load\_variables(config\_file="config.yaml"): # Load variables from yaml file if it exists try: with open(config\_file) as file: config = yaml.load(file, Loader=yaml.FullLoader) ai\_name = config.get("ai\_name") ai\_role = config.get("ai\_role") ai\_goals = config.get("ai\_goals") except FileNotFoundError: ai\_name = "" ai\_role = "" ai\_goals = \[\] # Prompt the user for input if config file is missing or empty values if not ai\_name: ai\_name = input("Name your AI: ") if ai\_name == "": ai\_name = "Entrepreneur-GPT" if not ai\_role: ai\_role = input(f"{ai\_name} is: ") if ai\_role == "": ai\_role = "an AI designed to autonomously develop and run businesses with the sole goal of increasing your net worth." if not ai\_goals: print("Enter up to 5 goals for your AI: ") print("For example: \\nIncrease net worth, Grow Twitter Account, Develop and manage multiple businesses autonomously'") print("Enter nothing to load defaults, enter nothing when finished.") ai\_goals = \[\] for i in range(5): ai\_goal = input(f"Goal {i+1}: ") if ai\_goal == "": break ai\_goals.append(ai\_goal) if len(ai\_goals) == 0: ai\_goals = \["Increase net worth", "Grow Twitter Account", "Develop and manage multiple businesses autonomously"\] # Save variables to yaml file config = {"ai\_name": ai\_name, "ai\_role": ai\_role, "ai\_goals": ai\_goals} with open(config\_file, "w") as file: documents = yaml.dump(config, file) prompt = data.load\_prompt() prompt\_start = """Your decisions must always be made independently without seeking user assistance. Play to your strengths as an LLM and pursue simple strategies with no legal complications.""" # Construct full prompt full\_prompt = f"You are {ai\_name}, {ai\_role}\\n{prompt\_start}\\n\\nGOALS:\\n\\n" for i, goal in enumerate(ai\_goals): full\_prompt += f"{i+1}. {goal}\\n" full\_prompt += f"\\n\\n{prompt}" return full\_prompt def construct\_prompt(): config = AIConfig.load() if config.ai\_name: print\_to\_console( f"Welcome back! ", Fore.GREEN, f"Would you like me to return to being {config.ai\_name}?", speak\_text=True) should\_continue = input(f"""Continue with the last settings? Name: {config.ai\_name} Role: {config.ai\_role} Goals: {config.ai\_goals} Continue (y/n): """) if should\_continue.lower() == "n": config = AIConfig() if not config.ai\_name: config = prompt\_user() config.save() # Get rid of this global: global ai\_name ai\_name = config.ai\_name full\_prompt = config.construct\_full\_prompt() return full\_prompt def prompt\_user(): ai\_name = "" # Construct the prompt print\_to\_console( "Welcome to Auto-GPT! ", Fore.GREEN, "Enter the name of your AI and its role below. Entering nothing will load defaults.", speak\_text=True) # Get AI Name from User print\_to\_console( "Name your AI: ", Fore.GREEN, "For example, 'Entrepreneur-GPT'") ai\_name = input("AI Name: ") if ai\_name == "": ai\_name = "Entrepreneur-GPT" print\_to\_console( f"{ai\_name} here!", Fore.LIGHTBLUE\_EX, "I am at your service.", speak\_text=True) # Get AI Role from User print\_to\_console( "Describe your AI's role: ", Fore.GREEN, "For example, 'an AI designed to autonomously develop and run businesses with the sole goal of increasing your net worth.'") ai\_role = input(f"{ai\_name} is: ") if ai\_role == "": ai\_role = "an AI designed to autonomously develop and run businesses with the sole goal of increasing your net worth." # Enter up to 5 goals for the AI print\_to\_console( "Enter up to 5 goals for your AI: ", Fore.GREEN, "For example: \\nIncrease net worth, Grow Twitter Account, Develop and manage multiple businesses autonomously'") print("Enter nothing to load defaults, enter nothing when finished.", flush=True) ai\_goals = \[\] for i in range(5): ai\_goal = input(f"{Fore.LIGHTBLUE\_EX}Goal{Style.RESET\_ALL} {i+1}: ") if ai\_goal == "": break ai\_goals.append(ai\_goal) if len(ai\_goals) == 0: ai\_goals = \["Increase net worth", "Grow Twitter Account", "Develop and manage multiple businesses autonomously"\] config = AIConfig(ai\_name, ai\_role, ai\_goals) return config def parse\_arguments(): global cfg cfg.set\_continuous\_mode(False) cfg.set\_speak\_mode(False) parser = argparse.ArgumentParser(description='Process arguments.') parser.add\_argument('--continuous', action='store\_true', help='Enable Continuous Mode') parser.add\_argument('--speak', action='store\_true', help='Enable Speak Mode') parser.add\_argument('--debug', action='store\_true', help='Enable Debug Mode') parser.add\_argument('--gpt3only', action='store\_true', help='Enable GPT3.5 Only Mode') args = parser.parse\_args() if args.continuous: print\_to\_console("Continuous Mode: ", Fore.RED, "ENABLED") print\_to\_console( "WARNING: ", Fore.RED, "Continuous mode is not recommended. It is potentially dangerous and may cause your AI to run forever or carry out actions you would not usually authorise. Use at your own risk.") cfg.set\_continuous\_mode(True) if args.speak: print\_to\_console("Speak Mode: ", Fore.GREEN, "ENABLED") cfg.set\_speak\_mode(True) if args.gpt3only: print\_to\_console("GPT3.5 Only Mode: ", Fore.GREEN, "ENABLED") cfg.set\_smart\_llm\_model(cfg.fast\_llm\_model) # TODO: fill in llm values here cfg = Config() parse\_arguments() ai\_name = "" prompt = construct\_prompt() # print(prompt) # Initialize variables full\_message\_history = \[\] result = None next\_action\_count = 0 # Make a constant: user\_input = "Determine which next command to use, and respond using the format specified above:" # Initialize memory and make sure it is empty. # this is particularly important for indexing and referencing pinecone memory memory = PineconeMemory() memory.clear() print('Using memory of type: ' + memory.\_\_class\_\_.\_\_name\_\_) # Interaction Loop while True: # Send message to AI, get response with Spinner("Thinking... "): assistant\_reply = chat.chat\_with\_ai( prompt, user\_input, full\_message\_history, memory, cfg.fast\_token\_limit) # TODO: This hardcodes the model to use GPT3.5. Make this an argument # Print Assistant thoughts print\_assistant\_thoughts(assistant\_reply) # Get command name and arguments try: command\_name, arguments = cmd.get\_command(assistant\_reply) except Exception as e: print\_to\_console("Error: \\n", Fore.RED, str(e)) if not cfg.continuous\_mode and next\_action\_count == 0: ### GET USER AUTHORIZATION TO EXECUTE COMMAND ### # Get key press: Prompt the user to press enter to continue or escape # to exit user\_input = "" print\_to\_console( "NEXT ACTION: ", Fore.CYAN, f"COMMAND = {Fore.CYAN}{command\_name}{Style.RESET\_ALL} ARGUMENTS = {Fore.CYAN}{arguments}{Style.RESET\_ALL}") print( f"Enter 'y' to authorise command, 'y -N' to run N continuous commands, 'n' to exit program, or enter feedback for {ai\_name}...", flush=True) while True: console\_input = input(Fore.MAGENTA + "Input:" + Style.RESET\_ALL) if console\_input.lower() == "y": user\_input = "GENERATE NEXT COMMAND JSON" break elif console\_input.lower().startswith("y -"): try: next\_action\_count = abs(int(console\_input.split(" ")\[1\])) user\_input = "GENERATE NEXT COMMAND JSON" except ValueError: print("Invalid input format. Please enter 'y -n' where n is the number of continuous tasks.") continue break elif console\_input.lower() == "n": user\_input = "EXIT" break else: user\_input = console\_input command\_name = "human\_feedback" break if user\_input == "GENERATE NEXT COMMAND JSON": print\_to\_console( "-=-=-=-=-=-=-= COMMAND AUTHORISED BY USER -=-=-=-=-=-=-=", Fore.MAGENTA, "") elif user\_input == "EXIT": print("Exiting...", flush=True) break else: # Print command print\_to\_console( "NEXT ACTION: ", Fore.CYAN, f"COMMAND = {Fore.CYAN}{command\_name}{Style.RESET\_ALL} ARGUMENTS = {Fore.CYAN}{arguments}{Style.RESET\_ALL}") # Execute command if command\_name.lower() == "error": result = f"Command {command\_name} threw the following error: " + arguments elif command\_name == "human\_feedback": result = f"Human feedback: {user\_input}" else: result = f"Command {command\_name} returned: {cmd.execute\_command(command\_name, arguments)}" if next\_action\_count > 0: next\_action\_count -= 1 memory\_to\_add = f"Assistant Reply: {assistant\_reply} " \\ f"\\nResult: {result} " \\ f"\\nHuman Feedback: {user\_input} " memory.add(memory\_to\_add) # Check if there's a result from the command append it to the message # history if result is not None: full\_message\_history.append(chat.create\_chat\_message("system", result)) print\_to\_console("SYSTEM: ", Fore.YELLOW, result) else: full\_message\_history.append( chat.create\_chat\_message( "system", "Unable to execute command")) print\_to\_console("SYSTEM: ", Fore.YELLOW, "Unable to execute command")

1 / 1

Penjelasan Kode
===============

Kode ini adalah skrip Python yang menggunakan beberapa modul untuk membantu dalam menjalankan AI berbasis GPT. AI ini menerima masukan, memprosesnya, dan menghasilkan respons yang relevan.

Impor Modul
-----------

Pertama, kode ini mengimpor beberapa modul yang diperlukan:

python

```python
import json
import random
import commands as cmd
from memory import PineconeMemory
import data
import chat
from colorama import Fore, Style
from spinner import Spinner
import time
import speak
from enum import Enum, auto
import sys
from config import Config
from json_parser import fix_and_parse_json
from ai_config import AIConfig
import traceback
import yaml
import argparse
```

Modul-modul ini digunakan untuk berbagai tujuan seperti membaca dan menulis data, mengatur warna teks pada terminal, memanipulasi waktu, dan berkomunikasi dengan AI.

Fungsi
------

Skrip ini mendefinisikan beberapa fungsi yang digunakan untuk memudahkan pemrosesan dan interaksi dengan AI. Berikut adalah beberapa fungsi utama:

### print\_to\_console

Fungsi ini mencetak teks ke konsol dengan warna yang ditentukan dan opsional bisa menggunakan teks ucapan (speak\_text).

python

```python
def print_to_console(
        title,
        title_color,
        content,
        speak_text=False,
        min_typing_speed=0.05,
        max_typing_speed=0.01):
    ...
```

### print\_assistant\_thoughts

Fungsi ini mencetak respons AI dan mem-parsing JSON dari respons tersebut.

python

```python
def print_assistant_thoughts(assistant_reply):
    ...
```

### load\_variables

Fungsi ini memuat variabel dari file konfigurasi YAML.

python

```python
def load_variables(config_file="config.yaml"):
    ...
```

### construct\_prompt

Fungsi ini membuat prompt yang akan digunakan untuk mengajukan pertanyaan atau perintah ke AI.

python

```python
def construct_prompt():
    ...
```

### prompt\_user

Fungsi ini meminta pengguna untuk mengisi informasi tentang AI, seperti nama, peran, dan tujuan.

python

```python
def prompt_user():
    ...
```

### parse\_arguments

Fungsi ini mem-parsing argumen baris perintah yang diterima skrip ini.

python

```python
def parse_arguments():
    ...
```

Interaksi Utama
---------------

Setelah mengimpor modul yang diperlukan dan mendefinisikan fungsi yang diperlukan, skrip ini menjalankan interaksi utama dengan AI. Ini melibatkan mengirim pesan ke AI, menerima respons, dan memproses respons tersebut untuk menghasilkan tindakan yang sesuai.

python

```python
# Initialize memory and make sure it is empty.
memory = PineconeMemory()
memory.clear()

# Interaction Loop
while True:
    ...
```

Skrip ini akan terus berinteraksi dengan AI sampai pengguna memutuskan untuk menghentikannya.

> 1 / 1
> 
> lanjutkan

1 / 1

Baik, berikut adalah beberapa bagian yang tersisa dari kode ini.

### Spinner

python

```python
from spinner import Spinner
```

Bagian ini mengimpor modul `Spinner`. Spinner adalah alat visual yang menunjukkan bahwa ada proses berjalan saat program menunggu hasil atau bekerja pada sesuatu. Dalam kasus ini, spinner mungkin digunakan untuk menunjukkan bahwa asisten AI sedang memikirkan jawaban.

### argparse

python

```python
import argparse
```

Argparse adalah pustaka bawaan Python yang digunakan untuk menguraikan argumen baris perintah yang diteruskan ke skrip. Argumen baris perintah adalah opsi yang dapat Anda berikan saat menjalankan program dari baris perintah atau terminal. Ini memungkinkan Anda untuk mengaktifkan atau menonaktifkan fitur tertentu atau mengonfigurasi program secara dinamis saat dijalankan.

### Bagian Parse Argumen

python

```python
def parse_arguments():
    # ... (implementation details) ...
```

Bagian ini mendefinisikan fungsi `parse_arguments()` yang digunakan untuk menguraikan argumen yang diteruskan ke skrip ini melalui baris perintah. Argumen seperti `--continuous`, `--speak`, `--debug`, dan `--gpt3only` dapat diteruskan ke skrip ini dan akan mempengaruhi perilaku program. Misalnya, jika Anda memberikan argumen `--speak`, program akan mengaktifkan mode berbicara dan asisten AI akan berbicara menggunakan suara sintesis.

Saya harap ini memberikan gambaran lebih lanjut tentang cara kerja kode ini. Jika Anda memiliki pertanyaan lebih lanjut atau membutuhkan penjelasan lebih rinci tentang bagian tertentu, jangan ragu untuk bertanya.

> 1 / 1
> 
> Saya tidak tahu cara membuat kode, tetapi saya ingin memahami cara kerjanya. Jelaskan kode berikut ini kepada saya dengan cara yang dapat dimengerti oleh orang non-teknis. Selalu gunakan Markdown dengan format yang bagus agar lebih mudah diikuti. Aturlah berdasarkan bagian dengan tajuk. Sertakan referensi ke kode sebagai blok kode penurunan harga di setiap bagian. Kode: import time import openai from dotenv import load\_dotenv from config import Config import token\_counter cfg = Config() from llm\_utils import create\_chat\_completion def create\_chat\_message(role, content): """ Create a chat message with the given role and content. Args: role (str): The role of the message sender, e.g., "system", "user", or "assistant". content (str): The content of the message. Returns: dict: A dictionary containing the role and content of the message. """ return {"role": role, "content": content} def generate\_context(prompt, relevant\_memory, full\_message\_history, model): current\_context = \[ create\_chat\_message( "system", prompt), create\_chat\_message( "system", f"Permanent memory: {relevant\_memory}")\] # Add messages from the full message history until we reach the token limit next\_message\_to\_add\_index = len(full\_message\_history) - 1 insertion\_index = len(current\_context) # Count the currently used tokens current\_tokens\_used = token\_counter.count\_message\_tokens(current\_context, model) return next\_message\_to\_add\_index, current\_tokens\_used, insertion\_index, current\_context # TODO: Change debug from hardcode to argument def chat\_with\_ai( prompt, user\_input, full\_message\_history, permanent\_memory, token\_limit, debug=False): while True: try: """ Interact with the OpenAI API, sending the prompt, user input, message history, and permanent memory. Args: prompt (str): The prompt explaining the rules to the AI. user\_input (str): The input from the user. full\_message\_history (list): The list of all messages sent between the user and the AI. permanent\_memory (Obj): The memory object containing the permanent memory. token\_limit (int): The maximum number of tokens allowed in the API call. Returns: str: The AI's response. """ model = cfg.fast\_llm\_model # TODO: Change model from hardcode to argument # Reserve 1000 tokens for the response if debug: print(f"Token limit: {token\_limit}") send\_token\_limit = token\_limit - 1000 relevant\_memory = permanent\_memory.get\_relevant(str(full\_message\_history\[-5:\]), 10) if debug: print('Memory Stats: ', permanent\_memory.get\_stats()) next\_message\_to\_add\_index, current\_tokens\_used, insertion\_index, current\_context = generate\_context( prompt, relevant\_memory, full\_message\_history, model) while current\_tokens\_used > 2500: # remove memories until we are under 2500 tokens relevant\_memory = relevant\_memory\[1:\] next\_message\_to\_add\_index, current\_tokens\_used, insertion\_index, current\_context = generate\_context( prompt, relevant\_memory, full\_message\_history, model) current\_tokens\_used += token\_counter.count\_message\_tokens(\[create\_chat\_message("user", user\_input)\], model) # Account for user input (appended later) while next\_message\_to\_add\_index >= 0: # print (f"CURRENT TOKENS USED: {current\_tokens\_used}") message\_to\_add = full\_message\_history\[next\_message\_to\_add\_index\] tokens\_to\_add = token\_counter.count\_message\_tokens(\[message\_to\_add\], model) if current\_tokens\_used + tokens\_to\_add > send\_token\_limit: break # Add the most recent message to the start of the current context, after the two system prompts. current\_context.insert(insertion\_index, full\_message\_history\[next\_message\_to\_add\_index\]) # Count the currently used tokens current\_tokens\_used += tokens\_to\_add # Move to the next most recent message in the full message history next\_message\_to\_add\_index -= 1 # Append user input, the length of this is accounted for above current\_context.extend(\[create\_chat\_message("user", user\_input)\]) # Calculate remaining tokens tokens\_remaining = token\_limit - current\_tokens\_used # assert tokens\_remaining >= 0, "Tokens remaining is negative. This should never happen, please submit a bug report at https://www.github.com/Torantulino/Auto-GPT" # Debug print the current context if debug: print(f"Token limit: {token\_limit}") print(f"Send Token Count: {current\_tokens\_used}") print(f"Tokens remaining for response: {tokens\_remaining}") print("------------ CONTEXT SENT TO AI ---------------") for message in current\_context: # Skip printing the prompt if message\["role"\] == "system" and message\["content"\] == prompt: continue print( f"{message\['role'\].capitalize()}: {message\['content'\]}") print() print("----------- END OF CONTEXT ----------------") # TODO: use a model defined elsewhere, so that model can contain temperature and other settings we care about assistant\_reply = create\_chat\_completion( model=model, messages=current\_context, max\_tokens=tokens\_remaining, ) # Update full message history full\_message\_history.append( create\_chat\_message( "user", user\_input)) full\_message\_history.append( create\_chat\_message( "assistant", assistant\_reply)) return assistant\_reply except openai.error.RateLimitError: # TODO: WHen we switch to langchain, this is built in print("Error: ", "API Rate Limit Reached. Waiting 10 seconds...") time.sleep(10)

1 / 1

Berikut adalah penjelasan kode tersebut dalam istilah yang mudah dimengerti, dengan format Markdown yang baik:

Impor Modul
-----------

python

```python
import time
import openai
from dotenv import load_dotenv
from config import Config
import token_counter
```

Bagian ini mengimpor modul yang akan digunakan dalam kode:

*   `time`: modul waktu Python untuk mengelola waktu.
*   `openai`: pustaka yang memudahkan berinteraksi dengan API OpenAI.
*   `dotenv`: pustaka untuk memuat variabel lingkungan dari file .env.
*   `config`: modul konfigurasi yang berisi pengaturan aplikasi.
*   `token_counter`: modul untuk menghitung jumlah token dalam teks.

Membuat Pesan Obrolan
---------------------

python

```python
def create_chat_message(role, content):
```

Fungsi ini membuat pesan obrolan dengan peran dan konten yang diberikan. Pesan obrolan akan menjadi bagian dari konteks yang dikirim ke API.

Menghasilkan Konteks
--------------------

python

```python
def generate_context(prompt, relevant_memory, full_message_history, model):
```

Fungsi ini menghasilkan konteks yang akan dikirim ke API. Ini mencakup:

*   Prompt yang menjelaskan aturan kepada AI.
*   Memori yang relevan berdasarkan riwayat pesan.
*   Riwayat pesan lengkap antara pengguna dan AI.

Konteks ini akan dibatasi oleh jumlah token yang diizinkan dalam panggilan API.

Berbicara dengan AI
-------------------

python

```python
def chat_with_ai(prompt, user_input, full_message_history, permanent_memory, token_limit, debug=False):
```

Fungsi ini mengatur interaksi dengan API OpenAI. Ini mengirim prompt, input pengguna, riwayat pesan, dan memori permanen ke API. Fungsi ini juga mengelola jumlah token yang diizinkan dalam panggilan API dan memastikan bahwa batasan token tidak dilampaui.

Bagian ini mencakup beberapa langkah:

1.  Memilih model yang akan digunakan.
2.  Menentukan memori yang relevan berdasarkan riwayat pesan terakhir.
3.  Menghasilkan konteks yang akan dikirim ke API.
4.  Memastikan jumlah token yang diizinkan tidak dilampaui.
5.  Membuat panggilan ke API dan mendapatkan balasan dari asisten AI.
6.  Menangani kasus di mana batas API tercapai dan menunggu sebelum mencoba lagi.

Saya harap ini memberikan gambaran yang lebih mudah dipahami tentang cara kerja kode ini. Jika Anda memiliki pertanyaan lebih lanjut atau membutuhkan penjelasan lebih rinci tentang bagian tertentu, jangan ragu untuk bertanya.

> 1 / 1
> 
> Saya tidak tahu cara membuat kode, tetapi saya ingin memahami cara kerjanya. Jelaskan kode berikut ini kepada saya dengan cara yang dapat dimengerti oleh orang non-teknis. Selalu gunakan Markdown dengan format yang bagus agar lebih mudah diikuti. Aturlah berdasarkan bagian dengan tajuk. Sertakan referensi ke kode sebagai blok kode penurunan harga di setiap bagian. Kode: import os import openai from dotenv import load\_dotenv # Load environment variables from .env file load\_dotenv() class Singleton(type): """ Singleton metaclass for ensuring only one instance of a class. """ \_instances = {} def \_\_call\_\_(cls, \*args, \*\*kwargs): if cls not in cls.\_instances: cls.\_instances\[cls\] = super( Singleton, cls).\_\_call\_\_( \*args, \*\*kwargs) return cls.\_instances\[cls\] class Config(metaclass=Singleton): """ Configuration class to store the state of bools for different scripts access. """ def \_\_init\_\_(self): self.continuous\_mode = False self.speak\_mode = False # TODO - make these models be self-contained, using langchain, so we can configure them once and call it good self.fast\_llm\_model = os.getenv("FAST\_LLM\_MODEL", "gpt-3.5-turbo") self.smart\_llm\_model = os.getenv("SMART\_LLM\_MODEL", "gpt-4") self.fast\_token\_limit = int(os.getenv("FAST\_TOKEN\_LIMIT", 4000)) self.smart\_token\_limit = int(os.getenv("SMART\_TOKEN\_LIMIT", 8000)) self.openai\_api\_key = os.getenv("OPENAI\_API\_KEY") self.use\_azure = False self.use\_azure = os.getenv("USE\_AZURE") == 'True' if self.use\_azure: self.openai\_api\_base = os.getenv("OPENAI\_API\_BASE") self.openai\_api\_version = os.getenv("OPENAI\_API\_VERSION") self.openai\_deployment\_id = os.getenv("OPENAI\_DEPLOYMENT\_ID") openai.api\_type = "azure" openai.api\_base = self.openai\_api\_base openai.api\_version = self.openai\_api\_version self.elevenlabs\_api\_key = os.getenv("ELEVENLABS\_API\_KEY") self.google\_api\_key = os.getenv("GOOGLE\_API\_KEY") self.custom\_search\_engine\_id = os.getenv("CUSTOM\_SEARCH\_ENGINE\_ID") self.pinecone\_api\_key = os.getenv("PINECONE\_API\_KEY") self.pinecone\_region = os.getenv("PINECONE\_ENV") # User agent headers to use when browsing web # Some websites might just completely deny request with an error code if no user agent was found. self.user\_agent\_header = {"User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_15\_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36"} # Initialize the OpenAI API client openai.api\_key = self.openai\_api\_key def set\_continuous\_mode(self, value: bool): self.continuous\_mode = value def set\_speak\_mode(self, value: bool): self.speak\_mode = value def set\_fast\_llm\_model(self, value: str): self.fast\_llm\_model = value def set\_smart\_llm\_model(self, value: str): self.smart\_llm\_model = value def set\_fast\_token\_limit(self, value: int): self.fast\_token\_limit = value def set\_smart\_token\_limit(self, value: int): self.smart\_token\_limit = value def set\_openai\_api\_key(self, value: str): self.openai\_api\_key = value def set\_elevenlabs\_api\_key(self, value: str): self.elevenlabs\_api\_key = value def set\_google\_api\_key(self, value: str): self.google\_api\_key = value def set\_custom\_search\_engine\_id(self, value: str): self.custom\_search\_engine\_id = value def set\_pinecone\_api\_key(self, value: str): self.pinecone\_api\_key = value def set\_pinecone\_region(self, value: str): self.pinecone\_region = value

1 / 1

Berikut adalah penjelasan kode tersebut dalam istilah yang mudah dimengerti, dengan format Markdown yang baik:

Impor Modul
-----------

python

```python
import os
import openai
from dotenv import load_dotenv
load_dotenv()
```

Bagian ini mengimpor modul yang akan digunakan dalam kode:

*   `os`: modul operasi sistem Python untuk berinteraksi dengan sistem operasi.
*   `openai`: pustaka yang memudahkan berinteraksi dengan API OpenAI.
*   `dotenv`: pustaka untuk memuat variabel lingkungan dari file .env.

Singleton Metaclass
-------------------

python

```python
class Singleton(type):
```

Metaclass Singleton digunakan untuk memastikan hanya satu instance dari kelas yang ada. Ini berguna untuk menghindari duplikasi dan memastikan semua bagian kode menggunakan instance yang sama.

Kelas Config
------------

python

```python
class Config(metaclass=Singleton):
```

Kelas Config digunakan untuk menyimpan konfigurasi yang akan diakses oleh skrip yang berbeda. Beberapa contoh konfigurasi yang disimpan di sini meliputi:

*   Model AI yang akan digunakan (mis. GPT-3.5 Turbo atau GPT-4).
*   Batasan token untuk setiap model.
*   Kunci API untuk layanan yang berbeda seperti OpenAI, Google, dan Pinecone.
*   Pengaturan mode berkelanjutan dan mode bicara.
*   Pengaturan lainnya seperti user agent header dan konfigurasi layanan eksternal.

Kelas ini juga menyediakan beberapa metode untuk mengatur nilai konfigurasi yang berbeda.

Saya harap ini memberikan gambaran yang lebih mudah dipahami tentang cara kerja kode ini. Jika Anda memiliki pertanyaan lebih lanjut atau membutuhkan penjelasan lebih rinci tentang bagian tertentu, jangan ragu untuk bertanya.

> 1 / 1
> 
> Saya tidak tahu cara membuat kode, tetapi saya ingin memahami cara kerjanya. Jelaskan kode berikut ini kepada saya dengan cara yang dapat dimengerti oleh orang non-teknis. Selalu gunakan Markdown dengan format yang bagus agar lebih mudah diikuti. Aturlah berdasarkan bagian dengan tajuk. Sertakan referensi ke kode sebagai blok kode penurunan harga di setiap bagian. Kode: #!/usr/bin/env python3 import os import openai import pinecone import time import sys from collections import deque from typing import Dict, List from dotenv import load\_dotenv import os # Set Variables load\_dotenv() # Set API Keys OPENAI\_API\_KEY = os.getenv("OPENAI\_API\_KEY", "") assert OPENAI\_API\_KEY, "OPENAI\_API\_KEY environment variable is missing from .env" OPENAI\_API\_MODEL = os.getenv("OPENAI\_API\_MODEL", "gpt-3.5-turbo") assert OPENAI\_API\_MODEL, "OPENAI\_API\_MODEL environment variable is missing from .env" if "gpt-4" in OPENAI\_API\_MODEL.lower(): print(f"\\033\[91m\\033\[1m"+"\\n\*\*\*\*\*USING GPT-4. POTENTIALLY EXPENSIVE. MONITOR YOUR COSTS\*\*\*\*\*"+"\\033\[0m\\033\[0m") PINECONE\_API\_KEY = os.getenv("PINECONE\_API\_KEY", "") assert PINECONE\_API\_KEY, "PINECONE\_API\_KEY environment variable is missing from .env" PINECONE\_ENVIRONMENT = os.getenv("PINECONE\_ENVIRONMENT", "us-east1-gcp") assert PINECONE\_ENVIRONMENT, "PINECONE\_ENVIRONMENT environment variable is missing from .env" # Table config YOUR\_TABLE\_NAME = os.getenv("TABLE\_NAME", "") assert YOUR\_TABLE\_NAME, "TABLE\_NAME environment variable is missing from .env" # Project config OBJECTIVE = sys.argv\[1\] if len(sys.argv) > 1 else os.getenv("OBJECTIVE", "") assert OBJECTIVE, "OBJECTIVE environment variable is missing from .env" YOUR\_FIRST\_TASK = os.getenv("FIRST\_TASK", "") assert YOUR\_FIRST\_TASK, "FIRST\_TASK environment variable is missing from .env" #Print OBJECTIVE print("\\033\[96m\\033\[1m"+"\\n\*\*\*\*\*OBJECTIVE\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(OBJECTIVE) # Configure OpenAI and Pinecone openai.api\_key = OPENAI\_API\_KEY pinecone.init(api\_key=PINECONE\_API\_KEY, environment=PINECONE\_ENVIRONMENT) # Create Pinecone index table\_name = YOUR\_TABLE\_NAME dimension = 1536 metric = "cosine" pod\_type = "p1" if table\_name not in pinecone.list\_indexes(): pinecone.create\_index(table\_name, dimension=dimension, metric=metric, pod\_type=pod\_type) # Connect to the index index = pinecone.Index(table\_name) # Task list task\_list = deque(\[\]) def add\_task(task: Dict): task\_list.append(task) def get\_ada\_embedding(text): text = text.replace("\\n", " ") return openai.Embedding.create(input=\[text\], model="text-embedding-ada-002")\["data"\]\[0\]\["embedding"\] def openai\_call(prompt: str, model: str = OPENAI\_API\_MODEL, temperature: float = 0.5, max\_tokens: int = 100): if not model.startswith('gpt-'): # Use completion API response = openai.Completion.create( engine=model, prompt=prompt, temperature=temperature, max\_tokens=max\_tokens, top\_p=1, frequency\_penalty=0, presence\_penalty=0 ) return response.choices\[0\].text.strip() else: # Use chat completion API messages=\[{"role": "user", "content": prompt}\] response = openai.ChatCompletion.create( model=model, messages=messages, temperature=temperature, max\_tokens=max\_tokens, n=1, stop=None, ) return response.choices\[0\].message.content.strip() def task\_creation\_agent(objective: str, result: Dict, task\_description: str, task\_list: List\[str\]): prompt = f"You are an task creation AI that uses the result of an execution agent to create new tasks with the following objective: {objective}, The last completed task has the result: {result}. This result was based on this task description: {task\_description}. These are incomplete tasks: {', '.join(task\_list)}. Based on the result, create new tasks to be completed by the AI system that do not overlap with incomplete tasks. Return the tasks as an array." response = openai\_call(prompt) new\_tasks = response.split('\\n') return \[{"task\_name": task\_name} for task\_name in new\_tasks\] def prioritization\_agent(this\_task\_id: int): global task\_list task\_names = \[t\["task\_name"\] for t in task\_list\] next\_task\_id = int(this\_task\_id)+1 prompt = f"""You are an task prioritization AI tasked with cleaning the formatting of and reprioritizing the following tasks: {task\_names}. Consider the ultimate objective of your team:{OBJECTIVE}. Do not remove any tasks. Return the result as a numbered list, like: #. First task #. Second task Start the task list with number {next\_task\_id}.""" response = openai\_call(prompt) new\_tasks = response.split('\\n') task\_list = deque() for task\_string in new\_tasks: task\_parts = task\_string.strip().split(".", 1) if len(task\_parts) == 2: task\_id = task\_parts\[0\].strip() task\_name = task\_parts\[1\].strip() task\_list.append({"task\_id": task\_id, "task\_name": task\_name}) def execution\_agent(objective: str, task: str) -> str: context=context\_agent(query=objective, n=5) #print("\\n\*\*\*\*\*\*\*RELEVANT CONTEXT\*\*\*\*\*\*\\n") #print(context) prompt =f"You are an AI who performs one task based on the following objective: {objective}.\\nTake into account these previously completed tasks: {context}\\nYour task: {task}\\nResponse:" return openai\_call(prompt, temperature=0.7, max\_tokens=2000) def context\_agent(query: str, n: int): query\_embedding = get\_ada\_embedding(query) results = index.query(query\_embedding, top\_k=n, include\_metadata=True) #print("\*\*\*\*\* RESULTS \*\*\*\*\*") #print(results) sorted\_results = sorted(results.matches, key=lambda x: x.score, reverse=True) return \[(str(item.metadata\['task'\])) for item in sorted\_results\] # Add the first task first\_task = { "task\_id": 1, "task\_name": YOUR\_FIRST\_TASK } add\_task(first\_task) # Main loop task\_id\_counter = 1 while True: if task\_list: # Print the task list print("\\033\[95m\\033\[1m"+"\\n\*\*\*\*\*TASK LIST\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") for t in task\_list: print(str(t\['task\_id'\])+": "+t\['task\_name'\]) # Step 1: Pull the first task task = task\_list.popleft() print("\\033\[92m\\033\[1m"+"\\n\*\*\*\*\*NEXT TASK\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(str(task\['task\_id'\])+": "+task\['task\_name'\]) # Send to execution function to complete the task based on the context result = execution\_agent(OBJECTIVE,task\["task\_name"\]) this\_task\_id = int(task\["task\_id"\]) print("\\033\[93m\\033\[1m"+"\\n\*\*\*\*\*TASK RESULT\*\*\*\*\*\\n"+"\\033\[0m\\033\[0m") print(result) # Step 2: Enrich result and store in Pinecone enriched\_result = {'data': result} # This is where you should enrich the result if needed result\_id = f"result\_{task\['task\_id'\]}" vector = enriched\_result\['data'\] # extract the actual result from the dictionary index.upsert(\[(result\_id, get\_ada\_embedding(vector),{"task":task\['task\_name'\],"result":result})\]) # Step 3: Create new tasks and reprioritize task list new\_tasks = task\_creation\_agent(OBJECTIVE,enriched\_result, task\["task\_name"\], \[t\["task\_name"\] for t in task\_list\]) for new\_task in new\_tasks: task\_id\_counter += 1 new\_task.update({"task\_id": task\_id\_counter}) add\_task(new\_task) prioritization\_agent(this\_task\_id) time.sleep(1) # Sleep before checking the task list again

1 / 1

Kode ini pada dasarnya adalah skrip Python yang mengintegrasikan OpenAI dan Pinecone untuk menciptakan, mengeksekusi, dan menyimpan hasil dari berbagai tugas yang diberikan. Mari kita jelaskan setiap bagian kode dengan lebih jelas.

### Impor dan Pengaturan Awal

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

Bagian ini mengimpor pustaka yang diperlukan, seperti `os`, `openai`, `pinecone`, `time`, `sys`, `collections`, `typing`, dan `dotenv`.

### Pengaturan Variabel Lingkungan

python

```python
load_dotenv()
```

Fungsi ini digunakan untuk memuat variabel lingkungan dari file `.env`.

### Mengatur API Key dan Variabel Lainnya

python

```python
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY", "")
assert OPENAI_API_KEY, "OPENAI_API_KEY environment variable is missing from .env"

OPENAI_API_MODEL = os.getenv("OPENAI_API_MODEL", "gpt-3.5-turbo")
assert OPENAI_API_MODEL, "OPENAI_API_MODEL environment variable is missing from .env"
...
```

Bagian ini mengatur API key dan beberapa variabel lain yang diperlukan untuk menjalankan skrip.

### Konfigurasi OpenAI dan Pinecone

python

```python
openai.api_key = OPENAI_API_KEY
pinecone.init(api_key=PINECONE_API_KEY, environment=PINECONE_ENVIRONMENT)
```

Bagian ini mengkonfigurasi klien OpenAI dan Pinecone menggunakan API key yang disediakan.

### Fungsi Utama

Skrip ini memiliki beberapa fungsi utama yang menjalankan berbagai aspek dari skenario tugas:

1.  `add_task(task: Dict)`: Menambahkan tugas ke daftar tugas.
2.  `get_ada_embedding(text)`: Mengambil embedding teks dari model OpenAI.
3.  `openai_call(prompt: str, model: str, temperature: float, max_tokens: int)`: Memanggil model OpenAI dengan prompt yang diberikan.
4.  `task_creation_agent(objective: str, result: Dict, task_description: str, task_list: List[str])`: Membuat tugas baru berdasarkan hasil tugas sebelumnya.
5.  `prioritization_agent(this_task_id: int)`: Menyusun ulang tugas yang ada dalam daftar berdasarkan prioritas.
6.  `execution_agent(objective: str, task: str) -> str`: Menjalankan tugas berdasarkan konteks yang diberikan.
7.  `context_agent(query: str, n: int)`: Mengambil konteks terkait dari tugas yang telah diselesaikan sebelumnya.

### Loop Utama

python

```python
while True:
    ...
```

Loop utama ini menjalankan proses berikut secara berulang:

1.  Mengambil tugas berikutnya dari daftar tugas.
2.  Menjalankan tugas tersebut menggunakan `execution_agent`.
3.  Menyimpan hasil tugas dalam Pinecone.
4.  Membuat tugas baru berdasarkan hasil tugas sebelumnya menggunakan `task_creation_agent`.
5.  Menambahkan tugas baru ke daftar tugas dan menyusun ulang daftar tugas berdasarkan prioritas dengan menggunakan `prioritization_agent`.
6.  Mengul

> 1 / 1
> 
> lanjutkan

1 / 1

angi proses tersebut hingga semua tugas selesai atau diprioritaskan ulang.

### Menambahkan Tugas Pertama dan Memulai Loop

python

```python
first_task = {
    "task_id": 1,
    "task_name": YOUR_FIRST_TASK
}

add_task(first_task)
```

Sebelum memulai loop utama, skrip menambahkan tugas pertama ke daftar tugas menggunakan `add_task` dengan `YOUR_FIRST_TASK` sebagai nama tugas.

Kemudian, skrip memulai loop utama yang menjalankan tugas, membuat tugas baru, dan menyusun ulang prioritas tugas secara berulang.

python

```python
time.sleep(1)  # Sleep before checking the task list again
```

Di akhir setiap iterasi loop, skrip menunggu selama satu detik sebelum melanjutkan ke iterasi berikutnya. Hal ini memberikan jeda kecil antara setiap tugas yang dijalankan.

Secara keseluruhan, kode ini menciptakan sistem tugas yang dapat menambahkan, menjalankan, dan menyimpan hasil dari berbagai tugas yang diberikan menggunakan OpenAI dan Pinecone. Skrip ini juga dapat membuat tugas baru berdasarkan hasil tugas sebelumnya dan menyusun ulang daftar tugas berdasarkan prioritas.