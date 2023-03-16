Model: GPT-4

> Buatkan script (html,css,javasript) untuk rekomendasi 10 film setiap hari dengan api themoviedb dan tampilan dari taildwind juga untuk memper indah dan buat per file nya juga secara detail dan pengistalan juga Jelaskan langkah demi langkah

Untuk membuat halaman web dengan rekomendasi 10 film setiap hari menggunakan API The Movie Database (TMDB) dan tampilan Tailwind CSS, ikuti langkah-langkah berikut:

1.  Buat akun di [https://www.themoviedb.org/](https://www.themoviedb.org/) dan dapatkan API key.
    
2.  Buat file HTML, CSS, dan JavaScript:
    
    *   index.html
    *   styles.css
    *   script.js
3.  Masukkan kode berikut ke dalam file yang telah dibuat:
    

index.html:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rekomendasi Film</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.16/dist/tailwind.min.css" rel="stylesheet">
  <link rel="stylesheet" href="styles.css">
</head>
<body class="bg-gray-100">
  <div class="container mx-auto px-4">
    <h1 class="text-4xl font-bold mt-8 mb-4">Rekomendasi Film Hari Ini</h1>
    <div id="movie-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
      <!-- Film akan ditampilkan di sini -->
    </div>
  </div>
  <script src="script.js"></script>
</body>
</html>
```

styles.css:

css

```css
/* Tambahkan gaya khusus jika diperlukan */
```

script.js:

javascript

```javascript
const apiKey = 'API_KEY_ANDA'; // Ganti dengan API key Anda
const movieList = document.getElementById('movie-list');

async function fetchMovies() {
  const response = await fetch(
    `https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=${apiKey}`
  );
  const data = await response.json();
  const movies = data.results.slice(0, 10);
  displayMovies(movies);
}

function displayMovies(movies) {
  const movieCards = movies
    .map((movie) => {
      return `
        <div class="bg-white rounded overflow-hidden shadow-lg">
          <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
          <div class="p-4">
            <h3 class="font-bold text-xl mb-2">${movie.title}</h3>
            <p class="text-gray-700 text-base">Tanggal Rilis: ${movie.release_date}</p>
            <p class="text-gray-700 text-base">Rating: ${movie.vote_average}</p>
          </div>
        </div>`;
    })
    .join('');
  movieList.innerHTML = movieCards;
}

fetchMovies();
```

4.  Ganti `API_KEY_ANDA` dalam script.js dengan API key yang Anda peroleh dari TMDB.
    
5.  Buka file `index.html` di browser Anda, dan Anda akan melihat 10 film yang direkomendasikan setiap hari.
    

> tambahkan fiture detail biar bisam menampilakn trailer nya juga dan detail movie nya secara responsif

Untuk menambahkan fitur detail film dan menampilkan trailernya, Anda perlu memodifikasi file HTML, CSS, dan JavaScript yang telah ada. Berikut adalah langkah-langkah yang perlu Anda lakukan:

1.  Tambahkan modal untuk menampilkan detail film dan trailernya di `index.html`:

html

```html
<!-- Tambahkan kode ini sebelum tag script -->
<div id="movie-modal" class="fixed inset-0 hidden z-50 flex items-center justify-center p-4 bg-black bg-opacity-80">
  <div class="relative bg-white rounded-lg w-full max-w-3xl mx-auto">
    <button id="close-modal" class="absolute top-0 right-0 m-4 p-2 rounded-lg focus:outline-none">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x">
        <line x1="18" y1="6" x2="6" y2="18"></line>
        <line x1="6" y1="6" x2="18" y2="18"></line>
      </svg>
    </button>
    <div id="movie-detail" class="p-4">
      <!-- Detail film akan ditampilkan di sini -->
    </div>
  </div>
</div>
```

2.  Tambahkan kode berikut ke file `script.js` untuk menangani tampilan detail film dan trailernya:

javascript

```javascript
const movieModal = document.getElementById('movie-modal');
const movieDetail = document.getElementById('movie-detail');
const closeModal = document.getElementById('close-modal');

function showMovieDetail(id) {
  fetchMovieDetail(id);
  movieModal.classList.remove('hidden');
}

async function fetchMovieDetail(id) {
  const response = await fetch(
    `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&append_to_response=videos`
  );
  const movie = await response.json();
  displayMovieDetail(movie);
}

function displayMovieDetail(movie) {
  const trailer = movie.videos.results.find((video) => video.type === 'Trailer');

  const detailHTML = `
    <div class="flex flex-col md:flex-row">
      <div class="w-full md:w-1/2">
        <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
      </div>
      <div class="w-full md:w-1/2 p-4">
        <h2 class="text-2xl font-bold mb-2">${movie.title}</h2>
        <p><strong>Tanggal Rilis:</strong> ${movie.release_date}</p>
        <p><strong>Rating:</strong> ${movie.vote_average}</p>
        <p><strong>Durasi:</strong> ${movie.runtime} menit</p>
        <p><strong>Overview:</strong> ${movie.overview}</p>
      </div>
    </div>
    ${trailer ? `<div class="mt-4">
      <h3 class="text-xl font-bold mb-2">Trailer</h3>
      <iframe class="w-full h-64 md:h-96" src="https://www.youtube.com/embed/${trailer.key}" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
    </div>` : ''}
  `;
```
 

> Tambahkan kode berikut ke file script.js untuk menangani tampilan detail film dan trailernya:

Terima kasih atas klarifikasi Anda. Berikut adalah kode yang harus ditambahkan ke file `script.js` untuk menangani tampilan detail film dan trailernya:

javascript

```javascript
// Tambahkan kode ini setelah fungsi fetchMovies()

const movieModal = document.getElementById('movie-modal');
const movieDetail = document.getElementById('movie-detail');
const closeModal = document.getElementById('close-modal');

function showMovieDetail(id) {
  fetchMovieDetail(id);
  movieModal.classList.remove('hidden');
}

async function fetchMovieDetail(id) {
  const response = await fetch(
    `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&append_to_response=videos`
  );
  const movie = await response.json();
  displayMovieDetail(movie);
}

function displayMovieDetail(movie) {
  const trailer = movie.videos.results.find((video) => video.type === 'Trailer');

  const detailHTML = `
    <div class="flex flex-col md:flex-row">
      <div class="w-full md:w-1/2">
        <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
      </div>
      <div class="w-full md:w-1/2 p-4">
        <h2 class="text-2xl font-bold mb-2">${movie.title}</h2>
        <p><strong>Tanggal Rilis:</strong> ${movie.release_date}</p>
        <p><strong>Rating:</strong> ${movie.vote_average}</p>
        <p><strong>Durasi:</strong> ${movie.runtime} menit</p>
        <p><strong>Overview:</strong> ${movie.overview}</p>
      </div>
    </div>
    ${trailer ? `<div class="mt-4">
      <h3 class="text-xl font-bold mb-2">Trailer</h3>
      <iframe class="w-full h-64 md:h-96" src="https://www.youtube.com/embed/${trailer.key}" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
    </div>` : ''}
  `;
  movieDetail.innerHTML = detailHTML;
}

// Tambahkan kode ini di akhir file script.js
closeModal.addEventListener('click', () => {
  movieModal.classList.add('hidden');
});

// Modifikasi fungsi displayMovies()
function displayMovies(movies) {
  const movieCards = movies
    .map((movie) => {
      return `
        <div class="bg-white rounded overflow-hidden shadow-lg cursor-pointer" onclick="showMovieDetail(${movie.id})">
          <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
          <div class="p-4">
            <h3 class="font-bold text-xl mb-2">${movie.title}</h3>
            <p class="text-gray-700 text-base">Tanggal Rilis: ${movie.release_date}</p>
            <p class="text-gray-700 text-base">Rating: ${movie.vote_average}</p>
          </div>
        </div>`;
    })
    .join('');
  movieList.innerHTML = movieCards;
}
```

Dengan kode di atas, Anda sekarang dapat menampilkan detail film dan trailernya dalam modal yang responsif. Ketika pengguna mengklik salah satu film, modal akan muncul dengan informasi tentang film tersebut dan trailer (jika ada).

> Tambahkan modal untuk menampilkan detail film dan trailernya di index.html: tulis full kodenya

Berikut adalah kode HTML lengkap untuk file `index.html` dengan modal yang ditambahkan untuk menampilkan detail film dan trailernya:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rekomendasi Film</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.16/dist/tailwind.min.css" rel="stylesheet">
  <link rel="stylesheet" href="styles.css">
</head>
<body class="bg-gray-100">
  <div class="container mx-auto px-4">
    <h1 class="text-4xl font-bold mt-8 mb-4">Rekomendasi Film Hari Ini</h1>
    <div id="movie-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
      <!-- Film akan ditampilkan di sini -->
    </div>
  </div>

  <!-- Tambahkan modal ini sebelum tag script -->
  <div id="movie-modal" class="fixed inset-0 hidden z-50 flex items-center justify-center p-4 bg-black bg-opacity-80">
    <div class="relative bg-white rounded-lg w-full max-w-3xl mx-auto">
      <button id="close-modal" class="absolute top-0 right-0 m-4 p-2 rounded-lg focus:outline-none">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x">
          <line x1="18" y1="6" x2="6" y2="18"></line>
          <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
      </button>
      <div id="movie-detail" class="p-4">
        <!-- Detail film akan ditampilkan di sini -->
      </div>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
```

Dengan menambahkan kode modal di atas, Anda akan dapat menampilkan detail film dan trailernya dalam modal yang responsif setelah mengklik salah satu film. Pastikan Anda juga telah menambahkan kode JavaScript yang sesuai ke file `script.js` untuk mengendalikan perilaku modal dan menampilkan informasi detail film.

> Tambahkan fiture agar bisa scroll di detail nya dan dan buat lebih responsif

Untuk menambahkan fitur scroll pada detail film dan membuatnya lebih responsif, Anda perlu memodifikasi kode HTML dan CSS yang ada. Berikut adalah langkah-langkah yang perlu Anda lakukan:

1.  Modifikasi modal dalam file `index.html` untuk menambahkan fitur scroll pada konten detail film:

html

```html
<!-- Ganti kode modal dengan kode berikut -->
<div id="movie-modal" class="fixed inset-0 hidden z-50 flex items-center justify-center p-4 bg-black bg-opacity-80">
  <div class="relative bg-white rounded-lg w-full max-w-3xl mx-auto max-h-full overflow-y-auto">
    <button id="close-modal" class="absolute top-0 right-0 m-4 p-2 rounded-lg focus:outline-none">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x">
        <line x1="18" y1="6" x2="6" y2="18"></line>
        <line x1="6" y1="6" x2="18" y2="18"></line>
      </svg>
    </button>
    <div id="movie-detail" class="p-4">
      <!-- Detail film akan ditampilkan di sini -->
    </div>
  </div>
</div>
```

2.  Tambahkan gaya CSS berikut ke file `styles.css` untuk membuat modal lebih responsif:

css

```css
/* styles.css */
@media (max-height: 640px) {
  #movie-modal .relative {
    max-height: calc(100% - 2rem);
  }
}

@media (max-width: 767px) {
  #movie-modal .relative {
    max-width: calc(100% - 2rem);
  }
}
```

Dengan perubahan di atas, konten detail film sekarang akan dapat di-scroll jika kontennya melebihi tinggi layar, dan modal akan menyesuaikan ukurannya sesuai dengan lebar layar untuk tampilan yang lebih responsif. Pastikan Anda juga telah menambahkan kode JavaScript yang sesuai ke file `script.js` untuk mengendalikan perilaku modal dan menampilkan informasi detail film.

> lanjutkan

Sekarang Anda telah berhasil menambahkan fitur scroll pada detail film dan membuat tampilan lebih responsif, Anda bisa melanjutkan dengan mengoptimalkan tampilan dan fungsionalitas lebih lanjut. Berikut beberapa ide yang bisa Anda tambahkan:

1.  Tambahkan paginasi atau infinite scroll untuk menampilkan lebih banyak film.
    
2.  Tambahkan fitur pencarian untuk mencari film berdasarkan judul atau kata kunci.
    
3.  Tambahkan filter berdasarkan genre, tahun rilis, atau rating.
    
4.  Tampilkan aktor dan kru film dalam detail film.
    
5.  Tambahkan fitur untuk menampilkan film terkait atau rekomendasi berdasarkan film yang sedang ditampilkan.
    

Berikut contoh kode untuk menambahkan fitur pencarian:

Tambahkan elemen input pencarian di file `index.html`:

html

```html
<!-- Tambahkan kode ini di bawah tag h1 -->
<div class="mb-4">
  <input id="search-input" type="text" class="w-full p-2 border border-gray-300 rounded" placeholder="Cari film...">
</div>
```

Tambahkan kode berikut ke file `script.js` untuk menangani pencarian film:

javascript

```javascript
const searchInput = document.getElementById('search-input');

searchInput.addEventListener('input', (event) => {
  const query = event.target.value;
  if (query.length > 2) {
    searchMovies(query);
  } else {
    fetchMovies();
  }
});

async function searchMovies(query) {
  const response = await fetch(
    `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}`
  );
  const data = await response.json();
  displayMovies(data.results);
}
```

Dengan kode di atas, Anda telah menambahkan fitur pencarian film ke aplikasi Anda. Sekarang, pengguna dapat mencari film berdasarkan judul atau kata kunci dengan memasukkan teks ke dalam kotak pencarian.

Untuk ide-ide lain yang disebutkan sebelumnya, Anda dapat mempelajari dokumentasi API The Movie Database dan mengeksplorasi fitur tambahan yang ingin Anda tambahkan ke aplikasi Anda. Pastikan untuk terus mengoptimalkan tampilan responsif dan mencoba aplikasi Anda di berbagai perangkat dan ukuran layar. Selamat mencoba!

> Tulis semua kode Tambahkan elemen input pencarian di file index.html:

Berikut adalah kode lengkap untuk file `index.html` dengan elemen input pencarian yang ditambahkan:

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rekomendasi Film</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.16/dist/tailwind.min.css" rel="stylesheet">
  <link rel="stylesheet" href="styles.css">
</head>
<body class="bg-gray-100">
  <div class="container mx-auto px-4">
    <h1 class="text-4xl font-bold mt-8 mb-4">Rekomendasi Film Hari Ini</h1>

    <!-- Tambahkan kode ini di bawah tag h1 -->
    <div class="mb-4">
      <input id="search-input" type="text" class="w-full p-2 border border-gray-300 rounded" placeholder="Cari film...">
    </div>

    <div id="movie-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
      <!-- Film akan ditampilkan di sini -->
    </div>
  </div>

  <div id="movie-modal" class="fixed inset-0 hidden z-50 flex items-center justify-center p-4 bg-black bg-opacity-80">
    <div class="relative bg-white rounded-lg w-full max-w-3xl mx-auto max-h-full overflow-y-auto">
      <button id="close-modal" class="absolute top-0 right-0 m-4 p-2 rounded-lg focus:outline-none">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x">
          <line x1="18" y1="6" x2="6" y2="18"></line>
          <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
      </button>
      <div id="movie-detail" class="p-4">
        <!-- Detail film akan ditampilkan di sini -->
      </div>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
```

Dengan menambahkan elemen input pencarian di atas, Anda telah memberikan cara bagi pengguna untuk mencari film berdasarkan judul atau kata kunci. Pastikan Anda juga telah menambahkan kode JavaScript yang sesuai ke file `script.js` untuk mengendalikan fitur pencarian film.

> tulis semua kode Tambahkan kode berikut ke file script.js untuk menangani pencarian film:

Berikut adalah kode lengkap untuk file `script.js` dengan kode untuk menangani pencarian film:

javascript

```javascript
const apiKey = 'your_api_key_here';
const movieList = document.getElementById('movie-list');
const movieModal = document.getElementById('movie-modal');
const movieDetail = document.getElementById('movie-detail');
const closeModal = document.getElementById('close-modal');
const searchInput = document.getElementById('search-input');

fetchMovies();

async function fetchMovies() {
  const response = await fetch(`https://api.themoviedb.org/3/trending/movie/day?api_key=${apiKey}`);
  const data = await response.json();
  displayMovies(data.results);
}

function displayMovies(movies) {
  const movieCards = movies
    .map((movie) => {
      return `
        <div class="bg-white rounded overflow-hidden shadow-lg cursor-pointer" onclick="showMovieDetail(${movie.id})">
          <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
          <div class="p-4">
            <h3 class="font-bold text-xl mb-2">${movie.title}</h3>
            <p class="text-gray-700 text-base">Tanggal Rilis: ${movie.release_date}</p>
            <p class="text-gray-700 text-base">Rating: ${movie.vote_average}</p>
          </div>
        </div>`;
    })
    .join('');
  movieList.innerHTML = movieCards;
}

function showMovieDetail(id) {
  fetchMovieDetail(id);
  movieModal.classList.remove('hidden');
}

async function fetchMovieDetail(id) {
  const response = await fetch(
    `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&append_to_response=videos`
  );
  const movie = await response.json();
  displayMovieDetail(movie);
}

function displayMovieDetail(movie) {
  const trailer = movie.videos.results.find((video) => video.type === 'Trailer');

  const detailHTML = `
    <div class="flex flex-col md:flex-row">
      <div class="w-full md:w-1/2">
        <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
      </div>
      <div class="w-full md:w-1/2 p-4">
        <h2 class="text-2xl font-bold mb-2">${movie.title}</h2>
        <p><strong>Tanggal Rilis:</strong> ${movie.release_date}</p>
        <p><strong>Rating:</strong> ${movie.vote_average}</p>
        <p><strong>Durasi:</strong> ${movie.runtime} menit</p>
        <p><strong>Overview:</strong> ${movie.overview}</p>
      </div>
    </div>
    ${trailer ? `<div class="mt-4">
      <h3 class="text-xl font-bold mb-2">Trailer</h3>
      <iframe class="w-full h-64 md:h-96" src="https://www.youtube.com/embed/${trailer.key}" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
    </div>` : ''}
  `;
  movieDetail.innerHTML = detailHTML;
}

closeModal.addEventListener('click', () => {
  movieModal.classList.add('hidden');
});

searchInput.addEventListener('input', (event) => {
  const query = event.target.value;
  if (query.length > 2) {
    searchMovies(query);
  } else {
    fetchMovies();
  }
});

async function
```

> Lanjutkan

Mohon maaf atas kesalahan dalam kode sebelumnya. Berikut adalah kode lengkap dan terakhir untuk file `script.js` dengan kode untuk menangani pencarian film:

javascript

```javascript
const apiKey = 'your_api_key_here';
const movieList = document.getElementById('movie-list');
const movieModal = document.getElementById('movie-modal');
const movieDetail = document.getElementById('movie-detail');
const closeModal = document.getElementById('close-modal');
const searchInput = document.getElementById('search-input');

fetchMovies();

async function fetchMovies() {
  const response = await fetch(`https://api.themoviedb.org/3/trending/movie/day?api_key=${apiKey}`);
  const data = await response.json();
  displayMovies(data.results);
}

function displayMovies(movies) {
  const movieCards = movies
    .map((movie) => {
      return `
        <div class="bg-white rounded overflow-hidden shadow-lg cursor-pointer" onclick="showMovieDetail(${movie.id})">
          <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
          <div class="p-4">
            <h3 class="font-bold text-xl mb-2">${movie.title}</h3>
            <p class="text-gray-700 text-base">Tanggal Rilis: ${movie.release_date}</p>
            <p class="text-gray-700 text-base">Rating: ${movie.vote_average}</p>
          </div>
        </div>`;
    })
    .join('');
  movieList.innerHTML = movieCards;
}

function showMovieDetail(id) {
  fetchMovieDetail(id);
  movieModal.classList.remove('hidden');
}

async function fetchMovieDetail(id) {
  const response = await fetch(
    `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&append_to_response=videos`
  );
  const movie = await response.json();
  displayMovieDetail(movie);
}

function displayMovieDetail(movie) {
  const trailer = movie.videos.results.find((video) => video.type === 'Trailer');

  const detailHTML = `
    <div class="flex flex-col md:flex-row">
      <div class="w-full md:w-1/2">
        <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
      </div>
      <div class="w-full md:w-1/2 p-4">
        <h2 class="text-2xl font-bold mb-2">${movie.title}</h2>
        <p><strong>Tanggal Rilis:</strong> ${movie.release_date}</p>
        <p><strong>Rating:</strong> ${movie.vote_average}</p>
        <p><strong>Durasi:</strong> ${movie.runtime} menit</p>
        <p><strong>Overview:</strong> ${movie.overview}</p>
      </div>
    </div>
    ${trailer ? `<div class="mt-4">
      <h3 class="text-xl font-bold mb-2">Trailer</h3>
      <iframe class="w-full h-64 md:h-96" src="https://www.youtube.com/embed/${trailer.key}" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
    </div>` : ''}
  `;
  movieDetail.innerHTML = detailHTML;
}

closeModal.addEventListener('click', () => {
  movieModal.classList.add('hidden');
});

searchInput.addEventListener('input', (event) => {
  const query = event.target.value;
  if (query.length > 2) {
```
   

> Lanjutkan kodenya dari sini closeModal.addEventListener('click', () => { movieModal.classList.add('hidden'); }); searchInput.addEventListener('input', (event) => { const query = event.target.value; if (query.length > 2) { searchMovies(query); } else { fetchMovies(); } }); async function

searchMovies(query) { const response = await fetch( `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}` ); const data = await response.json(); displayMovies(data.results); }

// Kode di atas adalah kode untuk menangani pencarian film // Anda dapat menambahkan lebih banyak fitur dan fungsi di sini sesuai kebutuhan

// Contoh: menambahkan fitur untuk menampilkan film berdasarkan genre async function fetchMoviesByGenre(genreId) { const response = await fetch( `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&with_genres=${genreId}` ); const data = await response.json(); displayMovies(data.results); }

// Contoh pemanggilan fungsi untuk menampilkan film dengan genre tertentu (misal genre ID 28 untuk Aksi) // fetchMoviesByGenre(28);

`Kode di atas menyelesaikan implementasi fitur pencarian film. Jika Anda ingin menambahkan lebih banyak fitur atau fungsi, Anda bisa melanjutkan dengan menulis kode tambahan di bawah ini. Contoh yang diberikan di atas adalah kode untuk menampilkan film berdasarkan genre. Anda bisa menyesuaikan dan menambahkan fitur serupa sesuai kebutuhan Anda.`

> Lanjutkan kodenya dari sini \`\`\` closeModal.addEventListener('click', () => { movieModal.classList.add('hidden'); }); searchInput.addEventListener('input', (event) => { const query = event.target.value; if (query.length > 2) { searchMovies(query); } else { fetchMovies(); } }); async function \`\`\`

searchMovies(query) { const response = await fetch( `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}` ); const data = await response.json(); displayMovies(data.results); }

// Kode di atas adalah kode untuk menangani pencarian film //

> Lanjutkan kode ini karna ada yang kurang const apiKey = 'your\_api\_key\_here'; const movieList = document.getElementById('movie-list'); const movieModal = document.getElementById('movie-modal'); const movieDetail = document.getElementById('movie-detail'); const closeModal = document.getElementById('close-modal'); const searchInput = document.getElementById('search-input'); fetchMovies(); async function fetchMovies() { const response = await fetch(\`https://api.themoviedb.org/3/trending/movie/day?api\_key=${apiKey}\`); const data = await response.json(); displayMovies(data.results); } function displayMovies(movies) { const movieCards = movies .map((movie) => { return \` <div class="bg-white rounded overflow-hidden shadow-lg cursor-pointer" onclick="showMovieDetail(${movie.id})"> <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster\_path}" alt="${movie.title}"> <div class="p-4"> <h3 class="font-bold text-xl mb-2">${movie.title}</h3> <p class="text-gray-700 text-base">Tanggal Rilis: ${movie.release\_date}</p> <p class="text-gray-700 text-base">Rating: ${movie.vote\_average}</p> </div> </div>\`; }) .join(''); movieList.innerHTML = movieCards; } function showMovieDetail(id) { fetchMovieDetail(id); movieModal.classList.remove('hidden'); } async function fetchMovieDetail(id) { const response = await fetch( \`https://api.themoviedb.org/3/movie/${id}?api\_key=${apiKey}&append\_to\_response=videos\` ); const movie = await response.json(); displayMovieDetail(movie); } function displayMovieDetail(movie) { const trailer = movie.videos.results.find((video) => video.type === 'Trailer'); const detailHTML = \` <div class="flex flex-col md:flex-row"> <div class="w-full md:w-1/2"> <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster\_path}" alt="${movie.title}"> </div> <div class="w-full md:w-1/2 p-4"> <h2 class="text-2xl font-bold mb-2">${movie.title}</h2> <p><strong>Tanggal Rilis:</strong> ${movie.release\_date}</p> <p><strong>Rating:</strong> ${movie.vote\_average}</p> <p><strong>Durasi:</strong> ${movie.runtime} menit</p> <p><strong>Overview:</strong> ${movie.overview}</p> </div> </div> ${trailer ? \`<div class="mt-4"> <h3 class="text-xl font-bold mb-2">Trailer</h3> <iframe class="w-full h-64 md:h-96" src="https://www.youtube.com/embed/${trailer.key}" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe> </div>\` : ''} \`; movieDetail.innerHTML = detailHTML; } closeModal.addEventListener('click', () => { movieModal.classList.add('hidden'); }); searchInput.addEventListener('input', (event) => { const query = event.target.value; if (query.length > 2) { searchMovies(query); } else { fetchMovies(); } }); async function

searchMovies(query) { const response = await fetch( `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}` ); const data = await response.json(); displayMovies(data.results); }

// Kode di atas adalah kode untuk menangani pencarian film // Anda dapat menambahkan lebih banyak fitur dan fungsi di sini sesuai kebutuhan

// Contoh: menambahkan fitur untuk menampilkan film berdasarkan genre async function fetchMoviesByGenre(genreId) { const response = await fetch( `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&with_genres=${genreId}` ); const data = await response.json(); displayMovies(data.results); }

// Contoh pemanggilan fungsi untuk menampilkan film dengan genre tertentu (misal genre ID 28 untuk Aksi) // fetchMoviesByGenre(28);

// Contoh: menambahkan fitur untuk menampilkan film berdasarkan tahun rilis async function fetchMoviesByYear(year) { const response = await fetch( `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&primary_release_year=${year}` ); const data = await response.json(); displayMovies(data.results); }

// Contoh pemanggilan fungsi untuk menampilkan film dengan tahun rilis tertentu (misal tahun 2020) // fetchMoviesByYear(2020);

`Kode di atas menyelesaikan implementasi fitur pencarian film dan menambahkan contoh fitur untuk menampilkan film berdasarkan genre dan tahun rilis. Anda bisa menyesuaikan dan menambahkan fitur serupa sesuai kebutuhan Anda.`

> tulis semua kode Tambahkan kode berikut ke file script.js untuk menangani pencarian film: secara clean code

Berikut adalah kode yang lebih bersih untuk menangani pencarian film dalam file `script.js`:

javascript

```javascript
const apiKey = 'your_api_key_here';
const movieList = document.getElementById('movie-list');
const movieModal = document.getElementById('movie-modal');
const movieDetail = document.getElementById('movie-detail');
const closeModal = document.getElementById('close-modal');
const searchInput = document.getElementById('search-input');

fetchMovies();

async function fetchMovies() {
  const response = await fetch(`https://api.themoviedb.org/3/trending/movie/day?api_key=${apiKey}`);
  const data = await response.json();
  displayMovies(data.results);
}

function displayMovies(movies) {
  const movieCards = movies.map(createMovieCard).join('');
  movieList.innerHTML = movieCards;
}

function createMovieCard(movie) {
  return `
    <div class="bg-white rounded overflow-hidden shadow-lg cursor-pointer" onclick="showMovieDetail(${movie.id})">
      <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
      <div class="p-4">
        <h3 class="font-bold text-xl mb-2">${movie.title}</h3>
        <p class="text-gray-700 text-base">Tanggal Rilis: ${movie.release_date}</p>
        <p class="text-gray-700 text-base">Rating: ${movie.vote_average}</p>
      </div>
    </div>`;
}

function showMovieDetail(id) {
  fetchMovieDetail(id);
  movieModal.classList.remove('hidden');
}

async function fetchMovieDetail(id) {
  const response = await fetch(
    `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&append_to_response=videos`
  );
  const movie = await response.json();
  displayMovieDetail(movie);
}

function displayMovieDetail(movie) {
  const trailer = movie.videos.results.find((video) => video.type === 'Trailer');
  movieDetail.innerHTML = createMovieDetailHtml(movie, trailer);
}

function createMovieDetailHtml(movie, trailer) {
  return `
    <div class="flex flex-col md:flex-row">
      <div class="w-full md:w-1/2">
        <img class="w-full" src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
      </div>
      <div class="w-full md:w-1/2 p-4">
        <h2 class="text-2xl font-bold mb-2">${movie.title}</h2>
        <p><strong>Tanggal Rilis:</strong> ${movie.release_date}</p>
        <p><strong>Rating:</strong> ${movie.vote_average}</p>
        <p><strong>Durasi:</strong> ${movie.runtime} menit</p>
        <p><strong>Overview:</strong> ${movie.overview}</p>
      </div>
    </div>
    ${trailer ? `<div class="mt-4">
      <h3 class="text-xl font-bold mb-2">Trailer</h3>
      <iframe class="w-full h-64 md:h-96" src="https://www.youtube.com/embed/${trailer.key}" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
    </div>` : ''}
  `;
}

closeModal.addEventListener('click', () => {
  movieModal.classList.add('hidden');
});

searchInput.addEventListener('input', handleSearchInput);

async function handleSearchInput(event) {
  const query = event.target.value;
  if (query.length > 2
```

> lanjutkan dengan format javascript

Teruskan dengan format JavaScript:

javascript

  ```javascript
) {
    searchMovies(query);
  } else {
    fetchMovies();
  }
}

async function searchMovies(query) {
  const response = await fetch(
    `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}`
  );
  const data = await response.json();
  displayMovies(data.results);
}

// Anda dapat menambahkan lebih banyak fitur dan fungsi di sini sesuai kebutuhan
// Contoh: menambahkan fitur untuk menampilkan film berdasarkan genre
async function fetchMoviesByGenre(genreId) {
  const response = await fetch(
    `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&with_genres=${genreId}`
  );
  const data = await response.json();
  displayMovies(data.results);
}

// Contoh pemanggilan fungsi untuk menampilkan film dengan genre tertentu (misal genre ID 28 untuk Aksi)
// fetchMoviesByGenre(28);

// Contoh: menambahkan fitur untuk menampilkan film berdasarkan tahun rilis
async function fetchMoviesByYear(year) {
  const response = await fetch(
    `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&primary_release_year=${year}`
  );
  const data = await response.json();
  displayMovies(data.results);
}

// Contoh pemanggilan fungsi untuk menampilkan film dengan tahun rilis tertentu (misal tahun 2020)
// fetchMoviesByYear(2020);
```

Kode di atas telah dioptimalkan dengan format JavaScript yang lebih bersih. Anda bisa menambahkan lebih banyak fitur atau fungsi sesuai kebutuhan Anda. Contoh yang diberikan di atas adalah kode untuk menampilkan film berdasarkan genre dan tahun rilis.

> tambahkan fiture film berdasarkan tempat streaming seperti netflix disney atau yang lainya dan pisahkan style css nya dengan javascript atau taildwindnya

Untuk menambahkan fitur film berdasarkan platform streaming seperti Netflix, Disney+, atau yang lainnya, Anda harus menggali informasi lebih lanjut tentang film yang ada pada platform tersebut. The Movie Database (TMDb) API menyediakan cara untuk mencari film berdasarkan platform streaming menggunakan ID penyedia layanan.

Sebagai contoh, berikut adalah cara untuk mencari film yang tersedia di Netflix (ID penyedia layanan: 8).

1.  Tambahkan kode berikut ke `script.js` untuk menangani pencarian film berdasarkan platform streaming:

javascript

```javascript
async function fetchMoviesByStreamingService(serviceId) {
  const response = await fetch(
    `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&with_watch_providers=${serviceId}&watch_region=US`
  );
  const data = await response.json();
  displayMovies(data.results);
}

// Contoh pemanggilan fungsi untuk menampilkan film di Netflix (ID penyedia layanan: 8)
// fetchMoviesByStreamingService(8);
```

2.  Untuk memisahkan CSS dari JavaScript atau Tailwind, Anda harus menulis CSS kustom Anda dalam file terpisah dan menghubungkannya dengan file HTML.

Buat file baru bernama `styles.css` dan tambahkan kode CSS kustom Anda di sana, seperti:

css

```css
/* styles.css */

/* Contoh kode CSS kustom */
.movie-card:hover {
  transform: scale(1.05);
  transition: transform 0.3s;
}
```

3.  Sambungkan file `styles.css` ke file `index.html` Anda:

html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.16/dist/tailwind.min.css" rel="stylesheet" />
    <!-- Tambahkan baris berikut untuk menghubungkan file styles.css -->
    <link rel="stylesheet" href="styles.css" />
    <title>Movie Recommender</title>
  </head>
  <body>
    <!-- Kode HTML di sini -->
    <script src="script.js"></script>
  </body>
</html>
```

Dengan melakukan langkah-langkah ini, Anda telah menambahkan fitur untuk mencari film berdasarkan platform streaming dan memisahkan CSS kustom Anda dari JavaScript atau Tailwind. Anda bisa menyesuaikan dan menambahkan fitur serupa sesuai kebutuhan Anda.