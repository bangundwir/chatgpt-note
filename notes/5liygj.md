Model: Default (GPT-3.5)

> Buatakan script python donwlaod subtitle youtube berdasarkan url tanpa api

Untuk membuat script Python untuk mengunduh subtitle dari video YouTube berdasarkan URL tanpa menggunakan API, Anda bisa menggunakan pustaka `youtube_transcript_api` dan `pytube`. Berikut adalah langkah-langkah untuk melakukannya:

1.  Pasang pustaka `youtube_transcript_api` dan `pytube` menggunakan pip:

bash

```bash
pip install youtube_transcript_api pytube
```

2.  Buat file Python dengan kode berikut:

python

```python
import re
import sys
from pytube import YouTube
from youtube_transcript_api import YouTubeTranscriptApi

def download_subtitle(url, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    with open(subtitle_filename, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {subtitle_filename}")

def format_time(seconds):
    hours, remainder = divmod(seconds, 3600)
    minutes, seconds = divmod(remainder, 60)
    return f"{int(hours):02d}:{int(minutes):02d}:{int(seconds):02d},{int(seconds % 1 * 1000):03d}"

if __name__ == "__main__":
    url = input("Masukkan URL video YouTube: ")
    download_subtitle(url)
```

3.  Jalankan script ini dan masukkan URL video YouTube saat diminta. Script akan mengunduh subtitle dalam bahasa Inggris (default) dan menyimpannya dalam format .srt dengan nama file yang sama dengan judul video.

Catatan: Meskipun script ini tidak menggunakan YouTube API resmi, pustaka `youtube_transcript_api` dan `pytube` masih mengakses data dari YouTube, dan perubahan dalam struktur situs web YouTube atau kebijakan akses data dapat mempengaruhi fungsionalitas script ini.

> simpan file nya di folder data\_subtitle

Untuk menyimpan file subtitle di folder `data_subtitle`, Anda perlu memodifikasi kode sedikit. Buat dulu folder `data_subtitle` di direktori yang sama dengan script Python Anda. Kemudian, ikuti langkah-langkah berikut:

1.  Tambahkan import `os` di bagian atas file Python:

python

```python
import os
```

2.  Ubah fungsi `download_subtitle` untuk menyimpan file di folder `data_subtitle`:

python

```python
def download_subtitle(url, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_path = os.path.join("data_subtitle", subtitle_filename)
    with open(output_path, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {output_path}")
```

Dengan perubahan ini, script sekarang akan menyimpan file subtitle di folder `data_subtitle` yang ada dalam direktori yang sama dengan script Python Anda.

> Tulis semua script nya

Berikut adalah script Python lengkap untuk mengunduh subtitle dari video YouTube berdasarkan URL dan menyimpannya dalam folder `data_subtitle`:

python

```python
import os
import re
import sys
from pytube import YouTube
from youtube_transcript_api import YouTubeTranscriptApi

def download_subtitle(url, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_path = os.path.join("data_subtitle", subtitle_filename)
    with open(output_path, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {output_path}")

def format_time(seconds):
    hours, remainder = divmod(seconds, 3600)
    minutes, seconds = divmod(remainder, 60)
    return f"{int(hours):02d}:{int(minutes):02d}:{int(seconds):02d},{int(seconds % 1 * 1000):03d}"

if __name__ == "__main__":
    url = input("Masukkan URL video YouTube: ")
    download_subtitle(url)
```

Untuk menjalankan script ini, pastikan Anda telah menginstal pustaka `youtube_transcript_api` dan `pytube` menggunakan pip:

bash

```bash
pip install youtube_transcript_api pytube
```

Kemudian, jalankan script Python, dan masukkan URL video YouTube saat diminta. Script akan mengunduh subtitle dalam bahasa Inggris (default) dan menyimpannya dalam format .srt di folder `data_subtitle`.

> Tambahkan fiture agar bisa donwload subtitle dari semua vidio di playlist youtube

