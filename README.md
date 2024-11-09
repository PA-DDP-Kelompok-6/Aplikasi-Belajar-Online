# Aplikasi-Belajar-Online
## Kelompok-6
- Fikri Abiyyu Rahman (2409116063)
- Jemis Movid (2409116070)
- Marcela Persa Lintin (2409116072)

## Penjelasan dan Panduan Program
### Import Modul
``` python
import pwinput
import csv
from datetime import datetime
from colorama import Fore, Back, Style
from prettytable import PrettyTable
```
Bagian ini berfungsi untuk mengimport modul pwinput, csv, datetime, colorama, dan prettytable. Masing-masing modul ini digunakan untuk:

- pwinput: Menerima input password secara tersembunyi, alias menyemunyikan isi inputnya.

- csv: Dipakai untuk menyimpan data dengan file CSV.

- datetime: Mendapatkan waktu saat ini.

- colorama: Memberikan warna pada output terminal (Untuk baris program tertentu).

- prettytable: Menampilkan tabel yang lebih mudah dibaca di terminal.
### Variable
``` python
kesempatan = 3
data_kelas = "DATA_KELAS.CSV"
```
- kesempatan adalah variabel untuk menghitung jumlah percobaan login yang tersisa.
- data_kelas menyimpan nama file CSV untuk DATA_KELAS.CSV.
### Menampilkan Struk
``` python
def tampilkan_struk():
    global table_struk
    print("\n================ CATATAN KELAS =================")

    table_struk = PrettyTable()
    table_struk.field_names = ["NAMA","KELAS","HARGA","WAKTU"]
    
    waktu = datetime.now()

    with open("DATA_INVENTORY.csv", mode='r') as file:
        csv_reader = csv.DictReader(file)
        inventory = list(csv_reader)
        for row in inventory:
            table_struk.add_row([row["username"], row["kelas"], row["Harga"], waktu])
    print(table_struk)
```
- Fungsi ini berguna untuk menampilkan struk pembelian atau struk pendaftaran kelas dalam bentuk tabel yang rapi.
- Data diambil dari file DATA_INVENTORY.csv.

Ini adalah contoh output struknya.

