Model: GPT-4

> buatakan website interaktif dengan gaya 90 an dan dapat update tulisda dari (HTML,css,javascript) dan buatakan generate md to html di folder blog mengunakan python dan dapat melis otomatis setiap hal yang sudah di buat

Untuk membuat website interaktif dengan gaya 90-an dan menggunakan HTML, CSS, dan JavaScript, Anda bisa mengikuti langkah-langkah berikut:

1.  Buat struktur dasar HTML:

html

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Website 90-an</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Selamat Datang di Website 90-an</h1>
    </header>
    <main>
        <section id="blog">
            <!-- Artikel akan diisi di sini -->
        </section>
    </main>
    <script src="scripts.js"></script>
</body>
</html>
```

2.  Buat gaya 90-an menggunakan CSS:

css

```css
/* styles.css */

body {
    background-color: #008080;
    font-family: 'Courier New', Courier, monospace;
    color: #ffffff;
}

header {
    text-align: center;
    margin-bottom: 2rem;
}

article {
    margin-bottom: 2rem;
}

h1, h2 {
    text-decoration: underline;
}
```

3.  Buat script JavaScript untuk mengambil data artikel dari server:

javascript

```javascript
// scripts.js

document.addEventListener('DOMContentLoaded', async function() {
    const blogSection = document.getElementById('blog');

    try {
        const response = await fetch('/blog');
        const articles = await response.json();

        articles.forEach(article => {
            const articleElement = `
                <article>
                    <h2>${article.title}</h2>
                    <p>${article.content}</p>
                </article>
            `;
            blogSection.innerHTML += articleElement;
        });
    } catch (error) {
        console.error('Error fetching articles:', error);
    }
});
```

4.  Buat script Python untuk mengkonversi Markdown ke HTML dan menyajikan file HTML:

python

```python
# server.py
import os
import json
from http.server import SimpleHTTPRequestHandler, HTTPServer
from urllib.parse import urlparse
import markdown

class CustomRequestHandler(SimpleHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/blog':
            self.send_response(200)
            self.send_header('Content-Type', 'application/json')
            self.end_headers()

            articles = []

            for filename in os.listdir('blog'):
                if filename.endswith('.md'):
                    with open(os.path.join('blog', filename), 'r', encoding='utf-8') as file:
                        content = file.read()
                        html_content = markdown.markdown(content)
                        title = os.path.splitext(filename)[0]

                        articles.append({'title': title, 'content': html_content})

            self.wfile.write(json.dumps(articles).encode('utf-8'))
        else:
            super().do_GET()

if __name__ == '__main__':
    os.chdir('public')  # Asumsikan file HTML, CSS, dan JS disimpan di folder "public"
    server_address = ('', 8000)
    httpd = HTTPServer(server_address, CustomRequestHandler)
    print('Server is running on port 8000...')
    httpd.serve_forever()
```

5.  Jalankan server Python:

`python server.py`

6.  Buka browser dan akses `http://localhost:8000`. Website interaktif dengan gaya 90-an akan ditampilkan, dan artikel-artikel dalam folder 'blog' akan diubah dari format Markdown ke HTML dan ditampilkan secara

> lanjutkan

otomatis di halaman web.

Untuk menambahkan artikel baru, ikuti langkah-langkah berikut:

1.  Buat file baru dalam folder "blog" dengan ekstensi ".md" (contoh: "artikel1.md").
2.  Tulis konten artikel dalam format Markdown.
3.  Simpan file tersebut dan refresh halaman web. Artikel baru akan ditampilkan secara otomatis di halaman web.

Demikian langkah-langkah yang perlu Anda lakukan untuk membuat website interaktif dengan gaya 90-an, menggunakan HTML, CSS, dan JavaScript, serta mengonversi Markdown ke HTML menggunakan Python. Jangan lupa untuk menyesuaikan script Python dan file HTML, CSS, dan JavaScript sesuai dengan kebutuhan Anda.