Задание на л.р. №8 ООП 24
Требуется написать объектно-ориентированную программу с графическим интерфейсом в соответствии со своим вариантом. 
В программе должны быть реализованы минимум один класс, три атрибута, четыре метода (функции). 
Ввод данных из файла с контролем правильности ввода. 
Базы данных использовать нельзя. При необходимости сохранять информацию в виде файлов, разделяя значения запятыми или пробелами. 
Для GUI использовать библиотеку tkinter.


Вариант 28
Объекты – звезды
Функции:	сегментация
визуализация
раскраска
перемещение на плоскости
import tkinter as tk
from tkinter import filedialog, messagebox
import random


class Star:
    def __init__(self, x, y, color, size):
        self.x = x
        self.y = y
        self.color = color
        self.size = size

    def move(self, dx, dy):
        self.x += dx
        self.y += dy

    def resize(self, new_size):
        self.size = new_size

    def change_color(self, new_color):
        self.color = new_color

    def draw(self, canvas):
        canvas.create_oval(self.x - self.size, self.y - self.size,
                           self.x + self.size, self.y + self.size,
                           fill=self.color)


class StarApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Star Visualization")
        self.canvas = tk.Canvas(root, width=600, height=600, bg="black")
        self.canvas.pack()
        self.stars = []

        self.load_button = tk.Button(root, text="Load Stars", command=self.load_stars)
        self.load_button.pack()

        self.move_button = tk.Button(root, text="Move Stars", command=self.move_stars)
        self.move_button.pack()

        self.color_button = tk.Button(root, text="Change Color", command=self.change_stars_color)
        self.color_button.pack()

    def load_stars(self):
        file_path = filedialog.askopenfilename(filetypes=[("Text Files", "*.txt")])
        if not file_path:
            return

        try:
            with open(file_path, 'r') as f:
                for line in f:
                    x, y, color, size = line.strip().split(',')
                    self.stars.append(Star(int(x), int(y), color.strip(), int(size)))
            self.visualize()
        except Exception as e:
            messagebox.showerror("Error", f"Failed to load stars: {e}")

    def visualize(self):
        self.canvas.delete("all")
        for star in self.stars:
            star.draw(self.canvas)

    def move_stars(self):
        for star in self.stars:
            dx = random.randint(-10, 10)
            dy = random.randint(-10, 10)
            star.move(dx, dy)
        self.visualize()

    def change_stars_color(self):
        for star in self.stars:
            new_color = f'#{random.randint(0, 0xFFFFFF):06x}'
            star.change_color(new_color)
        self.visualize()


if __name__ == "__main__":
    root = tk.Tk()
    app = StarApp(root)
    root.mainloop()
