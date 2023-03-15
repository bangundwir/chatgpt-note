Model: GPT-4

> buatakan script openai yang di streamlit yang bisa melkakuan index di bisa di simpan di pinecone gunakan gpt-3.5-turbo

Untuk membuat sebuah script yang menggunakan OpenAI GPT-3.5-turbo dan diintegrasikan dengan Streamlit serta Pinecone, Anda dapat mengikuti langkah-langkah berikut:

1.  Install package yang diperlukan

bash

```bash
pip install streamlit openai pinecone-client
```

2.  Buat file `app.py` dan letakkan kode berikut di dalamnya:

python

```python
import streamlit as st
import openai
import pinecone
from pinecone.deployment import ConnectionType

# Ganti dengan API key yang sesuai
OPENAI_API_KEY = "your_openai_api_key"
PINECONE_API_KEY = "your_pinecone_api_key"

openai.api_key = OPENAI_API_KEY
pinecone.init(api_key=PINECONE_API_KEY)

# Inisialisasi Pinecone
pinecone.deinit()
pinecone.init(api_key=PINECONE_API_KEY)

pinecone_service_name = "pinecone-gpt-3.5-index"
pinecone.create_deployment(service_name=pinecone_service_name, connection_type=ConnectionType.DIRECT)

pinecone_index = pinecone.Index(index_name="gpt-3.5-index")

def gpt_3_query(prompt):
    model_engine = "text-davinci-002"
    response = openai.Completion.create(
        engine=model_engine,
        prompt=prompt,
        max_tokens=100,
        n=1,
        stop=None,
        temperature=0.5,
    )
    return response.choices[0].text.strip()

def index_text(text_id, text):
    pinecone_index.upsert(items={text_id: text})

def delete_text(text_id):
    pinecone_index.delete(ids=[text_id])

# Streamlit app
st.title("OpenAI GPT-3.5 Turbo with Pinecone Integration")

user_prompt = st.text_area("Enter your prompt for GPT-3.5 Turbo:")

if st.button("Generate Response"):
    response = gpt_3_query(user_prompt)
    st.write("Generated Response: ", response)

st.header("Index Management")
text_id = st.text_input("Enter a text ID:")
text_content = st.text_area("Enter the text content to be indexed:")

if st.button("Index Text"):
    index_text(text_id, text_content)
    st.write("Text indexed successfully!")

if st.button("Delete Text"):
    delete_text(text_id)
    st.write("Text deleted successfully!")

pinecone.deinit()
```

3.  Ganti `your_openai_api_key` dan `your_pinecone_api_key` dengan kunci API yang sesuai.
    
4.  Jalankan aplikasi Streamlit:
    

bash

```bash
streamlit run app.py
```

Aplikasi ini memungkinkan Anda untuk:

*   Mengirim teks ke GPT-3.5 Turbo dan menampilkan hasilnya.
*   Menambahkan teks ke indeks Pinecone menggunakan ID teks dan konten teks.
*   Menghapus teks dari indeks Pinecone menggunakan ID teks.

Pastikan Anda memiliki akses ke OpenAI API dan Pinecone serta telah menginstal package yang diperlukan sebelum menjalankan skrip ini.