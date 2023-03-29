Model: GPT-4

> const axios = require('axios'); const cheerio = require('cheerio'); const url = 'https://winpoin.com/petinggi-microsoft-bard-milik-google-tertinggal-cukup-jauh-di-belakang/'; axios.get(url) .then(response => { const $ = cheerio.load(response.data); // Ambil teks di dalam tag <h1> sebagai judul const judul = $('h1').text(); console.log(\`Judul: \\n${judul}\\n\`); // Ambil semua tag <p> sebagai paragraf const paragraf = \[\]; $('p').each((i, element) => { paragraf.push($(element).text()); }); // Tampilkan semua paragraf dengan format rapi console.log('Paragraf:\\n'); paragraf.forEach((p, i) => { console.log(\`${p}\\n\`); }); }) .catch(error => { console.error('Terjadi kesalahan:', error); }); Buatakan frontendnya mengunakan nodejs,html,css,javascript jangan gunakan ejs dan agar bisa memasukan url apapun

Untuk membuat frontend yang memungkinkan pengguna memasukkan URL dan menampilkan konten yang diambil, Anda bisa menggunakan Express sebagai web server dan menggabungkannya dengan HTML, CSS, dan JavaScript murni. Berikut adalah langkah-langkah untuk membuat aplikasi tersebut:

1.  Buat folder untuk proyek Anda dan jalankan `npm init` untuk membuat file `package.json`.
2.  Pasang paket yang diperlukan: `npm install express axios cheerio`.
3.  Buat file `app.js` dan tambahkan kode berikut untuk mengonfigurasi server Express:

javascript

```javascript
const express = require('express');
const axios = require('axios');
const cheerio = require('cheerio');
const app = express();

app.use(express.static('public'));
app.use(express.json());

app.post('/scrape', async (req, res) => {
  try {
    const url = req.body.url;
    const response = await axios.get(url);
    const $ = cheerio.load(response.data);

    const judul = $('h1').text();
    const paragraf = [];
    $('p').each((i, element) => {
      paragraf.push($(element).text());
    });

    res.send({ judul, paragraf });
  } catch (error) {
    res.status(500).send('Terjadi kesalahan:', error);
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

4.  Buat folder `public` dan buat file `index.html`, `style.css`, dan `script.js` di dalamnya:

`public/index.html`:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Web Scraper</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Web Scraper</h1>
  <form id="urlForm">
    <label for="url">URL:</label>
    <input type="url" id="url" required>
    <button type="submit">Scrape</button>
  </form>
  <div id="results">
    <h2 id="judul"></h2>
    <div id="paragraf"></div>
  </div>
  <script src="script.js"></script>
</body>
</html>
```

`public/style.css`:

css

```css
body {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
}

input, button {
  margin-top: 0.5rem;
}

#results {
  margin-top: 2rem;
}

#paragraf p {
  margin-bottom: 1rem;
}
```

`public/script.js`:

javascript

```javascript
const urlForm = document.getElementById('urlForm');
const urlInput = document.getElementById('url');
const results = document.getElementById('results');
const judul = document.getElementById('judul');
const paragraf = document.getElementById('paragraf');

urlForm.addEventListener('submit', async (e) => {
  e.preventDefault();
  const url = urlInput.value;
  const response = await fetch('/scrape', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ url }),
  });

  if (response.ok) {
    const data =
```

> Tulis lengkap public/script.js

Berikut ini adalah kode lengkap untuk file `public/script.js`:

javascript

```javascript
const urlForm = document.getElementById('urlForm');
const urlInput = document.getElementById('url');
const results = document.getElementById('results');
const judul = document.getElementById('judul');
const paragraf = document.getElementById('paragraf');

urlForm.addEventListener('submit', async (e) => {
  e.preventDefault();
  const url = urlInput.value;
  const response = await fetch('/scrape', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ url }),
  });

  if (response.ok) {
    const data = await response.json();
    displayResults(data);
  } else {
    alert('Terjadi kesalahan saat mengambil data. Silakan coba lagi.');
  }
});

function displayResults(data) {
  judul.innerText = data.judul;
  paragraf.innerHTML = '';

  data.paragraf.forEach((p) => {
    const pElement = document.createElement('p');
    pElement.innerText = p;
    paragraf.appendChild(pElement);
  });

  results.style.display = 'block';
}
```

File `script.js` di atas berfungsi untuk mengirim permintaan ke endpoint `/scrape` yang telah kita buat di file `app.js`. Setelah mendapatkan respons, hasil akan ditampilkan dengan memanipulasi DOM.

Jangan lupa untuk menjalankan aplikasi Anda dengan perintah `node app.js` dan buka browser Anda di alamat `http://localhost:3000`. Sekarang Anda bisa memasukkan URL apa pun, dan aplikasi akan menampilkan judul dan paragraf dari halaman tersebut.

> Buataakn agar tampilanya lebih menarik dan tambahkan tombol copy text

Untuk membuat tampilan yang lebih menarik, Anda bisa memodifikasi `public/style.css` dan menambahkan tombol copy teks ke `public/index.html`. Berikut adalah cara melakukannya:

`public/style.css`:

css

```css
body {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
  background-color: #f5f5f5;
}

h1 {
  color: #2c3e50;
}

input, button {
  margin-top: 0.5rem;
}

form {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

input[type="url"] {
  padding: 0.5rem;
  width: 100%;
  box-sizing: border-box;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  padding: 0.5rem 1rem;
  margin-top: 1rem;
  border: none;
  background-color: #3498db;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  font-size: 16px;
}

button:hover {
  background-color: #2980b9;
}

#results {
  margin-top: 2rem;
  background-color: white;
  padding: 1.5rem;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

#paragraf p {
  margin-bottom: 1rem;
}

#copy-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  border: none;
  background-color: #2ecc71;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  font-size: 16px;
}

#copy-button:hover {
  background-color: #27ae60;
}
```

Tambahkan tombol copy teks ke `public/index.html`:

html

```html
...
<div id="results">
  <h2 id="judul"></h2>
  <div id="paragraf"></div>
  <button id="copy-button">Copy Text</button>
</div>
...
```

Tambahkan fungsi untuk menangani tombol copy teks di `public/script.js`:

javascript

```javascript
...
const copyButton = document.getElementById('copy-button');

copyButton.addEventListener('click', () => {
  const range = document.createRange();
  range.selectNode(paragraf);
  window.getSelection().removeAllRanges();
  window.getSelection().addRange(range);

  try {
    document.execCommand('copy');
    alert('Teks berhasil disalin!');
  } catch (err) {
    alert('Terjadi kesalahan saat menyalin teks. Silakan coba lagi.');
  }

  window.getSelection().removeAllRanges();
});
...
```

Setelah menambahkan perubahan ini, Anda akan melihat tampilan yang lebih menarik dan tombol "Copy Text" yang memungkinkan pengguna menyalin teks paragraf yang ditampilkan. Jangan lupa menjalankan aplikasi Anda dengan perintah `node app.js` dan buka `http://localhost:3000` di browser Anda.

> tambahkan fiture agar bisa menyimpa hasil nya di folder hasil\_data

Untuk menyimpan hasil scraping ke folder `hasil_data`, Anda perlu mengubah kode pada sisi server di `app.js`. Anda juga perlu menginstal paket `fs` dan `path` dari npm untuk membantu menulis file.

1.  Jalankan `npm install fs-extra` untuk menginstal modul `fs-extra` yang akan digunakan untuk membuat file.
2.  Tambahkan kode berikut di bagian atas file `app.js` untuk mengimpor modul yang diperlukan:

javascript

```javascript
const fs = require('fs-extra');
const path = require('path');
```

3.  Buat folder `hasil_data` di direktori utama proyek Anda.
4.  Tambahkan fungsi `saveResults` di file `app.js` untuk menyimpan hasil ke folder `hasil_data`:

javascript

```javascript
async function saveResults(judul, paragraf) {
  const fileName = `${judul.replace(/[/\\?%*:|"<>]/g, '')}.txt`; // Remove any characters that may cause issues with file names
  const filePath = path.join(__dirname, 'hasil_data', fileName);
  const fileContent = `Judul:\n${judul}\n\nParagraf:\n\n${paragraf.join('\n\n')}`;

  await fs.outputFile(filePath, fileContent);
}
```

5.  Panggil fungsi `saveResults` di dalam endpoint `/scrape` setelah mengambil `judul` dan `paragraf`:

javascript

```javascript
app.post('/scrape', async (req, res) => {
  try {
    const url = req.body.url;
    const response = await axios.get(url);
    const $ = cheerio.load(response.data);

    const judul = $('h1').text();
    const paragraf = [];
    $('p').each((i, element) => {
      paragraf.push($(element).text());
    });

    await saveResults(judul, paragraf);

    res.send({ judul, paragraf });
  } catch (error) {
    res.status(500).send('Terjadi kesalahan:', error);
  }
});
```

Setelah menambahkan perubahan ini, hasil scraping akan disimpan di folder `hasil_data` dengan format judul sebagai nama file dan konten file berisi judul dan paragraf. Jangan lupa menjalankan aplikasi Anda dengan perintah `node app.js` dan buka `http://localhost:3000` di browser Anda.

> Tulis lengkap app.js

Berikut adalah kode lengkap untuk file `app.js` dengan fitur menyimpan hasil scraping ke folder `hasil_data`:

javascript

```javascript
const express = require('express');
const axios = require('axios');
const cheerio = require('cheerio');
const fs = require('fs-extra');
const path = require('path');
const app = express();

app.use(express.static('public'));
app.use(express.json());

async function saveResults(judul, paragraf) {
  const fileName = `${judul.replace(/[/\\?%*:|"<>]/g, '')}.txt`; // Remove any characters that may cause issues with file names
  const filePath = path.join(__dirname, 'hasil_data', fileName);
  const fileContent = `Judul:\n${judul}\n\nParagraf:\n\n${paragraf.join('\n\n')}`;

  await fs.outputFile(filePath, fileContent);
}

app.post('/scrape', async (req, res) => {
  try {
    const url = req.body.url;
    const response = await axios.get(url);
    const $ = cheerio.load(response.data);

    const judul = $('h1').text();
    const paragraf = [];
    $('p').each((i, element) => {
      paragraf.push($(element).text());
    });

    await saveResults(judul, paragraf);

    res.send({ judul, paragraf });
  } catch (error) {
    res.status(500).send('Terjadi kesalahan:', error);
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

Dengan kode di atas, Anda telah menambahkan fitur untuk menyimpan hasil scraping ke folder `hasil_data`. Jangan lupa menjalankan aplikasi Anda dengan perintah `node app.js` dan buka `http://localhost:3000` di browser Anda. Setelah Anda melakukan scraping pada suatu URL, hasilnya akan tersimpan dalam folder `hasil_data` dengan format judul sebagai nama file dan konten file berisi judul dan paragraf yang di-scrape.

> Tambahkan tombol simpan

Untuk menambahkan tombol simpan, Anda perlu mengubah `public/index.html` dan menambahkan fungsionalitas di `public/script.js`.

Ubah `public/index.html` dengan menambahkan tombol simpan:

html

```html
...
<div id="results">
  <h2 id="judul"></h2>
  <div id="paragraf"></div>
  <button id="copy-button">Copy Text</button>
  <button id="save-button">Simpan</button>
</div>
...
```

Tambahkan CSS untuk tombol simpan di `public/style.css`:

css

```css
...
#save-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  border: none;
  background-color: #f1c40f;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  font-size: 16px;
}

#save-button:hover {
  background-color: #f39c12;
}
```

Tambahkan fungsi untuk menangani tombol simpan di `public/script.js`:

javascript

```javascript
...
const saveButton = document.getElementById('save-button');

saveButton.addEventListener('click', async () => {
  const fileName = `${judul.innerText.replace(/[/\\?%*:|"<>]/g, '')}.txt`;
  const fileContent = `Judul:\n${judul.innerText}\n\nParagraf:\n\n${paragraf.innerText}`;

  const response = await fetch('/save', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ fileName, fileContent }),
  });

  if (response.ok) {
    alert('Hasil berhasil disimpan!');
  } else {
    alert('Terjadi kesalahan saat menyimpan hasil. Silakan coba lagi.');
  }
});
...
```

Tambahkan endpoint baru `/save` di `app.js`:

javascript

```javascript
...
app.post('/save', async (req, res) => {
  try {
    const { fileName, fileContent } = req.body;
    const filePath = path.join(__dirname, 'hasil_data', fileName);
    await fs.outputFile(filePath, fileContent);
    res.sendStatus(200);
  } catch (error) {
    res.status(500).send('Terjadi kesalahan:', error);
  }
});
...
```

Sekarang Anda memiliki tombol simpan yang akan menyimpan hasil scraping ke folder `hasil_data` saat diklik. Jangan lupa menjalankan aplikasi Anda dengan perintah `node app.js` dan buka `http://localhost:3000` di browser Anda.

> Buatakan agar bisa ambil data dari the newyork time

Untuk mengambil data dari The New York Times, Anda perlu menyesuaikan kode scraping Anda. Artikel berita di situs web The New York Times memiliki struktur HTML yang berbeda dari situs web sebelumnya. Berikut ini cara mengubah kode Anda agar bekerja dengan situs web The New York Times:

1.  Pertama, Anda perlu menambahkan kode berikut ke file `app.js` untuk menggantikan bagian scraping judul dan paragraf:

javascript

```javascript
// Fungsi ini akan menyesuaikan cara scraping berdasarkan domain
async function scrapeData(url) {
  const response = await axios.get(url);
  const $ = cheerio.load(response.data);
  let judul;
  let paragraf = [];

  if (url.includes('nytimes.com')) {
    judul = $('h1[data-testid="headline"]').text();
    $('section[name="articleBody"] p').each((i, element) => {
      paragraf.push($(element).text());
    });
  } else {
    judul = $('h1').text();
    $('p').each((i, element) => {
      paragraf.push($(element).text());
    });
  }

  return { judul, paragraf };
}
```

2.  Ubah endpoint `/scrape` di file `app.js` dengan menggantikan bagian yang mengambil judul dan paragraf dengan pemanggilan fungsi `scrapeData`:

javascript

```javascript
app.post('/scrape', async (req, res) => {
  try {
    const url = req.body.url;
    const { judul, paragraf } = await scrapeData(url);

    await saveResults(judul, paragraf);

    res.send({ judul, paragraf });
  } catch (error) {
    res.status(500).send('Terjadi kesalahan:', error);
  }
});
```

Dengan perubahan ini, aplikasi Anda sekarang harus bekerja dengan situs web The New York Times serta situs web sebelumnya. Jangan lupa menjalankan aplikasi Anda dengan perintah `node app.js` dan buka `http://localhost:3000` di browser Anda.

Catatan: Beberapa situs web mungkin memiliki pembatasan akses terhadap bot scraping atau memerlukan autentikasi. Kode di atas mungkin tidak berfungsi dengan semua artikel di The New York Times karena adanya paywall atau pembatasan akses. Anda harus mematuhi kebijakan penggunaan situs web dan persyaratan layanan saat melakukan scraping.

> tulis lengkap app.js

Berikut adalah kode lengkap untuk file `app.js` dengan fitur untuk mengambil data dari The New York Times:

javascript

```javascript
const express = require('express');
const axios = require('axios');
const cheerio = require('cheerio');
const fs = require('fs-extra');
const path = require('path');
const app = express();

app.use(express.static('public'));
app.use(express.json());

async function saveResults(judul, paragraf) {
  const fileName = `${judul.replace(/[/\\?%*:|"<>]/g, '')}.txt`;
  const filePath = path.join(__dirname, 'hasil_data', fileName);
  const fileContent = `Judul:\n${judul}\n\nParagraf:\n\n${paragraf.join('\n\n')}`;

  await fs.outputFile(filePath, fileContent);
}

async function scrapeData(url) {
  const response = await axios.get(url);
  const $ = cheerio.load(response.data);
  let judul;
  let paragraf = [];

  if (url.includes('nytimes.com')) {
    judul = $('h1[data-testid="headline"]').text();
    $('section[name="articleBody"] p').each((i, element) => {
      paragraf.push($(element).text());
    });
  } else {
    judul = $('h1').text();
    $('p').each((i, element) => {
      paragraf.push($(element).text());
    });
  }

  return { judul, paragraf };
}

app.post('/scrape', async (req, res) => {
  try {
    const url = req.body.url;
    const { judul, paragraf } = await scrapeData(url);

    await saveResults(judul, paragraf);

    res.send({ judul, paragraf });
  } catch (error) {
    res.status(500).send('Terjadi kesalahan:', error);
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

Dengan kode di atas, Anda telah menambahkan fitur untuk mengambil data dari situs web The New York Times. Aplikasi Anda sekarang harus bekerja dengan situs web ini serta situs web sebelumnya. Jangan lupa menjalankan aplikasi Anda dengan perintah `node app.js` dan buka `http://localhost:3000` di browser Anda.

Catatan: Beberapa situs web mungkin memiliki pembatasan akses terhadap bot scraping atau memerlukan autentikasi. Kode di atas mungkin tidak berfungsi dengan semua artikel di The New York Times karena adanya paywall atau pembatasan akses. Anda harus mematuhi kebijakan penggunaan situs web dan persyaratan layanan saat melakukan scraping.