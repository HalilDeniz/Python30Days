### **Day 4: Conditional Statements**
Conditional statements are crucial for decision-making in your code.

**`if` Statement:**
- The `if` statement allows you to execute a block of code if a condition is True.
- Example:

```python
age = 18
if age >= 18:
    print("You are an adult.")
else:
    print("You are not an adult.")
```

**`elif` (else if) Statement:**
- The `elif` statement is used for multiple conditions within the same block of code.
- Example:

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

### **Day 5: Loops**
Loops are essential for performing repetitive tasks in your code.

**`for` Loop:**
- The `for` loop is used to iterate over a sequence, such as a list, and execute a block of code for each item.
- Example:

```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

**`while` Loop:**
- The `while` loop continues executing as long as a specified condition is True.
- Example:

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

### **Day 6: More on Loops**
You can use loops for various tasks and fine-tune their behavior.

**Looping Through a Range:**
- The `range()` function generates a sequence of numbers that can be used in loops.
- Example:

```python
for i in range(5):  # This will loop from 0 to 4.
    print(i)
```

**Using `break` and `continue` Statements:**
- The `break` statement exits a loop prematurely when a certain condition is met.
- The `continue` statement skips the current iteration and moves to the next one.
- Example:

```python
for i in range(10):
    if i == 3:
        continue  # Skip iteration when i equals 3.
    if i == 8:
        break  # Exit the loop when i equals 8.
    print(i)
```

Conditional statements and loops are fundamental constructs in Python programming. They provide you with the tools to control the flow of your program and efficiently handle repetitive tasks. Practicing with these examples will help you become more proficient in using them in your Python code.