# A trie in Python with step-by-step traversal and longest-prefix retrieval

A Python implementation of the trie data structure. This trie can store sequences of hashable objects. It is designed to allow for step-by-step traversal and to efficiently find the longest prefix of a given sequence that exists in the trie as a valid, i.e. previously inserted, sequence.

## Features

- Stores sequences of hashable items (e.g., lists of strings, numbers, or tuples).
- Treats bare strings as sequences of characters, allowing it to function as a classical character Trie.
- Provides a method to find the longest stored prefix of a given sequence.

## File Structure

- `trie.py`: Contains the implementation of `TrieNode` and `Trie` classes.
- `test_trie.py`: Contains unit tests for the Trie implementation.

## Usage

First, ensure `trie.py` is in your Python path or in the same directory as your script. Then, import the `Trie` class:

```python
from trie import Trie
```

### Initialization

Create a new Trie instance:

```python
my_trie = Trie()
```

### Inserting Sequences

You can insert sequences (iterables of hashable items). If you insert a string directly, it will be treated as a sequence of characters. To use strings as whole tokens, enclose them in a list or tuple.

```python
# Inserting a list of strings
my_trie.insert(["apple", "pie", "filling"])
my_trie.insert(["apple", "crumble"])

# Inserting a list of numbers
my_trie.insert([1, 2, 3])
my_trie.insert([1, 2, 5])

# Inserting a string (treated as a sequence of characters)
my_trie.insert("hello") # 'h', 'e', 'l', 'l', 'o'
my_trie.insert("help")  # 'h', 'e', 'l', 'p'

# To treat "hello" as a single token, wrap it in a list
my_trie.insert(["hello", "world"])
```

### Retrieving Nodes (`get_node`)

You can retrieve the node corresponding to a given prefix. This is useful for checking if a prefix exists or for further traversal. `get_node` returns the `TrieNode` if the prefix exists, otherwise `None`.

```python
# For sequences of items
node1 = my_trie.get_node(["apple", "pie"])
if node1:
    print(f"Node for ['apple', 'pie'] found. Key: {node1.key}, Is end: {node1.is_end_of_sequence}")
    # Node for ['apple', 'pie'] found. Key: pie, Is end: False (if only ["apple", "pie", "filling"] was inserted)

my_trie.insert(["apple", "pie"]) # Mark ["apple", "pie"] as a sequence end
node1_updated = my_trie.get_node(["apple", "pie"])
if node1_updated:
    print(f"Node for ['apple', 'pie'] updated. Key: {node1_updated.key}, Is end: {node1_updated.is_end_of_sequence}")
    # Node for ['apple', 'pie'] updated. Key: pie, Is end: True

non_existent_node = my_trie.get_node(["banana"])
print(f"Node for ['banana']: {non_existent_node}")
# Node for ['banana']: None

# For character sequences (strings)
node_hel = my_trie.get_node("hel") # equivalent to my_trie.get_node(['h', 'e', 'l'])
if node_hel:
    print(f"Node for 'hel' found. Key: {node_hel.key}, Is end: {node_hel.is_end_of_sequence}")
    # Node for 'hel' found. Key: l, Is end: False

node_hello = my_trie.get_node("hello")
if node_hello:
    print(f"Node for 'hello' found. Key: {node_hello.key}, Is end: {node_hello.is_end_of_sequence}")
    # Node for 'hello' found. Key: o, Is end: True
```
**Note:** `get_node` also accepts a single non-iterable item as a prefix, which it will treat as a single-element sequence. E.g., `my_trie.get_node(1)` is equivalent to `my_trie.get_node([1])`. However, if you pass a string like `"abc"`, it will be treated as `['a', 'b', 'c']`.

### Testing for Prefixes (`test_prefix`)

The `test_prefix` method finds the longest prefix of a given sequence that has been explicitly inserted into the Trie (i.e., its `is_end_of_sequence` flag is `True`).

```python
# Setup
search_trie = Trie()
search_trie.insert(["user", "login", "attempt"])
search_trie.insert(["user", "login"])
search_trie.insert(["user", "profile", "view"])
search_trie.insert(["user"])
search_trie.insert("admin") # 'a', 'd', 'm', 'i', 'n'
search_trie.insert("admiral") # 'a', 'd', 'm', 'i', 'r', 'a', 'l'

# Example 1: Longest prefix for a list of strings
query1 = ["user", "login", "attempt", "failed"]
longest1 = search_trie.test_prefix(query1)
print(f"Longest prefix for {query1}: {longest1}")
# Longest prefix for ['user', 'login', 'attempt', 'failed']: ['user', 'login', 'attempt']

# Example 2: Intermediate prefix is the longest valid one
query2 = ["user", "login", "info"]
longest2 = search_trie.test_prefix(query2)
print(f"Longest prefix for {query2}: {longest2}")
# Longest prefix for ['user', 'login', 'info']: ['user', 'login']

# Example 3: Only the shortest prefix is valid
query3 = ["user", "action"]
longest3 = search_trie.test_prefix(query3)
print(f"Longest prefix for {query3}: {longest3}")
# Longest prefix for ['user', 'action']: ['user']

# Example 4: No stored prefix found
query4 = ["guest", "access"]
longest4 = search_trie.test_prefix(query4)
print(f"Longest prefix for {query4}: {longest4}")
# Longest prefix for ['guest', 'access']: []

# Example 5: Using strings (character sequences)
query5 = "administration"
longest5 = search_trie.test_prefix(query5)
print(f"Longest prefix for '{query5}': {longest5}") # Output will be a list of characters
# Longest prefix for 'administration': ['a', 'd', 'm', 'i', 'n']

query6 = "admiring"
longest6 = search_trie.test_prefix(query6)
print(f"Longest prefix for '{query6}': {longest6}")
# Longest prefix for 'admiring': [] (because "admir" or "admi" were not inserted as complete sequences)

search_trie.insert("admi")
query7 = "admiring"
longest7 = search_trie.test_prefix(query7)
print(f"Longest prefix for '{query7}' after inserting 'admi': {longest7}")
# Longest prefix for 'admiring' after inserting 'admi': ['a', 'd', 'm', 'i']

```

## Running Tests

Unit tests are provided in `test_trie.py`. To run them, navigate to the project directory and execute:

```bash
python -m unittest test_trie.py
```
