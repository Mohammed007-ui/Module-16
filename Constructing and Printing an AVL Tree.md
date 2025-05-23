# Ex. No: 16A - Constructing and Printing an AVL Tree in Python

## AIM:
To write a Python program to construct an **AVL tree** and print the nodes of it using the appropriate packages and built-in function.

---

## ALGORITHM:

**Step 1**: Start the program.

**Step 2**: Define a function `getDictTree(tree)` to return the **dict_tree** of an AVL tree.

**Step 3**: Define a function `Construct_AVL(L)` to:
- Create an **AVL tree** from the list `L`.
- Get and print the **dict_tree** using `getDictTree(tree)`.

**Step 4**: Define a list `L` with integer values.

**Step 5**: Call `Construct_AVL(L)` to build the tree and print the result.

**Step 6**: End the program.

---

## PYTHON PROGRAM
```
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

def get_height(root):
    if not root:
        return 0
    return root.height

def get_balance(root):
    if not root:
        return 0
    return get_height(root.left) - get_height(root.right)

def right_rotate(z):
    y = z.left
    T3 = y.right
    y.right = z
    z.left = T3

    z.height = 1 + max(get_height(z.left), get_height(z.right))
    y.height = 1 + max(get_height(y.left), get_height(y.right))
    return y

def left_rotate(z):
    y = z.right
    T2 = y.left
    y.left = z
    z.right = T2

    z.height = 1 + max(get_height(z.left), get_height(z.right))
    y.height = 1 + max(get_height(y.left), get_height(y.right))
    return y

def insert(root, key):
    # Normal BST insert
    if not root:
        return Node(key)
    elif key < root.key:
        root.left = insert(root.left, key)
    else:
        root.right = insert(root.right, key)

    # Update height
    root.height = 1 + max(get_height(root.left), get_height(root.right))

    # Check balance
    balance = get_balance(root)

    # If unbalanced, apply rotations

    # Left Left Case
    if balance > 1 and key < root.left.key:
        return right_rotate(root)

    # Right Right Case
    if balance < -1 and key > root.right.key:
        return left_rotate(root)

    # Left Right Case
    if balance > 1 and key > root.left.key:
        root.left = left_rotate(root.left)
        return right_rotate(root)

    # Right Left Case
    if balance < -1 and key < root.right.key:
        root.right = right_rotate(root.right)
        return left_rotate(root)

    return root

def getDictTree(root):
    if root is None:
        return None
    return {
        'key': root.key,
        'left': getDictTree(root.left),
        'right': getDictTree(root.right)
    }

def Construct_AVL(L):
    root = None
    for key in L:
        root = insert(root, key)

    dict_tree = getDictTree(root)
    print("AVL Tree as nested dictionary:")
    print(dict_tree)

# Example input list
L = [30, 20, 40, 10, 25, 35, 50, 5]

Construct_AVL(L)

```

## OUTPUT

![image](https://github.com/user-attachments/assets/b45c0925-3c80-4755-854d-c6c64dccdd4a)


## RESULT
The program successfully constructs an AVL tree from the given list, maintains balance by rotations, and prints the tree structure as a nested dictionary showing each node and its left and right children.