3 / 3

Untuk mengunduh subtitle dari semua video dalam playlist YouTube, Anda perlu mengubah beberapa bagian dari script dan menggunakan `Playlist` dari pustaka `pytube`. Berikut adalah langkah-langkah yang perlu Anda lakukan:

1.  Tambahkan import `Playlist` dari `pytube`:

python

```python
from pytube import Playlist
```

2.  Buat fungsi baru yang akan mengunduh subtitle dari semua video dalam playlist:

python

```python
def download_subtitles_from_playlist(playlist_url, language='en'):
    # Dapatkan daftar video dalam playlist
    playlist = Playlist(playlist_url)

    # Unduh subtitle untuk setiap video dalam playlist
    for video_url in playlist.video_urls:
        download_subtitle(video_url, language)
```

3.  Ubah bagian `__main__` dari script untuk meminta URL playlist atau video:

python

```python
if __name__ == "__main__":
    url = input("Masukkan URL video atau playlist YouTube: ")

    if "watch?" in url and "list=" in url:
        print("Mengunduh subtitle dari playlist...")
        download_subtitles_from_playlist(url)
    else:
        print("Mengunduh subtitle dari video...")
        download_subtitle(url)
```

Sekarang, Anda dapat menjalankan script dan memasukkan URL video atau playlist YouTube. Jika URL playlist dimasukkan, script akan mengunduh subtitle dari semua video dalam playlist dalam bahasa Inggris (default) dan menyimpannya dalam format .srt di folder `data_subtitle`.

Berikut adalah script Python lengkap:

python

```python
import os
import re
import sys
from pytube import YouTube, Playlist
from youtube_transcript_api import YouTubeTranscriptApi

def download_subtitle(url, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_path = os.path.join("data_subtitle", subtitle_filename)
    with open(output_path, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {output_path}")

def format_time(seconds):
    hours, remainder = divmod(seconds, 3600)
    minutes, seconds = divmod(remainder, 60)
    return f"{int(hours):02d}:{int(minutes):02d}:{int(seconds):02d},{int(seconds % 1 * 1000):03d}"

def download_subtitles_from_playlist(playlist_url, language='en'):
    # Dapatkan daftar video dalam playlist
    playlist = Playlist(playlist_url)

    # Unduh
```

> simpan hasilnya di dalam dalam data\_subtitle dan buat folder sesuai nama playlistnya dan simpan di situ semua hasil file dalam playlist nya dan tulis lengkap kodenya

Untuk menyimpan hasil subtitle di dalam folder `data_subtitle` sesuai dengan nama playlist, Anda perlu memodifikasi kode sebelumnya. Berikut adalah langkah-langkah yang perlu Anda lakukan:

1.  Modifikasi fungsi `download_subtitle` untuk menerima argumen tambahan `output_folder`, yang akan menentukan folder tempat menyimpan subtitle:

python

```python
def download_subtitle(url, output_folder, language='en'):
    # (Kode sebelumnya)

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_path = os.path.join(output_folder, subtitle_filename)
    with open(output_path, 'w', encoding='utf-8') as f:
        # (Kode sebelumnya)

    print(f"Subtitle berhasil diunduh: {output_path}")
```

2.  Modifikasi fungsi `download_subtitles_from_playlist` untuk membuat folder dengan nama playlist di dalam folder `data_subtitle`:

python

```python
def download_subtitles_from_playlist(playlist_url
```

> Tulis kodenya secara lengkap

Berikut adalah kode Python lengkap yang diubah sesuai dengan permintaan Anda:

python

