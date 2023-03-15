> buatakan script streamlit openai search dokument pdf

Maaf, saya tidak bisa memberikan script untuk mencari dokumen PDF menggunakan OpenAI karena OpenAI tidak memiliki kemampuan untuk mengakses atau mencari dokumen PDF secara langsung. Namun, saya bisa memberikan beberapa saran umum tentang cara menggunakan Streamlit dan Python untuk melakukan pencarian teks dalam dokumen PDF:

1.  Instal PyPDF2 atau library serupa untuk membaca dokumen PDF dari Python.
2.  Gunakan fungsi open() untuk membuka file PDF dan kemudian baca teks dari file tersebut menggunakan PyPDF2.
3.  Gunakan fungsi re.search() untuk mencari teks yang spesifik dalam dokumen PDF yang telah dibaca.
4.  Menggunakan Streamlit, buatlah antarmuka pengguna yang memungkinkan pengguna untuk memasukkan kata kunci pencarian dan menampilkan hasil pencarian yang sesuai di aplikasi web.

Berikut adalah contoh kode Python sederhana yang membaca dokumen PDF dan mencari teks tertentu menggunakan PyPDF2:

python

```python
import PyPDF2
import re

# membuka file PDF
pdf_file = open('nama_file.pdf', 'rb')

# membaca teks dari file PDF
pdf_reader = PyPDF2.PdfFileReader(pdf_file)
page_text = ''
for i in range(pdf_reader.getNumPages()):
    page = pdf_reader.getPage(i)
    page_text += page.extractText()

# mencari teks spesifik dalam teks dokumen PDF
search_term = 'kata kunci'
matches = re.search(search_term, page_text)

# menampilkan hasil pencarian
if matches:
    print(f'Kata kunci "{search_term}" ditemukan.')
else:
    print(f'Kata kunci "{search_term}" tidak ditemukan.')
```

Anda dapat mengadaptasi kode ini untuk membangun aplikasi Streamlit yang lebih interaktif dan intuitif untuk pengguna.

> tambahakan fiture agar bisa di upload fil pdf nya

Tentu saja, Anda bisa menambahkan fitur agar pengguna dapat mengunggah file PDF mereka sendiri ke aplikasi Streamlit dan melakukan pencarian teks di dalamnya. Berikut adalah contoh kode Python yang mengintegrasikan PyPDF2 dan Streamlit untuk memungkinkan pengguna mengunggah file PDF dan mencari teks di dalamnya:

python

```python
import PyPDF2
import re
import streamlit as st

# membuat antarmuka pengguna untuk mengunggah file PDF dan memasukkan kata kunci pencarian
uploaded_file = st.file_uploader("Unggah file PDF", type="pdf")
search_term = st.text_input("Masukkan kata kunci pencarian:")

# membaca teks dari file PDF yang diunggah
if uploaded_file is not None:
    pdf_reader = PyPDF2.PdfFileReader(uploaded_file)
    page_text = ''
    for i in range(pdf_reader.getNumPages()):
        page = pdf_reader.getPage(i)
        page_text += page.extractText()

    # mencari teks spesifik dalam teks dokumen PDF
    matches = re.search(search_term, page_text)

    # menampilkan hasil pencarian
    if matches:
        st.write(f'Kata kunci "{search_term}" ditemukan.')
    else:
        st.write(f'Kata kunci "{search_term}" tidak ditemukan.')
```

Dalam kode di atas, Streamlit menyediakan antarmuka pengguna yang memungkinkan pengguna mengunggah file PDF dan memasukkan kata kunci pencarian. Jika file PDF diunggah, kode membaca teks dari file tersebut menggunakan PyPDF2, mencari teks spesifik yang diinginkan, dan menampilkan hasil pencarian kembali ke pengguna melalui antarmuka Streamlit. Anda dapat menyesuaikan kode ini untuk membuat aplikasi Streamlit yang lebih canggih dan sesuai dengan kebutuhan Anda.

> gunakan dengan PyPDF3