![Cuplikan layar 2024-11-09 220751](https://github.com/user-attachments/assets/8bddd889-b6ba-41e0-ba6a-fd0c84f76166)

### Fungsi yang menyimpan data user
``` python
def simpan_user(data):
    with open("DATA_PA.csv", mode= "w", newline= "")as file:
        fieldnames = ["username", "password", "saldo"]
        writer = csv.DictWriter(file, fieldnames=fieldnames)
        writer.writeheader()
        for simpan in data:
            writer.writerow(simpan)
```
Fungsi ini menyimpan data pengguna ke file DATA_PA.csv. Data yang disimpan meliputi username, password, dan saldo.
### Fungsi mengupdate saldo pengguna
``` python
def update_saldo_user(username, new_saldo):
    updated_rows = []
    with open("DATA_PA.csv", mode="r") as file:
        csv_reader = csv.DictReader(file)
        for user in csv_reader:
            if user["username"] == username:
                user["saldo"] = str(new_saldo)
            updated_rows.append(user)

    with open("DATA_PA.csv", mode="w", newline="") as file:
        fieldnames = ["username", "password", "saldo"]
        csv_writer = csv.DictWriter(file, fieldnames=fieldnames)
        csv_writer.writeheader()
        csv_writer.writerows(updated_rows)
```
- Digunakan untuk memperbarui saldo pengguna tertentu.
- Fungsi ini membaca data dari DATA_PA.csv, memperbarui saldo pengguna yang dituju tadi, lalu menulis ulang data yang telah diperbarui ke file yang sama.
### Fungsi login
``` python
def login():
    global kesempatan
    berhasil_login = False

    while kesempatan > 0:
        print("\n================ SILAHKAN LOGIN =================")
        print("-------------------------------------------------")
        global usernamelogin
        usernamelogin = input("Silahkan masukkan username: ")
        passwordlogin = pwinput.pwinput(prompt="Silahkan masukkan password: ")

        with open("DATA_PA.csv", mode="r") as file:
            csv_reader = csv.DictReader(file)

            for user in csv_reader:
                if user["username"] == usernamelogin and user["password"] == passwordlogin:
                    berhasil_login = True
                    menu_fitur_user()

            if not berhasil_login:
                    kesempatan -= 1
                    print(Fore.RED + "\nUsername atau password salah.")
                    print(f"Kesempatan tersisa: {kesempatan}" + Style.RESET_ALL)

                    if kesempatan == 0:
                        print("\nSesi telah habis!")
        break
```
- Fungsi untuk proses login pengguna biasa.
- Pengguna diberi tiga kali kesempatan untuk memasukkan username dan password dengan benar. Jika salah, kesempatan berkurang, dan jika login berhasil, mereka akan diarahkan ke menu fitur.
### Fungsi login untuk admin
``` python
def login_admin():
    global kesempatan
    berhasil_login = False

    while kesempatan > 0:
        print("\n================ SELAMAT DATANG ADMIN =================")
        print("-------------------------------------------------")
        global adminlogin
        adminlogin = input("Silahkan masukkan username admin: ")
        adminpassword = pwinput.pwinput(prompt="Silahkan masukkan password admin: ")

        with open ("DATA_ADMIN.csv", mode="r") as file:
            csv_reader = csv.DictReader(file)

            for admin in csv_reader:
                if admin["nama_admin"] == adminlogin and admin["pass_admin"] == adminpassword:
                    berhasil_login = True
                    menu_fitur_admin()

            if not berhasil_login:
                    kesempatan -= 1
                    print(Fore.RED + "\nUsername atau password salah.")
                    print(f"Kesempatan tersisa: {kesempatan}" + Style.RESET_ALL)
                    menu()

                    if kesempatan == 0:
                        print("\nSesi telah habis!")
        break
```
- Mirip dengan login(), tetapi hanya untuk admin.
- File DATA_ADMIN.csv digunakan sebagai basis data adminnya.
### Fungsi Regestrasi atau buat akun
``` python
def Registrasi():
    print("\n================ SILAHKAN REGISTRASI =================")
    print("------------------------------------------------------")
    username = input("Silahkan buat username : ")
    password = pwinput.pwinput(prompt = "Silahkan buat password : ")
    saldo = 0

    found = False

    with open ("DATA_PA.csv", mode="r") as file:
        csv_reader = csv.DictReader(file)
        for row in csv_reader:
            if row["username"] == username:
                found = True
                print(Fore.RED + "\n Username sudah ada, silahkan pilih username lain!" + Style.RESET_ALL)
                menu()
                break

        if not found:
            with open("DATA_PA.csv", mode='a', newline='') as file:
                csv_writer = csv.writer(file)
                csv_writer.writerow([username, password, saldo])
            print(Fore.GREEN+f"\nHalo {username}, silahkan pergi ke menu Login!" + Style.RESET_ALL)
            menu()
```
- Fungsi untuk mendaftarkan pengguna baru dengan memasukkan username dan password.
- Jika username belum ada, data pengguna baru akan disimpan di DATA_PA.csv.
### Fungsi menambah saldo pengguna
``` python
def isi_saldo():
    try:
        print("\n================ ISI SALDO =================")
        username = str(input("Masukkan username anda : "))
        nominal = int(input("Masukkan jumlah nominal pengisian : "))

        updated_rows = []
        found = False

        with open('DATA_PA.csv', mode='r') as file:
            csv_reader = csv.DictReader(file)
            
            for user in csv_reader:
                if user["username"] == username:
                    saldo_sekarang = int(user["saldo"])
                    user["saldo"] = str(saldo_sekarang + nominal)
                    found = True
                updated_rows.append(user)

        if not found:
            print(f"user dengan nama {username} tidak ditemukan.")
            return

        with open('DATA_PA.csv', mode='w', newline='') as file:
            fieldnames = ["username", "password", "saldo"]
            csv_writer = csv.DictWriter(file, fieldnames=fieldnames)
            csv_writer.writeheader()
            csv_writer.writerows(updated_rows)

        print(Fore.GREEN + f"\nSaldo anda berhasil diisi!" + Style.RESET_ALL)
        
        pilihan_isi = input("\nApakah anda ingin mengisi lagi? (Y/N) : ")
        if pilihan_isi.lower() == "y":
            isi_saldo()
        elif pilihan_isi.lower() == "n":
            menu_fitur_user()
        else:
            print("Pilihan tidak valid!")
            menu_fitur_user()

        if not found:
            print(Fore.RED + f"Username dengan nama {username} tidak ditemukan." + Style.RESET_ALL)
            return
        
    except ValueError:
        print("Silahkan masukan angka!")
        menu_fitur_user()

        with open("DATA_PA.csv", mode='w', newline='') as file:
            csv_writer = csv.writer(file)
            csv_writer.writerows(updated_rows)
            
        print(f"\nSaldo berhasil diisi. Saldo Anda sekarang adalah")
        nominal_isi = str(input("\nApakah anda ingin mengisi lagi? (Y/N) : "))
        if nominal_isi == "Y" or nominal_isi == "y":
            isi_saldo()
        elif nominal_isi == "N" or nominal_isi == "n":
            menu_fitur_user()
        else:
            print("Pilihan tidak valid!")
    except ValueError:
        print("Silahkan masukan angka!")
        menu_fitur_user()
```
- Fungsi ini memungkinkan pengguna untuk menambah saldo mereka.
- Setelah saldo berhasil ditambahkan, pengguna bisa memilih untuk mengisi saldo lagi atau kembali ke menu utama.
- Jika username tidak ditemukan atau ada kesalahan input, pengguna akan mendapatkan pesan notifikasi.
### Fungsi menampilkan kelas dan mendaftar kelas
``` python
def tampilkan_kelas_dan_daftar():
    global table_tampilkan_kelas
    print("\n================ MENAMPILKAN KELAS =================")
    
    table_tampilkan_kelas = PrettyTable()
    table_tampilkan_kelas.field_names = ["Kelas", "Kapasitas", "Harga"]

    with open(data_kelas, mode='r') as file:
        csv_reader = csv.DictReader(file)
        kelas_data = list(csv_reader)
        for kelas in kelas_data:
            table_tampilkan_kelas.add_row([kelas["Kelas"], kelas["Kapasitas"], kelas["Harga"]]) 
    print(table_tampilkan_kelas)

    pilihan_tampilkan_kelas = str(input("\nApakah anda ingin mendaftar kelas? (Y/N) : "))
    if pilihan_tampilkan_kelas.lower() == "y":
        global pengen_kelas
        pengen_kelas = str(input("\nMasukkan kelas yang diinginkan : "))
        kelas_ditemukan = False

        for kelas in kelas_data:
            if kelas["Kelas"] == pengen_kelas and int(kelas["Kapasitas"]) > 0:
                kelas["Kapasitas"] = str(int(kelas["Kapasitas"]) - 1)
                kelas_ditemukan = True

                print(Fore.LIGHTMAGENTA_EX + f"\nKelas {pengen_kelas} berhasil dipilih." + Style.RESET_ALL)
                
                with open("DATA_INVENTORY.csv", mode='a', newline='') as file:
                    fieldnames = ["username", "kelas", "harga"]
                    csv_writer = csv.DictWriter(file, fieldnames=fieldnames)
                    csv_writer.writerow({"username": usernamelogin, "kelas": pengen_kelas, "harga": kelas["Harga"]})

                with open(data_kelas, mode='w', newline='') as file:
                    fieldnames = ["Kelas", "Kapasitas", "Harga"]
                    csv_writer = csv.DictWriter(file, fieldnames=fieldnames)
                    csv_writer.writeheader()
                    csv_writer.writerows(kelas_data)
                    tampilkan_struk()

                kurang_saldo()
                break
        
        if not kelas_ditemukan:
            print(Fore.RED + "\nKelas tidak ditemukan atau kapasitas penuh." + Style.RESET_ALL)
            tampilkan_kelas_dan_daftar()
    elif pilihan_tampilkan_kelas.lower() == "n":
        print("\nBaiklah!")
        menu_fitur_user()
    else:
        print(Fore.RED + "\nPilihan tidak tersedia. Silahkan coba lagi." + Style.RESET_ALL)
        tampilkan_kelas_dan_daftar()
```
- Fungsi ini menampilkan daftar kelas yang tersedia untuk pengguna.
- Data kelas diambil dari file data_kelas dalam format CSV, yang memuat informasi seperti nama kelas dan biaya kelas.
- Setelah menampilkan daftar, pengguna bisa memilih untuk mendaftar ke salah satu kelas.
- Jika pengguna memilih untuk mendaftar, fungsi akan memverifikasi saldo pengguna. Jika saldo cukup, biaya kelas akan dipotong dari saldo, dan pengguna berhasil terdaftar. Jika saldo tidak mencukupi, pengguna akan mendapat notifikasi bahwa saldo mereka tidak cukup.
### Fungsi untuk mengurangi saldo pengguna
``` python
def kurang_saldo():
    saldo_awal = 0
    harga_kelas = 0
    
    with open("DATA_PA.csv", mode="r") as file:
        csv_reader = csv.DictReader(file)
        for user in csv_reader:
            if user["username"] == usernamelogin:
                saldo_awal = int(user["saldo"])
                break

    with open("DATA_KELAS.csv", mode="r") as file:
        csv_reader = csv.DictReader(file)
        for kelas in csv_reader:
            if kelas["Kelas"] == pengen_kelas:
                harga_kelas = int(kelas["Harga"])
                break
```
- Fungsi ini berguna untuk mengurangi saldo pengguna setelah ia membeli atau mendaftar kelas.
- Fungsi ini akan mencari nama berdasarkan username di file DATA_PA.csv dan mengurangi saldo mereka sesuai dengan harga kelasnya.
### Fungsi menu utama user
``` python
def menu_fitur_user():
    try:
        with open("DATA_PA.csv", mode="r") as file:
            csv_reader = csv.DictReader(file)

            for user in csv_reader:
                if user ["username"] == usernamelogin:
                    saldo_user = user.get("saldo", "0")
                    print(Fore.BLUE + f"\nHalo {usernamelogin}, saldo Anda saat ini adalah : Rp. {saldo_user}" + Style.RESET_ALL)
                    break

        print("\n================ PILIH FITUR ====================")
        print("-------------------------------------------------")
        print("\n[1] Tampilkan kelas dan Daftar!")
        print("[2] Isi saldo anda!")
        print("[3] Kembali ke menu login")
        input_user = int(input("\nSilahkan pilih (1/2/3) : "))
        if input_user == 1:
            tampilkan_kelas_dan_daftar()
        elif input_user == 2:
            isi_saldo()
        elif input_user == 3:
            menu()
        else:
            print(Fore.RED + "\nPilihan tidak tersedia. Silahkan coba lagi." + Style.RESET_ALL)
            menu_fitur_user()
    except ValueError:
        print(Fore.RED + "\nInput tidak valid, silahkan coba lagi." + Style.RESET_ALL)
        menu_fitur_user()
```
- Fungsi ini menampilkan menu utama bagi pengguna yang sudah login.
- Di dalam menu ini, pengguna bisa memilih berbagai fitur yang tersedia, seperti mengisi saldo (isi_saldo()), melihat struk transaksi (tampilkan_struk()), atau melihat dan mendaftar kelas (tampilkan_kelas_dan_daftar()).
- Fungsi ini juga memberikan pilihan untuk keluar (logout), sehingga pengguna dapat kembali ke menu awal jika selesai menggunakan aplikasi.
## Fungsi-fungsi untuk admin
### Fungsi untuk menambah kelas
``` python
def tambah_kelas():
    Kelas = input("\nMasukkan nama kelas baru : ")
    kapasitas = int(input(f"\nMasukkan kapasitas kelas {Kelas} : "))
    harga = int(input(f"\nMasukkan harga kelas {Kelas} : "))

    with open("DATA_KELAS.CSV", mode='a', newline='') as file:
        csv_writer = csv.writer(file)
        csv_writer.writerow([Kelas,kapasitas,harga])
    print(Fore.LIGHTMAGENTA_EX + f"\nKelas {Kelas} berhasil ditambahkan." + Style.RESET_ALL)
    lanjut_ga = input("\nApakah anda ingin menambah kelas lagi? (Y/N) : ")
    if lanjut_ga == "y" or lanjut_ga == "Y":
        tambah_kelas()
    elif lanjut_ga == "n" or lanjut_ga == "N":
        print("\nBaiklah!")
        menu_fitur_admin()
    else:
        print(Fore.RED + "\nPilihan tidak tersedia. Silahkan coba lagi." + Style.RESET_ALL)
        menu_fitur_admin()
```
- Fungsi ini memungkinkan admin untuk menambahkan data kelas baru ke dalam sistem.
- Admin bisa menginputkan nama kelas, kapasitasnya, dan biayanya. Data ini akan disimpan dalam file data_kelas dalam format CSV.
- Fungsi ini membantu memperbarui daftar kelas yang tersedia agar tetap relevan dengan kebutuhan pengguna.
### Fungsi untuk melihat data para pendaftar
``` python
    global table_data_pendaftar
    print("\n================ DATA PENDAFTAR =================")

    table_data_pendaftar = PrettyTable()
    table_data_pendaftar.field_names = ["Username","Kelas"]

    with open("DATA_INVENTORY.csv", mode='r') as file:
        csv_reader = csv.DictReader(file)
        inventory = list(csv_reader)
        for row in inventory:
            table_data_pendaftar.add_row([row["username"], row["kelas"]])
    print(table_data_pendaftar)
    menu_fitur_admin()
```
- Fungsi ini menampilkan daftar seluruh pengguna yang sudah mendaftar.
- Data pendaftar diambil dari file DATA_INVENTORY.csv.
### Fungsi update
``` python
def update():
    try:
        global table_tampilkan_kelas
        table_tampilkan_kelas = PrettyTable()
        table_tampilkan_kelas.field_names = ["Kelas", "Kapasitas", "Harga"]

        try:
            with open(data_kelas, mode='r') as file:
                csv_reader = csv.DictReader(file)
                kelas_data = list(csv_reader)
                for kelas in kelas_data:
                    table_tampilkan_kelas.add_row([kelas["Kelas"], kelas["Kapasitas"], kelas["Harga"]])
            
            print("\nData Kelas yang Tersedia:")
            print(table_tampilkan_kelas)

            nama_kelas = input("\nMasukkan nama kelas yang ingin diperbarui: ")
            kelas_ditemukan = False

            for kelas in kelas_data:
                if kelas["Kelas"] == nama_kelas:
                    kelas_ditemukan = True
                    nama_kelas_baru = input("\nMasukkan nama kelas baru: ")
                    kapasitas_baru = input("Masukkan kapasitas baru: ")
                    harga_baru = input("Masukkan harga baru: ")

                    if nama_kelas_baru:
                        kelas["Kelas"] = nama_kelas_baru.strip()
                    if kapasitas_baru:
                        kelas["Kapasitas"] = kapasitas_baru.strip()
                    if harga_baru:
                        kelas["Harga"] = harga_baru.strip()

                    with open(data_kelas, mode='w', newline='') as file:
                        fieldnames = ["Kelas", "Kapasitas", "Harga"]
                        csv_writer = csv.DictWriter(file, fieldnames=fieldnames)
                        csv_writer.writeheader()
                        csv_writer.writerows(kelas_data)
                    
                    print(Fore.LIGHTMAGENTA_EX + f"\nNama kelas {nama_kelas} berhasil diperbarui!" + Style.RESET_ALL)
                    break
            
            if not kelas_ditemukan:
                print(Fore.RED + f"Kelas dengan nama {nama_kelas} tersebut tidak ditemukan." + Style.RESET_ALL)
            
            menu_fitur_admin()

        except FileNotFoundError:
            print(Fore.RED +"Tidak Vadid!" + Style.RESET_ALL)
            update()
    except ValueError:
        print(Fore.RED + "Inputan tidak valid." + Style.RESET_ALL)
```
- Fungsi ini berguna untuk memperbarui informasi dari kelas yang ada.
- Admin bisa memanggil fungsi ini untuk mengubah data kelas tertentu seperti nama, kapasitas dan harga.
### Fungsi untuk melihat daftar kelas
``` python
def tampilkan_kelas():
    try:
        global table_tampilkan_kelas
        print("\n================ MENAMPILKAN KELAS =================")
        
        table_tampilkan_kelas = PrettyTable()
        table_tampilkan_kelas.field_names = ["Kelas", "Kapasitas", "Harga"]

        with open(data_kelas, mode='r') as file:
            csv_reader = csv.DictReader(file)
            kelas_data = list(csv_reader)
            for kelas in kelas_data:
                table_tampilkan_kelas.add_row([kelas["Kelas"], kelas["Kapasitas"], kelas["Harga"]])
        print(table_tampilkan_kelas)
    except:
        print(Fore.RED + "Tidak Valid!" + Style.RESET_ALL)
        tampilkan_kelas()
    menu_fitur_admin()
```
- Fungsi ini digunakan untuk menampilkan seluruh daftar kelas yang tersedia.
- Data kelas diambil dari file data_kelas dalam format CSV, yang menyimpan informasi mengenai nama dan biaya setiap kelas.
### Fungsi menghapus kelas
``` python
def hapus_kelas():
    try:
        hapus = input("\nMasukkan kelas yang ingin dihapus: ")
        
        updated_rows = []
        found = False

        with open("DATA_KELAS.CSV", mode='r') as file:
            csv_reader = csv.reader(file)
            for row in csv_reader:
                if row[0] == hapus:
                    found = True
                    print(Fore.BLUE + f"\nKelas {hapus} berhasil dihapus!" + Style.RESET_ALL)
                else:
                    updated_rows.append(row)

        if not found:
            print(Fore.RED + f"\nKelas {hapus} tidak ditemukan." + Style.RESET_ALL)
            menu_fitur_admin()

        with open("DATA_KELAS.CSV", mode='w', newline='') as file:
            csv_writer = csv.writer(file)
            csv_writer.writerows(updated_rows)
    except:
        print(Fore.RED + "Tidak Valid!" + Style.RESET_ALL)
        hapus_kelas()
```
- Fungsi ini memungkinkan admin untuk menghapus kelas dari daftar yang tersedia.
- Admin hanya perlu memasukkan nama_kelas yang ingin dihapus. Jika kelas tersebut ada dalam sistem, data kelas akan dihapus dari file data_kelas.
### Fungsi menu utama admin
``` python
def menu_fitur_admin():
    try:
        try:
            print(Fore.CYAN + f"\nHalo {adminlogin}, Selamat datang di menu Admin!" + Style.RESET_ALL)
            print("\n================ FITUR ADMIN ====================")
            print("-------------------------------------------------")
            print("[1] Tambah Kelas!")
            print("[2] Lihat Kelas")
            print("[3] Lihat Pendaftar")
            print("[4] Update Kelas")
            print("[5] Hapus Kelas")
            print("[6] Kembali ke menu login")
            input_user = int(input("\nSilahkan pilih (1/2/3/4/5) : "))
            if input_user == 1:
                tambah_kelas()
            elif input_user == 2:
                tampilkan_kelas()
            elif input_user == 3:
                tampilkan_data_pendaftar()
            elif input_user == 4:
                update()
            elif input_user == 5:
                hapus_kelas()
            elif input_user == 6:
                menu()
            else:
                print(Fore.RED + "Pilihan tidak tersedia. Silahkan coba lagi." + Style.RESET_ALL)
        except ValueError:
            print(Fore.RED + "Input tidak valid, silahkan coba lagi." + Style.RESET_ALL)
    except:
        print(Fore.RED + "Tidak Valid!" + Style.RESET_ALL)
        menu_fitur_admin()
```
- Fungsi ini menampilkan menu utama untuk admin yang sudah login.
- Admin memiliki akses ke beberapa fitur khusus yang tidak tersedia bagi pengguna biasa, seperti menambah data kelas, mengupdate data kelas, atau menghapus data kelas.
- Fitur-fitur ini memungkinkan admin untuk mengelola data kelas yang ada di dalam sistem.
### Fungsi menu utama saat pertama kali
``` python
def menu():
    global kesempatan
    while kesempatan > 0:
        print("\n================ SILAHKAN PILIH =================")
        print("-------------------------------------------------")
        print("[1] Login Admin")
        print("[2] Login")
        print("[3] Registrasi")
        print("[4] Keluar")
        try:
            pilihan_menu = int(input("\nPilihan menu (1/2/3) : "))
            if pilihan_menu == 1:
                login_admin()
            elif pilihan_menu == 2:
                login()
            elif pilihan_menu == 3:
                Registrasi()
            elif pilihan_menu == 4:
                print("Sampai bertemu lagi!")
                break
            else:
                kesempatan -= 1
                print(Fore.RED + f"\nPilihan tidak ada. Anda memiliki {kesempatan} kesempatan tersisa." + Style.RESET_ALL)
        except ValueError:
            kesempatan -= 1
            print(Fore.RED + f"\nInput tidak valid. Anda memiliki {kesempatan} kesempatan tersisa." + Style.RESET_ALL)
menu()
```
- Fungsi ini merupakan tampilan utama aplikasi, di mana pengguna diberikan empat pilihan: Login, Registrasi, Login Admin, dan keluar.
- Dari sini, pengguna bisa memilih untuk login sebagai pengguna biasa, mendaftarkan akun baru (registrasi), atau login sebagai admin.




