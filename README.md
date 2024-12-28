# Project 

# File System Simulator

## Overview

A File System Simulator is like a simple software program that imitates how a computer stores and organizes files. It’s not a real file system but works in a similar way to help people understand or test how file systems work.

# Files and Folders:
Just like your computer has files and folders, the simulator lets you create, delete, or organize files and folders virtually.

# Memory Management: 
It shows how space is used in storage and helps you see what happens when memory fills up.

# File Operations: 
You can perform actions like opening, reading, writing, or deleting files, just like in a real system.

# Learning Tool: 
It’s useful for learning how computers handle files behind the scenes, such as how files are stored in blocks or how directories keep track of them.

## Usage
### Prerequisites
- Python or Java installed on your system.
- Basic knowledge of file systems.

## System Requirements
- **Operating Systems**: Windows, Linux
- **Hardware**: Minimum 2 GB RAM, 500 MB free disk space

## Future Enhancements
- Advanced visualization tools for file and disk structures.
- Support for real-time collaboration or shared simulations.
- Extended support for various file system types.

## Project code 

import tkinter as tk
from tkinter import ttk, messagebox

# Main Window
root = tk.Tk()
root.title("File System Simulator")
root.geometry("800x600")
root.configure(bg="#1e1e1e")

# Sidebar
sidebar = tk.Frame(root, bg="#2d2d2d", width=250)
sidebar.pack(side="left", fill="y")

# Controls Section
controls = tk.Frame(sidebar, bg="#2d2d2d")
controls.pack(pady=10)

name_input = tk.Entry(controls, width=25, bg="#333", fg="#d4d4d4", insertbackground="#d4d4d4")
name_input.pack(pady=5)

def create_item(item_type):
    name = name_input.get().strip()
    if not name:
        messagebox.showerror("Error", "Enter a valid name.")
        return
    if item_type == "folder":
        tree.insert(current_folder, "end", text=f"📁 {name}", values=("folder", ""))
    else:
        tree.insert(current_folder, "end", text=f"📄 {name}", values=("file", ""))

tk.Button(controls, text="Add Folder", command=lambda: create_item("folder")).pack(pady=5)
tk.Button(controls, text="Add File", command=lambda: create_item("file")).pack(pady=5)
tk.Button(controls, text="Delete Selected", command=lambda: tree.delete(current_folder)).pack(pady=5)

# File System Tree
tree = ttk.Treeview(sidebar)
tree.pack(expand=True, fill="both")
current_folder = ""

def on_select(event):
    global current_folder
    selected = tree.selection()
    if selected:
        current_folder = selected[0]
        item_type = tree.item(current_folder, "values")[0]
        if item_type == "file":
            editor_text.config(state="normal")
            editor_text.delete("1.0", "end")
            # Retrieve the full content of the file and display it
            file_content = tree.item(current_folder, "values")[1]
            editor_text.insert("1.0", file_content)
        else:
            editor_text.config(state="disabled")

tree.bind("<<TreeviewSelect>>", on_select)
tree.insert("", "end", iid="root", text="📁 Root", values=("folder", ""))

# Editor Frame
editor_frame = tk.Frame(root, bg="#2d2d2d")
editor_frame.pack(side="right", expand=True, fill="both")

tk.Label(editor_frame, text="File Editor", bg="#2d2d2d", fg="#d4d4d4", font=("Segoe UI", 16)).pack(pady=10)

# White Text Field
editor_text = tk.Text(editor_frame, width=60, height=25, bg="white", fg="black", insertbackground="black", font=("Consolas", 12))
editor_text.pack(padx=20, pady=10)
editor_text.config(state="disabled")

def save_file():
    if current_folder and tree.item(current_folder, "values")[0] == "file":
        content = editor_text.get("1.0", "end").strip()
        # Save the content back to the file node in the tree
        tree.item(current_folder, text=f"📄 {content[:20]}...")  # Optionally update the file name to the first 20 characters
        tree.item(current_folder, values=("file", content))  # Store full content in the second value
        messagebox.showinfo("Success", "File saved!")
    else:
        messagebox.showerror("Error", "Select a file to save.")

tk.Button(editor_frame, text="Save", command=save_file).pack(pady=10)

root.mainloop()