```python
import os
import re
import sys
from pytube import YouTube, Playlist
from youtube_transcript_api import YouTubeTranscriptApi

def download_subtitle(url, output_folder, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_path = os.path.join(output_folder, subtitle_filename)
    with open(output_path, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {output_path}")

def format_time(seconds):
    hours, remainder = divmod(seconds, 3600)
    minutes, seconds = divmod(remainder, 60)
    return f"{int(hours):02d}:{int(minutes):02d}:{int(seconds):02d},{int(seconds % 1 * 1000):03d}"

def download_subtitles_from_playlist(playlist_url, language='en'):
    # Dapatkan daftar video dalam playlist
    playlist = Playlist(playlist_url)
    playlist_name = playlist.title

    # Buat folder dengan nama playlist di dalam folder data_subtitle
    output_folder = os.path.join("data_subtitle", playlist_name)
    os.makedirs(output_folder, exist_ok=True)

    # Unduh subtitle untuk setiap video dalam playlist
    for video_url in playlist.video_urls:
        download_subtitle(video_url, output_folder, language)

if __name__ == "__main__":
    url = input("Masukkan URL video atau playlist YouTube: ")

    if "watch?" in url and "list=" in url:
        print("Mengunduh subtitle dari playlist...")
        download_subtitles_from_playlist(url)
    else:
        print("Mengunduh subtitle dari video...")
        output_folder = os.path.join("data_subtitle", "single_videos")
        os.makedirs(output_folder, exist_ok=True)
        download_subtitle(url, output_folder)
```

Dengan kode ini, ketika Anda memasukkan URL playlist, script akan membuat folder dengan nama playlist di dalam folder `data_subtitle` dan menyimpan semua subtitle video dalam playlist tersebut di folder tersebut. Jika Anda memasukkan URL video tunggal, subtitle akan disimpan dalam folder `data_subtitle/single_videos`. Pastikan Anda telah menginstal pustaka `youtube_transcript_api` dan `pytube` menggunakan pip:

bash

```bash
pip install youtube_transcript_api pytube
```

> Tolong perbaiki script di atas karena mengalami error Traceback (most recent call last): File "/home/hades/dev/website\_serderhana/gitawiryawan\_podcast/2sub.py", line 74, in <module> download\_subtitle(url, output\_folder) File "/home/hades/dev/website\_serderhana/gitawiryawan\_podcast/2sub.py", line 13, in download\_subtitle video\_id = video\_id.group() ^^^^^^^^^^^^^^ AttributeError: 'NoneType' object has no attribute 'group'

Maaf atas kesalahan tersebut. Masalah ini terjadi karena URL yang Anda masukkan mungkin tidak cocok dengan ekspresi reguler yang digunakan dalam kode. Untuk mengatasi masalah ini, kita akan menambahkan pengecekan tambahan dan mencetak pesan kesalahan jika URL tidak valid. Berikut adalah kode yang diperbaiki:

python

```python
import os
import re
import sys
from pytube import YouTube, Playlist
from youtube_transcript_api import YouTubeTranscriptApi

def download_subtitle(url, output_folder, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    if not video_id:
        print("URL video tidak valid.")
        return
    video_id = video_id.group()

    # (Kode sebelumnya)

if __name__ == "__main__":
    url = input("Masukkan URL video atau playlist YouTube: ")

    if "watch?" in url and "list=" in url:
        print("Mengunduh subtitle dari playlist...")
        download_subtitles_from_playlist(url)
    else:
        print("Mengunduh subtitle dari video...")
        output_folder = os.path.join("data_subtitle", "single_videos")
        os.makedirs(output_folder, exist_ok=True)
        download_subtitle(url, output_folder)
```

Sekarang, jika URL yang dimasukkan tidak valid atau tidak cocok dengan pola yang diharapkan, script akan mencetak pesan kesalahan "URL video tidak valid." dan tidak akan mencoba mengunduh subtitle.

> tulis lengkap kode dengan language='id'

Berikut adalah kode Python lengkap dengan bahasa subtitle diatur ke 'id' (Indonesia):

python

