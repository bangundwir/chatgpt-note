> buatakan script streamlit converter img to txt

Berikut adalah contoh script Streamlit yang dapat digunakan untuk mengonversi gambar menjadi teks (text) menggunakan Tesseract OCR (Optical Character Recognition):

python

```python
import streamlit as st
import pytesseract
from PIL import Image

# Set page title
st.set_page_config(page_title="Image to Text Converter")

# Set page heading
st.title("Image to Text Converter")

# Add file uploader
file = st.file_uploader("Upload an image", type=["jpg", "jpeg", "png"])

# Add button to perform OCR
if st.button("Convert"):
    if file is not None:
        image = Image.open(file)
        text = pytesseract.image_to_string(image)
        st.write(text)
    else:
        st.warning("Please upload an image.")
```

Untuk menjalankan script ini, pastikan sudah menginstal streamlit (`pip install streamlit`), pytesseract (`pip install pytesseract`), dan PIL (`pip install Pillow`). Selain itu, juga perlu menginstal Tesseract OCR pada sistem operasi yang digunakan. Lebih detail mengenai instalasi Tesseract OCR dapat ditemukan di [https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract).

> tambahkan fiture agar bisa di donwload file nya dan dapat mengambil gambar lewat url

Berikut adalah contoh script Streamlit dengan fitur tambahan untuk mendownload file hasil konversi dan mengambil gambar dari URL:

python

```python
import streamlit as st
import requests
import pytesseract
from PIL import Image

# Set page title
st.set_page_config(page_title="Image to Text Converter")

# Set page heading
st.title("Image to Text Converter")

# Add file uploader
file = st.file_uploader("Upload an image", type=["jpg", "jpeg", "png"])

# Add URL image input
url_input = st.text_input("Input image URL")

# Add button to perform OCR
if st.button("Convert"):
    if file is not None:
        image = Image.open(file)
        text = pytesseract.image_to_string(image)
        st.write(text)
        # Add download button
        st.download_button("Download Text as TXT", data=text, file_name="text_output.txt")
    elif url_input != "":
        try:
            response = requests.get(url_input)
            image = Image.open(BytesIO(response.content))
            text = pytesseract.image_to_string(image)
            st.write(text)
            # Add download button
            st.download_button("Download Text as TXT", data=text, file_name="text_output.txt")
        except:
            st.warning("Invalid URL or unsupported image format.")
    else:
        st.warning("Please upload an image or input an image URL.")
```

Pada script di atas, fitur tambahan yang ditambahkan adalah:

1.  Input URL gambar: `url_input = st.text_input("Input image URL")`
2.  Pengambilan gambar dari URL:
    
    python
    
    ```python
    response = requests.get(url_input)
    image = Image.open(BytesIO(response.content))
    ```
    
3.  Tombol unduh untuk hasil konversi teks:
    
    python
    
    ```python
    st.download_button("Download Text as TXT", data=text, file_name="text_output.txt")
    ```
    
    Tombol ini akan mengunduh teks hasil konversi ke dalam format TXT saat diklik.

Untuk menjalankan script ini, pastikan sudah menginstal streamlit (`pip install streamlit`), requests (`pip install requests`), pytesseract (`pip install pytesseract`), dan PIL (`pip install Pillow`). Selain itu, juga perlu menginstal Tesseract OCR pada sistem operasi yang digunakan. Lebih detail mengenai instalasi Tesseract OCR dapat ditemukan di [https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract).

> tulis kodenya menjadi 1 file tanpa penjelasan

Berikut adalah contoh kode Streamlit untuk mengonversi gambar menjadi teks dengan fitur tambahan untuk mendownload hasil konversi dan mengambil gambar dari URL dalam satu file:

python

```python
import streamlit as st
import requests
import pytesseract
from PIL import Image
from io import BytesIO

# Set page title
st.set_page_config(page_title="Image to Text Converter")

# Set page heading
st.title("Image to Text Converter")

# Add file uploader
file = st.file_uploader("Upload an image", type=["jpg", "jpeg", "png"])

# Add URL image input
url_input = st.text_input("Input image URL")

# Add button to perform OCR
if st.button("Convert"):
    if file is not None:
        image = Image.open(file)
        text = pytesseract.image_to_string(image)
        st.write(text)
        # Add download button
        st.download_button("Download Text as TXT", data=text, file_name="text_output.txt")
    elif url_input != "":
        try:
            response = requests.get(url_input)
            image = Image.open(BytesIO(response.content))
            text = pytesseract.image_to_string(image)
            st.write(text)
            # Add download button
            st.download_button("Download Text as TXT", data=text, file_name="text_output.txt")
        except:
            st.warning("Invalid URL or unsupported image format.")
    else:
        st.warning("Please upload an image or input an image URL.")
```

Untuk menjalankan script ini, pastikan sudah menginstal streamlit (`pip install streamlit`), requests (`pip install requests`), pytesseract (`pip install pytesseract`), dan PIL (`pip install Pillow`). Selain itu, juga perlu menginstal Tesseract OCR pada sistem operasi yang digunakan. Lebih detail mengenai instalasi Tesseract OCR dapat ditemukan di [https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract).