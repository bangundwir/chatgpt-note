Model: GPT-4

> 1 / 1
> 
> import os import openai openai.api\_key = os.getenv("OPENAI\_API\_KEY") while True: question = input("\\033\[34mWhat is your question?\\n\\033\[0m") if question.lower() == "exit": print("\\033\[31mGoodbye!\\033\[0m") break completion = openai.ChatCompletion.create( model="gpt-3.5-turbo", messages=\[ {"role": "system", "content": "You are a helpful assistant. Answer the given question."}, {"role": "user", "content": question} \] ) print("\\033\[32m" + completion.choices\[0\].message.content + "\\n") Tolong tambahkan fiture menyimpan hasilnya di folder data dan meyimpanya sesuai nama dari yang inputkan dan buatakan agar tampilanya lebih menarik lagi

1 / 1

Untuk menyimpan hasil percakapan dan menyimpannya sesuai dengan nama yang diinputkan, Anda bisa menggunakan kode berikut ini:

python

```python
import os
import openai
import json

openai.api_key = os.getenv("OPENAI_API_KEY")

# Buat direktori 'data' jika belum ada
if not os.path.exists("data"):
    os.makedirs("data")

def save_to_file(filename, content):
    with open(f"data/{filename}.json", "w") as f:
        json.dump(content, f)

def input_with_color(prompt, color_code):
    return input(f"\033[{color_code}m{prompt}\033[0m")

while True:
    question = input_with_color("What is your question?\n", 34)

    if question.lower() == "exit":
        print("\033[31mGoodbye!\033[0m")
        break

    completion = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a helpful assistant. Answer the given question."},
            {"role": "user", "content": question}
        ]
    )

    answer = completion.choices[0].message.content
    print("\033[32m" + answer + "\n")

    user_name = input_with_color("Please enter your name: ", 34)
    conversation_data = {
        "user_name": user_name,
        "question": question,
        "answer": answer
    }
    save_to_file(user_name, conversation_data)

    print("\033[33mData saved successfully!\033[0m\n")
```

Kode ini akan menambahkan fitur penyimpanan hasil percakapan dalam folder 'data' dan menyimpannya sesuai dengan nama pengguna yang diinputkan. Selain itu, kode ini juga menggunakan pewarnaan teks untuk membuat tampilan lebih menarik.

> 1 / 1
> 
> tolong perbaiki memyipan nya bukan dengan username tapi dengan input question nya

1 / 1

Tentu, berikut adalah kode yang telah diperbarui untuk menyimpan hasil percakapan berdasarkan input pertanyaan sebagai nama file:

python

```python
import os
import openai
import json

openai.api_key = os.getenv("OPENAI_API_KEY")

# Buat direktori 'data' jika belum ada
if not os.path.exists("data"):
    os.makedirs("data")

def save_to_file(filename, content):
    with open(f"data/{filename}.json", "w") as f:
        json.dump(content, f)

def input_with_color(prompt, color_code):
    return input(f"\033[{color_code}m{prompt}\033[0m")

while True:
    question = input_with_color("What is your question?\n", 34)

    if question.lower() == "exit":
        print("\033[31mGoodbye!\033[0m")
        break

    completion = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a helpful assistant. Answer the given question."},
            {"role": "user", "content": question}
        ]
    )

    answer = completion.choices[0].message.content
    print("\033[32m" + answer + "\n")

    # Menggunakan pertanyaan sebagai nama file dengan menggantikan karakter yang tidak valid
    filename = ''.join(c if c.isalnum() or c.isspace() else '_' for c in question).rstrip()
    
    conversation_data = {
        "question": question,
        "answer": answer
    }
    save_to_file(filename, conversation_data)

    print("\033[33mData saved successfully!\033[0m\n")
```

Kode di atas akan menyimpan hasil percakapan dalam folder 'data' dengan menggunakan input pertanyaan sebagai nama file. Karakter yang tidak valid dalam pertanyaan akan digantikan dengan garis bawah (\_).

> 1 / 1
> 
> tambahkan fiture untuk mengetahui pengunaan tokenya

1 / 1

Untuk menambahkan fitur yang menampilkan jumlah token yang digunakan dalam permintaan API, Anda dapat memperbarui kode seperti berikut:

python

```python
import os
import openai
import json

openai.api_key = os.getenv("OPENAI_API_KEY")

# Buat direktori 'data' jika belum ada
if not os.path.exists("data"):
    os.makedirs("data")

def save_to_file(filename, content):
    with open(f"data/{filename}.json", "w") as f:
        json.dump(content, f)

def input_with_color(prompt, color_code):
    return input(f"\033[{color_code}m{prompt}\033[0m")

while True:
    question = input_with_color("What is your question?\n", 34)

    if question.lower() == "exit":
        print("\033[31mGoodbye!\033[0m")
        break

    completion = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a helpful assistant. Answer the given question."},
            {"role": "user", "content": question}
        ]
    )

    answer = completion.choices[0].message.content
    print("\033[32m" + answer + "\n")

    # Menampilkan jumlah token yang digunakan
    tokens_used = completion.choices[0].usage["total_tokens"]
    print("\033[36mTokens used: " + str(tokens_used) + "\033[0m\n")

    # Menggunakan pertanyaan sebagai nama file dengan menggantikan karakter yang tidak valid
    filename = ''.join(c if c.isalnum() or c.isspace() else '_' for c in question).rstrip()

    conversation_data = {
        "question": question,
        "answer": answer
    }
    save_to_file(filename, conversation_data)

    print("\033[33mData saved successfully!\033[0m\n")
```

Kode di atas menambahkan fitur untuk menampilkan jumlah token yang digunakan dalam permintaan API setelah menerima jawaban dari model. Jumlah token digunakan akan ditampilkan dalam warna cyan.