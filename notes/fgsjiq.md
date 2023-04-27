Model: GPT-4

> Tuliskan kode (dan beritahu saya cara menjalankannya) yang akan mengambil apa pun yang ada di clipboard Windows saya, kirimkan ke GPT, minta dia untuk meringkas hasilnya menggunakan text-davinci-003, dan tampilkan hasilnya di layar
> 
> 1 / 1

Untuk mengambil teks dari clipboard Windows, mengirimkannya ke GPT, dan menampilkan hasil ringkasannya, Anda dapat menggunakan Python bersama dengan beberapa library eksternal. Berikut adalah kode yang Anda perlukan:

1.  Pertama, Anda harus menginstal beberapa library yang diperlukan. Buka terminal dan jalankan perintah berikut:

`pip install pyperclip openai`

2.  Buat file Python (misalnya, `summarize_clipboard.py`) dan tempelkan kode berikut:

python

```python
import pyperclip
import openai
import os

# Ganti dengan API key Anda dari OpenAI
openai.api_key = "YOUR_OPENAI_API_KEY"

def get_clipboard_text():
    return pyperclip.paste()

def send_to_gpt(text):
    response = openai.Completion.create(
        engine="text-davinci-003",
        prompt=f"Please summarize the following text: {text}",
        max_tokens=60,
        n=1,
        stop=None,
        temperature=0.7,
    )

    summary = response.choices[0].text.strip()
    return summary

def main():
    clipboard_text = get_clipboard_text()
    if clipboard_text:
        summary = send_to_gpt(clipboard_text)
        print("Summary:")
        print(summary)
    else:
        print("No text found in clipboard.")

if __name__ == "__main__":
    main()
```

3.  Ganti `"YOUR_OPENAI_API_KEY"` dengan kunci API OpenAI Anda.
    
4.  Untuk menjalankan kode ini, simpan file dan buka terminal di direktori yang sama dengan file tersebut. Jalankan perintah berikut:
    

`python summarize_clipboard.py`

Aplikasi akan mengambil teks dari clipboard Windows Anda, mengirimkannya ke GPT (text-davinci-003), dan menampilkan hasil ringkasan di layar.

1 / 1

> Buatakan agar selalu berjalan ketika melakukan clipboard langsung otomatis meringkas hasilnya
> 
> 1 / 1

Untuk membuat aplikasi yang secara otomatis merangkum teks yang disalin ke clipboard, Anda dapat menggunakan library `pyperclip` dan `threading`. Berikut adalah kode yang diperbarui:

python

```python
import pyperclip
import openai
import os
import time
import threading

# Ganti dengan API key Anda dari OpenAI
openai.api_key = "YOUR_OPENAI_API_KEY"

def get_clipboard_text():
    return pyperclip.paste()

def send_to_gpt(text):
    response = openai.Completion.create(
        engine="text-davinci-003",
        prompt=f"Please summarize the following text: {text}",
        max_tokens=60,
        n=1,
        stop=None,
        temperature=0.7,
    )

    summary = response.choices[0].text.strip()
    return summary

def monitor_clipboard():
    recent_value = get_clipboard_text()
    while True:
        tmp_value = get_clipboard_text()
        if tmp_value != recent_value:
            recent_value = tmp_value
            try:
                summary = send_to_gpt(recent_value)
                print("Summary:")
                print(summary)
            except Exception as e:
                print("An error occurred while processing the text:")
                print(str(e))
        time.sleep(1)

if __name__ == "__main__":
    monitor_thread = threading.Thread(target=monitor_clipboard)
    monitor_thread.start()
```

Kode ini akan menjalankan aplikasi dalam lingkungan yang selalu aktif, memantau perubahan pada clipboard. Setiap kali Anda menyalin teks baru, aplikasi akan secara otomatis merangkum teks tersebut dan menampilkan hasilnya di layar.

Simpan file Python (misalnya, `summarize_clipboard_auto.py`) dan jalankan perintah berikut di terminal di direktori yang sama dengan file tersebut:

`python summarize_clipboard_auto.py`

Aplikasi akan berjalan dan secara otomatis merangkum teks yang disalin ke clipboard.

1 / 1