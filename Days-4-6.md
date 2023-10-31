**Day 4: Conditional Statements**

Conditional statements are used to make decisions in your code.

**Example of `if` statement:**
```python
age = 18
if age >= 18:
    print("You are an adult.")
else:
    print("You are not an adult.")
```


**Example of `elif` (else if) statement:**
```python
grade = 75
if grade >= 90:
    print("A")
elif grade >= 80:
    print("B")
elif grade >= 70:
    print("C")
else:
    print("Fail")
```

**Day 5: Loops**

Loops allow you to perform repetitive tasks in your code.

**Example of `for` loop:**
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

**Example of `while` loop:**
```python
count = 0
while count < 5:
    print(count)
    count += 1
```

**Day 6: More on Loops**

You can use loops for various tasks, such as iterating over ranges of numbers.

**Example of looping through a range:**
```python
for i in range(5):  # This will loop from 0 to 4.
    print(i)
```

**Example of using `break` and `continue` statements:**
```python
for i in range(10):
    if i == 3:
        continue  # Skip iteration when i equals 3.
    if i == 8:
        break  # Exit the loop when i equals 8.
    print(i)
```

Conditional statements and loops are fundamental building blocks in Python and in programming in general. They allow you to control the flow of your program and perform repetitive tasks efficiently. Practice with these examples to become more comfortable with using them in your Python code.
