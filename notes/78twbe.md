> if len(tweets\_df) > 0: csv = tweets\_df.to\_csv(index=False) b64 = base64.b64encode(csv.encode()).decode() filename = f'tweets\_{query.replace(" ", "\_")}.csv' href = f'<a href="data:file/csv;base64,{b64}" download="{filename}">Download CSV</a>' st.markdown(href, unsafe\_allow\_html=True) if len(tweets\_df) > 0: txt = "\\n\\n".join(tweets\_df\['Text'\].tolist()) b64 = base64.b64encode(txt.encode()).decode() txt\_filename = f"{query}\_tweets.txt" txt\_href = f'<a href="data:file/txt;base64,{b64}" download="{txt\_filename}">Download TXT</a>' st.markdown(txt\_href, unsafe\_allow\_html=True) Perbaiki script tersebut biar clean code

---

Berikut adalah perbaikan pada script tersebut agar menjadi clean code:

python

```python
if not tweets_df.empty:
    # Download CSV
    csv = tweets_df.to_csv(index=False)
    b64 = base64.b64encode(csv.encode()).decode()
    filename = f'tweets_{query.replace(" ", "_")}.csv'
    href = f'<a href="data:file/csv;base64,{b64}" download="{filename}">Download CSV</a>'
    st.markdown(href, unsafe_allow_html=True)

    # Download TXT
    txt = "\n\n".join(tweets_df['Text'].tolist())
    b64 = base64.b64encode(txt.encode()).decode()
    txt_filename = f"{query}_tweets.txt"
    txt_href = f'<a href="data:file/txt;base64,{b64}" download="{txt_filename}">Download TXT</a>'
    st.markdown(txt_href, unsafe_allow_html=True)
```

Penjelasan perubahan:

*   Mengganti `len(tweets_df) > 0` dengan `not tweets_df.empty` karena lebih mudah dibaca dan lebih spesifik dalam mengecek apakah DataFrame kosong atau tidak.
*   Menghilangkan duplikasi kode pada bagian download CSV dan TXT, sehingga menjadi lebih efisien dan mudah dipelihara.

---

> buatakan streamlit pretrained = "mdhugol/indonesia-bert-sentiment-classification" from transformers import pipeline from transformers import AutoTokenizer, AutoModelForSequenceClassification logging.info(f"loading model from {pretrained}...") model = AutoModelForSequenceClassification.from\_pretrained(pretrained) tokenizer = AutoTokenizer.from\_pretrained(pretrained) sentiment\_analysis = pipeline("sentiment-analysis", model=model, tokenizer=tokenizer) label\_index = {'LABEL\_0': 'positive', 'LABEL\_1': 'neutral', 'LABEL\_2': 'negative'}

---

Berikut ini adalah script untuk membuat pipeline sentiment analysis menggunakan model BERT pretrained `mdhugol/indonesia-bert-sentiment-classification` pada Streamlit:

python

```python
import logging
import streamlit as st
from transformers import pipeline, AutoTokenizer, AutoModelForSequenceClassification


pretrained = "mdhugol/indonesia-bert-sentiment-classification"

logging.info(f"loading model from {pretrained}...")

model = AutoModelForSequenceClassification.from_pretrained(pretrained)
tokenizer = AutoTokenizer.from_pretrained(pretrained)

sentiment_analysis = pipeline("sentiment-analysis", model=model, tokenizer=tokenizer)

label_index = {'LABEL_0': 'positive', 'LABEL_1': 'neutral', 'LABEL_2': 'negative'}

# Usage example
text = st.text_input("Input your text here:")
if text:
    result = sentiment_analysis(text)[0]
    st.write(f"Sentiment: {label_index[result['label']]}")
    st.write(f"Score: {result['score']}")
```

Penjelasan script:

*   `logging` digunakan untuk mencatat pesan log tentang proses loading model.
*   `streamlit` adalah framework untuk membangun aplikasi web interaktif.
*   `AutoTokenizer` dan `AutoModelForSequenceClassification` digunakan untuk memuat model dan tokenisasi teks dengan BERT tokenizer.
*   `pipeline` digunakan untuk membuat pipeline NLP dari model dan tokenizer.
*   `label_index` adalah dictionary yang menunjukkan korespondensi antara label dan sentimen, sehingga mudah untuk menginterpretasikan hasil prediksi.
*   Kemudian, pada contoh penggunaan, aplikasi memungkinkan pengguna untuk menginputkan teks melalui `st.text_input`. Jika terdapat input, maka model akan menganalisis sentimen pada teks tersebut dan hasilnya akan ditampilkan pada halaman web melalui `st.write`.

---

