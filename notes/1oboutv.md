Model: GPT-4

> Buatakan tasbih dengan nodejs,html,css,javascript dan simpan file terpisah localstorage nya

2 / 2

Untuk membuat aplikasi tasbih dengan Node.js, HTML, CSS, dan JavaScript serta menyimpan data di LocalStorage, ikuti langkah-langkah berikut:

1.  Buat folder untuk proyek Anda, misalnya "tasbih-app".
    
2.  Buka terminal atau command prompt, masuk ke folder proyek yang baru dibuat, dan jalankan perintah berikut untuk menginisialisasi proyek Node.js:
    

csharp

```csharp
npm init -y
```

3.  Buat file HTML, CSS, dan JavaScript di folder proyek:

*   index.html
*   style.css
*   script.js

4.  Edit file "index.html" dan tambahkan kode berikut:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tasbih App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Tasbih</h1>
        <p id="counter">0</p>
        <button id="increment">Tambah</button>
        <button id="reset">Reset</button>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

5.  Edit file "style.css" dan tambahkan kode berikut:

css

```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #f0f0f0;
}

.container {
    display: inline-block;
    background-color: white;
    padding: 2rem;
    border-radius: 1rem;
    box-shadow: 0 0 1rem rgba(0, 0, 0, 0.1);
}

h1 {
    margin-bottom: 1rem;
}

#counter {
    font-size: 3rem;
    margin-bottom: 1rem;
}

button {
    font-size: 1.2rem;
    padding: 0.5rem 1rem;
    margin: 0.5rem;
    cursor: pointer;
}

button:focus {
    outline: none;
}
```

6.  Edit file "script.js" dan tambahkan kode berikut:

javascript

```javascript
const counter = document.getElementById("counter");
const increment = document.getElementById("increment");
const reset = document.getElementById("reset");

// Mengambil data dari LocalStorage
let count = localStorage.getItem("count") || 0;
counter.textContent = count;

increment.addEventListener("click", () => {
    count++;
    counter.textContent = count;
    localStorage.setItem("count", count);
});

reset.addEventListener("click", () => {
    count = 0;
    counter.textContent = count;
    localStorage.setItem("count", count);
});
```

7.  Buka file "index.html" di browser untuk melihat aplikasi tasbih yang dibuat. Setiap kali Anda menekan tombol "Tambah", angka akan bertambah, dan jika Anda menekan tombol "Reset", angka akan kembali ke 0. Data akan disimpan di LocalStorage sehingga aplikasi akan mengingat angka terakhir bahkan jika Anda menutup browser dan membukanya kembali.

> cara menjalankan kode di atas

7 / 7

Untuk menjalankan kode di atas, Anda tidak perlu menggunakan Node.js karena kode tersebut hanya melibatkan HTML, CSS, dan JavaScript tanpa server-side code. Ikuti langkah-langkah berikut untuk menjalankan aplikasi:

1.  Pastikan Anda sudah memiliki file `index.html`, `style.css`, dan \`script.js

> cara agar bisa berjalan di nodejs

Untuk menjalankan aplikasi ini menggunakan Node.js, Anda harus membuat server yang menangani permintaan HTTP dan menyediakan file-file tersebut. Salah satu cara untuk melakukannya adalah dengan menggunakan framework Express.js.

1.  Pertama, install Express.js dengan menjalankan perintah berikut di terminal atau command prompt di dalam folder proyek Anda:

`npm install express`

2.  Buat file baru bernama `server.js` di folder proyek dan tambahkan kode berikut:

javascript

```javascript
const express = require("express");
const path = require("path");
const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.static(path.join(__dirname, "./")));

app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname, "./index.html"));
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

Kode di atas mengimpor modul `express` dan `path`, lalu membuat server yang menyajikan file statis (HTML, CSS, dan JavaScript) dari folder proyek saat ini. Server akan mendengarkan permintaan pada port 3000 atau port yang ditentukan oleh lingkungan.

