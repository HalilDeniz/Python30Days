# **Last Project: Simple Notepad Application with Classes**

## **Project Description:**
Create a Simple Notepad Application in Python using classes. This project will help you improve your file handling, user input, and basic data management skills.

### **Key Features:**
1. Create a user interface that prompts the user to enter a note title and content.
2. Save user-entered notes to a text file.
3. Add a "View Notes" option so that the user can list the saved notes.
4. Add options that allow the user to delete or edit a specific note.

### **Extra Tips:**
- Allow the user to provide a unique ID for each note, so they can manage notes more easily.
- Get confirmation from the user when editing or deleting notes and add security checks against errors.
- You can add a custom display layout to display your notes by formatting or coloring them properly.
 
**Project Implementation:**

```python
import os
import os

class Notepad:
    def __init__(self):
        self.notes = []

    def create_note(self):
        note_id = input("Enter a unique ID for the note: ")
        title = input("Enter the note title: ")
        content = input("Enter the note content: ")
        self.notes.append({"id": note_id, "title": title, "content": content})
        print("Note has been created.")

    def view_notes(self):
        if not self.notes:
            print("No notes found.")
        else:
            print("Available Notes:")
            for note in self.notes:
                print(f"ID: {note['id']}, Title: {note['title']}")

    def edit_note(self):
        note_id = input("Enter the ID of the note you want to edit: ")
        for note in self.notes:
            if note['id'] == note_id:
                new_title = input(f"Enter a new title for note {note_id}: ")
                new_content = input(f"Enter new content for note {note_id}: ")
                note['title'] = new_title
                note['content'] = new_content
                print(f"Note {note_id} has been edited.")
                return
        print(f"No note found with ID {note_id}.")

    def delete_note(self):
        note_id = input("Enter the ID of the note you want to delete: ")
        for note in self.notes:
            if note['id'] == note_id:
                self.notes.remove(note)
                print(f"Note {note_id} has been deleted.")
                return
        print(f"No note found with ID {note_id}.")

    def save_notes_to_file(self):
        with open("notes.txt", "w") as file:
            for note in self.notes:
                file.write(f"ID: {note['id']}\n")
                file.write(f"Title: {note['title']}\n")
                file.write(f"Content: {note['content']}\n")
                file.write("\n")

    def load_notes_from_file(self):
        if os.path.isfile("notes.txt"):
            with open("notes.txt", "r") as file:
                lines = file.readlines()
                note_info = {}
                for line in lines:
                    if line.strip() == "":
                        if note_info:
                            self.notes.append(note_info)
                            note_info = {}
                    else:
                        key, value = line.strip().split(": ")
                        note_info[key.lower()] = value
                if note_info:
                    self.notes.append(note_info)
        else:
            print("No notes file found.")

    def run(self):
        self.load_notes_from_file()
        while True:
            print("\nSimple Notepad Application Menu:")
            print("1. Create a Note")
            print("2. View Notes")
            print("3. Edit a Note")
            print("4. Delete a Note")
            print("5. Save Notes to File")
            print("6. Exit")
            choice = input("Enter your choice: ")
            
            if choice == '1':
                self.create_note()
            elif choice == '2':
                self.view_notes()
            elif choice == '3':
                self.edit_note()
            elif choice == '4':
                self.delete_note()
            elif choice == '5':
                self.save_notes_to_file()
            elif choice == '6':
                print("Exiting Notepad Application.")
                break
            else:
                print("Invalid choice. Please try again.")

if __name__ == "__main__":
    notepad = Notepad()
    notepad.run()
```

**Project Outcome:**
By completing this project, you will have a functional Notepad Application that allows users to create, view, edit, and delete notes, as well as save and load notes from a file. This project will enhance your Python skills and provide practical experience in building text-based applications.

Feel free to customize and expand this project as you see fit, adding more features or improving the user interface to make it even more user-friendly. Enjoy coding!