> buatakan api dari pretrained = "mdhugol/indonesia-bert-sentiment-classification" from transformers import pipeline from transformers import AutoTokenizer, AutoModelForSequenceClassification logging.info(f"loading model from {pretrained}...") model = AutoModelForSequenceClassification.from\_pretrained(pretrained) tokenizer = AutoTokenizer.from\_pretrained(pretrained) sentiment\_analysis = pipeline("sentiment-analysis", model=model, tokenizer=tokenizer) label\_index = {'LABEL\_0': 'positive', 'LABEL\_1': 'neutral', 'LABEL\_2': 'negative'}

---

Berikut ini adalah script untuk membuat API menggunakan Flask dengan menggunakan model BERT pretrained `mdhugol/indonesia-bert-sentiment-classification`:

python

```python
import logging
import flask
from transformers import pipeline, AutoTokenizer, AutoModelForSequenceClassification

app = flask.Flask(__name__)

pretrained = "mdhugol/indonesia-bert-sentiment-classification"

logging.info(f"loading model from {pretrained}...")

model = AutoModelForSequenceClassification.from_pretrained(pretrained)
tokenizer = AutoTokenizer.from_pretrained(pretrained)

sentiment_analysis = pipeline("sentiment-analysis", model=model, tokenizer=tokenizer)

label_index = {'LABEL_0': 'positive', 'LABEL_1': 'neutral', 'LABEL_2': 'negative'}

@app.route('/', methods=['GET'])
def home():
    return "Indonesian Sentiment Analysis API"

@app.route('/predict', methods=['POST'])
def predict():
    text = flask.request.json['text']
    result = sentiment_analysis(text)[0]
    sentiment = label_index[result['label']]
    score = result['score']
    response = {'sentiment': sentiment, 'score': score}
    return flask.jsonify(response)

if __name__ == '__main__':
    app.run(debug=True)
```

Penjelasan script:

*   `Flask` adalah web framework yang digunakan untuk membuat API.
*   `AutoTokenizer` dan `AutoModelForSequenceClassification` digunakan untuk memuat model dan tokenisasi teks dengan BERT tokenizer.
*   `pipeline` digunakan untuk membuat pipeline NLP dari model dan tokenizer.
*   `label_index` adalah dictionary yang menunjukkan korespondensi antara label dan sentimen, sehingga mudah untuk menginterpretasikan hasil prediksi.
*   Kemudian, pada fungsi `home()`, API akan memberikan respons pada url `http://localhost:5000/`.
*   Pada fungsi `predict()`, API akan menerima input teks yang dikirimkan sebagai JSON pada body request. Lalu, model akan menganalisis sentimen pada teks tersebut dan hasilnya akan dikirimkan sebagai JSON response pada body response.
*   `app.run()` digunakan untuk menjalankan API pada localhost dengan mode debug.

---

> buatakan api di atas agar bisa digunakan di streamlit

---

Berikut ini adalah contoh bagaimana API yang telah dibuat sebelumnya dapat diintegrasikan ke dalam aplikasi Streamlit:

python

```python
import streamlit as st
import requests

# Define API endpoint
API_ENDPOINT = "http://localhost:5000/predict"

# Create a function to send request to API
def analyze_sentiment(text):
    response = requests.post(API_ENDPOINT, json={'text': text})
    if response.status_code == 200:
        return response.json()
    else:
        return None

# Create Streamlit app
st.title("Indonesian Sentiment Analysis")

# Create a text input for users to input text
text_input = st.text_input("Input some Indonesian text")

# Add a button for users to submit text for analysis
if st.button("Analyze"):
    if text_input:
        # Call the analyze_sentiment function to get sentiment analysis result
        result = analyze_sentiment(text_input)
        if result:
            sentiment = result['sentiment']
            score = result['score']
            st.write(f"Sentiment: {sentiment}")
            st.write(f"Score: {score:.4f}")
        else:
            st.write("Error analyzing sentiment")
    else:
        st.write("Please input some text")
```

Penjelasan script:

*   `streamlit` digunakan untuk membuat aplikasi web yang interaktif.
*   `requests` digunakan untuk mengirimkan request ke API.
*   `API_ENDPOINT` adalah endpoint dari API yang telah dibuat sebelumnya.
*   `analyze_sentiment()` adalah fungsi yang digunakan untuk mengirimkan request ke API dan menerima hasilnya sebagai JSON.
*   Pada bagian Streamlit app, judul halaman ditampilkan menggunakan `st.title()`.
*   `text_input` adalah input teks yang diberikan oleh user menggunakan `st.text_input()`.
*   Tombol `Analyze` dibuat menggunakan `st.button()` dan akan memanggil fungsi `analyze_sentiment()` saat ditekan.
*   Hasil analisis sentimen akan ditampilkan menggunakan `st.write()`.

---

> ketika menjalankan aplikasi streamlit langsung menjalankan api nya juga

---

Anda dapat menjalankan API dan aplikasi Streamlit secara bersamaan dalam terminal atau command prompt yang berbeda. Berikut adalah contoh cara menjalankannya:

