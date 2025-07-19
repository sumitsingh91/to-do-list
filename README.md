# to-do-list
import tkinter as tk
from tkinter import messagebox

class ToDoListApp:
    def _init_(self, root):
        self.root = root
        self.root.title("To-Do List Application")
        self.root.geometry("400x400")
        self.root.configure(bg='lightblue')

        # Title label
        self.title_label = tk.Label(root, text="To-Do List", font=("Helvetica", 16, "bold"), bg='lightblue')
        self.title_label.pack(pady=10)

        # Frame for listbox and scrollbar
        frame = tk.Frame(root)
        frame.pack(pady=10)

        # Listbox to display tasks
        self.listbox = tk.Listbox(frame, width=40, height=10, font=("Helvetica", 12))
        self.listbox.pack(side=tk.LEFT, fill=tk.BOTH)

        # Scrollbar for the listbox
        scrollbar = tk.Scrollbar(frame)
        scrollbar.pack(side=tk.RIGHT, fill=tk.BOTH)
        self.listbox.config(yscrollcommand=scrollbar.set)
        scrollbar.config(command=self.listbox.yview)

        # Entry widget to add new tasks
        self.task_entry = tk.Entry(root, width=40, font=("Helvetica", 12))
        self.task_entry.pack(pady=10)

        # Buttons frame
        btn_frame = tk.Frame(root)
        btn_frame.pack(pady=10)

        # Add task button
        add_btn = tk.Button(btn_frame, text="Add Task", width=12, command=self.add_task)
        add_btn.grid(row=0, column=0, padx=5)

        # Delete task button
        del_btn = tk.Button(btn_frame, text="Delete Task", width=12, command=self.delete_task)
        del_btn.grid(row=0, column=1, padx=5)

        # Mark completed button
        mark_btn = tk.Button(btn_frame, text="Mark Completed", width=12, command=self.mark_completed)
        mark_btn.grid(row=0, column=2, padx=5)

    def add_task(self):
        task = self.task_entry.get().strip()
        if task == "":
            messagebox.showwarning("Warning", "Please enter a task.")
        else:
            self.listbox.insert(tk.END, task)
            self.task_entry.delete(0, tk.END)

    def delete_task(self):
        try:
            selected_index = self.listbox.curselection()[0]
            self.listbox.delete(selected_index)
        except IndexError:
            messagebox.showwarning("Warning", "Please select a task to delete.")

    def mark_completed(self):
        try:
            selected_index = self.listbox.curselection()[0]
            task = self.listbox.get(selected_index)
            if not task.endswith(" ✔"):
                self.listbox.delete(selected_index)
                self.listbox.insert(selected_index, task + " ✔")
        except IndexError:
            messagebox.showwarning("Warning", "Please select a task to mark as completed.")

if _name_ == "_main_":
    root = tk.Tk()
    app = ToDoListApp(root)
    root.mainloop()
