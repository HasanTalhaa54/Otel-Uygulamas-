# Otel-Uygulamas-


import tkinter as tk 
from tkinter import messagebox

class Hotel:
    def __init__(self):
        self.rooms = {}

    def admin_login(self, password):
        # Yönetici girişi kontrolü yapılabilir
        return password == "admin"

    def staff_login(self, username, password):
        # Personel girişi kontrolü yapılabilir
        return username == "staff" and password == "123456"

    def view_room_status(self):
        status = ""
        for room, info in self.rooms.items():
            if info["guest_info"]:
                status += f"Oda {room}: Dolu - {info['guest_info']['name']} {info['guest_info']['surname']}\n"
            else:
                status += f"Oda {room}: Boş\n"
        messagebox.showinfo("Oda Durumu", status)

    def check_in(self, room_number):
        if room_number in self.rooms:
            guest_name = input("Misafirin adını girin: ")
            guest_surname = input("Misafirin soyadını girin: ")
            guest_number = input("Misafirin telefon numarasını girin: ")

            guest_info = {
                "name": guest_name,
                "surname": guest_surname,
                "number": guest_number
            }

            self.rooms[room_number]["guest_info"] = guest_info
            self.rooms[room_number]["cleanliness"] = "Temiz"

            messagebox.showinfo("Müşteri Girişi", f"{room_number} numaralı odaya {guest_name} {guest_surname} kaydedildi.")
        else:
            messagebox.showerror("Hata", "Geçersiz oda numarası!")

    def check_out(self, room_number):
        if room_number in self.rooms:
            self.rooms[room_number]["guest_info"] = None
            self.rooms[room_number]["cleanliness"] = ""

            messagebox.showinfo("Müşteri Çıkışı", f"{room_number} numaralı oda boşaltıldı.")
        else:
            messagebox.showerror("Hata", "Geçersiz oda numarası!")

class Application:
    def __init__(self):
        self.hotel = Hotel()
        self.logged_in = False
        self.admin_layout = [
            tk.Button(text="Oda Durumu", command=self.view_room_status),
            tk.Button(text="Çıkış", command=self.logout)
        ]
        self.staff_layout = [
            tk.Button(text="Müşteri Girişi", command=self.check_in),
            tk.Button(text="Müşteri Çıkışı", command=self.check_out),
            tk.Button(text="Çıkış", command=self.logout)
        ]
        self.login_layout = [
            tk.Label(text="Kullanıcı Adı:"),
            tk.Entry(),
            tk.Label(text="Parola:"),
            tk.Entry(show="*"),
            tk.Button(text="Giriş", command=self.login),
            tk.Button(text="Çıkış", command=self.logout)
        ]
        self.window = tk.Tk()
        self.window.title("Otel Yönetim Sistemi")
        self.window.geometry("400x200")
        self.login_frame = tk.Frame(self.window)
        self.admin_frame = tk.Frame(self.window)
        self.staff_frame = tk.Frame(self.window)

    def login(self):
        username = self.login_frame.winfo_children()[1].get()
        password = self.login_frame.winfo_children()[3].get()

        if username == "admin" and password == "admin":
            self.logged_in = True
            self.update_layout("admin")
        elif username == "staff" and password == "123456":
            self.logged_in = True
            self.update_layout("staff")
        else:
            messagebox.showerror("Hata", "Geçersiz kullanıcı adı veya parola!")

    def logout(self):
        self.logged_in = False
        self.update_layout("")

    def view_room_status(self):
        self.hotel.view_room_status()

    def check_in(self):
        room_number = input("Giriş yapılacak oda numarasını girin: ")
        self.hotel.check_in(room_number)

    def check_out(self):
        room_number = input("Çıkış yapılacak oda numarasını girin: ")
        self.hotel.check_out(room_number)

    def update_layout(self, user_type):
        if user_type == "admin":
            self.login_frame.pack_forget()
            self.admin_frame.pack()
            self.staff_frame.pack_forget()
        elif user_type == "staff":
            self.login_frame.pack_forget()
            self.admin_frame.pack_forget()
            self.staff_frame.pack()
        else:
            self.login_frame.pack()
            self.admin_frame.pack_forget()
            self.staff_frame.pack_forget()

app = Application()
app.window.mainloop()