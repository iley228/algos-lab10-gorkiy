# Лабораторная работа №10

## Задание

Разработать графический интерфейс для списка задач (TODO-листа) с возможностью добавления, удаления и выбора задач.

1. Создать окно приложения с заголовком "Мой TODO-лист" и фиксированным размером.

2. Добавить Listbox для отображения списка задач.

3. Добавить поле ввода (Entry) для ввода новой задачи.

4. Добавить кнопки:

    "Добавить" — добавляет задачу в список.
    "Удалить" — удаляет выбранную задачу.
    "Очистить всё" — полностью очищает список.

<img src="./.repo/todo.png" />

## Теория

[Документация](https://metanit.com/python/tkinter/2.12.php)

**Listbox** — это виджет в tkinter, который отображает список элементов. Пользователь может добавлять, выбирать и удалять элементы.

### Создание Listbox

```python
listbox = tk.Listbox(root, width=30, height=5)
listbox.pack(pady=10)
```

### Добавление элементов

```python
listbox.insert(tk.END, "Элемент 1")
listbox.insert(tk.END, "Элемент 2")
listbox.insert(tk.END, "Элемент 3")

root.mainloop()
```

- _listbox = tk.Listbox(root, width=30, height=5)_ — создаёт список.
- _listbox.insert(tk.END, "Элемент")_ — добавляет элемент в конец списка.

### Выбор элемента в Listbox

Можно получить выделенный элемент с помощью curselection().

```python
def get_selected():
    selected = listbox.curselection()
    if selected:
        print("Выбрано:", listbox.get(selected[0]))

select_button = tk.Button(root, text="Выбрать", command=get_selected)
select_button.pack()
```

- _curselection()_ возвращает список индексов выделенных элементов.
- _get(index)_ возвращает текст элемента по индексу.

## Удаление элементов из Listbox

Удалить можно по индексу или все элементы сразу.

### Удаление выбранного элемента:

```python
def delete_selected():
    selected = listbox.curselection()
    if selected:
        listbox.delete(selected[0])

delete_button = tk.Button(root, text="Удалить", command=delete_selected)
delete_button.pack()
```

_Если ничего не выбрано, код ничего не делает._

### Очистка всего списка:

```python
listbox.delete(0, tk.END)
```
import tkinter as tk
from tkinter import ttk
from tkinter import messagebox


class TodoListApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Мой TODO-лист")
        self.root.geometry("400x500")  # Фиксированный размер окна
        self.root.resizable(False, False)  # Запрет изменения размера окна

        self.tasks = []  # Список для хранения задач

        self.create_widgets()

    def create_widgets(self):
        # Listbox для отображения задач
        self.task_listbox = tk.Listbox(self.root, width=50, height=15, selectbackground="lightblue", selectmode=tk.SINGLE)
        self.task_listbox.pack(pady=10, padx=10)

        # Поле ввода для новой задачи
        self.task_entry = tk.Entry(self.root, width=40)
        self.task_entry.pack(pady=5, padx=10)

        # Кнопки
        self.add_button = tk.Button(self.root, text="Добавить", command=self.add_task, width=10)
        self.add_button.pack(pady=5)

        self.delete_button = tk.Button(self.root, text="Удалить", command=self.delete_task, width=10)
        self.delete_button.pack(pady=5)

        self.clear_all_button = tk.Button(self.root, text="Очистить всё", command=self.clear_all_tasks, width=10)
        self.clear_all_button.pack(pady=5)

    def add_task(self):
        """Добавляет задачу в список."""
        task_text = self.task_entry.get().strip()  # Получаем текст из поля ввода

        if task_text:
            self.tasks.append(task_text)
            self.update_listbox()
            self.task_entry.delete(0, tk.END)  # Очищаем поле ввода
        else:
            messagebox.showwarning("Предупреждение", "Пожалуйста, введите задачу.")


    def delete_task(self):
        """Удаляет выбранную задачу из списка."""
        try:
            selected_index = self.task_listbox.curselection()[0]  # Получаем индекс выбранной задачи
            self.tasks.pop(selected_index)
            self.update_listbox()
        except IndexError:
            messagebox.showwarning("Предупреждение", "Пожалуйста, выберите задачу для удаления.")


    def clear_all_tasks(self):
        """Очищает весь список задач."""
        if messagebox.askyesno("Подтверждение", "Вы уверены, что хотите очистить весь список?"):
            self.tasks = []
            self.update_listbox()


    def update_listbox(self):
        """Обновляет содержимое Listbox."""
        self.task_listbox.delete(0, tk.END)  # Очищаем Listbox
        for task in self.tasks:
            self.task_listbox.insert(tk.END, task)


if __name__ == "__main__":
    root = tk.Tk()
    app = TodoListApp(root)
    root.mainloop()
