> buatkan script Convert PDF to TXT in Python
> 
> Berikut ini adalah script untuk mengkonversi file PDF menjadi teks dalam bahasa Python menggunakan library PyPDF2:
> 
> python
> 
> ```python
> import PyPDF2
> 
> # Membuka file PDF
> pdf_file = open('sample.pdf', 'rb')
> 
> # Membuat objek PDF reader
> pdf_reader = PyPDF2.PdfFileReader(pdf_file)
> 
> # Inisialisasi string teks
> text = ""
> 
> # Loop melalui halaman dalam file PDF
> for page in range(pdf_reader.numPages):
>     # Membaca halaman
>     pdf_page = pdf_reader.getPage(page)
>     # Ekstrak teks dari halaman
>     text += pdf_page.extractText()
> 
> # Menutup file PDF
> pdf_file.close()
> 
> # Menulis teks ke file teks
> text_file = open("sample.txt", "w")
> text_file.write(text)
> text_file.close()
> 
> print("File PDF berhasil dikonversi ke teks.")
> ```
> 
> Catatan: Library PyPDF2 tidak selalu dapat mengekstrak teks dengan tepat dari setiap halaman PDF, tergantung pada kualitas dan format file PDF.
> 
> tambahkan fiture dapat digunakan seperti python3 data.pdf
> 
> Berikut ini adalah script yang diperbarui untuk mengkonversi file PDF menjadi teks dalam bahasa Python dan dapat digunakan seperti `python3 data.pdf`:
> 
> python
> 
> ```python
> import sys
> import PyPDF2
> 
> if len(sys.argv) < 2:
>     print("Penggunaan: python3 pdf_to_txt.py <nama_file_pdf>")
>     sys.exit()
> 
> pdf_file_name = sys.argv[1]
> if not pdf_file_name.endswith(".pdf"):
>     print("Masukkan file PDF yang valid.")
>     sys.exit()
> 
> # Membuka file PDF
> pdf_file = open(pdf_file_name, 'rb')
> 
> # Membuat objek PDF reader
> pdf_reader = PyPDF2.PdfFileReader(pdf_file)
> 
> # Inisialisasi string teks
> text = ""
> 
> # Loop melalui halaman dalam file PDF
> for page in range(pdf_reader.numPages):
>     # Membaca halaman
>     pdf_page = pdf_reader.getPage(page)
>     # Ekstrak teks dari halaman
>     text += pdf_page.extractText()
> 
> # Menutup file PDF
> pdf_file.close()
> 
> # Membuat nama file teks dengan mengganti ekstensi .pdf dengan .txt
> txt_file_name = pdf_file_name.replace(".pdf", ".txt")
> 
> # Menulis teks ke file teks
> text_file = open(txt_file_name, "w")
> text_file.write(text)
> text_file.close()
> 
> print("File PDF '%s' berhasil dikonversi ke teks '%s'." % (pdf_file_name, txt_file_name))
> ```
> 
> Anda dapat menjalankan script ini dengan mengetikkan perintah `python3 pdf_to_txt.py data.pdf` pada terminal, di mana `data.pdf` adalah nama file PDF yang ingin Anda konversi.