3.  Buka `package.json` yang ada di folder proyek dan ubah bagian `"scripts"` menjadi:

json

```json
"scripts": {
  "start": "node server.js"
},
```

4.  Jalankan server dengan menjalankan perintah berikut di terminal atau command prompt:

sql

```sql
npm start
```

5.  Buka browser dan buka `http://localhost:3000`. Anda sekarang dapat menggunakan aplikasi tasbih yang berjalan pada server Node.js menggunakan Express.js.

> dari file index.html,style.css,script.js ada di dalam folder public tolong perbaiki server.js nya

Jika file `index.html`, `style.css`, dan `script.js` berada di dalam folder `public`, Anda perlu mengubah kode `server.js` untuk menyajikan file statis dari folder tersebut. Ikuti langkah-langkah berikut:

1.  Pastikan folder `public` berada di folder proyek yang sama dengan `server.js`.
    
2.  Ubah kode `server.js` menjadi:
    

javascript

```javascript
const express = require("express");
const path = require("path");
const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.static(path.join(__dirname, "public")));

app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname, "public", "index.html"));
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

Perubahan yang dibuat adalah menggantikan `app.use(express.static(path.join(__dirname, "./")));` dengan `app.use(express.static(path.join(__dirname, "public")));` dan menggantikan `res.sendFile(path.join(__dirname, "./index.html"));` dengan `res.sendFile(path.join(__dirname, "public", "index.html"));`. Hal ini memastikan server Express.js menyajikan file statis dari folder `public`.

3.  Jalankan server dengan menjalankan perintah berikut di terminal atau command prompt:

sql

```sql
npm start
```

4.  Buka browser dan buka `http://localhost:3000`. Aplikasi tasbih sekarang berjalan pada server Node.js menggunakan Express.js dengan file statis yang disajikan dari folder `public`.

> Tambahakan fiture tambah dan kurang dan simpan hisotry berdasarkan tanggal dan tampilkan hide nad show

2 / 2

Untuk menambahkan fitur pengurangan, penyimpanan riwayat berdasarkan tanggal, dan tombol tampilkan/sembunyikan, ikuti langkah-langkah berikut:

1.  Edit file `index.html` dan tambahkan tombol pengurangan serta kontainer riwayat:

html

```html
<!-- ... -->
<button id="increment">Tambah</button>
<button id="decrement">Kurangi</button> <!-- Tambahkan tombol pengurangan -->
<button id="reset">Reset</button>
<button id="toggleHistory">Tampilkan Riwayat</button> <!-- Tambahkan tombol tampilkan/sembunyikan riwayat -->

<div id="historyContainer" style="display:none;"> <!-- Tambahkan kontainer riwayat -->
  <h2>Riwayat</h2>
  <ul id="historyList"></ul>
</div>
<!-- ... -->
```

2.  Edit file `script.js` dan tambahkan fungsi pengurangan, penyimpanan riwayat, dan tampilkan/sembunyikan riwayat:

javascript

```javascript
// ... (kode sebelumnya)

const decrement = document.getElementById("decrement"); // Tambahkan variabel tombol pengurangan
const toggleHistory = document.getElementById("toggleHistory");
const historyContainer = document.getElementById("historyContainer");
const historyList = document.getElementById("historyList");

let history = JSON.parse(localStorage.getItem("history")) || [];

// Fungsi untuk menyimpan riwayat
function saveHistory(date, action) {
  history.push({ date, action });
  localStorage.setItem("history", JSON.stringify(history));
  displayHistory();
}

// Fungsi untuk menampilkan riwayat
function displayHistory() {
  historyList.innerHTML = "";
  history.forEach((entry) => {
    const listItem = document.createElement("li");
    listItem.textContent = `${entry.date} - ${entry.action}`;
    historyList.appendChild(listItem);
  });
}

decrement.addEventListener("click", () => { // Event listener untuk tombol pengurangan
  count--;
  counter.textContent = count;
  localStorage.setItem("count", count);
  saveHistory(new Date().toLocaleString(), "Kurangi"); // Simpan riwayat pengurangan
});

// ... (kode increment dan reset)

increment.addEventListener("click", () => {
  // ... (kode increment)
  saveHistory(new Date().toLocaleString(), "Tambah"); // Simpan riwayat penambahan
});

reset.addEventListener("click", () => {
  // ... (kode reset)
  saveHistory(new Date().toLocaleString(), "Reset"); // Simpan riwayat reset
});

toggleHistory.addEventListener("click", () => {
  if (historyContainer.style.display === "none") {
    historyContainer.style.display = "block";
    toggleHistory.textContent = "Sembunyikan Riwayat";
    displayHistory();
  } else {
    historyContainer.style.display = "none";
    toggleHistory.textContent = "Tampilkan Riwayat";
  }
});

// Tampilkan riwayat saat pertama kali membuka aplikasi
displayHistory();
```