1.  Buka command prompt atau terminal baru.
2.  Pindah ke direktori yang berisi file API dan jalankan API dengan perintah `python app.py`.
3.  Buka command prompt atau terminal baru lagi.
4.  Pindah ke direktori yang berisi file aplikasi Streamlit dan jalankan aplikasi Streamlit dengan perintah `streamlit run app.py`.

Dengan menjalankan kedua perintah tersebut secara bersamaan, Anda dapat mengakses aplikasi Streamlit melalui browser dan melakukan analisis sentimen menggunakan API yang telah dijalankan sebelumnya.

---

> jadikan api dari flask dan integrasikan ke streamlit dalam satu kode

---

Berikut ini adalah contoh bagaimana API Flask dan aplikasi Streamlit dapat digabungkan dalam satu kode:

python

```python
import streamlit as st
import requests
from flask import Flask, jsonify, request
from transformers import AutoTokenizer, AutoModelForSequenceClassification

app = Flask(__name__)

pretrained = "mdhugol/indonesia-bert-sentiment-classification"
model = AutoModelForSequenceClassification.from_pretrained(pretrained)
tokenizer = AutoTokenizer.from_pretrained(pretrained)

label_index = {'LABEL_0': 'positive', 'LABEL_1': 'neutral', 'LABEL_2': 'negative'}

@app.route('/predict', methods=['POST'])
def predict():
    text = request.json['text']
    inputs = tokenizer(text, return_tensors='pt')
    outputs = model(**inputs)
    logits = outputs.logits
    index = logits.argmax().item()
    sentiment = label_index[f'LABEL_{index}']
    score = logits.softmax(dim=1)[0][index].item()
    result = {'sentiment': sentiment, 'score': score}
    return jsonify(result)

def analyze_sentiment(text):
    response = requests.post('http://localhost:5000/predict', json={'text': text})
    if response.status_code == 200:
        return response.json()
    else:
        return None

st.title("Indonesian Sentiment Analysis")

text_input = st.text_input("Input some Indonesian text")

if st.button("Analyze"):
    if text_input:
        result = analyze_sentiment(text_input)
        if result:
            sentiment = result['sentiment']
            score = result['score']
            st.write(f"Sentiment: {sentiment}")
            st.write(f"Score: {score:.4f}")
        else:
            st.write("Error analyzing sentiment")
    else:
        st.write("Please input some text")

if __name__ == '__main__':
    app.run(debug=False)
```

Penjelasan script:

*   `streamlit` digunakan untuk membuat aplikasi web yang interaktif.
*   `requests` digunakan untuk mengirimkan request ke API.
*   `Flask` digunakan untuk membuat API.
*   `AutoTokenizer` dan `AutoModelForSequenceClassification` digunakan untuk memuat model BERT dan tokenizernya.
*   `label_index` adalah kamus yang menunjukkan korespondensi antara label yang dihasilkan oleh model BERT dan sentimen yang sesuai.
*   Endpoint `/predict` didefinisikan dalam fungsi `predict()`.
*   Saat menerima POST request dengan payload berupa JSON, fungsi `predict()` akan mengekstrak teks dari payload, melakukan tokenisasi dan inferensi sentimen menggunakan model BERT, dan mengembalikan hasil dalam bentuk JSON.
*   Fungsi `analyze_sentiment()` dipanggil saat user menekan tombol `Analyze` pada aplikasi Streamlit untuk mengirimkan teks ke API.
*   Bagian Streamlit app sama seperti contoh sebelumnya, hanya saja request ke API dilakukan melalui `requests.post()` dengan endpoint `/predict` di `http://localhost:5000/predict`.
*   Terakhir, `app.run()` digunakan untuk menjalankan API Flask di port 5000 dengan mode debug yang dinonaktifkan.

---

> cara menjalankan script di atas

---

Untuk menjalankan script tersebut, ikuti langkah-langkah berikut:

1.  Pastikan sudah terinstall library Flask dan Streamlit. Jika belum terinstall, jalankan perintah berikut pada terminal atau command prompt:

`pip install flask streamlit`

2.  Simpan script di atas dengan nama file misalnya `app.py`.
    
3.  Buka terminal atau command prompt dan masuk ke direktori di mana file `app.py` disimpan.
    
4.  Jalankan perintah berikut untuk menjalankan Flask API:
    

javascript

```javascript
export FLASK_APP=app.py
flask run
```

5.  Setelah API Flask berjalan, buka terminal baru dan masuk ke direktori yang sama dengan sebelumnya.
    
6.  Jalankan perintah berikut untuk menjalankan Streamlit app:
    

`streamlit run app.py`

7.  Setelah menjalankan perintah di atas, buka browser dan akses `http://localhost:8501`.
    
8.  Aplikasi Streamlit seharusnya sudah berjalan dan Anda dapat menggunakannya.
    

---