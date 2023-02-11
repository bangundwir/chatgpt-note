> buatkan scripting dengan python automation github git add \*, git commit -m " " dan git push
> 
> Berikut adalah script Python untuk mengeksekusi perintah `git add *`, `git commit -m " "`, dan `git push` secara otomatis:
> 
> scss
> 
> ```scss
> import os
> 
> def git_commit_push():
>     os.system("git add *")
>     os.system('git commit -m "Scripting automatic"')
>     os.system("git push")
>     
> if __name__ == "__main__":
>     git_commit_push()
> ```
> 
> Catatan: Script ini akan mengeksekusi perintah `git add *`, `git commit -m "Scripting automatic"`, dan `git push` secara bersamaan dan harus dijalankan pada direktori proyek Git. Jika Anda ingin mengubah pesan commit, Anda dapat mengubah string `"Scripting automatic"` pada baris `os.system('git commit -m "Scripting automatic"')`.
> 
> buatkan biyar bisa merename git commit di terminal
> 
> Untuk mengubah nama commit pada Git, Anda dapat menggunakan perintah `git rebase -i HEAD~n`, di mana `n` adalah jumlah commit yang ingin Anda ubah. Ini akan membuka editor teks interaktif, di mana Anda dapat mengubah nama commit dengan menggantikan perintah `pick` dengan `reword`.
> 
> Berikut adalah script Python untuk mengubah nama commit pada Git:
> 
> scss
> 
> ```scss
> import os
> 
> def git_rename_commit(n):
>     os.system("git rebase -i HEAD~{}".format(n))
>     
> if __name__ == "__main__":
>     n = input("Enter the number of commits you want to rename: ")
>     git_rename_commit(n)
> ```
> 
> Catatan: Script ini akan membuka editor teks interaktif dan meminta Anda memasukkan jumlah commit yang ingin Anda ubah. Jika Anda ingin mengubah nama commit dari