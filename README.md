main.py:
import os

# File to store tasks
TASKS_FILE = "tasks.txt"


# Load tasks from file
def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, "r") as file:
            tasks = [line.strip() for line in file.readlines()]
    else:
        tasks = []
    return tasks


# Save tasks to file
def save_tasks(tasks):
    with open(TASKS_FILE, "w") as file:
        for task in tasks:
            file.write(task + "\n")


# Display all tasks
def view_tasks(tasks):
    if not tasks:
        print("No tasks found.")
    else:
        for idx, task in enumerate(tasks, start=1):
            print(f"{idx}. {task}")


# Add a new task
def add_task(tasks):
    task = input("Enter the new task: ")
    tasks.append(task)
    save_tasks(tasks)
    print("Task added successfully!")


# Update a task as completed
def update_task(tasks):
    view_tasks(tasks)
    task_num = int(input("Enter the task number to mark as completed: "))
    if 0 < task_num <= len(tasks):
        tasks[task_num - 1] += " (completed)"
        save_tasks(tasks)
        print("Task updated successfully!")
    else:
        print("Invalid task number.")


# Delete a task
def delete_task(tasks):
    view_tasks(tasks)
    task_num = int(input("Enter the task number to delete: "))
    if 0 < task_num <= len(tasks):
        tasks.pop(task_num - 1)
        save_tasks(tasks)
        print("Task deleted successfully!")
    else:
        print("Invalid task number.")


# Main menu
def main_menu():
    tasks = load_tasks()

    while True:
        print("\nTo-Do List Application")
        print("1. View Tasks")
        print("2. Add Task")
        print("3. Update Task")
        print("4. Delete Task")
        print("5. Exit")

        choice = input("Choose an option: ")

        if choice == '1':
            view_tasks(tasks)
        elif choice == '2':
            add_task(tasks)
        elif choice == '3':
            update_task(tasks)
        elif choice == '4':
            delete_task(tasks)
        elif choice == '5':
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main_menu()


tudo gui:

import tkinter as tk
from tkinter import messagebox

TASKS_FILE = "tasks.txt"

def load_tasks():
    tasks = []
    try:
        with open(TASKS_FILE, "r") as file:
            tasks = [line.strip() for line in file.readlines()]
    except FileNotFoundError:
        pass
    return tasks

def save_tasks():
    with open(TASKS_FILE, "w") as file:
        tasks = listbox.get(0, tk.END)
        for task in tasks:
            file.write(task + "\n")

def add_task():
    task = entry.get()
    if task:
        listbox.insert(tk.END, task)
        entry.delete(0, tk.END)
        save_tasks()
    else:
        messagebox.showwarning("Warning", "You must enter a task.")

def delete_task():
    try:
        selected_task_index = listbox.curselection()[0]
        listbox.delete(selected_task_index)
        save_tasks()
    except IndexError:
        messagebox.showwarning("Warning", "You must select a task.")

def update_task():
    try:
        selected_task_index = listbox.curselection()[0]
        new_task = entry.get()
        if new_task:
            listbox.delete(selected_task_index)
            listbox.insert(selected_task_index, new_task)
            entry.delete(0, tk.END)
            save_tasks()
        else:
            messagebox.showwarning("Warning", "You must enter a new task.")
    except IndexError:
        messagebox.showwarning("Warning", "You must select a task to update.")

root = tk.Tk()
root.title("To-Do List")

frame = tk.Frame(root)
frame.pack(pady=10)

listbox = tk.Listbox(frame, width=50, height=10)
listbox.pack(side=tk.LEFT)

scrollbar = tk.Scrollbar(frame)
scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

listbox.config(yscrollcommand=scrollbar.set)
scrollbar.config(command=listbox.yview)

entry = tk.Entry(root, width=50)
entry.pack(pady=10)

button_frame = tk.Frame(root)
button_frame.pack(pady=10)

add_button = tk.Button(button_frame, text="Add Task", command=add_task)
add_button.pack(side=tk.LEFT)

update_button = tk.Button(button_frame, text="Update Task", command=update_task)
update_button.pack(side=tk.LEFT)

delete_button = tk.Button(button_frame, text="Delete Task", command=delete_task)
delete_button.pack(side=tk.LEFT)

# Load tasks when the application starts
tasks = load_tasks()
for task in tasks:
    listbox.insert(tk.END, task)

root.mainloop()
