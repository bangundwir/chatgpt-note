Model: Default (GPT-3.5)

> import os import openai import webbrowser # Setup API key openai.api\_key = os.environ\['OPENAI\_API\_KEY'\] def generate\_response(prompt): completions = openai.Completion.create( engine="text-davinci-002", prompt=prompt, max\_tokens=1024, n=1, stop=None, temperature=0.5, ) message = completions.choices\[0\].text return message # Generate responses to questions questions = \[ "What is Python?", "What are the benefits of using Python?", "What can you do with Python?", "What are some popular libraries in Python?" \] responses = \[\] for question in questions: response = generate\_response(question) responses.append(response) # Generate HTML output html = "<html>\\n" html += "<head>\\n" html += "<title>Blog created with OpenAI's GPT-3 API</title>\\n" html += "</head>\\n" html += "<body>\\n" for i in range(len(questions)): html += "<h2>" + questions\[i\] + "</h2>\\n" html += "<p>" + responses\[i\] + "</p>\\n" html += "</body>\\n" html += "</html>" # Write HTML to file filename = "blog.html" with open(filename, "w") as file: file.write(html) # Open HTML in browser webbrowser.open(filename) Tambahkan css Biar Tampilan lebih menarik buatakan tampilanya menjadi random

Untuk menambahkan CSS ke dalam HTML, Anda bisa menambahkan tag `<style>` di dalam tag `<head>`. Anda bisa menambahkan CSS styling yang Anda inginkan di dalam tag tersebut.

Berikut contoh CSS styling sederhana yang bisa Anda tambahkan ke dalam kode HTML tersebut:

php

```php
<style>
  body {
    background-color: #f0f0f0;
    font-family: Arial, sans-serif;
    font-size: 16px;
    line-height: 1.5;
    margin: 0;
    padding: 0;
  }

  h2 {
    color: #333;
    font-size: 24px;
    margin: 30px 0 10px;
    text-align: center;
  }

  p {
    color: #666;
    font-size: 18px;
    margin: 0 auto;
    max-width: 800px;
    padding: 20px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }
</style>
```

Anda bisa menempelkan CSS styling di atas ke dalam tag `<head>` di HTML Anda.

Untuk membuat tampilan menjadi lebih acak, Anda bisa menggunakan fungsi `random` dari modul `random` di Python untuk mengacak urutan pertanyaan dan jawaban yang ditampilkan di halaman HTML. Berikut contoh implementasinya:

makefile

```makefile
import random

# Generate HTML output
html = "<html>\n"
html += "<head>\n"
html += "<title>Blog created with OpenAI's GPT-3 API</title>\n"
html += "<style>\n"
html += "  /* Your CSS styling here */\n"
html += "</style>\n"
html += "</head>\n"
html += "<body>\n"

# Shuffle the questions and responses
random.shuffle(questions)
random.shuffle(responses)

for i in range(len(questions)):
    html += "<h2>" + questions[i] + "</h2>\n"
    html += "<p>" + responses[i] + "</p>\n"

html += "</body>\n"
html += "</html>"

# Write HTML to file
filename = "blog.html"
with open(filename, "w") as file:
    file.write(html)

# Open HTML in browser
webbrowser.open(filename)
```

Dalam contoh kode di atas, fungsi `random.shuffle()` digunakan untuk mengacak urutan pertanyaan dan jawaban di dalam list `questions` dan `responses`. Anda bisa menempatkan kode ini sebelum loop `for` yang menampilkan pertanyaan dan jawaban di halaman HTML.

> butakan dalam satu script agar ada css nya juga

Berikut adalah contoh script Python yang menghasilkan halaman HTML dengan CSS styling yang didefinisikan di dalam tag `<style>`:

python

```python
import os
import openai
import webbrowser
import random

# Setup API key
openai.api_key = os.environ['OPENAI_API_KEY']

def generate_response(prompt):
    completions = openai.Completion.create(
        engine="text-davinci-002",
        prompt=prompt,
        max_tokens=1024,
        n=1,
        stop=None,
        temperature=0.5,
    )

    message = completions.choices[0].text
    return message

# Generate responses to questions
questions = [
    "What is Python?",
    "What are the benefits of using Python?",
    "What can you do with Python?",
    "What are some popular libraries in Python?"
]

responses = []
for question in questions:
    response = generate_response(question)
    responses.append(response)

# Shuffle the questions and responses
random.shuffle(questions)
random.shuffle(responses)

# Generate HTML output
html = f"""
<html>
<head>
<title>Blog created with OpenAI's GPT-3 API</title>
<style>
  body {{
    background-color: #f0f0f0;
    font-family: Arial, sans-serif;
    font-size: 16px;
    line-height: 1.5;
    margin: 0;
    padding: 0;
  }}

  h2 {{
    color: #333;
    font-size: 24px;
    margin: 30px 0 10px;
    text-align: center;
  }}

  p {{
    color: #666;
    font-size: 18px;
    margin: 0 auto;
    max-width: 800px;
    padding: 20px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }}
</style>
</head>
<body>
"""

for i in range(len(questions)):
    html += f"<h2>{questions[i]}</h2>\n"
    html += f"<p>{responses[i]}</p>\n"

html += """
</body>
</html>
"""

# Write HTML to file
filename = "blog.html"
with open(filename, "w") as file:
    file.write(html)

# Open HTML in browser
webbrowser.open(filename)
```

Anda dapat menempelkan kode CSS styling di dalam tag `<style>` di bagian `<head>` dari kode HTML yang dihasilkan. Gunakan tanda kurung kurawal (`{{` dan `}}`) untuk menghindari bentrok antara tanda kurung kurawal dengan format string Python (`{}`) saat menuliskan CSS styling.

Kode di atas juga menggunakan fungsi `random.shuffle()` untuk mengacak urutan pertanyaan dan jawaban sebelum menampilkan mereka di halaman HTML.