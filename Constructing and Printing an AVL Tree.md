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
# Node class for AVL Tree
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

# Function to get height of a node
def getHeight(node):
    if not node:
        return 0
    return node.height

# Function to get balance factor
def getBalance(node):
    if not node:
        return 0
    return getHeight(node.left) - getHeight(node.right)

# Right rotate
def rightRotate(y):
    x = y.left
    T2 = x.right
    x.right = y
    y.left = T2
    y.height = 1 + max(getHeight(y.left), getHeight(y.right))
    x.height = 1 + max(getHeight(x.left), getHeight(x.right))
    return x

# Left rotate
def leftRotate(x):
    y = x.right
    T2 = y.left
    y.left = x
    x.right = T2
    x.height = 1 + max(getHeight(x.left), getHeight(x.right))
    y.height = 1 + max(getHeight(y.left), getHeight(y.right))
    return y

# Insert into AVL tree
def insert(root, key):
    if not root:
        return Node(key)
    elif key < root.key:
        root.left = insert(root.left, key)
    else:
        root.right = insert(root.right, key)

    root.height = 1 + max(getHeight(root.left), getHeight(root.right))
    balance = getBalance(root)

    # Balancing conditions
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

# Function to get dictionary tree
def getDictTree(root):
    if not root:
        return None
    return {
        'key': root.key,
        'left': getDictTree(root.left),
        'right': getDictTree(root.right)
    }

# Function to construct AVL tree and print dictionary
def Construct_AVL(L):
    print("Input List:", L)
    root = None
    for val in L:
        root = insert(root, val)
    print("\nAVL Tree in Dictionary Format:")
    print(getDictTree(root))

# Main Program
L = [30, 10, 20, 40, 50, 25]
Construct_AVL(L)

```

## OUTPUT
![image](https://github.com/user-attachments/assets/8466822e-6112-430b-8d9f-687f28197f4b)


## RESULT
Thus, the Python program to construct and print an AVL tree was successfully implemented and verified.
