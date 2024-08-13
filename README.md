import tkinter as tk
from tkinter import messagebox
import re
import random


def check_password_strength(password):
    if (len(password) >= 8 and
            re.search(r"[A-Z]", password) and
            re.search(r"[a-z]", password) and
            re.search(r"\d", password)):
        return True
    return False


def open_registration_window():
    reg_window = tk.Toplevel(root)
    reg_window.title("Üye Ol")

    tk.Label(reg_window, text="İsim:").grid(row=0, column=0, padx=10, pady=5)
    tk.Entry(reg_window).grid(row=0, column=1, padx=10, pady=5)

    tk.Label(reg_window, text="Soyisim:").grid(row=1, column=0, padx=10, pady=5)
    tk.Entry(reg_window).grid(row=1, column=1, padx=10, pady=5)

    tk.Label(reg_window, text="E-posta:").grid(row=2, column=0, padx=10, pady=5)
    tk.Entry(reg_window).grid(row=2, column=1, padx=10, pady=5)

    tk.Label(reg_window, text="Parola:").grid(row=3, column=0, padx=10, pady=5)
    password_entry = tk.Entry(reg_window, show="*")
    password_entry.grid(row=3, column=1, padx=10, pady=5)

    def register():
        password = password_entry.get()
        if check_password_strength(password):
            messagebox.showinfo("Kayıt Başarılı", "Başarıyla Kayıt Oldunuz!")
            reg_window.destroy()
        else:
            messagebox.showerror("Hata",
                                 "Parola en az 8 karakter, bir büyük harf, bir küçük harf ve bir rakam içermelidir.")

    tk.Button(reg_window, text="Kaydol", command=register).grid(row=4, columnspan=2, pady=10)


def animate_background():
    for circle in circles:
        x0, y0, x1, y1 = canvas.coords(circle)
        dx, dy = velocities[circle]
        if x1 >= 300 or x0 <= 0:
            dx = -dx
        if y1 >= 200 or y0 <= 0:
            dy = -dy
        canvas.move(circle, dx, dy)
        velocities[circle] = (dx, dy)
    root.after(50, animate_background)


def animate_window(window, step, max_size):
    if step < max_size:
        window.place_configure(relwidth=step, relheight=step)
        step += 0.01
        root.after(10, animate_window, window, step, max_size)


def login():
    main_window = tk.Toplevel(root)
    main_window.title("Hoşgeldiniz")
    main_window.config(bg="black", bd=5, relief="solid", highlightbackground="red", highlightcolor="red",
                       highlightthickness=3)

    sub_window1 = tk.Frame(main_window, bg="black", bd=5, relief="solid", highlightbackground="red",
                           highlightcolor="red", highlightthickness=3)
    sub_window1.place(relx=0.5, rely=0.5, anchor="center", relwidth=0, relheight=0)

    sub_window2 = tk.Frame(sub_window1, bg="black", bd=5, relief="solid", highlightbackground="red",
                           highlightcolor="red", highlightthickness=3)
    sub_window2.place(relx=0.5, rely=0.5, anchor="center", relwidth=0, relheight=0)

    welcome_label = tk.Label(sub_window2, text="Hoşgeldin", fg="white", bg="black")
    welcome_label.pack(expand=True)

    animate_window(sub_window1, 0.01, 0.8)
    root.after(500, animate_window, sub_window2, 0.01, 0.8)  # 500ms sonra ikinci pencereyi animasyonla


# Ana pencere oluşturuluyor
root = tk.Tk()
root.title("Giriş Yap")
root.geometry("300x200")  # Pencere boyutları

# Canvas ile arka plan animasyonu
canvas = tk.Canvas(root, width=300, height=200, bg="black")
canvas.pack(fill="both", expand=True)

# Daireler oluşturuluyor
circles = []
velocities = {}

for _ in range(5):
    x0 = random.randint(10, 290)
    y0 = random.randint(10, 190)
    x1 = x0 + 20
    y1 = y0 + 20
    circle = canvas.create_oval(x0, y0, x1, y1, fill="white")
    circles.append(circle)
    velocities[circle] = (random.choice([-2, 2]), random.choice([-2, 2]))

animate_background()  # Arka plan animasyonu başlatılıyor

# Giriş arayüzü oluşturuluyor
tk.Label(root, text="Kullanıcı Adı:", fg="white", bg="black").pack(pady=5)
tk.Entry(root).pack(pady=5)

tk.Label(root, text="Parola:", fg="white", bg="black").pack(pady=5)
tk.Entry(root, show="*").pack(pady=5)

tk.Button(root, text="Giriş Yap", command=login).pack(pady=5)
tk.Button(root, text="Üye Ol", command=open_registration_window).pack(pady=5)

root.mainloop()
