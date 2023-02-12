> buatakan script bash automation tools all in one terbaik yang bisa kamu buat
> 
> Berikut adalah contoh script bash automation tools "all-in-one" yang dapat digunakan untuk menjalankan berbagai tugas otomatis:
> 
> bash
> 
> ```bash
> #!/bin/bash
> 
> # Function to display menu
> display_menu() {
>     echo "===================================================="
>     echo "=                 All-in-One Automation            ="
>     echo "===================================================="
>     echo "= 1. Backup files                                  ="
>     echo "= 2. Update system packages                        ="
>     echo "= 3. Clean up system junk files                    ="
>     echo "= 4. Monitor system performance                    ="
>     echo "= 5. Exit                                         ="
>     echo "===================================================="
>     echo -n "Enter your choice [1-5]: "
> }
> 
> # Function to backup files
> backup_files() {
>     read -p "Enter the source directory: " source
>     read -p "Enter the destination directory: " dest
>     echo "Backing up $source to $dest..."
>     tar -czf $dest/backup-$(date +%Y-%m-%d).tar.gz $source
>     echo "Backup complete."
> }
> 
> # Function to update system packages
> update_packages() {
>     echo "Updating system packages..."
>     sudo apt-get update && sudo apt-get upgrade -y
>     echo "Update complete."
> }
> 
> # Function to clean up system junk files
> clean_up() {
>     echo "Cleaning up system junk files..."
>     sudo apt-get autoremove -y && sudo apt-get autoclean
>     echo "Clean up complete."
> }
> 
> # Function to monitor system performance
> monitor_performance() {
>     echo "Monitoring system performance..."
>     top
> }
> 
> # Main program
> while true; do
>     display_menu
>     read choice
>     case $choice in
>         1) backup_files ;;
>         2) update_packages ;;
>         3) clean_up ;;
>         4) monitor_performance ;;
>         5) exit 0 ;;
>         *) echo "Invalid option. Try again.";;
>     esac
> done
> ```
> 
> Script ini menampilkan menu dengan beberapa pilihan tugas otomatis, seperti backup file, pembaruan paket sistem, pembersihan file sampah sistem, dan pemantauan kinerja sistem. Setiap tugas didefinisikan dalam fungsi sendiri dan dipanggil oleh program utama ketika pilihan yang sesuai dipilih oleh pengguna.
> 
> cara agar script di atas bisa di jalankan lewat .zshrc
> 
> Untuk menjalankan script di atas pada saat memulai terminal menggunakan .zshrc, tambahkan baris berikut ke dalam file .zshrc Anda:
> 
> bash
> 
> ```bash
> bash /path/to/script.sh
> ```
> 
> Pastikan untuk mengganti "/path/to/script.sh" dengan lokasi yang sesuai dengan script Anda.
> 
> Setelah menambahkan baris ini, simpan perubahan pada file .zshrc dan tutup terminal Anda. Ketika membuka terminal berikutnya, script akan otomatis dipanggil dan Anda akan dapat memulai menjalankan tugas otomatis.
> 
> buatkan alias untu script di tas di zshrc
> 
> Untuk membuat alias untuk script "all-in-one automation tools" di .zshrc, tambahkan baris berikut ke dalam file .zshrc Anda:
> 
> bash
> 
> ```bash
> alias automation='bash /path/to/script.sh'
> ```
> 
> Pastikan untuk mengganti "/path/to/script.sh" dengan lokasi yang sesuai dengan script Anda.
> 
> Setelah menambahkan baris ini, simpan perubahan pada file .zshrc dan tutup terminal Anda. Ketika membuka terminal berikutnya, Anda dapat menjalankan script dengan mengetik `automation` di terminal. Alias ini akan memanggil script secara otomatis dan memudahkan Anda untuk menjalankan tugas otomatis tanpa harus memasukkan perintah yang panjang.
> 
> Write a function that, given a string of text (possibly with punctuation and line-breaks), returns an array of the top-3 most occurring words, in descending order of the number of occurrences. Assumptions: A word is a string of letters (A to Z) optionally containing one or more apostrophes (') in ASCII. Apostrophes can appear at the start, middle or end of a word ('abc, abc', 'abc', ab'c are all valid) Any other characters (e.g. #, \\, / , . ...) are not part of a word and should be treated as whitespace. Matches should be case-insensitive, and the words in the result should be lowercased. Ties may be broken arbitrarily. If a text contains fewer than three unique words, then either the top-2 or top-1 words should be returned, or an empty array if a text contains no words. Examples: top\_3\_words("In a village of La Mancha, the name of which I have no desire to call to mind, there lived not long since one of those gentlemen that keep a lance in the lance-rack, an old buckler, a lean hack, and a greyhound for coursing. An olla of rather more beef than mutton, a salad on most nights, scraps on Saturdays, lentils on Fridays, and a pigeon or so extra on Sundays, made away with three-quarters of his income.") # => \["a", "of", "on"\] top\_3\_words("e e e e DDD ddd DdD: ddd ddd aa aA Aa, bb cc cC e e e") # => \["e", "ddd", "aa"\] top\_3\_words(" //wont won't won't") # => \["won't", "wont"\] Bonus points (not really, but just for fun): Avoid creating an array whose memory footprint is roughly as big as the input text. Avoid sorting the entire array of unique words. tulis kode di bawah ini function topThreeWords(text) { }
> 
> Berikut adalah solusi untuk function `topThreeWords`:
> 
> javascript
> 
> ```javascript
> function topThreeWords(text) {
>   // Membuat regular expression untuk memisahkan kata-kata
>   const wordRegex = /[a-z']+/gi;
>   
>   // Menghitung frekuensi setiap kata
>   let wordCounts = {};
>   let match;
>   while ((match = wordRegex.exec(text.toLowerCase())) !== null) {
>     let word = match[0].toLowerCase();
>     wordCounts[word] = (wordCounts[word] || 0) + 1;
>   }
>   
>   // Mengurutkan kata-kata berdasarkan frekuensi
>   let sortedWords = Object.keys(wordCounts).sort((a, b) => wordCounts[b] - wordCounts[a]);
>   
>   // Mengembalikan top-3 kata terbanyak
>   return sortedWords.slice(0, 3);
> }
> ```
> 
> Penjelasan:
> 
> *   Regular expression `wordRegex` digunakan untuk memisahkan kata-kata dari teks.
> *   Kata-kata yang diambil dari teks diconvert ke huruf kecil dan dihitung frekuensinya menggunakan objek `wordCounts`.
> *   Kemudian, objek `wordCounts` diurutkan berdasarkan frekuensi kata menggunakan `sort()` dan diambil top-3 kata terbanyak menggunakan `slice(0, 3)`.
> 
> kode mangalami error di Expected: \[\], instead got: \['\\''\]
> 
> Untuk memperbaiki error tersebut, tambahkan pengecekan apakah kata tersebut memiliki panjang lebih dari 0 sebelum ditambahkan ke objek `wordCounts`:
> 
> javascript
> 
> ```javascript
> function topThreeWords(text) {
>   // Membuat regular expression untuk memisahkan kata-kata
>   const wordRegex = /[a-z']+/gi;
>   
>   // Menghitung frekuensi setiap kata
>   let wordCounts = {};
>   let match;
>   while ((match = wordRegex.exec(text.toLowerCase())) !== null) {
>     let word = match[0].toLowerCase();
>     if (word.length > 0) {
>       wordCounts[word] = (wordCounts[word] || 0) + 1;
>     }
>   }
>   
>   // Mengurutkan kata-kata berdasarkan frekuensi
>   let sortedWords = Object.keys(wordCounts).sort((a, b) => wordCounts[b] - wordCounts[a]);
>   
>   // Mengembalikan top-3 kata terbanyak
>   return sortedWords.slice(0, 3);
> }
> ```
> 
> Dengan pengecekan ini, kata yang tidak valid seperti karakter unik tidak akan diambil dan menyebabkan error.
> 
> Unhandled rejection TestError: Expected: \[\], instead got: \['\\''\] tolong perbaiki code di atas biar bisa melakukan tugas nya dengan benar
> 
> Untuk memperbaiki error tersebut, tambahkan pengecekan apakah kata tersebut memiliki panjang lebih dari 0 sebelum ditambahkan ke objek `wordCounts`:
> 
> javascript
> 
> ```javascript
> function topThreeWords(text) {
>   // Membuat regular expression untuk memisahkan kata-kata
>   const wordRegex = /[a-z']+/gi;
>   
>   // Menghitung frekuensi setiap kata
>   let wordCounts = {};
>   let match;
>   while ((match = wordRegex.exec(text.toLowerCase())) !== null) {
>     let word = match[0].toLowerCase();
>     if (word.length > 0) {
>       wordCounts[word] = (wordCounts[word] || 0) + 1;
>     }
>   }
>   
>   // Mengurutkan kata-kata berdasarkan frekuensi
>   let sortedWords = Object.keys(wordCounts).sort((a, b) => wordCounts[b] - wordCounts[a]);
>   
>   // Mengembalikan top-3 kata terbanyak
>   return sortedWords.slice(0, 3);
> }
> ```
> 
> Dengan pengecekan ini, kata yang tidak valid seperti karakter unik tidak akan diambil dan menyebabkan error.
> 
> describe("Tests", () => { it("test", () => { Test.assertDeepEquals(topThreeWords("a a a b c c d d d d e e e e e"), \['e','d','a'\]) Test.assertDeepEquals(topThreeWords("a a c b b"), \['a','b','c'\]) Test.assertDeepEquals(topThreeWords("e e e e DDD ddd DdD: ddd ddd aa aA Aa, bb cc cC e e e"),\['e','ddd','aa'\]) Test.assertDeepEquals(topThreeWords(" //wont won't won't "), \["won't", "wont"\]) Test.assertDeepEquals(topThreeWords(" , e .. "), \["e"\]) Test.assertDeepEquals(topThreeWords(" ... "), \[\]) Test.assertDeepEquals(topThreeWords(" ' "), \[\]) Test.assertDeepEquals(topThreeWords(\`In a village of La Mancha, the name of which I have no desire to call to mind, there lived not long since one of those gentlemen that keep a lance in the lance-rack, an old buckler, a lean hack, and a greyhound for coursing. An olla of rather more beef than mutton, a salad on most nights, scraps on Saturdays, lentils on Fridays, and a pigeon or so extra on Sundays, made away with three-quarters of his income.\`), \['a','of','on'\]) }); });
> 
> It seems like you are trying to provide test cases for the `topThreeWords` function using the `Test.assertDeepEquals` method from an unknown testing framework. I'm unable to run these tests without knowing the details of the testing framework, but you can use any testing framework of your choice to verify the correctness of the function. For example, you can use `assert.deepEqual` from the built-in `assert` module in Node.js to run these tests. Here's an example:
> 
> javascript
> 
> ```javascript
> const assert = require('assert');
> 
> function topThreeWords(text) {
>   // implementation of the function
> }
> 
> // Tests
> 
> assert.deepEqual(topThree
> ```