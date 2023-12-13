# To-Do List Application

This simple To-Do List application helps you keep track of your tasks and manage them efficiently. It is developed using Python and utilizes an SQLite database for data storage.

## About the Project

This To-Do List application allows users to add, list, mark as complete, or delete tasks. It uses a simple checkbox to track the completion status of tasks and stores tasks permanently using an SQLite database.

## Project Structure

- The application contains a `ToDoApp` class responsible for handling SQLite database operations.
- It uses a table named "todos" in the database.
- It provides a basic command-line interface for user interaction.

## How to Use

1. Create an SQLite database file named `todo.db` in the project directory or rename it as needed.
2. Run the application: `python todo_app.py`.
3. When the application starts, you can perform the following operations:
   - Choose "1" to add a task.
   - Select "2" to list tasks.
   - Perform task marking or deletion operations.
   - Choose "5" to exit the application.

## Why This Project

This project helps you enhance your Python programming skills while learning how to work with databases. It also involves creating a basic application structure and handling user input.

## Suggested Enhancements

To customize or extend this basic To-Do List application, consider the following enhancements:

- Create a more user-friendly command-line interface.
- Add features to categorize tasks.
- Consider adding task dates and reminders.
- Manage task priorities.
- Develop a web-based version or improve the existing application interface.

## Code Examples

```python
import sqlite3

class ToDoApp:
    def __init__(self, db_name):
        self.conn = sqlite3.connect(db_name)
        self.cursor = self.conn.cursor()
        self.create_table()

    def create_table(self):
        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS todos (
                id INTEGER PRIMARY KEY,
                task TEXT,
                completed BOOLEAN
            )
        ''')
        self.conn.commit()

    def add_task(self, task):
        self.cursor.execute("INSERT INTO todos (task, completed) VALUES (?, ?)", (task, False))
        self.conn.commit()

    def list_tasks(self):
        self.cursor.execute("SELECT * FROM todos")
        tasks = self.cursor.fetchall()
        return tasks

    def mark_task_completed(self, task_id):
        self.cursor.execute("UPDATE todos SET completed = ? WHERE id = ?", (True, task_id))
        self.conn.commit()

    def delete_task(self, task_id):
        self.cursor.execute("DELETE FROM todos WHERE id = ?", (task_id,))
        self.conn.commit()

    def close(self):
        self.conn.close()

def main():
    todo_app = ToDoApp("todo.db")

    while True:
        print("\nTo-Do List Application")
        print("1. Add Task")
        print("2. List Tasks")
        print("3. Mark Task Completed")
        print("4. Delete Task")
        print("5. Exit")

        choice = input("Select an operation: ")

        if choice == "1":
            task = input("Enter the task: ")
            todo_app.add_task(task)
            print("Task added.")

        elif choice == "2":
            tasks = todo_app.list_tasks()
            if tasks:
                print("\nTask List:")
                for task in tasks:
                    task_id, task_text, completed = task
                    status = "Completed" if completed else "Not Completed"
                    print(f"Task ID: {task_id}, Task: {task_text}, Status: {status}")
            else:
                print("No tasks found.")

        elif choice == "3":
            task_id = input("Enter the task ID to mark as completed: ")
            todo_app.mark_task_completed(task_id)
            print("Task marked as completed.")

        elif choice == "4":
            task_id = input("Enter the task ID to delete: ")
            todo_app.delete_task(task_id)
            print("Task deleted.")

        elif choice == "5":
            todo_app.close()
            break

if __name__ == "__main__":
    main()
```
Feel free to customize it further to include specific details about your To-Do List application.




