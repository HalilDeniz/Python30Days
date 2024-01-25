# **Dictionary Application**

The "Dictionary Application" is a simple Python program that allows users to manage a dictionary of words, their meanings, and example sentences. This project is designed to help you practice your Python programming skills and gain experience in working with databases. It offers the following features:

### Key Features:

1. **Add Word:** Users can add new words to the dictionary, along with their meanings and example sentences.
2. **List Words:** The application displays a list of all the words in the dictionary, including their meanings and example sentences.
3. **Search Word:** Users can search for a specific word in the dictionary to find its meaning and example sentence.
4. **Delete Word:** The application allows users to delete words from the dictionary.

### Project Structure:

- The program uses an SQLite database to store word data, with a table named "words" to manage entries.
- It includes a `DictionaryApp` class that encapsulates the functionality of the application, making it organized and easy to maintain.
- The user interacts with the application through a simple command-line interface.

### How to Use:

1. Run the Python program.
2. Select from the available options to add words, list words, search for words, delete words, or exit the application.

### Why This Project?

This project serves as a practical exercise for beginner and intermediate Python programmers. It covers fundamental concepts such as working with databases, user input, and class-based application structure. By completing this project, you will improve your Python skills and be better equipped for more complex programming tasks.


### Suggested Enhancements:

To further expand and enhance this project, you can consider adding features like:

- Update/Edit Word: Allow users to modify the meaning or example sentence of an existing word.
- User Authentication: Implement user accounts and authentication for managing personal dictionaries.
- Better User Interface: Develop a graphical user interface (GUI) for a more user-friendly experience.

Feel free to explore and customize this project to suit your learning goals and interests. Happy coding!



```python
import sqlite3

class DictionaryApp:
    def __init__(self,db_name):
        self.conn = sqlite3.connect(db_name)
        self.cursor = self.conn.cursor()
        self.create_table()

    def create_table(self):
        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS words(
                id INTEGER PRIMARY KEY,
                word TEXT,
                meaning TEXT,
                sentence TEXT            
            )
            ''')
        self.conn.commit()

    def add_word(self,word,meaning,sentence):
        self.cursor.execute("INSERT INTO words (word, meaning,sentence) VALUES ( ?, ?, ?)",(word,meaning,sentence))
        self.conn.commit()

    def list_words(self):
        self.cursor.execute("SELECT * FROM words")
        words = self.cursor.fetchall()
        return words
    
    def search_word(self, word):
        self.cursor.execute("SELECT * FROM words WHERE word LIKE ? ORDER BY word ASC", ('%' + word + '%',))
        word = self.cursor.fetchall()
        return word
    
    def delete_word(self,word):
        self.cursor.execute('DELETE FROM words WHERE word = ?',(word,))
        self.conn.commit()

    def close(self):
        self.conn.close()

def main():
    dictionary  = DictionaryApp("words.db")
    while True:
        print("\n Dictionary Application")
        print("1 : Add Words")        
        print("2 : list Words")
        print("3 : Search Words")
        print("4 : Delete Words")
        print("5 : Exit")

        choice = input("Select an operations : ")
        if choice == "1" :
            word = input("Word: ")
            meaning = input("Meaning: ")
            sentence = input("Example Sentence: ")
            dictionary.add_word(word,meaning,sentence)
            print("Word added successfully...")

        elif choice == "2" :
            words = dictionary.list_words()
            if words:
                print("\nWord List : ")
                for word in words:
                    print(f"Words : {word[1]}, \nMeaning : {word[2]}, \nExample Sentence : {word[3]}")
            else :
                print("no words are added here now")

        elif choice == "3":
            word = input("Enter Word to Search: ")
            print(word)
            result = dictionary.search_word(word)
            if result:
                for word in result:
                    print(f"Words : {word[1]}, \nMeaning : {word[2]}, \nExample Sentence : {word[3]}")
            else:
                print("Word not found.")

        elif choice == "4":
            word = input("Enter Word to Delete: ")
            dictionary.delete_word(word)
            print("Word deleted.")

        elif choice == "5":
            dictionary.close()
            break

if __name__ == "__main__":
    main()

```

This program creates a class called `DictionaryApp` that allows users to add words, list words, search for words, and delete words. It uses an SQLite database named "words.db" to store word data. Before running the program, make sure to create the "words.db" SQLite database file in the same directory or change the database name accordingly.