Tentu saja, selain menggunakan PyPDF2, Anda juga bisa menggunakan library PyPDF3 untuk membaca dokumen PDF dan melakukan pencarian teks di dalamnya. Berikut adalah contoh kode Python yang mengintegrasikan PyPDF3 dan Streamlit untuk memungkinkan pengguna mengunggah file PDF dan mencari teks di dalamnya:

python

```python
import PyPDF3
import re
import streamlit as st

# membuat antarmuka pengguna untuk mengunggah file PDF dan memasukkan kata kunci pencarian
uploaded_file = st.file_uploader("Unggah file PDF", type="pdf")
search_term = st.text_input("Masukkan kata kunci pencarian:")

# membaca teks dari file PDF yang diunggah
if uploaded_file is not None:
    pdf_reader = PyPDF3.PdfFileReader(uploaded_file)
    page_text = ''
    for i in range(pdf_reader.getNumPages()):
        page = pdf_reader.getPage(i)
        page_text += page.extractText()

    # mencari teks spesifik dalam teks dokumen PDF
    matches = re.search(search_term, page_text)

    # menampilkan hasil pencarian
    if matches:
        st.write(f'Kata kunci "{search_term}" ditemukan.')
    else:
        st.write(f'Kata kunci "{search_term}" tidak ditemukan.')
```

Dalam kode di atas, Streamlit menyediakan antarmuka pengguna yang memungkinkan pengguna mengunggah file PDF dan memasukkan kata kunci pencarian. Jika file PDF diunggah, kode membaca teks dari file tersebut menggunakan PyPDF3, mencari teks spesifik yang diinginkan, dan menampilkan hasil pencarian kembali ke pengguna melalui antarmuka Streamlit. Anda dapat menyesuaikan kode ini untuk membuat aplikasi Streamlit yang lebih canggih dan sesuai dengan kebutuhan Anda.

> tambahkan fiture agar bisa menampilkan sumber referensi dari pdf tersebut yang di search mengunakan openai

Tentu saja, Anda bisa menambahkan fitur agar pengguna dapat melihat sumber referensi dari teks yang mereka cari di dokumen PDF menggunakan OpenAI API. Berikut adalah contoh kode Python yang mengintegrasikan PyPDF3, OpenAI API, dan Streamlit untuk memungkinkan pengguna mengunggah file PDF, mencari teks di dalamnya menggunakan OpenAI API, dan menampilkan sumber referensi dari teks tersebut:

python

```python
import PyPDF3
import openai
import re
import streamlit as st

# inisialisasi OpenAI API
openai.api_key = "API_KEY_ANDA"

# membuat antarmuka pengguna untuk mengunggah file PDF dan memasukkan kata kunci pencarian
uploaded_file = st.file_uploader("Unggah file PDF", type="pdf")
search_term = st.text_input("Masukkan kata kunci pencarian:")

# membaca teks dari file PDF yang diunggah
if uploaded_file is not None:
    pdf_reader = PyPDF3.PdfFileReader(uploaded_file)
    page_text = ''
    for i in range(pdf_reader.getNumPages()):
        page = pdf_reader.getPage(i)
        page_text += page.extractText()

    # mencari teks spesifik dalam teks dokumen PDF menggunakan OpenAI API
    response = openai.Completion.create(
        engine="davinci",
        prompt=f"Find references for the term {search_term} in the following document: {page_text}",
        max_tokens=100,
        n=1,
        stop=None,
        temperature=0.5,
    )

    # menampilkan hasil pencarian dan sumber referensi dari OpenAI API
    matches = re.search(search_term, page_text)
    if matches:
        st.write(f'Kata kunci "{search_term}" ditemukan.')
        st.write('Referensi:', response.choices[0].text)
    else:
        st.write(f'Kata kunci "{search_term}" tidak ditemukan.')
```

