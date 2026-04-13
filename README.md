# 📘 Chapter 9: Dictionaries and Sets
### *Starting Out with Python — Textbook Companion & Practice Guide*

---

## 📚 Table of Contents

1. [Dictionaries](#1-dictionaries)
2. [Retrieving Values](#2-retrieving-values)
3. [Adding and Modifying Elements](#3-adding-and-modifying-elements)
4. [Deleting Elements](#4-deleting-elements)
5. [Iterating Over a Dictionary](#5-iterating-over-a-dictionary)
6. [Dictionary Methods](#6-dictionary-methods)
7. [Dictionary Comprehensions](#7-dictionary-comprehensions)
8. [Sets](#8-sets)
9. [Modifying a Set](#9-modifying-a-set)
10. [Set Operations](#10-set-operations)
11. [Subsets and Supersets](#11-subsets-and-supersets)
12. [Set Comprehensions](#12-set-comprehensions)
13. [Serializing Objects](#13-serializing-objects)

---

## 1. Dictionaries

A **dictionary** is a collection of data where each element is stored as a **key-value pair**. Instead of using a numeric index to access data (as with lists), you use a **key** — a unique label that maps directly to its associated value.

### Creating a Dictionary

```python
# Format: dictionary = {key1: value1, key2: value2}
student = {
    'name'  : 'Alex Johnson',
    'grade' : 92,
    'major' : 'Computer Science'
}
```

### Key Rules

- Keys must be **immutable** objects (strings, numbers, or tuples)
- Keys must be **unique** — no two entries can share the same key
- Values can be **any type** — strings, numbers, lists, even other dictionaries
- Dictionaries are **unordered** — elements are not stored in a guaranteed sequence

```python
# Mixed value types are fine
contact = {
    'name'   : 'Maria',
    'age'    : 28,
    'scores' : [88, 92, 95]
}
```

> 💡 Dictionaries are ideal whenever data naturally comes in labeled pairs — like a contact book (name → phone), a grade book (student → score), or a settings file (option → value).

**Quick Check:**

<details>
<summary>What is the difference between a dictionary and a list?</summary>

A **list** uses numeric indexes to access elements — position matters. A **dictionary** uses keys — meaningful labels — to access values. Position does not matter.

```python
# List — accessed by index number
student_list = ['Alex', 92, 'CS']
print(student_list[0])      # Output: Alex

# Dictionary — accessed by key name
student_dict = {'name': 'Alex', 'grade': 92, 'major': 'CS'}
print(student_dict['name']) # Output: Alex
```

Dictionaries are preferred when data has natural labels. Lists are preferred when order and position matter.

</details>

<details>
<summary>Why must dictionary keys be immutable?</summary>

Python uses each key's value to compute a **hash** — a fixed numeric code used to quickly look up data. Mutable objects (like lists) can change after being added, which would break the lookup system.

```python
# Valid keys — all immutable
d = {
    'name'  : 'Alex',   # string key
    42      : 'answer', # integer key
    (1, 2)  : 'point'   # tuple key
}

# Invalid key — lists are mutable
d[[1, 2]] = 'point'  # ❌ TypeError: unhashable type: 'list'
```

In practice, string keys are by far the most common.

</details>

---

## 2. Retrieving Values

Use the key inside square brackets to retrieve its associated value:

```python
student = {'name': 'Alex', 'grade': 92}

print(student['name'])    # Output: Alex
print(student['grade'])   # Output: 92
```

> ⚠️ If the key does not exist, Python raises a **KeyError** exception. Always verify a key exists before accessing it, or use the `get()` method (covered in Section 6).

### Checking for a Key with `in`

Use the `in` operator to safely check whether a key exists before accessing it:

```python
if 'grade' in student:
    print(student['grade'])

print('major' in student)      # Output: False
print('major' not in student)  # Output: True
```

**Quick Check:**

<details>
<summary>What happens if you try to access a key that doesn't exist?</summary>

Python raises a **KeyError** exception, which will crash your program if not handled:

```python
student = {'name': 'Alex', 'grade': 92}
print(student['major'])   # ❌ KeyError: 'major'
```

Two safe alternatives:

```python
# Option 1 — check first with in
if 'major' in student:
    print(student['major'])

# Option 2 — use get() with a default value
print(student.get('major', 'Not declared'))   # Output: Not declared
```

</details>

---

## 3. Adding and Modifying Elements

Dictionaries are **mutable** — you can add new key-value pairs or change existing values at any time.

### Adding a New Pair

```python
student = {'name': 'Alex', 'grade': 92}
student['major'] = 'Computer Science'   # Adds new key-value pair
print(student)
# Output: {'name': 'Alex', 'grade': 92, 'major': 'Computer Science'}
```

### Updating an Existing Value

Using the same syntax with an existing key **overwrites** the old value:

```python
student['grade'] = 98    # Updates existing key
print(student['grade'])  # Output: 98
```

> 💡 Python uses the same syntax for adding and updating. If the key already exists, the value is replaced. If it doesn't exist, a new pair is created.

**Quick Check:**

<details>
<summary>How do you add a new key-value pair to an existing dictionary?</summary>

Use square bracket notation with the new key on the left and the value on the right:

```python
inventory = {'apples': 50, 'oranges': 20}

inventory['bananas'] = 30    # Add new pair
inventory['apples']  = 65    # Update existing value

print(inventory)
# Output: {'apples': 65, 'oranges': 20, 'bananas': 30}
```

There is no separate "add" method — the same assignment syntax handles both adding and updating.

</details>

---

## 4. Deleting Elements

Use the `del` statement to remove a key-value pair from a dictionary:

```python
student = {'name': 'Alex', 'grade': 92, 'major': 'CS'}

del student['major']
print(student)   # Output: {'name': 'Alex', 'grade': 92}
```

> ⚠️ If the key does not exist, `del` raises a **KeyError**. Use `in` to check first if you're unsure.

To remove all elements at once, use the `clear()` method:

```python
student.clear()
print(student)   # Output: {}
```

**Quick Check:**

<details>
<summary>What is the difference between del and clear() on a dictionary?</summary>

- **`del dictionary[key]`** removes one specific key-value pair.
- **`dictionary.clear()`** removes all key-value pairs, leaving an empty dictionary.

```python
scores = {'Alice': 88, 'Bob': 74, 'Charlie': 95}

del scores['Bob']       # Remove one pair
print(scores)           # Output: {'Alice': 88, 'Charlie': 95}

scores.clear()          # Remove everything
print(scores)           # Output: {}
```

</details>

---

## 5. Iterating Over a Dictionary

Use a `for` loop to visit every key in a dictionary. By default, the loop iterates over keys:

```python
scores = {'Alice': 88, 'Bob': 74, 'Charlie': 95}

for name in scores:
    print(name)
# Output:
# Alice
# Bob
# Charlie
```

### Using `.keys()`, `.values()`, and `.items()`

```python
# Iterate over keys only
for name in scores.keys():
    print(name)

# Iterate over values only
for score in scores.values():
    print(score)

# Iterate over key-value pairs together
for name, score in scores.items():
    print(name, 'scored', score)
# Output:
# Alice scored 88
# Bob scored 74
# Charlie scored 95
```

> 💡 `.items()` is the most versatile — it gives you both the key and value in each iteration, unpacked into two variables.

**Quick Check:**

<details>
<summary>What does .items() return and how do you use it in a loop?</summary>

`.items()` returns a view of the dictionary's key-value pairs, where each pair is a **tuple**. When used in a `for` loop, you can unpack each tuple into two separate variables:

```python
grades = {'Alice': 88, 'Bob': 74}

for name, score in grades.items():
    print(f'{name}: {score}')
# Output:
# Alice: 88
# Bob: 74
```

You can also access the tuple directly without unpacking:

```python
for pair in grades.items():
    print(pair)
# Output:
# ('Alice', 88)
# ('Bob', 74)
```

</details>

---

## 6. Dictionary Methods

Python dictionaries include several built-in methods for working with their data.

### Method Reference

| Method | Description |
|--------|-------------|
| `clear()` | Removes all key-value pairs from the dictionary |
| `get(key, default)` | Returns the value for `key`, or `default` if the key is not found — never raises `KeyError` |
| `items()` | Returns all key-value pairs as a sequence of tuples |
| `keys()` | Returns all keys as a sequence |
| `values()` | Returns all values as a sequence |
| `pop(key, default)` | Returns and removes the value for `key`, or returns `default` if not found |
| `popitem()` | Returns and removes the last added key-value pair as a tuple |

### The `get()` Method

`get()` is the safe alternative to `dictionary[key]` when you're unsure if a key exists:

```python
student = {'name': 'Alex', 'grade': 92}

print(student.get('grade', 0))    # Output: 92  (key found)
print(student.get('major', 'N/A')) # Output: N/A (key not found — no error)
```

**Quick Check:**

<details>
<summary>When should you use get() instead of bracket notation?</summary>

Use `get()` whenever a key might not exist and you want to provide a fallback value instead of crashing:

```python
scores = {'Alice': 88, 'Bob': 74}

# Bracket notation — crashes if key missing
print(scores['Charlie'])              # ❌ KeyError

# get() — safe, returns default instead
print(scores.get('Charlie', 0))       # ✅ Output: 0
print(scores.get('Charlie', 'N/A'))   # ✅ Output: N/A
```

A common pattern is using `get()` with a default of `0` when building a frequency counter:

```python
word_count = {}
words = ['apple', 'banana', 'apple', 'cherry']

for word in words:
    word_count[word] = word_count.get(word, 0) + 1

print(word_count)   # Output: {'apple': 2, 'banana': 1, 'cherry': 1}
```

</details>

<details>
<summary>What does pop() do and how is it different from del?</summary>

- **`del dictionary[key]`** removes the pair but gives you nothing back.
- **`dictionary.pop(key, default)`** removes the pair AND returns the value, so you can use it before it's gone.

```python
scores = {'Alice': 88, 'Bob': 74, 'Charlie': 95}

removed = scores.pop('Bob', 0)    # Removes 'Bob' and returns 74
print(removed)                    # Output: 74
print(scores)                     # Output: {'Alice': 88, 'Charlie': 95}
```

</details>

---

## 7. Dictionary Comprehensions

A **dictionary comprehension** creates a new dictionary in a single compact expression — similar to a list comprehension, but producing key-value pairs instead.

### Syntax

```python
{key_expression : value_expression for variable in iterable}
```

### Examples

```python
# Create a dictionary of squares
numbers = [1, 2, 3, 4]
squares = {item : item**2 for item in numbers}
print(squares)   # Output: {1: 1, 2: 4, 3: 9, 4: 16}

# Map strings to their lengths
names = ['Jeremy', 'Kate', 'Peg']
lengths = {name : len(name) for name in names}
print(lengths)   # Output: {'Jeremy': 6, 'Kate': 4, 'Peg': 3}

# Copy a dictionary
dict1 = {'A': 1, 'B': 2, 'C': 3}
dict2 = {k : v for k, v in dict1.items()}
print(dict2)     # Output: {'A': 1, 'B': 2, 'C': 3}
```

### Filtering with `if`

```python
# Keep only cities with population over 2 million
populations = {
    'New York'    : 8398748,
    'Los Angeles' : 3990456,
    'Phoenix'     : 1660272
}

large_cities = {k : v for k, v in populations.items() if v > 2000000}
print(large_cities)
# Output: {'New York': 8398748, 'Los Angeles': 3990456}
```

**Quick Check:**

<details>
<summary>How is a dictionary comprehension different from a list comprehension?</summary>

A **list comprehension** produces a list and uses square brackets `[]`. A **dictionary comprehension** produces a dictionary and uses curly braces `{}` with a `key:value` pair as the result expression.

```python
numbers = [1, 2, 3, 4]

# List comprehension — produces a list
squares_list = [n**2 for n in numbers]
print(squares_list)    # Output: [1, 4, 9, 16]

# Dictionary comprehension — produces a dictionary
squares_dict = {n : n**2 for n in numbers}
print(squares_dict)    # Output: {1: 1, 2: 4, 3: 9, 4: 16}
```

</details>

---

## 8. Sets

A **set** is a collection that stores unique, unordered elements — like a mathematical set. If you add a duplicate, it is silently ignored.

### Creating a Set

```python
# Use set() with a list argument
colors = set(['red', 'blue', 'green', 'red', 'blue'])
print(colors)   # Output: {'red', 'blue', 'green'}  — duplicates removed

# Create an empty set — must use set(), NOT {}
empty = set()   # ✅ Empty set
empty = {}      # ❌ This creates an empty DICTIONARY, not a set
```

### Key Properties

- **Unordered** — elements have no guaranteed position
- **Unique** — duplicates are automatically discarded
- **Mutable** — you can add and remove elements
- Elements must be **immutable** (strings, numbers, tuples)

> 💡 Sets are ideal for removing duplicates from a collection, or for performing mathematical set operations like union and intersection.

**Quick Check:**

<details>
<summary>How is a set different from a list?</summary>

| Feature | List | Set |
|---------|------|-----|
| Ordered? | ✅ Yes | ❌ No |
| Allows duplicates? | ✅ Yes | ❌ No |
| Indexed access? | ✅ Yes | ❌ No |
| Mutable? | ✅ Yes | ✅ Yes |

```python
my_list = [1, 2, 2, 3, 3, 3]
my_set  = set(my_list)

print(my_list)   # Output: [1, 2, 2, 3, 3, 3]  — duplicates kept
print(my_set)    # Output: {1, 2, 3}            — duplicates removed
```

A common pattern is converting a list to a set to quickly remove duplicates, then converting back if needed:

```python
unique = list(set(my_list))
```

</details>

---

## 9. Modifying a Set

### Adding Elements

```python
colors = {'red', 'blue'}

colors.add('green')           # Add one element
colors.update(['yellow', 'orange'])  # Add multiple elements
print(colors)   # Output: {'red', 'blue', 'green', 'yellow', 'orange'}
```

### Removing Elements

| Method | Behavior when element not found |
|--------|--------------------------------|
| `remove(item)` | Raises a **KeyError** |
| `discard(item)` | Does nothing — no error raised |
| `clear()` | Removes all elements |

```python
colors = {'red', 'blue', 'green'}

colors.remove('blue')     # ✅ Removes 'blue'
colors.remove('purple')   # ❌ KeyError — 'purple' not in set

colors.discard('green')   # ✅ Removes 'green'
colors.discard('purple')  # ✅ No error — silently ignored
```

> 💡 Use `discard()` when you're not certain the element exists. Use `remove()` only when you know it's there and want an error if it isn't.

**Quick Check:**

<details>
<summary>What is the difference between remove() and discard() on a set?</summary>

Both remove a specified element, but they handle a missing element differently:

```python
fruits = {'apple', 'banana', 'cherry'}

fruits.remove('banana')    # ✅ Removes 'banana'
fruits.remove('grape')     # ❌ KeyError: 'grape'

fruits.discard('cherry')   # ✅ Removes 'cherry'
fruits.discard('grape')    # ✅ No error — nothing happens
```

Use `discard()` for safety when the element may or may not be in the set.

</details>

---

## 10. Set Operations

Python sets support the four classic mathematical set operations.

### Union — all elements from both sets

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a.union(b))   # Output: {1, 2, 3, 4, 5}
print(a | b)        # Output: {1, 2, 3, 4, 5}   (same result)
```

### Intersection — only elements found in both sets

```python
print(a.intersection(b))  # Output: {3}
print(a & b)              # Output: {3}
```

### Difference — elements in the first set but not the second

```python
print(a.difference(b))    # Output: {1, 2}
print(a - b)              # Output: {1, 2}
```

### Symmetric Difference — elements in either set but not both

```python
print(a.symmetric_difference(b))  # Output: {1, 2, 4, 5}
print(a ^ b)                      # Output: {1, 2, 4, 5}
```

**Quick Check:**

<details>
<summary>What is the difference between union and intersection?</summary>

- **Union** (`|`) gives you everything from both sets combined — no duplicates.
- **Intersection** (`&`) gives you only what both sets have in common.

```python
enrolled_mon = {'Alice', 'Bob', 'Charlie'}
enrolled_wed = {'Bob', 'Diana', 'Charlie'}

# Who is enrolled on either day?
print(enrolled_mon | enrolled_wed)
# Output: {'Alice', 'Bob', 'Charlie', 'Diana'}

# Who is enrolled on both days?
print(enrolled_mon & enrolled_wed)
# Output: {'Bob', 'Charlie'}
```

</details>

<details>
<summary>What does symmetric difference return?</summary>

The symmetric difference returns elements that are in one set or the other, but **not in both** — it's the opposite of intersection.

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a ^ b)   # Output: {1, 2, 5, 6}
# 3 and 4 are in both, so they are excluded
```

Think of it as "everything except what they share."

</details>

---

## 11. Subsets and Supersets

A **subset** is a set whose elements are all contained in another set. A **superset** is the reverse — it contains all elements of another set.

```python
a = {1, 2, 3}
b = {1, 2, 3, 4, 5}

# Is a a subset of b?
print(a.issubset(b))    # Output: True
print(a <= b)           # Output: True

# Is b a superset of a?
print(b.issuperset(a))  # Output: True
print(b >= a)           # Output: True
```

**Quick Check:**

<details>
<summary>What is the difference between a subset and a superset?</summary>

- Set A is a **subset** of set B if every element of A is also in B.
- Set B is a **superset** of set A if B contains every element of A.

They describe the same relationship from opposite directions:

```python
required = {'Python', 'SQL'}
student_skills = {'Python', 'SQL', 'JavaScript', 'HTML'}

print(required.issubset(student_skills))    # True — required is inside student_skills
print(student_skills.issuperset(required))  # True — student_skills contains required
```

</details>

---

## 12. Set Comprehensions

A **set comprehension** creates a new set using a compact single-line expression — identical in structure to a list comprehension, but enclosed in curly braces `{}`.

```python
# Copy a set
set1 = {1, 2, 3, 4, 5}
set2 = {item for item in set1}
print(set2)   # Output: {1, 2, 3, 4, 5}

# Create a set of squares
set3 = {item**2 for item in set1}
print(set3)   # Output: {1, 4, 9, 16, 25}

# Filter with if
set4 = {item for item in {1, 20, 2, 40, 3, 50} if item < 10}
print(set4)   # Output: {1, 2, 3}
```

> 💡 Because sets are unordered, the output order may vary each time the program runs — but the elements will always be correct.

**Quick Check:**

<details>
<summary>How do you tell a set comprehension apart from a dictionary comprehension?</summary>

Both use curly braces `{}`, but the key difference is the result expression:

- **Set comprehension** — one expression per element: `{item**2 for item in numbers}`
- **Dictionary comprehension** — a `key:value` pair per element: `{item: item**2 for item in numbers}`

```python
numbers = [1, 2, 3]

set_result  = {n**2 for n in numbers}       # Set:  {1, 4, 9}
dict_result = {n: n**2 for n in numbers}    # Dict: {1: 1, 2: 4, 3: 9}
```

</details>

---

## 13. Serializing Objects

**Serialization** (also called **pickling**) converts a Python object — like a dictionary or list — into a stream of bytes that can be saved to a file and reloaded later.

### Pickling (Saving)

```python
import pickle

my_dict = {'name': 'Alex', 'grade': 92}

# Open file in binary write mode
with open('data.pkl', 'wb') as f:
    pickle.dump(my_dict, f)   # Serialize and write
```

### Unpickling (Loading)

```python
import pickle

# Open file in binary read mode
with open('data.pkl', 'rb') as f:
    loaded_dict = pickle.load(f)   # Read and deserialize

print(loaded_dict)   # Output: {'name': 'Alex', 'grade': 92}
```

> 💡 You can pickle multiple objects to the same file by calling `pickle.dump()` multiple times before closing. Unpickle them in the same order using multiple `pickle.load()` calls.

**Quick Check:**

<details>
<summary>What is pickling and why would you use it?</summary>

Pickling converts a Python object into bytes so it can be stored in a file and restored later. This is useful when you want to save the state of a program between runs — for example, saving a dictionary of scores so the data persists after the program closes.

```python
import pickle

scores = {'Alice': 88, 'Bob': 74}

# Save
with open('scores.pkl', 'wb') as f:
    pickle.dump(scores, f)

# Load back later
with open('scores.pkl', 'rb') as f:
    scores = pickle.load(f)

print(scores)   # Output: {'Alice': 88, 'Bob': 74}
```

The `'wb'` mode means write binary, and `'rb'` means read binary — pickle files must always be opened in binary mode.

</details>

---

## 📝 Key Terms Summary

| Term | Definition |
|------|------------|
| **Dictionary** | A collection of key-value pairs where each key maps to a value |
| **Key** | The label used to access a value in a dictionary; must be immutable |
| **Value** | The data associated with a key in a dictionary |
| **KeyError** | Exception raised when accessing a key that does not exist |
| **`get()`** | Dictionary method that returns a value for a key, or a default if not found |
| **`items()`** | Returns all key-value pairs as tuples for iteration |
| **Dictionary comprehension** | A concise expression that creates a dictionary by iterating over a sequence |
| **Set** | An unordered collection of unique elements |
| **Union** | A set operation returning all elements from both sets (`\|`) |
| **Intersection** | A set operation returning only elements found in both sets (`&`) |
| **Difference** | A set operation returning elements in the first set but not the second (`-`) |
| **Symmetric difference** | Elements in either set but not in both (`^`) |
| **Subset** | A set whose elements are all contained in another set |
| **Superset** | A set that contains all elements of another set |
| **Serialization** | Converting an object to bytes for storage (pickling) |
| **Pickling** | Python's built-in serialization using the `pickle` module |

---

*Chapter 9 — Starting Out with Python, Fifth Edition by Tony Gaddis*