```python
import os
import re
import sys
from pytube import YouTube, Playlist
from youtube_transcript_api import YouTubeTranscriptApi

def download_subtitle(url, output_folder, language='id'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    if not video_id:
        print("URL video tidak valid.")
        return
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_path = os.path.join(output_folder, subtitle_filename)
    with open(output_path, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {output_path}")

def format_time(seconds):
    hours, remainder = divmod(seconds, 3600)
    minutes, seconds = divmod(remainder, 60)
    return f"{int(hours):02d}:{int(minutes):02d}:{int(seconds):02d},{int(seconds % 1 * 1000):03d}"

def download_subtitles_from_playlist(playlist_url, language='id'):
    # Dapatkan daftar video dalam playlist
    playlist = Playlist(playlist_url)
    playlist_name = playlist.title

    # Buat folder dengan nama playlist di dalam folder data_subtitle
    output_folder = os.path.join("data_subtitle", playlist_name)
    os.makedirs(output_folder, exist_ok=True)

    # Unduh subtitle untuk setiap video dalam playlist
    for video_url in playlist.video_urls:
        download_subtitle(video_url, output_folder, language)

if __name__ == "__main__":
    url = input("Masukkan URL video atau playlist YouTube: ")

    if "watch?" in url and "list=" in url:
        print("Mengunduh subtitle dari playlist...")
        download_subtitles_from_playlist(url)
    else:
        print("Mengunduh subtitle dari video...")
        output_folder = os.path.join("data_subtitle", "single_videos")
        os.makedirs(output_folder, exist_ok=True)
        download_subtitle(url, output_folder)
```

Dalam kode ini, kami mengganti argumen default `language` dalam fungsi `download_subtitle` dan `download_subtitles_from_playlist` menjadi `'id'` (Indonesia). Anda dapat menjalankan kode ini untuk mengunduh subtitle dalam bahasa Indonesia. Pastikan Anda telah menginstal pustaka `youtube_transcript_api` dan `pytube` menggunakan pip:

bash

```bash
pip install youtube_transcript_api pytube
```

> buatakan agar hasil donwload subtitle nya tadi di jadikan html dan bisa klik waktu ke episdoe yang waktiunya di klik

Untuk mengkonversi file subtitle yang diunduh menjadi file HTML dan membuat tautan yang dapat diklik ke waktu yang spesifik dalam video, Anda perlu mengubah kode sebelumnya. Berikut adalah langkah-langkah yang perlu Anda lakukan:

1.  Tambahkan fungsi `convert_srt_to_html` untuk mengkonversi file subtitle .srt menjadi file HTML:

python

```python
def convert_srt_to_html(srt_path, video_url, output_folder):
    with open(srt_path, 'r', encoding='utf-8') as f:
        srt_content = f.read()

    html_content = "<html><head><title>Subtitle HTML</title></head><body>"

    for line in srt_content.split('\n'):
        if re.match(r'\d+', line):
            continue
        elif re.match(r'\d{2}:\d{2}:\d{2},\d{3} --> \d{2}:\d{2}:\d{2},\d{3}', line):
            start_time = line.split(' ')[0]
            start_seconds = time_str_to_seconds(start_time)
            html_content += f'<a href="{video_url}?t={start_seconds}">{start_time}</a> - '
        else:
            html_content += line + '<br>'

    html_content += "</body></html>"

    output_filename = os.path.basename(srt_path).replace(".srt", ".html")
    output_path = os.path.join(output_folder, output_filename)
    with open(output_path, 'w', encoding='utf-8') as f:
        f.write(html_content)

    return output_path
```

2.  Tambahkan fungsi `time_str_to_seconds` untuk mengubah string waktu menjadi detik:

python

```python
def time_str_to_seconds(time_str):
    hours, minutes, seconds_milliseconds = time_str.split(':')
    seconds, milliseconds = seconds_milliseconds.split(',')
    return int(hours) * 3600 + int(minutes) * 60 + int(seconds)
```

3.  Sesuaikan fungsi `download_subtitle` untuk meng

> simpan file nya di folder File\_html dan emded vidio nya juga di setiap episode

The previous model used in this conversation is unavailable. We've switched you to the latest default model

