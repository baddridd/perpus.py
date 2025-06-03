import tkinter as tk
from tkinter import messagebox

class Buku:
    def __init__(self, judul, penulis, jumlah):
        self.judul = judul
        self.penulis = penulis
        self.jumlah = jumlah

class Perpustakaan:
    def __init__(self):
        self.daftar_buku = []

    def tambah_buku(self, buku):
        self.daftar_buku.append(buku)

    def pinjam_buku(self, index):
        if 0 <= index < len(self.daftar_buku):
            buku = self.daftar_buku[index]
            if buku.jumlah > 0:
                buku.jumlah -= 1
                return f"Anda berhasil meminjam buku: {buku.judul}"
            else:
                return f"Maaf, buku '{buku.judul}' sedang tidak tersedia."
        return "Pilihan tidak valid."

# GUI
class App:
    def __init__(self, root):
        self.root = root
        self.root.title("Aplikasi Peminjaman Buku")
        self.perpustakaan = Perpustakaan()

        # Tambahkan buku-buku
        self.perpustakaan.tambah_buku(Buku("Laskar Pelangi", "Andrea Hirata", 3))
        self.perpustakaan.tambah_buku(Buku("Bumi Manusia", "Pramoedya Ananta Toer", 2))
        self.perpustakaan.tambah_buku(Buku("Filosofi Teras", "Henry Manampiring", 1))
        self.perpustakaan.tambah_buku(Buku("Negeri 5 Menara", "Ahmad Fuadi", 4))
        self.perpustakaan.tambah_buku(Buku("Laut Bercerita", "Leila S. Chudori", 2))
        self.perpustakaan.tambah_buku(Buku("Anak Semua Bangsa", "Pramoedya Ananta Toer", 1))
        self.perpustakaan.tambah_buku(Buku("9 Summers 10 Autumns", "Iwan Setyawan", 2))

        self.label = tk.Label(root, text="Klik tombol untuk meminjam buku:", font=("Arial", 12))
        self.label.pack(pady=10)

        self.buku_frame = tk.Frame(root)
        self.buku_frame.pack()

        self.tampilkan_tombol_buku()

        self.keluar_button = tk.Button(root, text="Keluar", command=root.quit)
        self.keluar_button.pack(pady=10)

    def tampilkan_tombol_buku(self):
        for widget in self.buku_frame.winfo_children():
            widget.destroy()  # Bersihkan tombol lama

        for index, buku in enumerate(self.perpustakaan.daftar_buku):
            teks = f"{buku.judul} ({buku.jumlah})"
            warna = "lightgreen" if buku.jumlah > 0 else "lightcoral"
            tombol = tk.Button(
                self.buku_frame,
                text=teks,
                width=40,
                bg=warna,
                command=lambda i=index: self.pinjam_buku(i)
            )
            tombol.pack(pady=2)

    def pinjam_buku(self, index):
        hasil = self.perpustakaan.pinjam_buku(index)
        self.tampilkan_tombol_buku()
        messagebox.showinfo("Informasi", hasil)

# Jalankan aplikasi
if __name__ == "__main__":
    root = tk.Tk()
    app = App(root)
    root.mainloop()