Sekarang Anda memiliki fitur pengurangan, penyimpanan riwayat berdasarkan tanggal, dan tombol untuk menampilkan/sembunyikan riwayat. Setiap kali Anda menambahkan atau mengurangi angka, riwayat akan disimpan di LocalStorage dan akan ditampilkan ketika Anda mengklik tombol "Tampilkan Riwayat".

> maksut saya riwaya dari nilai tasbih tersebut bukan tindakanya yang di simpan

Untuk menyimpan riwayat nilai tasbih, Anda hanya perlu mengganti bagian yang menyimpan riwayat tindakan dengan riwayat nilai tasbih. Ikuti langkah-langkah berikut:

1.  Edit file `script.js` dan ubah fungsi `saveHistory` serta bagian yang menggunakan fungsi tersebut:

javascript

```javascript
// ...

// Fungsi untuk menyimpan riwayat
function saveHistory(date, value) {
  history.push({ date, value });
  localStorage.setItem("history", JSON.stringify(history));
  displayHistory();
}

// Fungsi untuk menampilkan riwayat (tanpa perubahan)

// ...

decrement.addEventListener("click", () => {
  // ...
  saveHistory(new Date().toLocaleString(), count); // Simpan riwayat nilai tasbih saat ini
});

// ...

increment.addEventListener("click", () => {
  // ...
  saveHistory(new Date().toLocaleString(), count); // Simpan riwayat nilai tasbih saat ini
});

reset.addEventListener("click", () => {
  // ...
  saveHistory(new Date().toLocaleString(), count); // Simpan riwayat nilai tasbih saat ini
});

// ...
```

2.  Ubah bagian dalam fungsi `displayHistory` yang menampilkan riwayat:

javascript

```javascript
// Fungsi untuk menampilkan riwayat
function displayHistory() {
  historyList.innerHTML = "";
  history.forEach((entry) => {
    const listItem = document.createElement("li");
    listItem.textContent = `${entry.date} - Nilai: ${entry.value}`; // Tampilkan nilai tasbih
    historyList.appendChild(listItem);
  });
}
```

Sekarang, aplikasi akan menyimpan riwayat nilai tasbih setiap kali Anda menambahkan, mengurangi, atau mereset nilai tersebut. Riwayat ini akan ditampilkan ketika Anda mengklik tombol "Tampilkan Riwayat".

> Buatkan fiture hapus riwayat

Untuk menambahkan fitur menghapus riwayat, ikuti langkah-langkah berikut:

1.  Edit file `index.html` dan tambahkan tombol hapus riwayat:

html

```html
<!-- ... -->
<button id="deleteHistory">Hapus Riwayat</button> <!-- Tambahkan tombol hapus riwayat -->
<!-- ... -->
```

2.  Edit file `script.js` dan tambahkan event listener untuk tombol hapus riwayat:

javascript

```javascript
// ... (kode sebelumnya)

const deleteHistory = document.getElementById("deleteHistory");

// ... (kode sebelumnya)

deleteHistory.addEventListener("click", () => {
  history = []; // Kosongkan array riwayat
  localStorage.removeItem("history"); // Hapus riwayat dari localStorage
  displayHistory(); // Perbarui tampilan riwayat
});
```