2 / 2

Untuk menyimpan file HTML yang dihasilkan di dalam folder `File_html` dan menambahkan video embed di setiap episode, Anda perlu memodifikasi kode sebelumnya. Berikut adalah langkah-langkah yang perlu Anda lakukan:

1.  Modifikasi fungsi `download_subtitle` untuk menerima argumen tambahan `output_html_folder`, yang akan menentukan folder tempat menyimpan file HTML:

python

```python
def download_subtitle(url, output_subtitle_folder, output_html_folder, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    if not video_id:
        print("URL video tidak valid.")
        return
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_subtitle_path = os.path.join(output_subtitle_folder, subtitle_filename)
    with open(output_subtitle_path, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {output_subtitle_path}")

    # Konversi subtitle ke HTML
    output_html_path = convert_srt_to_html(output_subtitle_path, url, output_html_folder)
    print(f"File HTML berhasil dibuat: {output_html_path}")

    # Tambahkan video embed di setiap episode
    with open(output_html_path, 'r+', encoding='utf-8') as f:
        html_content = f.read()
        video_embed = f'<iframe width="560" height="315" src="https://www.youtube.com/embed/{video_id}" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>'
        html_content = html_content.replace("</body>", f"{video_embed}</body>")
        f.seek(0)
        f.write(html_content)
        f.truncate()
```

2.  Tambahkan argumen `output_html_folder` ke dalam pemanggilan fungsi `download_subtitle` pada fungsi `download_subtitles_from_playlist`:

python

```python
def download_subtitles_from_playlist(playlist_url, output_html_folder, language='en'):
    # Dapatkan daftar video dalam playlist
    playlist = Playlist(playlist_url)
    playlist_name = playlist.title

    # Buat folder dengan nama playlist di dalam folder data_subtitle
    output_subtitle_folder = os.path.join("data_subtitle", playlist_name)
    os.makedirs(output_subtitle_folder, exist_ok=True)

    # Buat folder dengan nama playlist di dalam folder File_html
    output_html_folder = os.path.join("File_html", playlist_name)
    os.makedirs(output_html_folder, exist_ok=True)

    # Unduh subtitle untuk setiap video dalam playlist
```

> tulis semua scriptnya

Berikut adalah kode Python lengkap untuk mengunduh subtitle dari video YouTube, mengubahnya menjadi file HTML, dan menambahkan video embed di setiap episode jika Anda ingin:

python

```python
import os
import re
import sys
from pytube import YouTube, Playlist
from youtube_transcript_api import YouTubeTranscriptApi

def download_subtitle(url, output_subtitle_folder, output_html_folder, language='en'):
    # Dapatkan video_id dari URL
    video_id = re.search(r'(?<=v=)[^&#]+', url)
    if not video_id:
        video_id = re.search(r'(?<=be/)[^&#]+', url)
    if not video_id:
        print("URL video tidak valid.")
        return
    video_id = video_id.group()

    # Dapatkan transcript dengan video_id
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id, languages=[language])
    except Exception as e:
        print("Tidak dapat mengunduh subtitle, alasan:", e)
        sys.exit(1)

    # Dapatkan judul video
    yt = YouTube(url)
    video_title = yt.title

    # Buat file subtitle (format .srt)
    subtitle_filename = f"{video_title}.srt"
    output_subtitle_path = os.path.join(output_subtitle_folder, subtitle_filename)
    with open(output_subtitle_path, 'w', encoding='utf-8') as f:
        for i, entry in enumerate(transcript):
            start_time = entry['start']
            duration = entry['duration']
            end_time = start_time + duration
            text = entry['text']

            f.write(f"{i + 1}\n")
            f.write(f"{format_time(start_time)} --> {format_time(end_time)}\n")
            f.write(f"{text}\n\n")

    print(f"Subtitle berhasil diunduh: {output_subtitle_path}")

    # Konversi subtitle ke HTML
    output_html_path = convert_srt_to_html(output_subtitle_path, url, output_html_folder)
    print(f"File HTML berhasil dibuat: {output_html_path}")

    # Tambahkan video embed di setiap episode
    with open(output_html_path, 'r+', encoding='utf-8') as f:
        html_content = f.read()
        video_embed = f'<iframe width="560" height="315" src="https://www.youtube.com/embed/{video_id}" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>'
        html_content = html_content.replace("</body>", f"{video_embed}</body>")
        f.seek(0)
        f.write(html_content)
        f.truncate()

def convert_srt_to_html(srt_path, video_url, output_folder):
    with open(srt_path, 'r', encoding='utf-8') as f:
        srt_content = f.read()

    html_content = "<html><head><title>Subtitle HTML</title></head><body>"

    for line in srt_content.split('\n'):
        if re.match(r'\d+', line):
            continue
        elif re.match(r'\d{2}:\d{2}:\d{2},\d{3} --> \d{2}:\d{2}:\d{2},\d{3}', line):
            start_time = line.split(' ')[0]
            start_seconds = time_str_to_seconds(start_time)
            html_content += f'<a href="{video_url}?t={start_seconds}">{start_time}</a> - '
        else:
            html_content += line + '<br>'

    html
```

