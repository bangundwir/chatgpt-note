> setting ini di .zshrc WARNING: The script watchmedo is installed in '/home/hades/.local/bin' which is not on PATH. Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. WARNING: The script pysemver is installed in '/home/hades/.local/bin' which is not on PATH. Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. WARNING: The scripts f2py, f2py3 and f2py3.10 are installed in '/home/hades/.local/bin' which is not on PATH. Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. WARNING: The script plasma\_store is installed in '/home/hades/.local/bin' which is not on PATH. Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. WARNING: The script markdown-it is installed in '/home/hades/.local/bin' which is not on PATH. Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. WARNING: The script streamlit is installed in '/home/hades/.local/bin' which is not on PATH. Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
> 
> The messages you are seeing suggest that there are certain scripts installed in the `/home/hades/.local/bin` directory that are not on the PATH environment variable. This means that these scripts may not be executable from any location in the terminal unless you specify their absolute path.
> 
> To add the `/home/hades/.local/bin` directory to the PATH environment variable, you can add the following line to your `.zshrc` file:
> 
> bash
> 
> ```bash
> export PATH="$PATH:/home/hades/.local/bin"
> ```
> 
> This will add the `/home/hades/.local/bin` directory to the existing PATH variable, so that any scripts in that directory can be executed from anywhere in the terminal. Once you've made this change, you can either open a new terminal window or run the following command to source the updated `.zshrc` file:
> 
> bash
> 
> ```bash
> source ~/.zshrc
> ```
> 
> After sourcing the file, you should no longer see the warnings when you run those scripts.
> 
> buatakan script openai dengan streamlit dan dapat memasukan api di sidebar
> 
> Berikut adalah contoh script Streamlit dengan OpenAI API yang dapat menampilkan input teks dan menghasilkan output berupa teks dan gambar di sidebar:
> 
> python
> 
> ```python
> import openai
> import streamlit as st
> 
> # Setup OpenAI API credentials
> openai.api_key = "YOUR_API_KEY"
> 
> # Define function to generate text with OpenAI API
> def generate_text(prompt):
>     response = openai.Completion.create(
>         engine="davinci",
>         prompt=prompt,
>         max_tokens=1024,
>         n=1,
>         stop=None,
>         temperature=0.7,
>     )
>     text = response.choices[0].text
>     return text
> 
> # Define function to generate image with OpenAI API
> def generate_image(prompt):
>     response = openai.Image.create(
>         prompt=prompt,
>         n=1,
>         size="512x512",
>         response_format="url"
>     )
>     image_url = response['data'][0]['url']
>     return image_url
> 
> # Streamlit app
> st.set_page_config(page_title="OpenAI Demo", page_icon=":robot_face:")
> st.title("OpenAI Demo")
> 
> # Sidebar with API key input
> with st.sidebar:
>     st.markdown("## API Key")
>     api_key = st.text_input("Enter your OpenAI API key:", type="password")
> 
> # Authenticate OpenAI API with entered key
> if api_key:
>     openai.api_key = api_key
> 
> # Main app
> with st.form(key="my_form"):
>     st.markdown("## Input")
>     prompt = st.text_area("Enter text prompt:", height=100)
>     submit_button = st.form_submit_button(label="Generate")
> 
> if submit_button and prompt:
>     with st.spinner("Generating..."):
>         # Generate text and display in main area
>         generated_text = generate_text(prompt)
>         st.markdown("## Output (text)")
>         st.write(generated_text)
> 
>         # Generate image and display in sidebar
>         generated_image_url = generate_image(prompt)
>         st.markdown("## Output (image)")
>         st.image(generated_image_url, width=256)
> ```
> 
> Pada contoh di atas, Anda harus mengganti `YOUR_API_KEY` dengan kunci API Anda yang diberikan oleh OpenAI. Setelah itu, Anda dapat memulai aplikasi Streamlit dengan menjalankan perintah `streamlit run nama_file.py` pada terminal (pastikan untuk mengganti `nama_file.py` dengan nama file yang sesuai).
> 
> tambahkan fiture dapat melihat history dan bisa memberikan tone dan tipe
> 
> Berikut adalah contoh kode Streamlit yang diperbarui dengan fitur untuk melihat riwayat input teks dan memilih tipe dan nada yang berbeda:
> 
> python
> 
> ```python
> import openai
> import streamlit as st
> from datetime import datetime
> 
> # Setup OpenAI API credentials
> openai.api_key = "YOUR_API_KEY"
> 
> # Define function to generate text with OpenAI API
> def generate_text(prompt, model, temperature, max_tokens):
>     response = openai.Completion.create(
>         engine=model,
>         prompt=prompt,
>         max_tokens=max_tokens,
>         n=1,
>         stop=None,
>         temperature=temperature,
>     )
>     text = response.choices[0].text
>     return text
> 
> # Define function to generate image with OpenAI API
> def generate_image(prompt):
>     response = openai.Image.create(
>         prompt=prompt,
>         n=1,
>         size="512x512",
>         response_format="url"
>     )
>     image_url = response['data'][0]['url']
>     return image_url
> 
> # Streamlit app
> st.set_page_config(page_title="OpenAI Demo", page_icon=":robot_face:")
> st.title("OpenAI Demo")
> 
> # Sidebar with API key input
> with st.sidebar:
>     st.markdown("## API Key")
>     api_key = st.text_input("Enter your OpenAI API key:", type="password")
> 
> # Authenticate OpenAI API with entered key
> if api_key:
>     openai.api_key = api_key
> 
> # Main app
> with st.form(key="my_form"):
>     st.markdown("## Input")
>     prompt = st.text_area("Enter text prompt:", height=100)
> 
>     # Show input history
>     history = st.session_state.get('history', [])
>     if history:
>         st.markdown("## Input History")
>         for i, entry in enumerate(history):
>             st.write(f"{i+1}. {entry['prompt']}")
> 
>     st.markdown("## Configuration")
>     temperature = st.slider("Temperature", min_value=0.1, max_value=1.0, step=0.1, value=0.5)
>     max_tokens = st.number_input("Max Tokens", min_value=1, value=64)
>     model = st.selectbox("Model", ["davinci", "curie", "babbage", "ada"])
> 
>     tone = st.selectbox("Tone", ["default", "friendly", "professional", "academic"])
>     tipe = st.selectbox("Type", ["text", "image"])
> 
>     submit_button = st.form_submit_button(label="Generate")
> 
> if submit_button and prompt:
>     # Add current input to history
>     history.append({
>         "prompt": prompt,
>         "model": model,
>         "temperature": temperature,
>         "max_tokens": max_tokens,
>         "tone": tone,
>         "type": tipe,
>         "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
>     })
>     st.session_state.history = history
> 
>     with st.spinner("Generating..."):
>         # Generate text and display in main area
>         if tipe == "text":
>             generated_text = generate_text(prompt, model, temperature, max_tokens)
>             st.markdown("## Output (text)")
>             st.write(generated_text)
> 
>         # Generate image and display in sidebar
>         elif tipe == "image":
>             generated_image_url = generate_image(prompt)
>             st.markdown("## Output (image)")
>             st.image(generated_image_url, width=256)
> 
>     # Show tone analysis
>     if tone != "default":
>         response = openai.Completion.create(
>             engine="text-davinci-002",
>             prompt=f"Analyze the tone of the
> ```
> 
> lanjutkan
> 
> generated text: {generated\_text}", temperature=0.5, max\_tokens=32, n=1, stop=None, presence\_penalty=0.6, frequency\_penalty=0.6, echo=True, logprobs=10, user=tone, model="text-davinci-002" )
> 
> lua
> 
>     ```lua
> st.markdown("## Tone Analysis")
>     st.write(response.choices[0].text)
> ```
> 
> buatakan streamlit pdf to txt
> 
> Untuk mengonversi file PDF menjadi teks menggunakan Streamlit, Anda dapat menggunakan pustaka PyPDF2. Berikut ini adalah contoh implementasinya:
> 
> scss
> 
> ```scss
> import streamlit as st
> import PyPDF2
> 
> def read_pdf(file):
>     pdfReader = PyPDF2.PdfFileReader(file)
>     count = pdfReader.numPages
>     text = ""
> 
>     for i in range(count):
>         page = pdfReader.getPage(i)
>         text += page.extractText()
> 
>     return text
> 
> def main():
>     st.title("PDF to Text Converter")
>     file = st.file_uploader("Upload a PDF file", type=["pdf"])
> 
>     if file is not None:
>         text = read_pdf(file)
>         st.write(text)
> 
> if __name__ == '__main__':
>     main()
> ```
> 
> Pertama, kita mengimpor PyPDF2 dan modul streamlit. Kemudian, kita menentukan fungsi `read_pdf` untuk membaca file PDF dan mengembalikan teks. Fungsi ini membaca setiap halaman file PDF dan menambahkan teks ke variabel `text`. Kemudian, kita menentukan fungsi `main` yang memungkinkan pengguna mengunggah file PDF dan menampilkan teks hasil konversi.
> 
> Setelah menjalankan aplikasi Streamlit, pengguna dapat memilih file PDF yang ingin dikonversi menggunakan tombol "Upload a PDF file". Setelah memilih file PDF, teks hasil konversi akan ditampilkan di bawahnya.
> 
> buatakan script pdf to text dengan streamlit mengunakan ocr dan dapat bisa di download di button
> 
> Untuk mengonversi file PDF menjadi teks menggunakan OCR (Optical Character Recognition) dan mengizinkan pengguna untuk mengunduh hasilnya sebagai file teks, Anda dapat menggunakan pustaka Tesseract-OCR dan Streamlit. Berikut adalah contoh implementasinya:
> 
> scss
> 
> ```scss
> import streamlit as st
> import pytesseract
> from PIL import Image
> import io
> import PyPDF2
> 
> pytesseract.pytesseract.tesseract_cmd = '/usr/bin/tesseract'
> 
> def read_pdf(file):
>     pdfReader = PyPDF2.PdfFileReader(file)
>     count = pdfReader.numPages
>     text = ""
> 
>     for i in range(count):
>         page = pdfReader.getPage(i)
>         text += page.extractText()
> 
>     return text
> 
> def ocr_core(file):
>     text = pytesseract.image_to_string(file)
>     return text
> 
> def main():
>     st.title("PDF to Text Converter with OCR")
> 
>     file = st.file_uploader("Upload a PDF file", type=["pdf"])
> 
>     if file is not None:
>         text = read_pdf(file)
> 
>         st.markdown("## Extracted Text from PDF")
>         st.write(text)
> 
>         st.markdown("## OCR")
> 
>         image_file = st.file_uploader("Upload a scanned image of the PDF", type=["jpg", "png"])
> 
>         if image_file is not None:
>             image = Image.open(image_file)
>             text = ocr_core(image)
> 
>             st.write(text)
> 
>         st.markdown("## Download Text")
> 
>         download_text = st.button("Download as Text")
> 
>         if download_text:
>             text_file = io.StringIO(text)
>             st.download_button(
>                 label="Download Text",
>                 data=text_file.getvalue(),
>                 file_name="converted_text.txt",
>                 mime="text/plain",
>             )
> 
> if __name__ == '__main__':
>     main()
> ```
> 
> Pertama, kita mengimpor pustaka yang diperlukan, yaitu streamlit, pytesseract, PIL (Python Imaging Library), io, dan PyPDF2. Kemudian, kita menentukan fungsi `read_pdf` untuk membaca file PDF dan mengembalikan teks, dan fungsi `ocr_core` untuk melakukan OCR pada gambar yang diunggah dan mengembalikan teks.
> 
> Setelah itu, kita menentukan fungsi `main` yang memungkinkan pengguna untuk mengunggah file PDF dan menampilkan teks hasil konversi dan hasil OCR. Pengguna juga dapat mengunduh hasil konversi sebagai file teks dengan mengeklik tombol "Download as Text".
> 
> Setelah menjalankan aplikasi Streamlit, pengguna dapat memilih file PDF yang ingin dikonversi menggunakan tombol "Upload a PDF file". Setelah memilih file PDF, teks hasil konversi akan ditampilkan di bawahnya. Pengguna juga dapat mengunggah gambar yang berisi teks yang ingin di-OCR menggunakan tombol "Upload a scanned image of the PDF". Setelah mengunggah gambar, teks hasil OCR akan ditampilkan di bawahnya. Akhirnya, pengguna dapat mengunduh teks hasil konversi atau hasil OCR dengan mengeklik tombol "Download as Text".
> 
> buatakan semantic search dari database google spreadsheet di streamlit https://docs.google.com/spreadsheets/d/1fPP4f5-f1DJzIOysJsiZ0FBS1SVn0T0hTHqQhzQtfDQ/edit?usp=sharing
> 
> Untuk membuat semantic search dari database Google Spreadsheet di Streamlit, Anda perlu terlebih dahulu mengambil data dari spreadsheet menggunakan Google Sheets API. Kemudian, Anda dapat menggunakan pustaka Natural Language Toolkit (NLTK) untuk melakukan pemrosesan bahasa alami pada data teks dan membuat model pencarian semantik.
> 
> Berikut adalah contoh implementasi semantic search dari database Google Spreadsheet di Streamlit:
> 
> python
> 
> ```python
> import streamlit as st
> import gspread
> from oauth2client.service_account import ServiceAccountCredentials
> import nltk
> from sklearn.feature_extraction.text import TfidfVectorizer
> from sklearn.metrics.pairwise import cosine_similarity
> 
> nltk.download('punkt')
> 
> # Setup Google Sheets API credentials
> scope = ['https://spreadsheets.google.com/feeds',
>          'https://www.googleapis.com/auth/drive']
> creds = ServiceAccountCredentials.from_json_keyfile_name('creds.json', scope)
> client = gspread.authorize(creds)
> 
> # Open Google Spreadsheet
> sheet = client.open('my_spreadsheet').sheet1
> data = sheet.get_all_records()
> 
> # Extract text data from spreadsheet
> documents = []
> for row in data:
>     documents.append(row['text'])
> 
> # Perform semantic search
> tfidf = TfidfVectorizer(tokenizer=nltk.word_tokenize, stop_words='english')
> tfidf_matrix = tfidf.fit_transform(documents)
> cosine_sim = cosine_similarity(tfidf_matrix)
> 
> def search(query, documents, cosine_sim):
>     query_tfidf = tfidf.transform([query])
>     scores = cosine_sim.dot(query_tfidf.T).flatten()
>     sorted_scores_indices = scores.argsort()[::-1]
>     sorted_scores = sorted(scores, reverse=True)
>     results = []
>     for i in range(len(sorted_scores)):
>         if sorted_scores[i] > 0:
>             results.append((documents[sorted_scores_indices[i]], sorted_scores[i]))
>     return results
> 
> # Set up Streamlit app
> st.title("Semantic Search from Google Spreadsheet")
> query = st.text_input("Enter your search query:")
> if query:
>     results = search(query, documents, cosine_sim)
>     st.write("Number of results:", len(results))
>     for result in results:
>         st.write("Score:", result[1])
>         st.write("Text:", result[0])
> ```
> 
> Pertama, kita mengimpor pustaka yang diperlukan, yaitu streamlit, gspread, oauth2client, nltk, TfidfVectorizer, dan cosine\_similarity. Kemudian, kita mengambil kredensial Google Sheets API dari file `creds.json` dan membuka spreadsheet menggunakan pustaka gspread. Setelah itu, kita mengekstrak data teks dari spreadsheet dan menyimpannya dalam list `documents`.
> 
> Selanjutnya, kita menggunakan TfidfVectorizer untuk membuat vektor representasi dokumen dan cosine similarity untuk mengukur kemiripan antara dokumen dan query pencarian. Fungsi `search` digunakan untuk melakukan pencarian semantik berdasarkan query pencarian yang diberikan.
> 
> Terakhir, kita menentukan aplikasi Streamlit dan membuat input text untuk query pencarian. Jika pengguna memasukkan query pencarian, kita melakukan pencarian semantik menggunakan fungsi `search` dan menampilkan hasilnya di bawahnya.
> 
> library yang perlu di install
> 
> Untuk menjalankan contoh implementasi semantic search dari database Google Spreadsheet di Streamlit, Anda perlu menginstall beberapa pustaka Python, yaitu:
> 
> *   streamlit: untuk membuat aplikasi web interaktif
> *   gspread: untuk mengakses Google Sheets API
> *   oauth2client: untuk mengautentikasi ke Google Sheets API
> *   nltk: untuk melakukan pemrosesan bahasa alami
> *   scikit-learn: untuk melakukan vectorization dan cosine similarity
> 
> Anda dapat menginstall pustaka-pustaka tersebut menggunakan pip. Jalankan perintah berikut di terminal atau command prompt:
> 
> `pip install streamlit gspread oauth2client nltk scikit-learn`
> 
> Setelah pustaka-pustaka tersebut terpasang, Anda dapat menjalankan script implementasi semantic search di atas.
> 
> cara mengunakan creds.json nya bagaiman
> 
> `creds.json` berisi informasi kredensial yang dibutuhkan untuk mengakses Google Sheets API. Untuk menggunakan file `creds.json`, Anda perlu melakukan beberapa langkah berikut:
> 
> 1.  Buat proyek di Google Cloud Console dan aktifkan Google Sheets API.
> 2.  Buatlah Service Account Key dengan role Editor dan simpan file JSON di komputer Anda.
> 3.  Berikan akses ke file spreadsheet yang ingin diakses oleh service account yang dibuat pada langkah 2.
> 4.  Setelah file `creds.json` diperoleh, simpan file tersebut di direktori yang sama dengan file Python yang mengakses Google Sheets API.
> 
> Setelah file `creds.json` disimpan di direktori yang tepat, Anda dapat menggunakan kode berikut untuk memuat file tersebut dan mengautentikasi ke Google Sheets API:
> 
> python
> 
> ```python
> import gspread
> from oauth2client.service_account import ServiceAccountCredentials
> 
> # memuat file creds.json dan mengambil informasi kredensial
> creds = ServiceAccountCredentials.from_json_keyfile_name('creds.json', ['https://www.googleapis.com/auth/spreadsheets'])
> 
> # mengautentikasi ke Google Sheets API
> client = gspread.authorize(creds)
> 
> # membuka spreadsheet
> sheet = client.open('nama_spreadsheet').sheet1
> 
> # melakukan operasi pada spreadsheet
> ```
> 
> Pastikan bahwa `nama_spreadsheet` diganti dengan nama spreadsheet yang ingin Anda akses. Setelah mengautentikasi, Anda dapat melakukan operasi pada spreadsheet seperti membaca data atau menulis data ke dalam sel-sel spreadsheet.
> 
> buatakan dari url ini https://docs.google.com/spreadsheets/d/1fPP4f5-f1DJzIOysJsiZ0FBS1SVn0T0hTHqQhzQtfDQ/edit#gid=1778635312
> 
> Berikut adalah contoh implementasi semantic search dari database Google Spreadsheet menggunakan Streamlit dan Google Sheets API. Pastikan Anda telah menginstall semua pustaka yang diperlukan dan telah mengunduh file `creds.json` dari Google Cloud Console dan menyimpannya di direktori yang sama dengan file Python yang akan dijalankan.
> 
> python
> 
> ```python
> import streamlit as st
> import gspread
> from oauth2client.service_account import ServiceAccountCredentials
> import nltk
> from sklearn.feature_extraction.text import TfidfVectorizer
> from sklearn.metrics.pairwise import cosine_similarity
> 
> # memuat file creds.json dan mengambil informasi kredensial
> creds = ServiceAccountCredentials.from_json_keyfile_name('creds.json', ['https://www.googleapis.com/auth/spreadsheets'])
> 
> # mengautentikasi ke Google Sheets API
> client = gspread.authorize(creds)
> 
> # membuka spreadsheet
> sheet_url = 'https://docs.google.com/spreadsheets/d/1fPP4f5-f1DJzIOysJsiZ0FBS1SVn0T0hTHqQhzQtfDQ/edit#gid=1778635312'
> sheet_key = sheet_url.split('/')[-2]
> sheet = client.open_by_key(sheet_key).sheet1
> 
> # membaca data dari spreadsheet
> data = sheet.get_all_records()
> 
> # mengambil teks dari setiap baris data
> texts = [row['Teks'] for row in data]
> 
> # membuat TfidfVectorizer untuk mengubah teks menjadi vektor
> tfidf = TfidfVectorizer(tokenizer=nltk.word_tokenize)
> tfidf.fit(texts)
> 
> # menambahkan fitur pencarian berdasarkan teks pada Streamlit
> st.title("Semantic Search dari Database Google Spreadsheet")
> search_term = st.text_input("Masukkan teks pencarian:")
> 
> if search_term:
>     # mengubah teks pencarian menjadi vektor
>     query_vector = tfidf.transform([search_term])
> 
>     # menghitung cosine similarity antara teks pencarian dengan teks pada setiap baris data
>     similarities = cosine_similarity(query_vector, tfidf.transform(texts)).flatten()
> 
>     # mengurutkan hasil pencarian berdasarkan cosine similarity
>     results = [(similarity, data[i]['Judul'], data[i]['Teks']) for i, similarity in enumerate(similarities)]
>     results = sorted(results, key=lambda x: x[0], reverse=True)
> 
>     # menampilkan hasil pencarian pada Streamlit
>     st.write(f"Ditemukan {len(results)} hasil pencarian:")
>     for similarity, title, text in results:
>         st.write(f"Judul: {title}")
>         st.write(f"Similarity: {similarity:.2f}")
>         st.write(f"Isi teks: {text}")
>         st.write('---')
> ```
> 
> Pastikan untuk mengganti `sheet_url` dengan URL spreadsheet yang ingin Anda akses. Kemudian, jalankan script tersebut dengan perintah:
> 
> `streamlit run nama_file.py`
> 
> di mana `nama_file.py` adalah nama file Python yang berisi script di atas. Setelah itu, buka aplikasi web Streamlit di browser dan coba masukkan teks pencarian pada kolom input yang tersedia. Hasil pencarian yang cocok akan ditampilkan di bawah kolom input.
> 
> masukan api ke creds.json ini api : AIzaSyBITPye2e-VFcC8E9WUR6bg4WF7nU4\_ih0
> 
> Maaf, saya hanya sebuah AI Language Model dan tidak memiliki akses untuk memodifikasi file atau dokumen apapun, termasuk file `creds.json` atau Google Spreadsheet yang diacu pada pertanyaan sebelumnya.
> 
> Namun, untuk menambahkan API key ke file `creds.json`, Anda dapat membuka file tersebut dengan editor teks dan menambahkan API key pada bagian yang sesuai dengan format yang sesuai, seperti:
> 
> json
> 
> ```json
> {
>     "type": "service_account",
>     "project_id": "nama-project",
>     "private_key_id": "XXXX",
>     "private_key": "-----BEGIN PRIVATE KEY-----\nXXXX\n-----END PRIVATE KEY-----\n",
>     "client_email": "email@project.iam.gserviceaccount.com",
>     "client_id": "XXXX",
>     "auth_uri": "XXXX",
>     "token_uri": "XXXX",
>     "auth_provider_x509_cert_url": "XXXX",
>     "client_x509_cert_url": "XXXX",
>     "api_key": "AIzaSyBITPye2e-VFcC8E9WUR6bg4WF7nU4_ih0"
> }
> ```
> 
> Pastikan untuk menyimpan file `creds.json` setelah menambahkan API key.
> 
> buatakan scraping twitter dengan streamlit dan snscrape
> 
> Baik, berikut adalah contoh sederhana bagaimana melakukan scraping data dari Twitter menggunakan `snscrape` dan menampilkannya di Streamlit:
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> 
> st.title('Scraping Twitter with Streamlit and Snscrape')
> 
> # Input query and number of tweets to scrape
> query = st.text_input('Enter a search query:', 'data science')
> num_tweets = st.number_input('Number of tweets to scrape:', min_value=1, max_value=1000, step=1)
> 
> # Scrape tweets and store data in a dataframe
> tweets_list = []
> for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query + ' lang:id').get_items()):
>     if i >= num_tweets:
>         break
>     tweets_list.append([tweet.date, tweet.id, tweet.content, tweet.user.username, tweet.user.followersCount])
> tweets_df = pd.DataFrame(tweets_list, columns=['Datetime', 'Tweet Id', 'Text', 'Username', 'Followers'])
> 
> # Display data
> st.write(tweets_df)
> ```
> 
> Penjelasan singkat mengenai kode tersebut:
> 
> *   Pertama, kita meng-import modul Streamlit, `snscrape`, dan `pandas`.
> *   Kemudian, kita membuat judul untuk aplikasi Streamlit menggunakan fungsi `st.title()`.
> *   Berikutnya, kita membuat input untuk query dan jumlah tweet yang akan di-scrape menggunakan `st.text_input()` dan `st.number_input()`.
> *   Setelah itu, kita melakukan scraping tweet menggunakan loop dan menyimpan data di dalam sebuah list `tweets_list`.
> *   Setelah loop selesai, kita membuat dataframe dari list tersebut menggunakan `pd.DataFrame()`.
> *   Terakhir, kita menampilkan data tersebut di Streamlit menggunakan `st.write()`.
> 
> Pastikan Anda telah meng-install `snscrape` dengan menjalankan perintah `!pip install snscrape` di terminal atau di Jupyter Notebook sebelum mencoba kode tersebut. Jangan lupa juga untuk menambahkan `lang:id` pada query jika ingin menampilkan tweet yang berbahasa Indonesia.
> 
> tambahakan fiture bisa download file txt nya atau csv nya
> 
> Untuk menambahkan fitur download file hasil scraping sebagai file `.txt` atau `.csv`, Anda bisa menambahkan tombol download di aplikasi Streamlit menggunakan `st.download_button()` dan menambahkan logika untuk menyimpan dataframe ke dalam file tersebut. Berikut adalah contoh kodenya:
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> 
> st.title('Scraping Twitter with Streamlit and Snscrape')
> 
> # Input query and number of tweets to scrape
> query = st.text_input('Enter a search query:', 'data science')
> num_tweets = st.number_input('Number of tweets to scrape:', min_value=1, max_value=1000, step=1)
> 
> # Scrape tweets and store data in a dataframe
> tweets_list = []
> for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query + ' lang:id').get_items()):
>     if i >= num_tweets:
>         break
>     tweets_list.append([tweet.date, tweet.id, tweet.content, tweet.user.username, tweet.user.followersCount])
> tweets_df = pd.DataFrame(tweets_list, columns=['Datetime', 'Tweet Id', 'Text', 'Username', 'Followers'])
> 
> # Display data
> st.write(tweets_df)
> 
> # Download as text file
> if st.button('Download as Text File'):
>     t = '\t'.join(tweets_df.columns) + '\n'
>     for index, row in tweets_df.iterrows():
>         t += '\t'.join([str(elem) for elem in row.values]) + '\n'
>     with open('tweets.txt', 'w') as f:
>         f.write(t)
>     st.download_button(
>         label="Download Tweets as Text",
>         data=t,
>         file_name='tweets.txt',
>         mime='text/plain'
>     )
> 
> # Download as csv file
> if st.button('Download as CSV File'):
>     csv = tweets_df.to_csv(index=False)
>     with open('tweets.csv', 'w') as f:
>         f.write(csv)
>     st.download_button(
>         label="Download Tweets as CSV",
>         data=csv,
>         file_name='tweets.csv',
>         mime='text/csv'
>     )
> ```
> 
> Pada contoh kode di atas, setelah menampilkan dataframe hasil scraping, kita menambahkan dua tombol untuk mengunduh file: satu untuk file `.txt` dan satu untuk file `.csv`. Ketika salah satu tombol tersebut ditekan, maka aplikasi akan menyimpan dataframe ke dalam file yang sesuai dan menampilkan tombol download menggunakan `st.download_button()`. Ketika tombol tersebut ditekan, maka file akan diunduh ke dalam komputer pengguna.
> 
> tambahakan fitur lagi dapa memlihi mengambil dengan berdasakan waktu tweet id jam id username followers tweet url dan dapat secara interaktif mengunakan nya dengan tampilan yang intutif dan informatif
> 
> Berikut ini adalah contoh script untuk melakukan scraping Twitter dengan snscrape dan menampilkan hasilnya dengan Streamlit. Script ini akan memungkinkan pengguna untuk memasukkan kata kunci untuk mencari tweet, menentukan tanggal awal dan akhir, memilih kolom yang akan ditampilkan dalam output, serta mengekspor output ke file CSV atau TXT.
> 
> Sebelum memulai, pastikan sudah menginstal library snscrape dan streamlit dengan menggunakan perintah `pip install snscrape streamlit`.
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> 
> # Function to scrape tweets
> def scrape_tweets(keyword, start_date, end_date, num_tweets):
>     
>     # Convert dates to Twitter API format
>     start_date = start_date.strftime("%Y-%m-%d")
>     end_date = end_date.strftime("%Y-%m-%d")
>     
>     # Define search query
>     query = f"{keyword} since:{start_date} until:{end_date}"
>     
>     # Use snscrape to scrape tweets
>     tweets = []
>     for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query).get_items()):
>         if i >= num_tweets:
>             break
>         tweets.append([tweet.date, tweet.id, tweet.user.username, tweet.user.followersCount, tweet.content, tweet.url])
>     
>     # Create dataframe from tweets
>     df = pd.DataFrame(tweets, columns=['Date', 'Tweet ID', 'Username', 'Followers', 'Text', 'URL'])
>     
>     return df
> 
> # Define Streamlit app
> def app():
>     # Set page title
>     st.set_page_config(page_title="Twitter Scraper")
>     
>     # Set page layout
>     col1, col2 = st.columns(2)
>     
>     # Set default values for input widgets
>     default_start_date = pd.to_datetime('2022-01-01')
>     default_end_date = pd.to_datetime('2022-02-01')
>     default_num_tweets = 100
>     
>     # Get user input
>     with col1:
>         st.header("Twitter Scraper")
>         keyword = st.text_input("Enter keyword(s) to search for:", "#OpenAI")
>         start_date = st.date_input("Start date:", default_start_date)
>         end_date = st.date_input("End date:", default_end_date)
>         num_tweets = st.number_input("Number of tweets to scrape:", min_value=1, value=default_num_tweets, step=1)
>     with col2:
>         st.header("Columns to Display")
>         show_date = st.checkbox("Date", value=True)
>         show_tweet_id = st.checkbox("Tweet ID", value=False)
>         show_username = st.checkbox("Username", value=True)
>         show_followers = st.checkbox("Followers", value=True)
>         show_text = st.checkbox("Text", value=True)
>         show_url = st.checkbox("URL", value=False)
>     
>     # Scrape tweets
>     df = scrape_tweets(keyword, start_date, end_date, num_tweets)
>     
>     # Display output
>     if not df.empty:
>         if not (show_date or show_tweet_id or show_username or show_followers or show_text or show_url):
>             st.warning("Please select at least one column to display.")
>         else:
>             if not show_date:
>                 df = df.drop('Date', axis=1)
>             if not show_tweet_id:
>                 df = df.drop('Tweet ID', axis=1)
>             if not show_username:
>                 df = df.drop('Username', axis=1)
>             if not show_followers:
>                 df = df.drop('Followers', axis=1)
>             if
> ```
> 
> lanjutkan
> 
> Berikut ini adalah kode lengkap untuk melakukan scraping Twitter menggunakan Streamlit dan snscrape, dengan fitur untuk memilih kriteria pencarian dan men-download hasil scraping dalam format CSV atau TXT:
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> 
> # set page title
> st.set_page_config(page_title="Twitter Scraper")
> 
> # create function to scrape tweets
> def scrape_tweets(search_term, lang, date_since, max_tweets):
>     # initialize empty list to store tweets
>     tweets_list = []
>     # set search query
>     query = f'{search_term} lang:{lang} since:{date_since}'
>     # use snscrape to scrape tweets
>     for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query).get_items()):
>         if i >= max_tweets:
>             break
>         tweets_list.append([tweet.date, tweet.id, tweet.user.username, tweet.user.followersCount, tweet.content, tweet.url])
>     # create dataframe from tweets list
>     tweets_df = pd.DataFrame(tweets_list, columns=['Datetime', 'Tweet Id', 'Username', 'Followers', 'Tweet', 'URL'])
>     # return dataframe
>     return tweets_df
> 
> # create sidebar
> st.sidebar.title("Twitter Scraper")
> search_term = st.sidebar.text_input("Search term", "#COVID19")
> lang = st.sidebar.selectbox("Language", ["en", "id"])
> date_since = st.sidebar.date_input("Date since", value=pd.to_datetime('today').date())
> max_tweets = st.sidebar.number_input("Max tweets", min_value=1, max_value=10000, value=1000)
> file_type = st.sidebar.selectbox("File type", ["CSV", "TXT"])
> 
> # create button to initiate scraping and downloading
> if st.sidebar.button("Scrape and Download"):
>     # scrape tweets
>     tweets_df = scrape_tweets(search_term, lang, date_since, max_tweets)
>     # download file
>     if file_type == "CSV":
>         csv = tweets_df.to_csv(index=False)
>         b64 = base64.b64encode(csv.encode()).decode()
>         href = f'<a href="data:file/csv;base64,{b64}" download="tweets.csv">Download CSV</a>'
>     else:
>         txt = tweets_df.to_csv(index=False, sep="\t")
>         b64 = base64.b64encode(txt.encode()).decode()
>         href = f'<a href="data:file/txt;base64,{b64}" download="tweets.txt">Download TXT</a>'
>     st.markdown(href, unsafe_allow_html=True)
> 
> # display tweets dataframe
> st.dataframe(tweets_df)
> ```
> 
> Anda perlu menginstall library `snscrape` dengan menggunakan pip. Untuk menjalankan aplikasi ini, Anda cukup mengetikkan perintah `streamlit run namafile.py` pada terminal, dan kemudian membuka aplikasi pada browser. Kemudian, Anda dapat memasukkan kriteria pencarian pada sidebar, dan menekan tombol "Scrape and Download" untuk memulai scraping dan mengunduh file hasil scraping dalam format CSV atau TXT. Hasil scraping juga akan ditampilkan pada aplikasi sebagai dataframe.
> 
> sempurnakan code di atas dengan menambahkan fiture rantang pada tanggal
> 
> Untuk menambahkan fitur range tanggal pada scraping Twitter dengan streamlit dan snscrape, kita bisa menambahkan dua input date picker dengan format `yyyy-mm-dd`. Input pertama akan menjadi tanggal awal range yang ingin diambil, sedangkan input kedua akan menjadi tanggal akhir range yang ingin diambil.
> 
> Setelah itu, kita akan memodifikasi fungsi `scrape_tweets` dengan menambahkan dua argumen baru yaitu `start_date` dan `end_date`. Argumen-argumen ini akan digunakan untuk membangun query snscrape yang digunakan untuk menentukan range tanggal yang ingin diambil.
> 
> Berikut adalah contoh kode yang telah diperbarui:
> 
> python
> 
> ```python
> import streamlit as st
> import pandas as pd
> import snscrape.modules.twitter as sntwitter
> 
> st.title("Scraping Twitter with snscrape")
> 
> # Function to scrape tweets
> def scrape_tweets(search_term, num_tweets, start_date, end_date):
>     tweets_list = []
>     query = f"{search_term} since:{start_date} until:{end_date}"
>     for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query).get_items()):
>         if i >= num_tweets:
>             break
>         tweets_list.append([tweet.date, tweet.id, tweet.user.username, tweet.user.followersCount, tweet.content, tweet.url])
>     return tweets_list
> 
> # Get user input
> search_term = st.text_input("Enter search term")
> num_tweets = st.number_input("Enter number of tweets to scrape", min_value=1, max_value=10000, value=100)
> start_date = st.date_input("Enter start date")
> end_date = st.date_input("Enter end date")
> 
> # Scrape tweets and display results
> if st.button("Scrape"):
>     with st.spinner("Scraping tweets..."):
>         tweets = scrape_tweets(search_term, num_tweets, start_date.strftime("%Y-%m-%d"), end_date.strftime("%Y-%m-%d"))
>     if len(tweets) > 0:
>         st.write(f"Displaying {len(tweets)} tweets")
>         df = pd.DataFrame(tweets, columns=["Datetime", "Tweet ID", "Username", "Followers", "Tweet", "URL"])
>         st.dataframe(df)
>         csv = df.to_csv(index=False)
>         b64 = base64.b64encode(csv.encode()).decode()
>         href = f'<a href="data:file/csv;base64,{b64}" download="tweets.csv">Download CSV</a>'
>         st.markdown(href, unsafe_allow_html=True)
>     else:
>         st.write("No tweets found")
> ```
> 
> Dalam kode di atas, input tanggal awal dan tanggal akhir diproses dengan menggunakan `strftime` untuk mengubahnya menjadi format `yyyy-mm-dd`. Kemudian, pada saat fungsi `scrape_tweets` dipanggil, kita menggunakan variabel `start_date` dan `end_date` yang telah diformat untuk membangun query snscrape yang digunakan untuk membatasi rentang tanggal tweet yang akan diambil.
> 
> tambahkan fiture untuk download txt atau csv dan dapat kan tweet.user.id juga
> 
> Berikut adalah contoh implementasi fitur untuk melakukan download file hasil scraping dalam format txt atau csv dan menambahkan kolom `tweet.user.id` pada hasil scraping Twitter dengan menggunakan Snscrape dan Streamlit:
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> 
> # function to scrape twitter data based on user input
> def get_tweets(keyword, start_date, end_date, num_tweets):
>     # create list to hold tweets
>     tweets_list = []
> 
>     # define search query
>     search_query = f"{keyword} since:{start_date} until:{end_date}"
> 
>     # loop through search results and append to list
>     for i, tweet in enumerate(sntwitter.TwitterSearchScraper(search_query).get_items()):
>         if i >= num_tweets:
>             break
>         tweets_list.append([tweet.date, tweet.id, tweet.username, tweet.user.id, tweet.content, tweet.url])
> 
>     # convert list to dataframe
>     tweets_df = pd.DataFrame(tweets_list, columns=['Datetime', 'Tweet Id', 'Username', 'User Id', 'Text', 'Tweet URL'])
> 
>     return tweets_df
> 
> # set page title
> st.title('Twitter Scraper')
> 
> # get user input
> keyword = st.text_input('Keyword to search')
> start_date = st.date_input('Start date')
> end_date = st.date_input('End date')
> num_tweets = st.slider('Number of tweets', min_value=10, max_value=1000, step=10)
> 
> # scrape twitter data and show in table
> if st.button('Scrape Twitter'):
>     tweets_df = get_tweets(keyword, start_date, end_date, num_tweets)
>     st.write(tweets_df)
> 
>     # add a download button to download scraped data as txt or csv
>     file_type = st.selectbox('Download as', ['TXT', 'CSV'])
>     if st.button('Download'):
>         if file_type == 'TXT':
>             file_extension = 'txt'
>             file_content = '\n'.join(tweets_df['Text'])
>         else:
>             file_extension = 'csv'
>             file_content = tweets_df.to_csv(index=False)
>         st.download_button(
>             label=f'Download as {file_extension}',
>             data=file_content.encode('utf-8'),
>             file_name=f'tweets.{file_extension}',
>             mime='text/plain'
>         )
> ```
> 
> Dalam contoh di atas, pengguna dapat memilih format file yang akan diunduh melalui dropdown menu "Download as". Kemudian, ketika pengguna menekan tombol "Download", file yang sesuai akan diunduh sesuai dengan format yang dipilih. Tampilan tabel hasil scraping Twitter dapat dilihat pada tampilan Streamlit dan kolom `tweet.user.id` ditambahkan dalam hasil scraping.
> 
> tambahakn fiture lagu dapat memasuakan input di Number of tweets
> 
> Berikut adalah contoh kode untuk menambahkan fitur untuk memasukkan input pada jumlah tweet yang ingin diambil:
> 
> python
> 
> ```python
> import streamlit as st
> import pandas as pd
> import snscrape.modules.twitter as sntwitter
> import csv
> 
> # Create function to scrape tweets
> def get_tweets(keyword, num_tweets):
>     # Set base URL for snscrape
>     base_url = 'https://twitter.com'
> 
>     # Initialize empty list to hold tweets
>     tweets_list = []
> 
>     # Define search query and set up scraper
>     query = f'{keyword} since:2019-01-01 until:2023-01-01'
>     for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query).get_items()):
>         if i >= num_tweets:
>             break
>         tweets_list.append([tweet.date, tweet.id, tweet.content, tweet.user.id, base_url + tweet.url])
> 
>     # Convert list of tweets to Pandas DataFrame
>     tweets_df = pd.DataFrame(tweets_list, columns=['Datetime', 'Tweet Id', 'Text', 'User Id', 'URL'])
> 
>     return tweets_df
> 
> # Create Streamlit app
> st.title("Twitter Scraper")
> 
> # Get keyword input from user
> keyword = st.text_input("Enter a keyword to search on Twitter")
> 
> # Get number of tweets input from user
> num_tweets = st.number_input("Number of tweets to scrape", min_value=1, max_value=10000, value=100)
> 
> # Scrape tweets and display as table
> if st.button("Scrape Tweets"):
>     with st.spinner(f"Scraping {num_tweets} tweets..."):
>         tweets_df = get_tweets(keyword, num_tweets)
>     st.write(tweets_df)
> 
>     # Add download link for CSV file
>     csv = tweets_df.to_csv(index=False)
>     b64 = base64.b64encode(csv.encode()).decode()
>     href = f'<a href="data:file/csv;base64,{b64}" download="tweets.csv">Download CSV file</a>'
>     st.markdown(href, unsafe_allow_html=True)
> ```
> 
> Dalam kode di atas, kami menambahkan input `num_tweets` dengan menggunakan `st.number_input` sehingga pengguna dapat memilih jumlah tweet yang ingin diambil. Kami juga menambahkan fitur download untuk hasil scraping dalam format CSV dengan menggunakan modul `base64` dan mengenkripsinya dalam format byte sebelum menambahkannya ke link download.
> 
> tambahkan fiture untuk download txt atau csv dan dapat kan tweet.user.id juga
> 
> Berikut ini adalah contoh kodenya untuk menambahkan fitur download txt atau csv dan juga menambahkan kolom `tweet_user_id` pada hasil scraping:
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> 
> def search_tweets(keyword, since_date, until_date, num_tweets):
>     tweets_list = []
>     for i, tweet in enumerate(sntwitter.TwitterSearchScraper(
>         f'{keyword} since:{since_date} until:{until_date}').get_items()):
>         if i >= num_tweets:
>             break
>         tweets_list.append([tweet.id, tweet.date, tweet.content, tweet.user.username, tweet.user.followersCount, tweet.url])
>     tweets_df = pd.DataFrame(tweets_list, columns=['tweet_id', 'tweet_datetime', 'tweet_text', 'tweet_user', 'tweet_user_followers', 'tweet_url'])
>     tweets_df['tweet_user_id'] = tweets_df.apply(lambda row: row.tweet_url.split('/')[-1], axis=1)
>     return tweets_df
> 
> st.title("Twitter Scraper")
> 
> st.sidebar.header("Search Settings")
> keyword = st.sidebar.text_input("Keyword")
> since_date = st.sidebar.date_input("Since")
> until_date = st.sidebar.date_input("Until")
> num_tweets = st.sidebar.number_input("Number of tweets", min_value=1, max_value=1000, value=100)
> 
> if st.sidebar.button("Search"):
>     tweets_df = search_tweets(keyword, since_date, until_date, num_tweets)
>     st.write(tweets_df)
>     if st.button("Download as CSV"):
>         csv = tweets_df.to_csv(index=False)
>         b64 = base64.b64encode(csv.encode()).decode()
>         href = f'<a href="data:file/csv;base64,{b64}" download="tweets.csv">Download CSV</a>'
>         st.markdown(href, unsafe_allow_html=True)
>     if st.button("Download as TXT"):
>         txt = "\n".join(list(tweets_df.tweet_text))
>         b64 = base64.b64encode(txt.encode()).decode()
>         href = f'<a href="data:file/txt;base64,{b64}" download="tweets.txt">Download TXT</a>'
>         st.markdown(href, unsafe_allow_html=True)
> ```
> 
> Pada bagian `search_tweets()`, kolom `tweet_user_id` ditambahkan dengan memecah URL tweet dan mengambil bagian terakhir. Kemudian pada bagian `if st.button("Download as CSV"):` dan `if st.button("Download as TXT"):` ditambahkan fitur download file CSV dan TXT dengan mengkonversi dataframe ke file CSV dan menggabungkan semua tweet text ke dalam satu string dan menyimpannya dalam file TXT.
> 
> perbaiki tombol donwload csv dan txt tidak bisa digunakan
> 
> Untuk memperbaiki tombol download CSV dan TXT, Anda dapat mengikuti langkah-langkah berikut:
> 
> 1.  Buat dua fungsi, satu untuk menghasilkan file CSV dan satu lagi untuk file TXT. Misalnya, fungsi `create_csv_file()` dan `create_txt_file()`.
>     
> 2.  Dalam kedua fungsi tersebut, buat variabel `output` yang berisi semua data tweet yang telah diambil dari Twitter.
>     
> 3.  Untuk fungsi `create_csv_file()`, buat dataframe menggunakan library pandas dari `output`. Kemudian simpan dataframe sebagai file CSV menggunakan fungsi `to_csv()` dari pandas.
>     
> 4.  Untuk fungsi `create_txt_file()`, buat variabel `text` dengan menggabungkan semua tweet yang ada dalam `output` dengan karakter "\\n" sebagai pemisah. Kemudian simpan variabel `text` sebagai file TXT menggunakan fungsi `open()`.
>     
> 5.  Pada file app.py, tambahkan sebuah tombol dengan menggunakan fungsi `st.download_button()`. Berikan parameter pada fungsi tersebut dengan nama file yang ingin di-download dan fungsi yang akan menghasilkan file tersebut.
>     
> 
> Contoh implementasi di dalam app.py:
> 
> python
> 
> ```python
> import streamlit as st
> import snscrape.modules.twitter as sntwitter
> import pandas as pd
> 
> def create_csv_file(output):
>     df = pd.DataFrame(output, columns=['date', 'tweet.id', 'username', 'followers', 'tweet.url', 'tweet'])
>     df.to_csv('tweets.csv', index=False)
> 
> def create_txt_file(output):
>     text = "\n".join(output)
>     with open('tweets.txt', 'w') as f:
>         f.write(text)
> 
> # ...
> 
> # Tambahkan tombol download CSV
> if st.button('Download CSV'):
>     create_csv_file(output)
>     with open('tweets.csv', 'rb') as f:
>         st.download_button(label='Download CSV', data=f, file_name='tweets.csv', mime='text/csv')
> 
> # Tambahkan tombol download TXT
> if st.button('Download TXT'):
>     create_txt_file(output)
>     with open('tweets.txt', 'rb') as f:
>         st.download_button(label='Download TXT', data=f, file_name='tweets.txt', mime='text/plain')
> ```
> 
> Dalam contoh di atas, terdapat dua tombol download: satu untuk file CSV dan satu lagi untuk file TXT. Jika pengguna mengeklik salah satu tombol, maka akan memanggil fungsi yang sesuai dan menghasilkan file yang akan didownload. File akan didownload menggunakan fungsi `st.download_button()` dengan label sesuai nama file yang ingin didownload, data dari file yang dihasilkan, dan mime type sesuai dengan tipe file.
> 
> tambahkan fiture untuk download txt atau csv dan dapat kan tweet.user.id juga dan dapat diguanakan secara interaktif dan intuitif dan tambahakan fiture lagi dapat mencari berdasarkan negara dan dapat mengatau berapa twett yang akan di proses di tombol input
> 
> Berikut adalah contoh kode untuk menambahkan fitur download ke dalam program Twitter scraper yang sudah dibuat sebelumnya, dengan tambahan fitur untuk mencari berdasarkan negara dan jumlah tweet yang akan diproses. Untuk mencari berdasarkan negara, kita akan menggunakan filter `near:` dan untuk membatasi jumlah tweet yang akan diproses, kita akan menambahkan widget `slider`.
> 
> python
> 
> ```python
> import streamlit as st
> import pandas as pd
> import snscrape.modules.twitter as sntwitter
> 
> # Function to clean the text of the tweet
> def clean_tweet_text(tweet):
>     cleaned_tweet = tweet.replace('\n', ' ').replace('\r', '').replace('\t', ' ')
>     return cleaned_tweet
> 
> # Streamlit app
> st.title("Twitter Scraper")
> 
> # Set up Twitter API credentials
> consumer_key = st.secrets["consumer_key"]
> consumer_secret = st.secrets["consumer_secret"]
> access_token = st.secrets["access_token"]
> access_token_secret = st.secrets["access_token_secret"]
> 
> # Get user input for query, date range, and number of tweets
> query = st.text_input("Enter search query")
> start_date = st.date_input("Start date")
> end_date = st.date_input("End date")
> num_tweets = st.slider("Number of tweets", min_value=10, max_value=1000, step=10, value=100)
> 
> # Get user input for country
> country_code = st.text_input("Enter country code (ISO 3166-1 alpha-2)")
> 
> # Set up Twitter API query
> query += f" near:{country_code} within:15mi"
> since_date = start_date.strftime('%Y-%m-%d')
> until_date = end_date.strftime('%Y-%m-%d')
> query += f" since:{since_date} until:{until_date}"
> query += f" lang:en"
> 
> # Run Twitter API query
> tweets = []
> for i, tweet in enumerate(sntwitter.TwitterSearchScraper(query).get_items()):
>     if i >= num_tweets:
>         break
>     tweets.append([tweet.id, tweet.date, tweet.username, tweet.user.followersCount, tweet.content, tweet.url])
> 
> # Create pandas dataframe of tweets
> df = pd.DataFrame(tweets, columns=['tweet_id', 'datetime', 'username', 'followers', 'text', 'url'])
> 
> # Add a cleaned text column
> df['cleaned_text'] = df['text'].apply(clean_tweet_text)
> 
> # Show dataframe of tweets
> st.write(df)
> 
> # Allow user to download dataframe as CSV or TXT file
> if st.button("Download as CSV"):
>     csv = df.to_csv(index=False)
>     b64 = base64.b64encode(csv.encode()).decode()
>     href = f'<a href="data:file/csv;base64,{b64}" download="tweets.csv">Download CSV</a>'
>     st.markdown(href, unsafe_allow_html=True)
> 
> if st.button("Download as TXT"):
>     txt = '\n'.join(df['cleaned_text'])
>     b64 = base64.b64encode(txt.encode()).decode()
>     href = f'<a href="data:file/txt;base64,{b64}" download="tweets.txt">Download TXT</a>'
>     st.markdown(href, unsafe_allow_html=True)
> ```
> 
> Dalam kode di atas, kita menggunakan widget `text_input` untuk meminta input dari user tentang kode negara dan widget `slider` untuk meminta input tentang jumlah tweet yang akan diproses. Kita juga menambahkan dua tombol baru untuk men-download dataframe sebagai file CSV atau TXT. Tombol-tombol ini menggunakan modul `base64` untuk mengenkripsi dataframe sebagai string base64 sebelum men-generate tautan
> 
> buatakan script secrets.toml dengan ini
> 
> Untuk membuat file `secrets.toml`, kamu dapat mengikuti langkah-langkah berikut:
> 
> 1.  Buatlah sebuah file kosong bernama `secrets.toml`.
> 2.  Buka file tersebut menggunakan editor teks favoritmu.
> 3.  Tambahkan informasi rahasia atau kredensialmu pada file tersebut dalam format TOML. Misalnya, jika kamu ingin menyimpan API key Twitter, kamu dapat menambahkan baris berikut pada file `secrets.toml`:
> 
> csharp
> 
> ```csharp
> [api_keys]
> twitter = "your_twitter_api_key_here"
> ```
> 
> 4.  Simpan file tersebut.
> 
> Pastikan untuk tidak membagikan file `secrets.toml` dengan orang lain atau memasukkannya ke dalam repositori publik. Sebaiknya simpan file tersebut di tempat yang aman dan hanya memberikan akses ke orang-orang yang perlu tahu.
> 
> tambhkan fiture yang lebih informatif dan intuitif dan dapat menganti negara berdasarkan id,en dll
> 
> Berikut adalah contoh script Streamlit yang memperbarui fitur-fitur yang disebutkan:
> 
> python
> 
> ```python
> import streamlit as st
> import pandas as pd
> import snscrape.modules.twitter as sntwitter
> import itertools
> import csv
> 
> # Load API keys from secrets.toml file
> import os
> from dotenv import load_dotenv
> load_dotenv()
> 
> consumer_key = os.getenv('CONSUMER_KEY')
> consumer_secret = os.getenv('CONSUMER_SECRET')
> access_token = os.getenv('ACCESS_TOKEN')
> access_token_secret = os.getenv('ACCESS_TOKEN_SECRET')
> 
> st.set_page_config(
>     page_title="Twitter Search",
>     page_icon=":bird:",
>     layout="wide",
>     initial_sidebar_state="expanded",
> )
> 
> st.sidebar.title("Twitter Search Settings")
> search_term = st.sidebar.text_input("Search Term", "#OpenAI")
> max_tweets = st.sidebar.number_input("Number of Tweets", min_value=1, max_value=10000, value=1000, step=10)
> since_date = st.sidebar.date_input("Start Date", value=None)
> until_date = st.sidebar.date_input("End Date", value=None)
> 
> lang_list = ["ar", "bn", "cs", "da", "de", "el", "en", "es", "et", "fa", "fi", "fil", "fr", "he", "hi", "hr", "hu", "hy", "id", "it", "ja", "ko", "lt", "lv", "ml", "mr", "msa", "nl", "no", "pl", "ps", "pt", "ro", "ru", "sk", "sl", "sr", "sv", "ta", "te", "th", "tl", "tr", "uk", "ur", "vi", "zh-cn", "zh-tw"]
> lang = st.sidebar.selectbox("Language", lang_list, index=6)
> 
> place_list = ["", "United States", "India", "Indonesia", "Malaysia", "Singapore"]
> place = st.sidebar.selectbox("Place", place_list, index=0)
> 
> if place == "":
>     search_query = search_term
> else:
>     search_query = search_term + " place:{}".format(place)
> 
> if since_date is not None:
>     search_query += " since:{}".format(since_date.strftime('%Y-%m-%d'))
> if until_date is not None:
>     search_query += " until:{}".format(until_date.strftime('%Y-%m-%d'))
> 
> with st.spinner("Searching for tweets..."):
>     # Define CSV file writer for storing the tweet data
>     csv_file = open('tweets.csv', 'w', newline='', encoding='utf-8')
>     csv_writer = csv.writer(csv_file)
>     csv_writer.writerow(["id", "date", "tweet", "username", "user_id", "followers", "url"])
> 
>     # Define Twitter search query
>     twitter_query = search_query + " lang:{}".format(lang)
> 
>     # Loop through each tweet and write its details to CSV file
>     for i, tweet in enumerate(itertools.islice(sntwitter.TwitterSearchScraper(
>         twitter_query + ' filter:links min_faves:10 min_retweets:10').get_items(), max_tweets)):
>         csv_writer.writerow([tweet.id, tweet.date.strftime('%Y-%m-%d %H:%M:%S'), tweet.content, tweet.user.username, tweet.user.id, tweet.user.followersCount, tweet.url])
>     
>     # Close the CSV file writer
>     csv_file.close()
> 
> # Load the tweet data from CSV file
> tweet_df = pd.read_csv('tweets.csv')
> 
> # Display the tweet data in Streamlit app
> st.header("Twitter Search
> ```
> 
> lanjutkan script di atas
> 
> Berikut adalah script lengkap untuk scraping Twitter dengan menggunakan snscrape dan Streamlit, dengan fitur yang dijelaskan sebelumnya dan juga secrets.toml:
> 
> python
> 
> ```python
> import streamlit as st
> import pandas as pd
> import os
> import snscrape.modules.twitter as sntwitter
> import csv
> from datetime import datetime
> import pytz
> import toml
> 
> # Load secrets from secrets.toml file
> secrets = toml.load("secrets.toml")
> 
> # Set Twitter API credentials
> os.environ["TWITTER_API_KEY"] = secrets["twitter"]["api_key"]
> os.environ["TWITTER_API_SECRET_KEY"] = secrets["twitter"]["api_secret_key"]
> os.environ["TWITTER_ACCESS_TOKEN"] = secrets["twitter"]["access_token"]
> os.environ["TWITTER_ACCESS_TOKEN_SECRET"] = secrets["twitter"]["access_token_secret"]
> 
> # Function to scrape tweets
> def scrape_tweets(search_query, num_tweets, lang, start_date, end_date, country):
>     tweets_list = []
>     # Define the time zone
>     tz = pytz.timezone('Asia/Jakarta')
>     # Define the search query
>     search_words = search_query + ' lang:' + lang + ' until:' + end_date + ' since:' + start_date
>     if country != '':
>         search_words += ' place_country:' + country
>     # Using TwitterSearchScraper to scrape data and append tweets to list
>     for i, tweet in enumerate(sntwitter.TwitterSearchScraper(search_words).get_items()):
>         if i >= num_tweets:
>             break
>         tweets_list.append([tweet.id, tweet.date.astimezone(tz), tweet.user.username, tweet.user.followersCount, tweet.content, tweet.url])
>     # Create a dataframe from the tweets list
>     tweets_df = pd.DataFrame(tweets_list, columns=['id', 'datetime', 'username', 'followers', 'text', 'url'])
>     return tweets_df
> 
> # Function to download tweets as CSV
> def download_tweets_csv(df):
>     csv = df.to_csv(index=False)
>     b64 = base64.b64encode(csv.encode()).decode()
>     href = f'<a href="data:file/csv;base64,{b64}" download="tweets.csv">Download CSV File</a>'
>     return href
> 
> # Function to download tweets as text
> def download_tweets_txt(df):
>     txt = df.to_csv(index=False, sep='\t')
>     b64 = base64.b64encode(txt.encode()).decode()
>     href = f'<a href="data:file/txt;base64,{b64}" download="tweets.txt">Download TXT File</a>'
>     return href
> 
> # Title of the app
> st.title('Twitter Scraper')
> 
> # Input fields
> search_query = st.text_input('Enter the search query:', '#covid19')
> num_tweets = st.number_input('Number of tweets:', min_value=1, value=10)
> lang = st.selectbox('Language:', ['id', 'en'])
> start_date = st.date_input('Start date:', datetime(2022, 1, 1))
> end_date = st.date_input('End date:', datetime.now())
> country = st.selectbox('Country:', ['', 'ID', 'US', 'GB'])
> 
> # Scrape tweets when button is clicked
> if st.button('Scrape Tweets'):
>     st.write('Scraping tweets, please wait...')
>     # Format start and end date
>     start_date_str = start_date.strftime("%Y-%m-%d")
>     end_date_str = end_date.strftime("%Y-%m-%d")
>     # Scrape tweets
>     tweets_df = scrape_tweets(search_query, num_tweets, lang, start_date_str, end_date_str, country)
>     # Display scraped tweets
>     st.write
> ```