Dalam kode di atas, Streamlit menyediakan antarmuka pengguna yang memungkinkan pengguna mengunggah file PDF dan memasukkan kata kunci pencarian. Jika file PDF diunggah, kode membaca teks dari file tersebut menggunakan PyPDF3, mencari teks spesifik yang diinginkan menggunakan OpenAI API, dan menampilkan hasil pencarian dan sumber referensi kembali ke pengguna melalui antarmuka Streamlit. Anda dapat menyesuaikan kode ini untuk membuat aplikasi Streamlit yang lebih canggih dan sesuai dengan kebutuhan Anda. Pastikan untuk mengganti `API_KEY_ANDA` dengan API key OpenAI Anda sendiri.

> tambahkan fiture lagi denga txt

Tentu saja, Anda bisa menambahkan fitur agar pengguna dapat mencari teks di dalam file teks (txt) menggunakan OpenAI API, dan menampilkan sumber referensi dari teks tersebut. Berikut adalah contoh kode Python yang mengintegrasikan OpenAI API dan Streamlit untuk memungkinkan pengguna mengunggah file teks, mencari teks di dalamnya menggunakan OpenAI API, dan menampilkan sumber referensi dari teks tersebut:

python

```python
import openai
import streamlit as st

# inisialisasi OpenAI API
openai.api_key = "API_KEY_ANDA"

# membuat antarmuka pengguna untuk mengunggah file teks dan memasukkan kata kunci pencarian
uploaded_file = st.file_uploader("Unggah file teks", type="txt")
search_term = st.text_input("Masukkan kata kunci pencarian:")

# membaca teks dari file teks yang diunggah
if uploaded_file is not None:
    text = uploaded_file.getvalue().decode("utf-8")

    # mencari teks spesifik dalam teks menggunakan OpenAI API
    response = openai.Completion.create(
        engine="davinci",
        prompt=f"Find references for the term {search_term} in the following text: {text}",
        max_tokens=100,
        n=1,
        stop=None,
        temperature=0.5,
    )

    # menampilkan hasil pencarian dan sumber referensi dari OpenAI API
    matches = search_term in text
    if matches:
        st.write(f'Kata kunci "{search_term}" ditemukan.')
        st.write('Referensi:', response.choices[0].text)
    else:
        st.write(f'Kata kunci "{search_term}" tidak ditemukan.')
```

Dalam kode di atas, Streamlit menyediakan antarmuka pengguna yang memungkinkan pengguna mengunggah file teks dan memasukkan kata kunci pencarian. Jika file teks diunggah, kode membaca teks dari file tersebut, mencari teks spesifik yang diinginkan menggunakan OpenAI API, dan menampilkan hasil pencarian dan sumber referensi kembali ke pengguna melalui antarmuka Streamlit. Anda dapat menyesuaikan kode ini untuk membuat aplikasi Streamlit yang lebih canggih dan sesuai dengan kebutuhan Anda. Pastikan untuk mengganti `API_KEY_ANDA` dengan API key OpenAI Anda sendiri.

> tambahkan fiture tersebut agar bisa search tanpa harus match dan bisa searrch apapun

Tentu saja, Anda dapat menambahkan fitur agar pengguna dapat mencari kata kunci apa pun dalam file teks (baik PDF atau TXT) menggunakan OpenAI API. Berikut adalah contoh kode Python yang mengintegrasikan OpenAI API dan Streamlit untuk memungkinkan pengguna mengunggah file teks, mencari kata kunci di dalamnya menggunakan OpenAI API, dan menampilkan hasil pencarian:

python

```python
import PyPDF3
import openai
import streamlit as st

# inisialisasi OpenAI API
openai.api_key = "API_KEY_ANDA"

# membuat antarmuka pengguna untuk mengunggah file dan memasukkan kata kunci pencarian
uploaded_file = st.file_uploader("Unggah file PDF atau TXT", type=["pdf", "txt"])
search_term = st.text_input("Masukkan kata kunci pencarian:")

# membaca teks dari file PDF atau TXT yang diunggah
if uploaded_file is not None:
    if uploaded_file.type == "application/pdf":
        pdf_reader = PyPDF3.PdfFileReader(uploaded_file)
        text = ""
        for page in range(pdf_reader.getNumPages()):
            text += pdf_reader.getPage(page).extractText()
    elif uploaded_file.type == "text/plain":
        text = uploaded_file.getvalue().decode("utf-8")

    # mencari kata kunci dalam teks menggunakan OpenAI API
    response = openai.Completion.create(
        engine="davinci",
        prompt=f"Find references for the term {search_term} in the following text: {text}",
        max_tokens=100,
        n=1,
        stop=None,
        temperature=0.5,
    )

    # menampilkan hasil pencarian dan sumber referensi dari OpenAI API
    st.write('Hasil pencarian:')
    st.write(response.choices[0].text)
```