> lanjutkan

lua

```lua
# Simpan file HTML
output_filename = os.path.basename(srt_path).replace(".srt", ".html")
output_path = os.path.join(output_folder, output_filename)
with open(output_path, 'w', encoding='utf-8') as f:
    f.write(html_content)

return output_path
```

def format\_time(seconds): hours, remainder = divmod(seconds, 3600) minutes, seconds = divmod(remainder, 60) return f"{int(hours):02d}:{int(minutes):02d}:{int(seconds):02d},{int(seconds % 1 \* 1000):03d}"

def time\_str\_to\_seconds(time\_str): hours, minutes, seconds\_milliseconds = time\_str.split(':') seconds, milliseconds = seconds\_milliseconds.split(',') return int(hours) \* 3600 + int(minutes) \* 60 + int(seconds)

def download\_subtitles\_from\_playlist(playlist\_url, language='en'): # Dapatkan daftar video dalam playlist playlist = Playlist(playlist\_url) playlist\_name = playlist.title

python

```python
# Buat folder dengan nama playlist di dalam folder data_subtitle
output_subtitle_folder = os.path.join("data_subtitle", playlist_name)
os.makedirs(output_subtitle_folder, exist_ok=True)

# Buat folder dengan nama playlist di dalam folder File_html
output_html_folder = os.path.join("File_html", playlist_name)
os.makedirs(output_html_folder, exist_ok=True)

# Unduh subtitle untuk setiap video dalam playlist
for video_url in playlist.video_urls:
    download_subtitle(video_url, output_subtitle_folder, output_html_folder, language)
```

if **name** == "**main**": url = input("Masukkan URL video atau playlist YouTube: ")

lua

```lua
if "watch?" in url and "list=" in url:
    print("Mengunduh subtitle dari playlist...")
    download_subtitles_from_playlist(url)
else:
    print("Mengunduh subtitle dari video...")
    output_subtitle_folder = os.path.join("data_subtitle", "single_videos")
    os.makedirs(output_subtitle_folder, exist_ok=True)
    output_html_folder = os.path.join("File_html", "single_videos")
    os.makedirs(output_html_folder, exist_ok=True)
    download_subtitle(url, output_subtitle_folder, output_html_folder)
```

javascript

```javascript
Pastikan Anda telah menginstal pustaka `youtube_transcript_api` dan `pytube` menggunakan pip:

```bash
pip install youtube_transcript_api pytube
```

Selain itu, pastikan Anda telah membuat folder `data_subtitle` dan `File_html` dalam direktori kerja Anda. Hasil konversi file subtitle akan disimpan di dalam folder `data_subtitle`, sedangkan file HTML akan disimpan di dalam folder `File_html`.