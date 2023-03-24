Model: Default (GPT-3.5)

> gunakan cheerio untuk mengambil data dari https://esai.nauv.al/tidak-semua-mampu-kuliah Ambil text dari <h1> post-title ambil text nya aj lalu <h2> ambil text nya aja, data-title abile tanggal nya aja dan ambil <p>

2 / 2

Untuk menggunakan cheerio dan mengambil data dari [https://esai.nauv.al/tidak-semua-mampu-kuliah](https://esai.nauv.al/tidak-semua-mampu-kuliah), Anda perlu mengikuti beberapa langkah berikut:

1.  Install package cheerio melalui npm dengan menjalankan perintah berikut di terminal:
    
    `npm install cheerio`
    
2.  Buat file JavaScript dan impor modul cheerio.
    
    javascript
    
    ```javascript
    const cheerio = require('cheerio');
    const axios = require('axios');
    ```
    
3.  Gunakan axios untuk mengambil HTML dari halaman web yang ingin Anda ambil datanya.
    
    javascript
    
    ```javascript
    const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';
    axios.get(url).then((response) => {
      const html = response.data;
      const $ = cheerio.load(html);
      // Lakukan operasi Cheerio di sini
    });
    ```
    
4.  Setelah mengambil HTML, gunakan operasi Cheerio untuk mengambil data yang diinginkan dari elemen HTML.
    
    javascript
    
    ```javascript
    const postTitle = $('h1.post-title').text().trim();
    const subTitle = $('h2').text().trim();
    const date = $('div[data-title]').attr('data-title');
    const paragraph = $('p').text().trim();
    ```
    
5.  Tampilkan data yang telah diambil ke konsol.
    
    lua
    
    ```lua
    console.log(postTitle);
    console.log(subTitle);
    console.log(date);
    console.log(paragraph);
    ```
    

Berikut adalah contoh kode lengkap yang dapat Anda gunakan:

javascript

```javascript
const cheerio = require('cheerio');
const axios = require('axios');

const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';

axios.get(url).then((response) => {
  const html = response.data;
  const $ = cheerio.load(html);

  const postTitle = $('h1.post-title').text().trim();
  const subTitle = $('h2').text().trim();
  const date = $('div[data-title]').attr('data-title');
  const paragraph = $('p').text().trim();

  console.log(postTitle);
  console.log(subTitle);
  console.log(date);
  console.log(paragraph);
}).catch((error) => {
  console.log(error);
});
```

Semoga membantu!

> simpan file nya di folde hasil berformat .txt

5 / 5

Untuk menyimpan file hasil dalam format .txt, Anda bisa menggunakan modul bawaan Node.js yaitu `fs` (file system). Berikut adalah kode lengkapnya:

javascript

```javascript
const cheerio = require('cheerio');
const axios = require('axios');
const fs = require('fs');

const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';

axios.get(url).then((response) => {
  const html = response.data;
  const $ = cheerio.load(html);

  const postTitle = $('h1.post-title').text().trim();
  const subTitle = $('h2').text().trim();
  const date = $('div[data-title]').attr('data-title');
  const paragraph = $('p').text().trim();

  // Simpan hasil ke dalam file .txt
  const fileName = 'hasil.txt';
  const fileContent = `Judul: ${postTitle}\nSub Judul: ${subTitle}\nTanggal: ${date}\nIsi: ${paragraph}`;
  fs.writeFile(fileName, fileContent, (error) => {
    if (error) {
      console.log(error);
    } else {
      console.log(`File ${fileName} berhasil disimpan.`);
    }
  });
}).catch((error) => {
  console.log(error);
});
```

Dalam kode di atas, kita menggunakan fungsi `fs.writeFile` untuk menyimpan hasil ke dalam file. Fungsi ini menerima tiga parameter: nama file, isi file, dan fungsi callback yang akan dipanggil setelah file berhasil disimpan atau terjadi kesalahan dalam menyimpan file.

Semoga membantu!

> simpan sesuai nama judul text nya

Untuk menyimpan file dengan nama sesuai judul text, kita dapat mengambil nilai judul text dan gunakan sebagai nama file. Namun, kita perlu memperhatikan beberapa hal seperti menghapus karakter yang tidak diperbolehkan dalam nama file dan menambahkan ekstensi `.txt`.

Berikut adalah kode lengkapnya:

javascript

```javascript
const cheerio = require('cheerio');
const axios = require('axios');
const fs = require('fs');

const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';

axios.get(url).then((response) => {
  const html = response.data;
  const $ = cheerio.load(html);

  const postTitle = $('h1.post-title').text().trim();
  const subTitle = $('h2').text().trim();
  const date = $('div[data-title]').attr('data-title');
  const paragraph = $('p').text().trim();

  // Buat nama file dari judul text
  const fileName = `${postTitle.replace(/[^\w\s]/gi, '')}.txt`;

  // Simpan hasil ke dalam file .txt
  const fileContent = `Judul: ${postTitle}\nSub Judul: ${subTitle}\nTanggal: ${date}\nIsi: ${paragraph}`;
  fs.writeFile(fileName, fileContent, (error) => {
    if (error) {
      console.log(error);
    } else {
      console.log(`File ${fileName} berhasil disimpan.`);
    }
  });
}).catch((error) => {
  console.log(error);
});
```

Dalam kode di atas, kita menggunakan fungsi `replace` untuk menghapus karakter yang tidak diperbolehkan dalam nama file dengan menggantinya dengan spasi. Kemudian kita menambahkan ekstensi `.txt` pada nama file.

Semoga membantu!

> Dan rapikan tulisanya juga agar setiap paragraf

Untuk merapikan tulisan agar setiap paragraf dipisahkan dengan newline (`\n`), kita bisa menggunakan fungsi `replace` pada variabel `paragraph`.

Berikut adalah kode lengkapnya:

javascript

```javascript
const cheerio = require('cheerio');
const axios = require('axios');
const fs = require('fs');

const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';

axios.get(url).then((response) => {
  const html = response.data;
  const $ = cheerio.load(html);

  const postTitle = $('h1.post-title').text().trim();
  const subTitle = $('h2').text().trim();
  const date = $('div[data-title]').attr('data-title');
  const paragraph = $('p').text().trim().replace(/\n/g, '').replace(/\. /g, '.\n\n');

  // Buat nama file dari judul text
  const fileName = `${postTitle.replace(/[^\w\s]/gi, '')}.txt`;

  // Simpan hasil ke dalam file .txt
  const fileContent = `Judul: ${postTitle}\nSub Judul: ${subTitle}\nTanggal: ${date}\nIsi: ${paragraph}`;
  fs.writeFile(fileName, fileContent, (error) => {
    if (error) {
      console.log(error);
    } else {
      console.log(`File ${fileName} berhasil disimpan.`);
    }
  });
}).catch((error) => {
  console.log(error);
});
```

Dalam kode di atas, kita menggunakan dua fungsi `replace`. Pertama, kita menghapus newline (`\n`) yang sudah ada pada variabel `paragraph`. Kedua, kita mengganti setiap `.` yang diikuti spasi dengan newline (`\n\n`) agar setiap paragraf dipisahkan dengan newline.

Semoga membantu!

> ambil judul nya dari h1

Untuk mengambil judul dari elemen `h1`, kita dapat menggunakan fungsi `text` pada elemen tersebut. Berikut adalah kode lengkapnya:

javascript

```javascript
const cheerio = require('cheerio');
const axios = require('axios');
const fs = require('fs');

const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';

axios.get(url).then((response) => {
  const html = response.data;
  const $ = cheerio.load(html);

  const postTitle = $('h1').text().trim();
  const subTitle = $('h2').text().trim();
  const date = $('div[data-title]').attr('data-title');
  const paragraph = $('p').text().trim().replace(/\n/g, '').replace(/\. /g, '.\n\n');

  // Buat nama file dari judul
  const fileName = `${postTitle.replace(/[^\w\s]/gi, '')}.txt`;

  // Simpan hasil ke dalam file .txt
  const fileContent = `Judul: ${postTitle}\nSub Judul: ${subTitle}\nTanggal: ${date}\nIsi: ${paragraph}`;
  fs.writeFile(fileName, fileContent, (error) => {
    if (error) {
      console.log(error);
    } else {
      console.log(`File ${fileName} berhasil disimpan.`);
    }
  });
}).catch((error) => {
  console.log(error);
});
```

Dalam kode di atas, kita menggunakan `h1` sebagai parameter pada fungsi `$` untuk mendapatkan elemen `h1`. Kemudian, kita menggunakan fungsi `text` pada elemen tersebut untuk mendapatkan teks di dalamnya.

Semoga membantu!

> h1 nya cuma ambil text nya aja

Untuk mengambil hanya teks dari elemen `h1`, kita dapat menggunakan fungsi `text` pada elemen tersebut. Berikut adalah kode lengkapnya:

javascript

```javascript
const cheerio = require('cheerio');
const axios = require('axios');
const fs = require('fs');

const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';

axios.get(url).then((response) => {
  const html = response.data;
  const $ = cheerio.load(html);

  const postTitle = $('h1').text().trim();
  const subTitle = $('h2').text().trim();
  const date = $('div[data-title]').attr('data-title');
  const paragraph = $('p').text().trim().replace(/\n/g, '').replace(/\. /g, '.\n\n');

  // Buat nama file dari judul
  const fileName = `${postTitle.replace(/[^\w\s]/gi, '')}.txt`;

  // Simpan hasil ke dalam file .txt
  const fileContent = `Judul: ${postTitle}\nSub Judul: ${subTitle}\nTanggal: ${date}\nIsi: ${paragraph}`;
  fs.writeFile(fileName, fileContent, (error) => {
    if (error) {
      console.log(error);
    } else {
      console.log(`File ${fileName} berhasil disimpan.`);
    }
  });
}).catch((error) => {
  console.log(error);
});
```

Dalam kode di atas, kita hanya menggunakan fungsi `text` pada elemen `h1` untuk mendapatkan teks di dalamnya.

Semoga membantu!

> Cumma ambil isi text nya aja di dalam <h1></h1>

Untuk mengambil isi teks yang berada di dalam elemen `h1`, kita dapat menggunakan fungsi `text` pada elemen tersebut. Berikut adalah kode lengkapnya:

javascript

```javascript
const cheerio = require('cheerio');
const axios = require('axios');
const fs = require('fs');

const url = 'https://esai.nauv.al/tidak-semua-mampu-kuliah';

axios.get(url).then((response) => {
  const html = response.data;
  const $ = cheerio.load(html);

  const postTitle = $('h1').contents().filter(function() {
    return this.nodeType === 3;
  }).text().trim();
  const subTitle = $('h2').text().trim();
  const date = $('div[data-title]').attr('data-title');
  const paragraph = $('p').text().trim().replace(/\n/g, '').replace(/\. /g, '.\n\n');

  // Buat nama file dari judul
  const fileName = `${postTitle.replace(/[^\w\s]/gi, '')}.txt`;

  // Simpan hasil ke dalam file .txt
  const fileContent = `Judul: ${postTitle}\nSub Judul: ${subTitle}\nTanggal: ${date}\nIsi: ${paragraph}`;
  fs.writeFile(fileName, fileContent, (error) => {
    if (error) {
      console.log(error);
    } else {
      console.log(`File ${fileName} berhasil disimpan.`);
    }
  });
}).catch((error) => {
  console.log(error);
});
```

Dalam kode di atas, kita menggunakan fungsi `contents` pada elemen `h1` untuk mengambil semua elemen yang berada di dalamnya, termasuk teks dan elemen lainnya. Kemudian, kita menggunakan fungsi `filter` untuk memfilter hanya teks yang ada di dalamnya dengan mengecek tipe nodenya. Setelah itu, kita menggunakan fungsi `text` pada hasil filter tersebut untuk mendapatkan teks yang berada di dalam elemen `h1`.

Semoga membantu!