Sekarang, ketika Anda mengklik tombol "Hapus Riwayat", riwayat yang ada akan dihapus, dan tampilan riwayat akan diperbarui.

> buatakan agar tampilanya responsif dan lebih menarik di css nya

Untuk membuat tampilan responsif dan lebih menarik, Anda dapat mengupdate `style.css` dengan gaya berikut:

css

```css
* {
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  text-align: center;
  background-color: #f0f0f0;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.container {
  display: inline-flex;
  flex-direction: column;
  background-color: white;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
}

h1 {
  margin-bottom: 1rem;
  color: #007bff;
}

#counter {
  font-size: 3rem;
  margin-bottom: 1rem;
  color: #007bff;
}

button {
  font-size: 1.2rem;
  padding: 0.5rem 1rem;
  margin: 0.5rem;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 0.25rem;
  transition: background-color 0.2s;
}

button:focus {
  outline: none;
}

button:hover {
  background-color: #0056b3;
}

#historyContainer {
  margin-top: 2rem;
  border-top: 1px solid #007bff;
  padding-top: 2rem;
}

#historyList {
  list-style-type: none;
  padding: 0;
  text-align: left;
}

@media screen and (max-width: 480px) {
  .container {
    padding: 1rem;
  }

  h1 {
    font-size: 1.5rem;
  }

  #counter {
    font-size: 2rem;
  }

  button {
    font-size: 1rem;
    padding: 0.25rem 0.5rem;
  }
}
```

Perubahan ini meliputi:

*   Gaya umum untuk membuat tampilan yang lebih menarik, seperti warna dan efek transisi.
*   Menggunakan flexbox untuk menyusun elemen dalam `container`.
*   Menambahkan media query untuk layar kecil (lebar maksimum 480px), sehingga tampilan responsif di perangkat dengan layar yang berbeda.

Sekarang tampilan aplikasi tasbih Anda akan lebih menarik dan responsif di berbagai ukuran layar.

> Buatkan agar angka nya lebih besar dan menarik di lihat

Untuk membuat angka lebih besar dan menarik, Anda bisa mengubah gaya pada elemen `#counter` di file `style.css`. Berikut adalah contoh gaya yang dapat Anda gunakan:

css

```css
#counter {
  font-size: 6rem;
  margin-bottom: 1rem;
  color: #007bff;
  font-weight: bold;
  text-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
}
```

Perubahan ini meliputi:

*   Meningkatkan ukuran font menjadi `6rem`.
*   Menambahkan `font-weight: bold` agar angka terlihat lebih tebal.
*   Menambahkan `text-shadow` untuk menambahkan sedikit bayangan pada angka, membuatnya terlihat lebih menarik.

Anda bisa menyesuaikan ukuran font dan efek lainnya sesuai keinginan Anda.

> buatakan file baru javascript nya agar bisa memberikan suara ketika angkan ada

2 / 2

Untuk memberikan suara ketika angka berubah, Anda perlu membuat file JavaScript baru yang berisi kode untuk memainkan suara. Ikuti langkah-langkah

> buatakan agar bisa pwa

Untuk mengubah aplikasi tasbih ini menjadi Progressive Web App (PWA), ikuti langkah-langkah berikut:

1.  Buat file `manifest.json` di folder `public` dengan konten berikut:

json

```json
{
  "name": "Tasbih App",
  "short_name": "Tasbih",
  "description": "A simple tasbih counter app",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#f0f0f0",
  "theme_color": "#007bff",
  "icons": [
    {
      "src": "icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

Pastikan Anda menggantikan `icon-192x192.png` dan `icon-512x512.png` dengan ikon yang sesuai untuk aplikasi Anda.

2.  Tambahkan link ke `manifest.json` di file `index.html`:

html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- ... -->
    <link rel="manifest" href="manifest.json" />
    <!-- ... -->
  </head>
  <!-- ... -->
</html>
```

