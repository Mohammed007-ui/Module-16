# Ex. No: 16B - Constructing and Balancing an AVL Tree in Python

## AIM:
To write a Python program to construct an **AVL tree**, balance it, and print the nodes **before and after balancing** using the appropriate packages and built-in function.

---

## ALGORITHM:

**Step 1**: Start the program.

**Step 2**: Define a method `getDictTree(tree)` to return the `dict_tree` or structure of the AVL tree.

**Step 3**: Define a method `Construct_AVL(L)` to:
- Create a binary tree from the list `L`.
- Print the tree **before balancing**.
- Sort and reinsert the nodes in a balanced manner (simulating AVL behavior).
- Print the tree **after balancing**.

**Step 4**: Create a list `L` of integers.

**Step 5**: Call `Construct_AVL(L)` to build and balance the tree.

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


def getHeight(root):
    if not root:
        return 0
    return root.height


def getBalance(root):
    if not root:
        return 0
    return getHeight(root.left) - getHeight(root.right)


def rightRotate(y):
    x = y.left
    T2 = x.right
    x.right = y
    y.left = T2
    y.height = max(getHeight(y.left), getHeight(y.right)) + 1
    x.height = max(getHeight(x.left), getHeight(x.right)) + 1
    return x


def leftRotate(x):
    y = x.right
    T2 = y.left
    y.left = x
    x.right = T2
    x.height = max(getHeight(x.left), getHeight(x.right)) + 1
    y.height = max(getHeight(y.left), getHeight(y.right)) + 1
    return y


def insert(root, key):
    if not root:
        return Node(key)
    elif key < root.key:
        root.left = insert(root.left, key)
    else:
        root.right = insert(root.right, key)

    root.height = 1 + max(getHeight(root.left), getHeight(root.right))
    balance = getBalance(root)

    # Balancing
    if balance > 1 and key < root.left.key:
        return rightRotate(root)
    if balance < -1 and key > root.right.key:
        return leftRotate(root)
    if balance > 1 and key > root.left.key:
        root.left = leftRotate(root.left)
        return rightRotate(root)
    if balance < -1 and key < root.right.key:
        root.right = rightRotate(root.right)
        return leftRotate(root)

    return root


def preOrder(root):
    if not root:
        return
    print(root.key, end=" ")
    preOrder(root.left)
    preOrder(root.right)


def getDict

```

## OUTPUT
![image](https://github.com/user-attachments/assets/7bcff6db-140d-4a56-bb5c-17400b773460)


## RESULT
Thus, the Python program to construct and balance an AVL tree was successfully implemented and tested.

