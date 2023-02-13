> buatkan script atomation downloader dengan pilihan mengunakan curl atau wget secara interaktif dan intuitif
> 
> Berikut adalah contoh script automation downloader menggunakan pilihan interaktif antara `curl` atau `wget`:
> 
> bash
> 
> ```bash
> #!/bin/bash
> 
> # Function to download using curl
> function download_with_curl {
>   echo "Downloading $1 using curl..."
>   curl -O $1
> }
> 
> # Function to download using wget
> function download_with_wget {
>   echo "Downloading $1 using wget..."
>   wget $1
> }
> 
> # Prompt user to select download tool
> echo "Select download tool:"
> echo "1. curl"
> echo "2. wget"
> read -p "Enter your choice [1/2]: " choice
> 
> # Prompt user to enter URL
> read -p "Enter URL to download: " URL
> 
> # Call appropriate download function based on user choice
> if [ $choice -eq 1 ]; then
>   download_with_curl $URL
> elif [ $choice -eq 2 ]; then
>   download_with_wget $URL
> else
>   echo "Invalid choice. Exiting..."
> fi
> ```
> 
> Script ini akan menampilkan prompt untuk meminta pengguna memilih alat download yang ingin digunakan (`curl` atau `wget`) dan meminta URL file yang ingin didownload. Kemudian, fungsi yang sesuai akan dipanggil untuk menjalankan download.
> 
> buatakan screat monitoring processor dengan bash sheell htop secara interaktif
> 
> Berikut adalah contoh script monitoring processor menggunakan alat `htop` secara interaktif:
> 
> bash
> 
> ```bash
> #!/bin/bash
> 
> # Function to start htop
> function start_htop {
>   echo "Starting htop..."
>   htop
> }
> 
> # Prompt user to start htop
> read -p "Do you want to start htop to monitor your processor? [y/n]: " choice
> 
> # Start htop if user choice is y
> if [ "$choice" == "y" ]; then
>   start_htop
> else
>   echo "Exiting..."
> fi
> ```
> 
> Script ini akan menampilkan prompt untuk meminta pengguna memulai `htop` untuk memantau processor. Jika pengguna memilih `y`, maka `htop` akan dijalankan. Jika tidak, maka script akan berakhir.
> 
> buatakan script bash shall untuk download google drive
> 
> Untuk mengunduh file dari Google Drive menggunakan script bash, Anda dapat menggunakan tool bernama `gdrive`. Berikut adalah contoh script untuk mengunduh file dari Google Drive:
> 
> bash
> 
> ```bash
> #!/bin/bash
> 
> # Function to download file from Google Drive
> function download_from_gdrive {
>   # Check if gdrive is installed
>   if ! [ -x "$(command -v gdrive)" ]; then
>     echo "gdrive not found. Installing..."
>     sudo apt-get update && sudo apt-get install gdrive
>   fi
> 
>   # Get file ID from URL
>   file_id=$(echo $1 | awk -F/ '{print $NF}')
> 
>   # Download file using gdrive
>   gdrive download $file_id
> }
> 
> # Prompt user to enter Google Drive file URL
> read -p "Enter Google Drive file URL: " URL
> 
> # Call download function
> download_from_gdrive $URL
> ```
> 
> Script ini akan meminta pengguna untuk memasukkan URL file Google Drive yang ingin didownload. Kemudian, fungsi `download_from_gdrive` akan dipanggil untuk mengunduh file menggunakan `gdrive`. Jika `gdrive` belum terpasang, script akan menginstalnya terlebih dahulu.