3.  Buat file `sw.js` (service worker) di folder `public` dengan konten berikut:

javascript

```javascript
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("tasbih-cache").then((cache) => {
      return cache.addAll(["index.html", "style.css", "script.js"]);
    })
  );
});

self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

4.  Tambahkan kode berikut ke file `script.js` untuk mendaftarkan service worker:

javascript

```javascript
if ("serviceWorker" in navigator) {
  window.addEventListener("load", () => {
    navigator.serviceWorker.register("sw.js").then(
      (registration) => {
        console.log("Service Worker registered: ", registration);
      },
      (error) => {
        console.log("Service Worker registration failed: ", error);
      }
    );
  });
}
```

Sekarang aplikasi tasbih Anda telah menjadi PWA. Aplikasi ini akan dapat diakses secara offline dan dapat diinstal pada perangkat yang mendukung PWA. Jangan lupa untuk mengganti ikon dengan ikon yang sesuai untuk aplikasi Anda.

> buatkan tombol dan kuranginya lebih besar dan angkan nya juga lebih besar di mobile

3 / 3

Untuk membuat tombol dan angka lebih besar pada perangkat mobile, Anda dapat menambahkan gaya CSS khusus dalam media query yang sudah ada di file `style.css`. Ikuti langkah-langkah berikut:

1.  Tambahkan gaya berikut dalam media query `@media screen and (max-width: 480px)` yang ada di `style.css`:

css

```css
/* ... (kode sebelumnya) */

@media screen and (max-width: 480px) {
  /* ... (kode sebelumnya) */

  #counter {
    font-size: 4rem;
  }

  button {
    font-size: 1.5rem;
    padding: 0.75rem 1.5rem;
  }
}
```

Perubahan ini akan membuat angka dan tombol lebih besar pada perangkat dengan lebar layar maksimum 480px. Anda dapat menyesuaikan nilai `font-size` dan `padding` sesuai keinginan Anda.

Sekarang, angka dan tombol akan menjadi lebih besar di perangkat mobile, sehingga lebih mudah dilihat dan digunakan.

> Buatkan angka nya di buat kotak besar yang menunjukan nilai dan font nya juga besar walaupuun di mobile

Untuk membuat kotak besar yang menampilkan angka dengan font yang besar di perangkat mobile, Anda bisa menggabungkan gaya berikut pada file `style.css`:

css

```css
#counter {
  font-size: 3rem;
  margin-bottom: 1rem;
  color: #007bff;
  font-weight: bold;
  text-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
  border: 2px solid #007bff;
  border-radius: 1rem;
  padding: 1rem 2rem;
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.1);
}

@media screen and (max-width: 480px) {
  /* ... (kode sebelumnya) */

  #counter {
    font-size: 4rem;
    padding: 1.5rem 3rem;
  }
}
```

Perubahan ini meliputi:

*   Membuat latar belakang `#counter` berwarna putih.
*   Menambahkan border dengan warna yang sama dengan angka.
*   Menambahkan `border-radius` untuk membuat sudut kotak melengkung.
*   Menambahkan `padding` untuk memberi ruang di dalam kotak.
*   Menambahkan `box-shadow` untuk memberikan efek bayangan pada kotak.
*   Menyesuaikan ukuran font dan padding pada perangkat mobile dalam media query.

Sekarang, angka akan ditampilkan dalam kotak besar dengan font yang besar, baik di perangkat desktop maupun mobile. Anda bisa menyesuaikan ukuran font dan padding sesuai keinginan Anda.

> Buatakan fiture on of suara dengan icon of dan on

Untuk menambahkan fitur mengaktifkan dan menonaktifkan suara dengan ikon on dan off, ikuti langkah-langkah berikut:

1.  Tambahkan ikon untuk suara on dan off di file `index.html`. Anda bisa menggunakan ikon dari font-awesome ([https://fontawesome.com/](https://fontawesome.com/)) atau ikon lain yang Anda inginkan. Pastikan Anda menambahkan `<link>` yang sesuai jika Anda menggunakan font-awesome:

html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- ... -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css" />
    <!-- ... -->
  </head>
  <body>
    <!-- ... -->
    <div id="soundControl">
      <i id="soundOn" class="fas fa-volume-up"></i>
      <i id="soundOff" class="fas fa-volume-mute" style="display: none;"></i>
    </div>
    <!-- ... -->
  </body>
</html>
```

2.  Tambahkan gaya untuk ikon suara di file `style.css`:

css

```css
#soundControl {
  font-size: 1.5rem;
  cursor: pointer;
  display: inline-block;
  margin-top: 1rem;
  user-select: none;
}

#soundControl i {
  vertical-align: middle;
}

@media screen and (max-width: 480px) {
  #soundControl {
    font-size: 2rem;
  }
}
```

3.  Tambahkan kode berikut ke file `script.js` untuk mengaktifkan dan menonaktifkan suara:

javascript

```javascript
const soundOn = document.getElementById("soundOn");
const soundOff = document.getElementById("soundOff");
let soundEnabled = true;

soundOn.addEventListener("click", () => {
  soundEnabled = false;
  soundOn.style.display = "none";
  soundOff.style.display = "inline";
});

soundOff.addEventListener("click", () => {
  soundEnabled = true;
  soundOff.style.display = "none";
  soundOn.style.display = "inline";
});

function playSound() {
  if (!soundEnabled) return;
  const audio = new Audio("click.mp3");
  audio.play();
}

increment.addEventListener("click", () => {
  // ...
  playSound();
});

decrement.addEventListener("click", () => {
  // ...
  playSound();
});

reset.addEventListener("click", () => {
  // ...
  playSound();
});
```

Pastikan Anda memiliki file suara `click.mp3` atau ganti dengan nama file suara yang Anda inginkan.

Sekarang, aplikasi Anda akan memiliki fitur mengaktifkan dan menonaktifkan suara dengan ikon on dan off. Anda bisa mengklik ikon untuk mengganti antara suara aktif dan non-aktif.

> Buatakan agar hasil riwayatn ya menarik tampilanya

Untuk membuat tampilan riwayat lebih menarik, Anda dapat menambahkan beberapa gaya CSS pada elemen yang relevan. Berikut ini adalah contoh gaya yang dapat Anda gunakan di file `style.css`:

css

```css
#historyContainer {
  margin-top: 2rem;
  padding-top: 2rem;
  text-align: left;
}

#historyTitle {
  font-size: 1.5rem;
  color: #007bff;
  font-weight: bold;
  margin-bottom: 1rem;
  border-bottom: 2px solid #007bff;
  padding-bottom: 0.5rem;
}

#historyList {
  list-style-type: none;
  padding: 0;
}

#historyList li {
  background-color: #f0f0f0;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 0.5rem;
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

#historyList li span {
  font-weight: bold;
  color: #007bff;
}

#historyList li .date {
  font-size: 0.9rem;
  color: #777;
}

@media screen and (max-width: 480px) {
  #historyTitle {
    font-size: 1.2rem;
  }

  #historyList li {
    padding: 0.75rem;
    margin-bottom: 0.75rem;
  }
}
```

Perubahan ini meliputi:

*   Menambahkan gaya pada `#historyTitle` untuk membuat judul riwayat lebih menarik.
*   Membuat latar belakang item riwayat berwarna abu-abu terang.
*   Menambahkan `padding`, `border-radius`, dan `box-shadow` pada item riwayat.
*   Mengatur teks item riwayat menggunakan `display: flex` untuk menyusun tanggal dan angka.
*   Menyesuaikan ukuran font dan `padding` pada perangkat mobile dalam media query.

Sekarang, tampilan riwayat akan lebih menarik dan mudah dibaca. Anda bisa menyesuaikan gaya ini sesuai keinginan Anda.