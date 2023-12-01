# Random Password Generator

The "Random Password Generator" is a Python program that allows users to generate random passwords based on their preferences. This project is designed to help you practice Python programming and gain experience in working with strings, randomization, and user interaction.

## Key Features:

1. **Generate Password:** Users can create random passwords of specified lengths with options for uppercase letters, lowercase letters, digits, and symbols.
2. **Save Password:** The program offers the option to save generated passwords to a file for later use.
3. **User-Friendly Interface:** The user interacts with the program through a command-line interface, making it easy to use.

## How to Use:

1. Run the Python program.
2. Specify the desired password length.
3. Choose whether to include uppercase letters, lowercase letters, digits, and symbols in the password.
4. The program generates a random password based on your preferences.
5. You can choose to save the password to a file.

## Suggested Enhancements:

To further expand and enhance this project, you can consider adding features like:

- Password Strength Evaluation: Implement a feature that evaluates the strength of the generated passwords.
- Password History: Allow users to store and manage a history of generated passwords.
- Custom Character Sets: Enable users to define their custom character sets for password generation.

Feel free to explore and customize this project to suit your learning goals and interests. Happy coding!


```python
import random
import string

class PasswordGenerator:
    def __init__(self):
        pass

    def generate_password(self, length, use_uppercase, use_lowercase, use_digits, use_symbols):
        # Ready-made character sets
        uppercase_letters = string.ascii_uppercase
        lowercase_letters = string.ascii_lowercase
        digits = string.digits
        symbols = string.punctuation

        # Create the character set based on user choices
        characters = ""
        if use_uppercase:
            characters += uppercase_letters
        if use_lowercase:
            characters += lowercase_letters
        if use_digits:
            characters += digits
        if use_symbols:
            characters += symbols

        # Generate a random password of the specified length
        if characters:
            password = ''.join(random.choice(characters) for _ in range(length))
            return password
        else:
            return "Character set not selected."

    def save_password_to_file(self, password, filename):
        with open(filename, "w") as file:
            file.write(password)

    def main(self):
        print("Random Password Generator")

        length = int(input("Password Length: "))

        use_uppercase = input("Use Uppercase Letters? (Y/N): ").lower() == "y"
        use_lowercase = input("Use Lowercase Letters? (Y/N): ").lower() == "y"
        use_digits = input("Use Digits? (Y/N): ").lower() == "y"
        use_symbols = input("Use Symbols? (Y/N): ").lower() == "y"

        password = self.generate_password(length, use_uppercase, use_lowercase, use_digits, use_symbols)

        print("\nGenerated Password:", password)

        save_to_file = input("Save the password to a file? (Y/N): ").lower() == "y"
        if save_to_file:
            filename = input("Enter the filename: ")
            self.save_password_to_file(password, filename)
            print(f"Password saved to {filename}")

if __name__ == "__main__":
    password_generator = PasswordGenerator()
    password_generator.main()
```

## Example Output:

```bash
$ python random_password_generator.py

Random Password Generator

Password Length: 12
Use Uppercase Letters? (Y/N): Y
Use Lowercase Letters? (Y/N): Y
Use Digits? (Y/N): Y
Use Symbols? (Y/N): N

Generated Password: AbCdeFgH1234

Save the password to a file? (Y/N): Y
Enter the filename: my_password.txt
Password saved to my_password.txt
```

Before running the program, make sure you have Python installed on your system. Enjoy generating random passwords!