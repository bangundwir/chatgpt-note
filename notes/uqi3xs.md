Model: GPT-4

> Buatkan website Podcast dari playslist youtube membuat per episode beserta caption dan subutiole nya yang bisa di klik setiap menitnya

Untuk membuat sebuah website Podcast berdasarkan playlist YouTube, Anda perlu mengikuti langkah-langkah berikut:

1.  Domain dan Hosting:

Pertama, Anda harus memilih dan membeli nama domain serta hosting untuk website Anda. Beberapa penyedia layanan yang populer meliputi:

*   Namecheap ([www.namecheap.com](http://www.namecheap.com))
*   GoDaddy (

> Buatkan node js create website Podcast dari playslist youtube membuat per episode beserta caption dan subutiole nya yang bisa di klik setiap menitnya dan direct ke youtube nay

Untuk membuat sebuah aplikasi Node.js yang membuat website Podcast dari playlist YouTube, Anda perlu mengikuti langkah-langkah berikut:

1.  Buat proyek Node.js

Buat folder baru untuk proyek Anda dan jalankan `npm init` di dalam folder tersebut. Ikuti petunjuk di layar untuk membuat file `package.json`.

2.  Instal modul yang diperlukan

Anda akan memerlukan beberapa modul tambahan untuk proyek ini. Instal mereka menggunakan `npm install`:

bash

```bash
npm install express ejs youtube-api-search youtube-captions-scraper axios
```

3.  Buat file server.js

Buat file `server.js` di dalam folder proyek Anda. Ini akan menjadi titik awal aplikasi Anda.

javascript

```javascript
const express = require("express");
const app = express();
const axios = require("axios");
const yts = require("youtube-api-search");
const ycs = require("youtube-captions-scraper");

const API_KEY = "YOUR_YOUTUBE_API_KEY";
const PORT = process.env.PORT || 3000;

app.set("view engine", "ejs");
app.use(express.static("public"));

app.get("/", async (req, res) => {
  // Ambil playlist ID dari query parameter
  const playlistId = req.query.playlistId;

  if (!playlistId) {
    return res.render("index", { episodes: [] });
  }

  // Dapatkan video dari playlist
  const episodes = await getEpisodes(playlistId);

  // Dapatkan subjudul untuk setiap episode
  const subtitlesPromises = episodes.map((episode) =>
    getSubtitles(episode.id.videoId)
  );
  const subtitles = await Promise.all(subtitlesPromises);

  // Gabungkan informasi episode dan subjudul
  const data = episodes.map((episode, index) => ({
    ...episode,
    subtitles: subtitles[index],
  }));

  res.render("index", { episodes: data });
});

async function getEpisodes(playlistId) {
  const searchOptions = {
    key: API_KEY,
    type: "video",
    part: "snippet",
    maxResults: 50,
    playlistId,
  };

  return new Promise((resolve, reject) => {
    yts(searchOptions, (err, results) => {
      if (err) {
        return reject(err);
      }
      resolve(results);
    });
  });
}

async function getSubtitles(videoId) {
  try {
    const captions = await ycs.getSubtitles({
      videoID: videoId,
      lang: "en", // Ubah ini ke bahasa yang diinginkan
    });

    return captions.map((caption) => ({
      start: caption.start,
      end: caption.end,
      text: caption.text,
      url: `https://www.youtube.com/watch?v=${videoId}&t=${Math.floor(
        caption.start
      )}`,
    }));
  } catch (error) {
    console.error("Error fetching subtitles:", error);
    return [];
  }
}

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

4.  Buat file `views/index.ejs`

Buat folder `views` di dalam folder proyek Anda dan buat file `index.ejs` di dalam folder `views`. Ini akan menjadi template untuk halaman web Anda.

html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Pod
```

> lanjutkan

3 / 3

Selanjutnya, Anda perlu melanjutkan dengan menulis template `index.ejs`. Ini akan menjadi tampilan dari halaman web Anda.

html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Podcast dari Playlist YouTube</title>
    <link
      rel="stylesheet"
      href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
    />
  </head>
  <body>
    <div class="container">
      <h1 class="mt-5">Podcast dari Playlist YouTube</h1>
      <form class="mt-3">
        <div class="form-group">
          <label for="playlistId">Playlist ID</label>
          <input
            type="text"
            class="form-control"
            id="playlistId"
            name="playlistId"
            placeholder="Masukkan Playlist ID"
            value="<%= typeof playlistId !== 'undefined' ? playlistId : '' %>"
          />
        </div>
        <button type="submit" class="btn btn-primary">Tampilkan Podcast</button>
      </form>
      <div class="mt-5">
        <% episodes.forEach((episode, index) => { %>
          <div class="card my-3">
            <div class="card-body">
              <h5 class="card-title"><%= episode.snippet.title %></h5>
              <p class="card-text"><%= episode.snippet.description %></p>
              <% episode.subtitles.forEach((subtitle) => { %>
                <p>
                  <a
                    href="<%= subtitle.url %>"
                    target="_blank"
                    rel="noopener noreferrer"
                    ><%= subtitle.text %></a
                  >
                </p>
              <% }); %>
            </div>
          </div>
        <% }); %>
      </div>
    </div>
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
  </body>
</html>
```

5.  Jalankan aplikasi

Untuk menjalankan aplikasi, pastikan Anda telah menggantikan `YOUR_YOUTUBE_API_KEY` dengan kunci API YouTube yang valid di file `server.js`. Kemudian, jalankan perintah berikut di terminal atau command prompt:

bash

```bash
node server.js
```

6.  Akses aplikasi di peramban

Buka peramban web Anda dan akses aplikasi di [http://localhost:3000](http://localhost:3000). Anda akan melihat formulir untuk memasukkan ID playlist YouTube. Masukkan ID playlist yang valid, lalu tekan tombol "Tampilkan Podcast" untuk melihat daftar episode beserta teks subtitle yang dapat di-klik untuk membuka video YouTube dengan timestamp yang sesuai.

Selamat, Anda telah berhasil membuat aplikasi Node.js yang menampilkan Podcast dari playlist YouTube beserta subtitle yang dapat di-klik setiap menitnya!

> Buatakan script whisper api to srt audio

Untuk membuat script yang mengkonversi hasil dari Whisper API (pengenalan suara otomatis) ke format SRT (SubRip Text), Anda dapat menggunakan bahasa pemrograman Python. Berikut adalah langkah-langkahnya:

1.  Instalasi library yang diperlukan

Sebelum memulai, Anda perlu menginstal library `requests` untuk mengakses Whisper API dan `srt` untuk menghasilkan file SRT. Anda bisa menginstalnya menggunakan pip:

bash

```bash
pip install requests srt
```

2.  Buat file `whisper_to_srt.py`

Buat file dengan nama `whisper_to_srt.py` dan masukkan kode berikut:

python

```python
import requests
import json
import sys
from datetime import timedelta
import srt

WHISPER_API_KEY = 'your_whisper_api_key'
AUDIO_FILE_PATH = 'path/to/your/audio/file.wav'
SRT_OUTPUT_PATH = 'path/to/your/output/file.srt'

def submit_audio_to_whisper(file_path):
    headers = {
        'Authorization': f'Bearer {WHISPER_API_KEY}'
    }
    url = 'https://api.openai.com/v1/whispers/submissions'

    with open(file_path, 'rb') as audio_file:
        response = requests.post(
            url,
            headers=headers,
            files={'file': audio_file}
        )

    if response.status_code != 201:
        print(f'Error submitting audio: {response.status_code}')
        sys.exit(1)

    return response.json()['id']

def get_transcription(submission_id):
    headers = {
        'Authorization': f'Bearer {WHISPER_API_KEY}'
    }
    url = f'https://api.openai.com/v1/whispers/submissions/{submission_id}'

    while True:
        response = requests.get(url, headers=headers)

        if response.status_code != 200:
            print(f'Error getting transcription: {response.status_code}')
            sys.exit(1)

        status = response.json()['status']

        if status == 'done':
            return response.json()['transcription']
        elif status == 'failed':
            print('Transcription failed')
            sys.exit(1)

        time.sleep(5)

def transcription_to_srt(transcription):
    srt_data = []

    for idx, item in enumerate(transcription['words'], start=1):
        start = timedelta(seconds=item['start'])
        end = timedelta(seconds=item['end'])
        text = item['word']

        srt_data.append(srt.Subtitle(index=idx, start=start, end=end, content=text))

    return srt.compose(srt_data)

def main():
    submission_id = submit_audio_to_whisper(AUDIO_FILE_PATH)
    transcription = get_transcription(submission_id)
    srt_output = transcription_to_srt(transcription)

    with open(SRT_OUTPUT_PATH, 'w') as srt_file:
        srt_file.write(srt_output)

if __name__ == '__main__':
    main()
```

Gantilah `your_whisper_api_key`, `path/to/your/audio/file.wav`, dan `path/to/your/output/file.srt` dengan kunci API Whisper yang valid, jalur file audio Anda, dan jalur file keluaran SRT Anda.

3.  Jalankan script

Jalankan script dengan menggunakan Python:

bash

```bash
python whisper_to_srt.py
```

Script ini akan mengirim file audio ke Whisper API, menunggu transkripsi selesai, dan kemudian mengonversi hasil transkripsi ke format SRT. Hasil konversi akan disimpan dalam file output yang

> buatakan script youtubesearchpython

`youtubesearchpython` adalah pustaka Python yang memungkinkan Anda mencari video, channel, dan playlist di YouTube tanpa menggunakan API resmi YouTube. Berikut adalah langkah-langkah untuk menggunakan pustaka ini:

1.  Instalasi pustaka `youtubesearchpython`

Anda perlu menginstal pustaka `youtubesearchpython` terlebih dahulu. Anda bisa menginstalnya menggunakan pip:

bash

```bash
pip install youtubesearchpython
```

2.  Buat file `youtube_search.py`

Buat file dengan nama `youtube_search.py` dan masukkan kode berikut:

python

```python
from youtubesearchpython import VideosSearch, ChannelsSearch, PlaylistsSearch
import sys

def search_videos(query, max_results=10):
    video_search = VideosSearch(query, limit=max_results)
    results = video_search.result()
    return results['result']

def search_channels(query, max_results=10):
    channel_search = ChannelsSearch(query, limit=max_results)
    results = channel_search.result()
    return results['result']

def search_playlists(query, max_results=10):
    playlist_search = PlaylistsSearch(query, limit=max_results)
    results = playlist_search.result()
    return results['result']

def main():
    if len(sys.argv) != 3:
        print("Usage: python youtube_search.py [videos|channels|playlists] query")
        sys.exit(1)

    search_type = sys.argv[1]
    query = sys.argv[2]

    if search_type == 'videos':
        results = search_videos(query)
    elif search_type == 'channels':
        results = search_channels(query)
    elif search_type == 'playlists':
        results = search_playlists(query)
    else:
        print("Invalid search type. Choose from [videos|channels|playlists].")
        sys.exit(1)

    for item in results:
        print(item['title'], '-', item['link'])

if __name__ == '__main__':
    main()
```

3.  Jalankan script

Untuk menjalankan script, gunakan perintah berikut:

bash

```bash
python youtube_search.py [videos|channels|playlists] query
```

Gantilah `[videos|channels|playlists]` dengan jenis pencarian yang ingin Anda lakukan, dan `query` dengan istilah pencarian yang ingin Anda gunakan. Contoh:

bash

```bash
python youtube_search.py videos "openai codex"
```

Script ini akan mencari video, channel, atau playlist sesuai dengan jenis pencarian yang Anda tentukan dan mencetak hasil pencarian dalam format "judul - tautan".

> Gunakan lybrary dari ini pip install youtube-search-python

4 / 4

Saya memahami bahwa Anda ingin menggunakan pustaka `youtube-search-python`. Berikut ini adalah langkah-langkah untuk menggunakan pustaka ini:

1.  Instalasi pustaka `youtube-search-python`

Anda perlu menginstal pustaka `youtube-search-python` terlebih dahulu. Anda bisa menginstalnya menggunakan pip:

bash

```bash
pip install youtube-search-python
```

2.  Buat file `youtube_search.py`

Buat file dengan nama `youtube_search.py` dan masukkan kode berikut:

python

```python
from youtube_search import YoutubeSearch
import sys

def search_videos(query, max_results=10):
    video_search = YoutubeSearch(query, max_results=max_results)
    results = video_search.to_dict()
    return results

def main():
    if len(sys.argv) != 2:
        print("Usage: python youtube_search.py query")
        sys.exit(1)

    query = sys.argv[1]
    results = search_videos(query)

    for item in results:
        print(item['title'], '-', f"https://youtube.com{item['url_suffix']}")

if __name__ == '__main__':
    main()
```

3.  Jalankan script

Untuk menjalankan script, gunakan perintah berikut:

bash

```bash
python youtube_search.py query
```

Gantilah `query` dengan istilah pencarian yang ingin Anda gunakan. Contoh:

bash

```bash
python youtube_search.py "openai codex"
```

Script ini akan mencari video sesuai dengan istilah pencarian yang Anda tentukan dan mencetak hasil pencarian dalam format "judul - tautan". Perlu dicatat bahwa pustaka `youtube-search-python` hanya mendukung pencarian video, tidak mencakup pencarian channel atau playlist.

> from youtubesearchpython import VideosSearch, ChannelsSearch, PlaylistsSearch import sys def search\_videos(query, max\_results=10): video\_search = VideosSearch(query, limit=max\_results) results = video\_search.result() return results\['result'\] def search\_channels(query, max\_results=10): channel\_search = ChannelsSearch(query, limit=max\_results) results = channel\_search.result() return results\['result'\] def search\_playlists(query, max\_results=10): playlist\_search = PlaylistsSearch(query, limit=max\_results) results = playlist\_search.result() return results\['result'\] def main(): if len(sys.argv) != 3: print( "Usage: python youtube\_search.py \[videos|channels|playlists\] query") sys.exit(1) search\_type = sys.argv\[1\] query = sys.argv\[2\] if search\_type == 'videos': results = search\_videos(query) elif search\_type == 'channels': results = search\_channels(query) elif search\_type == 'playlists': results = search\_playlists(query) else: print("Invalid search type. Choose from \[videos|channels|playlists\].") sys.exit(1) for item in results: print(item\['title'\], '-', item\['link'\]) if \_\_name\_\_ == '\_\_main\_\_': main() Buatakan agar bisa berjalan di streamlit dan simpan hasil nya di folder hasil menjadi format csv

Untuk membuat aplikasi ini berjalan dengan Streamlit dan menyimpan hasilnya dalam format CSV, ikuti langkah-langkah berikut:

1.  Instalasi Streamlit

Jika Anda belum menginstal Streamlit, Anda bisa menginstalnya menggunakan pip:

bash

```bash
pip install streamlit
```

2.  Buat file `streamlit_app.py`

Buat file dengan nama `streamlit_app.py` dan masukkan kode berikut:

python

```python
import streamlit as st
import pandas as pd
from youtubesearchpython import VideosSearch, ChannelsSearch, PlaylistsSearch

def search_videos(query, max_results=10):
    video_search = VideosSearch(query, limit=max_results)
    results = video_search.result()
    return results['result']

def search_channels(query, max_results=10):
    channel_search = ChannelsSearch(query, limit=max_results)
    results = channel_search.result()
    return results['result']

def search_playlists(query, max_results=10):
    playlist_search = PlaylistsSearch(query, limit=max_results)
    results = playlist_search.result()
    return results['result']

def search_to_dataframe(results):
    data = []

    for item in results:
        data.append({
            'title': item['title'],
            'link': item['link']
        })

    return pd.DataFrame(data)

st.title("Pencarian YouTube")
search_type = st.selectbox("Jenis Pencarian", ["videos", "channels", "playlists"])
query = st.text_input("Masukkan Query Pencarian")

if st.button("Cari"):
    if search_type == 'videos':
        results = search_videos(query)
    elif search_type == 'channels':
        results = search_channels(query)
    elif search_type == 'playlists':
        results = search_playlists(query)

    if results:
        df = search_to_dataframe(results)
        st.write(df)

        st.download_button(
            label="Unduh Hasil CSV",
            data=df.to_csv(index=False).encode('utf-8'),
            file_name="hasil_pencarian_youtube.csv",
            mime="text/csv"
        )
    else:
        st.warning("Tidak ada hasil yang ditemukan.")
```

3.  Jalankan aplikasi Streamlit

Jalankan aplikasi Streamlit dengan menggunakan perintah berikut:

bash

```bash
streamlit run streamlit_app.py
```

Buka browser Anda dan akses aplikasi di [http://localhost:8501](http://localhost:8501). Anda akan melihat antarmuka pencarian YouTube dengan pilihan jenis pencarian dan bidang teks untuk memasukkan query pencarian. Setelah pencarian dilakukan, hasilnya akan ditampilkan dalam bentuk tabel dan tombol "Unduh Hasil CSV" akan muncul di bawah tabel. Anda dapat mengunduh hasil dalam format CSV dengan mengklik tombol tersebut.

> buakan streamlit dari youtube-search-python 1.6.6 dan simpan di data menjadi csv

Untuk membuat aplikasi Streamlit dengan pustaka `youtube-search-python` versi 1.6.6 dan menyimpan hasil pencarian dalam format CSV, ikuti langkah-langkah berikut:

1.  Instalasi Streamlit dan pustaka `youtube-search-python` versi 1.6.6

Jika Anda belum menginstal Streamlit, Anda bisa menginstalnya menggunakan pip:

bash

```bash
pip install streamlit
```

Instalasi `youtube-search-python` versi 1.6.6:

bash

```bash
pip install youtube-search-python==1.6.6
```

2.  Buat file `streamlit_app.py`

Buat file dengan nama `streamlit_app.py` dan masukkan kode berikut:

python

```python
import streamlit as st
import pandas as pd
from youtube_search import YoutubeSearch

def search_videos(query, max_results=10):
    video_search = YoutubeSearch(query, max_results=max_results)
    results = video_search.to_dict()
    return results

def search_to_dataframe(results):
    data = []

    for item in results:
        data.append({
            'title': item['title'],
            'link': f"https://youtube.com{item['url_suffix']}"
        })

    return pd.DataFrame(data)

st.title("Pencarian YouTube")
query = st.text_input("Masukkan Query Pencarian")

if st.button("Cari"):
    results = search_videos(query)

    if results:
        df = search_to_dataframe(results)
        st.write(df)

        st.download_button(
            label="Unduh Hasil CSV",
            data=df.to_csv(index=False).encode('utf-8'),
            file_name="hasil_pencarian_youtube.csv",
            mime="text/csv"
        )
    else:
        st.warning("Tidak ada hasil yang ditemukan.")
```

3.  Jalankan aplikasi Streamlit

Jalankan aplikasi Streamlit dengan menggunakan perintah berikut:

bash

```bash
streamlit run streamlit_app.py
```

Buka browser Anda dan akses aplikasi di [http://localhost:8501](http://localhost:8501). Anda akan melihat antarmuka pencarian YouTube dengan bidang teks untuk memasukkan query pencarian. Setelah pencarian dilakukan, hasilnya akan ditampilkan dalam bentuk tabel dan tombol "Unduh Hasil CSV" akan muncul di bawah tabel. Anda dapat mengunduh hasil dalam format CSV dengan mengklik tombol tersebut.

Perlu dicatat bahwa pustaka `youtube-search-python` versi 1.6.6 hanya mendukung pencarian video, tidak mencakup pencarian channel atau playlist.

> from youtubesearchpython import VideosSearch, ChannelsSearch, PlaylistsSearch import sys def search\_videos(query, max\_results=10): video\_search = VideosSearch(query, limit=max\_results) results = video\_search.result() return results\['result'\] def search\_channels(query, max\_results=10): channel\_search = ChannelsSearch(query, limit=max\_results) results = channel\_search.result() return results\['result'\] def search\_playlists(query, max\_results=10): playlist\_search = PlaylistsSearch(query, limit=max\_results) results = playlist\_search.result() return results\['result'\] def main(): if len(sys.argv) != 3: print( "Usage: python youtube\_search.py \[videos|channels|playlists\] query") sys.exit(1) search\_type = sys.argv\[1\] query = sys.argv\[2\] if search\_type == 'videos': results = search\_videos(query) elif search\_type == 'channels': results = search\_channels(query) elif search\_type == 'playlists': results = search\_playlists(query) else: print("Invalid search type. Choose from \[videos|channels|playlists\].") sys.exit(1) for item in results: print(item\['title'\], '-', item\['link'\]) if \_\_name\_\_ == '\_\_main\_\_': main() Tambahakan fiture simpan di file data hasil dari search nya dengan format csv,json,txt sesuain nama query yang di masukan

Untuk menambahkan fitur menyimpan hasil pencarian dalam format CSV, JSON, dan TXT, ikuti langkah-langkah berikut:

1.  Impor modul yang diperlukan

python

```python
import csv
import json
```

2.  Buat fungsi untuk menyimpan hasil dalam format yang berbeda

python

```python
def save_to_csv(results, filename):
    with open(filename, 'w', newline='', encoding='utf-8') as csvfile:
        fieldnames = ['title', 'link']
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
        writer.writeheader()
        for item in results:
            writer.writerow({'title': item['title'], 'link': item['link']})

def save_to_json(results, filename):
    with open(filename, 'w', encoding='utf-8') as jsonfile:
        json.dump(results, jsonfile, ensure_ascii=False, indent=4)

def save_to_txt(results, filename):
    with open(filename, 'w', encoding='utf-8') as txtfile:
        for item in results:
            txtfile.write(f"{item['title']} - {item['link']}\n")
```

3.  Tambahkan pemanggilan fungsi untuk menyimpan hasil pencarian dalam format yang berbeda pada akhir fungsi `main()`

python

```python
if __name__ == '__main__':
    main()

    file_base_name = query.replace(" ", "_")
    save_to_csv(results, f"{file_base_name}.csv")
    save_to_json(results, f"{file_base_name}.json")
    save_to_txt(results, f"{file_base_name}.txt")
```

Kode lengkap akan terlihat seperti ini:

python

```python
from youtubesearchpython import VideosSearch, ChannelsSearch, PlaylistsSearch
import sys
import csv
import json

def search_videos(query, max_results=10):
    video_search = VideosSearch(query, limit=max_results)
    results = video_search.result()
    return results['result']

def search_channels(query, max_results=10):
    channel_search = ChannelsSearch(query, limit=max_results)
    results = channel
```

> simpan hasil nya di folder data dan tulis kode secara lengkap

10 / 10

Untuk menyimpan hasil pencarian di folder bernama `data` dan menyajikan kode secara lengkap, ikuti langkah-langkah berikut:

1.  Buat folder `data` di direktori yang sama dengan skrip Python.
    
2.  Tambahkan os.path untuk menggabungkan jalur folder 'data' dengan nama file.
    

Berikut adalah kode lengkap dengan perubahan yang diperlukan:

python

```python
import sys
import os
import csv
import json
from youtubesearchpython import VideosSearch, ChannelsSearch, PlaylistsSearch

def search_videos(query, max_results=10):
    video_search = VideosSearch(query, limit=max_results)
    results = video_search.result()
    return results['result']

def search_channels(query, max_results=10):
    channel_search = ChannelsSearch(query, limit=max_results)
    results = channel_search.result()
    return results['result']

def search_playlists(query, max_results=10):
    playlist_search = PlaylistsSearch(query, limit=max_results)
    results = playlist_search.result()
    return results['result']

def save_to_csv(results, filename):
    with open(filename, 'w', newline='', encoding='utf-8') as csvfile:
        fieldnames = ['title', 'link']
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
        writer.writeheader()
        for item in results:
            writer.writerow({'title': item['title'], 'link': item['link']})

def save_to_json(results, filename):
    with open(filename, 'w', encoding='utf-8') as jsonfile:
        json.dump(results, jsonfile, ensure_ascii=False, indent=4)

def save_to_txt(results, filename):
    with open(filename, 'w', encoding='utf-8') as txtfile:
        for item in results:
            txtfile.write(f"{item['title']} - {item['link']}\n")

def main():
    if len(sys.argv) != 3:
        print("Usage: python youtube_search.py [videos|channels|playlists] query")
        sys.exit(1)

    search_type = sys.argv[1]
    query = sys.argv[2]

    if search_type == 'videos':
        results = search_videos(query)
    elif search_type == 'channels':
        results = search_channels(query)
    elif search_type == 'playlists':
        results = search_playlists(query)
    else:
        print("Invalid search type. Choose from [videos|channels|playlists].")
        sys.exit(1)

    for item in results:
        print(item['title'], '-', item['link'])

    file_base_name = query.replace(" ", "_")
    data_folder = 'data'

    save_to_csv(results, os.path.join(data_folder, f"{file_base_name}.csv"))
    save_to_json(results, os.path.join(data_folder, f"{file_base_name}.json"))
    save_to_txt(results, os.path.join(data_folder, f"{file_base_name}.txt"))

if __name__ == '__main__':
    main()
```

Dengan kode ini, hasil pencarian akan disimpan dalam format CSV, JSON, dan TXT di folder `data`. Pastikan folder `data` ada di direktori yang sama dengan skrip Python. Jika folder tidak ada, Anda perlu membuatnya terlebih dahulu.

> Buatakan script donwload subtitle dari pip install download-youtube-subtitle

Untuk menggunakan pustaka `download-youtube-subtitle` untuk mengunduh subtitle video YouTube, ikuti langkah-langkah berikut:

1.  Instalasi pustaka

Jika Anda belum menginstal pustaka `download-youtube-subtitle`, Anda bisa menginstalnya menggunakan pip:

bash

```bash
pip install download-youtube-subtitle
```

2.  Buat file `download_subtitle.py`

Buat file dengan nama `download_subtitle.py` dan masukkan kode berikut:

python

```python
import sys
from download_youtube_subtitle import downloader

def download_subtitle(video_id, output_format='srt', output_name=None, language='en'):
    if output_name is None:
        output_name = video_id
    
    try:
        subtitle = downloader.get_auto_subs(video_id, output_format=output_format, lang=language)
        if subtitle is not None:
            with open(f"{output_name}.{output_format}", 'w', encoding='utf-8') as f:
                f.write(subtitle)
            print(f"Subtitle berhasil diunduh: {output_name}.{output_format}")
        else:
            print("Tidak ada subtitle yang tersedia.")
    except Exception as e:
        print(f"Gagal mengunduh subtitle: {e}")

def main():
    if len(sys.argv) < 2:
        print("Usage: python download_subtitle.py video_id [output_format] [output_name] [language]")
        sys.exit(1)

    video_id = sys.argv[1]
    output_format = 'srt' if len(sys.argv) < 3 else sys.argv[2]
    output_name = None if len(sys.argv) < 4 else sys.argv[3]
    language = 'en' if len(sys.argv) < 5 else sys.argv[4]

    download_subtitle(video_id, output_format=output_format, output_name=output_name, language=language)

if __name__ == '__main__':
    main()
```

3.  Menggunakan skrip

Anda dapat menggunakan skrip `download_subtitle.py` untuk mengunduh subtitle dengan memberikan argumen yang diperlukan. Berikut adalah beberapa contoh penggunaan:

bash

```bash
# Contoh 1: Mengunduh subtitle dalam format srt (default) dan bahasa Inggris (default)
python download_subtitle.py YOUR_VIDEO_ID

# Contoh 2: Mengunduh subtitle dalam format vtt
python download_subtitle.py YOUR_VIDEO_ID vtt

# Contoh 3: Mengunduh subtitle dalam format srt dan menyimpannya dengan nama yang berbeda
python download_subtitle.py YOUR_VIDEO_ID srt custom_filename

# Contoh 4: Mengunduh subtitle dalam format srt dan bahasa Indonesia
python download_subtitle.py YOUR_VIDEO_ID srt None id
```

Gantilah `YOUR_VIDEO_ID` dengan ID video YouTube yang ingin Anda unduh subtitlenya. ID video adalah bagian dari URL setelah `https://www.youtube.com/watch?v=`, misalnya, untuk URL `https://www.youtube.com/watch?v=dQw4w9WgXcQ`, ID videonya adalah `dQw4w9WgXcQ`.

Pastikan untuk menggantikan contoh ID video dengan ID video yang valid sebelum menjalankan skrip.

> Tambahakan fiture simpan hasil nya di folder data\_subtitle

Untuk menyimpan hasil unduhan subtitle ke dalam folder `data_subtitle`, ikuti langkah-langkah berikut:

1.  Buat folder `data_subtitle` di direktori yang sama dengan skrip Python.
    
2.  Tambahkan os.path untuk menggabungkan jalur folder 'data\_subtitle' dengan nama file.
    

Berikut adalah kode lengkap dengan perubahan yang diperlukan:

python

```python
import sys
import os
from download_youtube_subtitle import downloader

def download_subtitle(video_id, output_format='srt', output_name=None, language='en'):
    if output_name is None:
        output_name = video_id
    
    try:
        subtitle = downloader.get_auto_subs(video_id, output_format=output_format, lang=language)
        if subtitle is not None:
            output_folder = 'data_subtitle'
            output_path = os.path.join(output_folder, f"{output_name}.{output_format}")
            with open(output_path, 'w', encoding='utf-8') as f:
                f.write(subtitle)
            print(f"Subtitle berhasil diunduh: {output_path}")
        else:
            print("Tidak ada subtitle yang tersedia.")
    except Exception as e:
        print(f"Gagal mengunduh subtitle: {e}")

def main():
    if len(sys.argv) < 2:
        print("Usage: python download_subtitle.py video_id [output_format] [output_name] [language]")
        sys.exit(1)

    video_id = sys.argv[1]
    output_format = 'srt' if len(sys.argv) < 3 else sys.argv[2]
    output_name = None if len(sys.argv) < 4 else sys.argv[3]
    language = 'en' if len(sys.argv) < 5 else sys.argv[4]

    download_subtitle(video_id, output_format=output_format, output_name=output_name, language=language)

if __name__ == '__main__':
    main()
```

Dengan kode ini, subtitle yang diunduh akan disimpan dalam folder `data_subtitle`. Pastikan folder `data_subtitle` ada di direktori yang sama dengan skrip Python. Jika folder tidak ada, Anda perlu membuatnya terlebih dahulu.

> tambahakan fiture donwload nya dengan url seperti ini python donwload\_subtitle.py pip install download-youtube-subtitle

Untuk mendukung input berupa URL dalam skrip

> mail@lucacadalora.com tambahakan fiture donwload nya dengan url seperti ini python donwload\_subtitle.py https://www.youtube.com/watch?v=7-SF3CtM8Hs

Untuk mendukung input berupa URL dalam skrip `download_subtitle.py`, Anda perlu mengekstrak video ID dari URL yang diberikan. Berikut adalah kode yang diperbarui:

python

```python
import sys
import os
import re
from download_youtube_subtitle import downloader

def extract_video_id(url):
    video_id_pattern = r'(?:v=|\/)([0-9A-Za-z_-]{10}[048AEIMQUYcgkosw])'
    match = re.search(video_id_pattern, url)
    return match.group(1) if match else None

def download_subtitle(video_id, output_format='srt', output_name=None, language='en'):
    if output_name is None:
        output_name = video_id
    
    try:
        subtitle = downloader.get_auto_subs(video_id, output_format=output_format, lang=language)
        if subtitle is not None:
            output_folder = 'data_subtitle'
            output_path = os.path.join(output_folder, f"{output_name}.{output_format}")
            with open(output_path, 'w', encoding='utf-8') as f:
                f.write(subtitle)
            print(f"Subtitle berhasil diunduh: {output_path}")
        else:
            print("Tidak ada subtitle yang tersedia.")
    except Exception as e:
        print(f"Gagal mengunduh subtitle: {e}")

def main():
    if len(sys.argv) < 2:
        print("Usage: python download_subtitle.py video_url [output_format] [output_name] [language]")
        sys.exit(1)

    url = sys.argv[1]
    video_id = extract_video_id(url)
    if video_id is None:
        print("URL tidak valid.")
        sys.exit(1)

    output_format = 'srt' if len(sys.argv) < 3 else sys.argv[2]
    output_name = None if len(sys.argv) < 4 else sys.argv[3]
    language = 'en' if len(sys.argv) < 5 else sys.argv[4]

    download_subtitle(video_id, output_format=output_format, output_name=output_name, language=language)

if __name__ == '__main__':
    main()
```

Sekarang Anda dapat menjalankan skrip dengan URL sebagai argumen:

bash

```bash
python download_subtitle.py https://www.youtube.com/watch?v=7-SF3CtM8Hs
```

Fungsi `extract_video_id` akan mengekstrak ID video dari URL yang diberikan, dan kemudian skrip akan melanjutkan untuk mengunduh subtitle seperti sebelumnya.