Dalam kode di atas, Streamlit menyediakan antarmuka pengguna yang memungkinkan pengguna mengunggah file PDF atau TXT dan memasukkan kata kunci pencarian. Jika file PDF atau TXT diunggah, kode membaca teks dari file tersebut, mencari kata kunci yang diinginkan menggunakan OpenAI API, dan menampilkan hasil pencarian kembali ke pengguna melalui antarmuka Streamlit.

Perlu diingat bahwa pencarian kata kunci apa pun dalam file teks mungkin akan memerlukan lebih banyak waktu dan sumber daya dari OpenAI API. Anda dapat membatasi jumlah kata kunci atau menggunakan parameter lain dalam API OpenAI untuk mengoptimalkan pencarian dan waktu respons.

> tambahkan firute tanpa ada batasan antarau huruf kecil dan besarnya

Untuk menambahkan fitur pencarian tanpa memperdulikan huruf besar atau kecil, Anda dapat memodifikasi kode Python yang digunakan untuk memfilter kata kunci pencarian menjadi huruf kecil atau besar tergantung pada kebutuhan pencarian. Berikut adalah contoh kode Python yang dimodifikasi untuk mengonversi kata kunci pencarian ke huruf kecil dan menghapus karakter non-alfanumerik sebelum melakukan pencarian menggunakan OpenAI API:

python

```python
import PyPDF3
import openai
import re
import streamlit as st

# inisialisasi OpenAI API
openai.api_key = "API_KEY_ANDA"

# membuat antarmuka pengguna untuk mengunggah file dan memasukkan kata kunci pencarian
uploaded_file = st.file_uploader("Unggah file PDF atau TXT", type=["pdf", "txt"])
search_term = st.text_input("Masukkan kata kunci pencarian:")

# mengubah kata kunci pencarian menjadi huruf kecil dan menghapus karakter non-alfanumerik
search_term = re.sub(r'\W+', ' ', search_term.lower()).strip()

# membaca teks dari file PDF atau TXT yang diunggah
if uploaded_file is not None:
    if uploaded_file.type == "application/pdf":
        pdf_reader = PyPDF3.PdfFileReader(uploaded_file)
        text = ""
        for page in range(pdf_reader.getNumPages()):
            text += pdf_reader.getPage(page).extractText()
    elif uploaded_file.type == "text/plain":
        text = uploaded_file.getvalue().decode("utf-8")

    # mencari kata kunci dalam teks menggunakan OpenAI API
    response = openai.Completion.create(
        engine="davinci",
        prompt=f"Find references for the term {search_term} in the following text: {text}",
        max_tokens=100,
        n=1,
        stop=None,
        temperature=0.5,
    )

    # menampilkan hasil pencarian dan sumber referensi dari OpenAI API
    st.write('Hasil pencarian:')
    st.write(response.choices[0].text)
```

Dalam kode di atas, Streamlit menyediakan antarmuka pengguna untuk mengunggah file PDF atau TXT dan memasukkan kata kunci pencarian. Sebelum melakukan pencarian, kode mengubah kata kunci pencarian menjadi huruf kecil dan menghapus karakter non-alfanumerik menggunakan modul regex. Hal ini memungkinkan pencarian untuk mengabaikan perbedaan huruf besar dan kecil serta karakter non-alfanumerik dalam kata kunci pencarian.

Setelah kata kunci pencarian diolah, kode membaca teks dari file PDF atau TXT yang diunggah, mencari kata kunci yang diinginkan menggunakan OpenAI API, dan menampilkan hasil pencarian kembali ke pengguna melalui antarmuka Streamlit.