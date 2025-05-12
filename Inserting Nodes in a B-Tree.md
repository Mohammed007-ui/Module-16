# Ex. No: 16C - Inserting Nodes in a B-Tree in Python

## AIM:
To write a Python function `def insert(self, k):` to insert the nodes in a **B-Tree**.

---

## ALGORITHM:

**Step 1**: Start the program.

**Step 2**: Define the `BTreeNode` class to represent a node:
- Contains a list of keys.
- Contains a list of children.
- Indicates whether it is a leaf.

**Step 3**: Define the `BTree` class with:
- Methods for inserting keys.
- Handling node splitting.
- Tree traversal and printing.

**Step 4**: Implement `insert()`:
- Insert a key into the tree.
- Handle full root case and invoke node splitting if needed.

**Step 5**: Implement `insert_non_full()` to insert a key into a node that is not full.

**Step 6**: Implement `split_child()` to split a full child during insertion.

**Step 7**: Define `print_tree()` to recursively print the structure of the B-Tree.

---

## PYTHON PROGRAM

```
class BTreeNode:
    def __init__(self, t, leaf=False):
        self.t = t  # Minimum degree
        self.leaf = leaf
        self.keys = []
        self.children = []

class BTree:
    def __init__(self, t):
        self.root = BTreeNode(t, True)
        self.t = t

    def insert(self, k):
        root = self.root
        if len(root.keys) == 2 * self.t - 1:
            new_root = BTreeNode(self.t, False)
            new_root.children.insert(0, self.root)
            self._split_child(new_root, 0)
            self._insert_non_full(new_root, k)
            self.root = new_root
        else:
            self._insert_non_full(root, k)

    def _insert_non_full(self, node, k):
        i = len(node.keys) - 1
        if node.leaf:
            node.keys.append(0)
            while i >= 0 and k < node.keys[i]:
                node.keys[i + 1] = node.keys[i]
                i -= 1
            node.keys[i + 1] = k
        else:
            while i >= 0 and k < node.keys[i]:
                i -= 1
            i += 1
            if len(node.children[i].keys) == 2 * self.t - 1:
                self._split_child(node, i)
                if k > node.keys[i]:
                    i += 1
            self._insert_non_full(node.children[i], k)

    def _split_child(self, parent, i):
        t = self.t
        y = parent.children[i]
        z = BTreeNode(t, y.leaf)
        parent.children.insert(i + 1, z)
        parent.keys.insert(i, y.keys[t - 1])
        z.keys = y.keys[t:(2 * t - 1)]
        y.keys = y.keys[0:t - 1]

        if not y.leaf:
            z.children = y.children[t:(2 * t)]
            y.children = y.children[0:t]

    def print_tree(self, node=None, level=0):
        if node is None:
            node = self.root
        print("Level", level, "Keys:", node.keys)
        for child in node.children:
            self.print_tree(child, level + 1)


# Example usage:
btree = BTree(3)  # B-Tree of minimum degree 3
elements = [10, 20, 5, 6, 12, 30, 7, 17]

for elem in elements:
    btree.insert(elem)

print("\nB-Tree Structure After Insertions:")
btree.print_tree()

```

## OUTPUT

![image](https://github.com/user-attachments/assets/046aa2df-b30d-4a2f-82f2-13a0cb0021b3)


## RESULT
Thus, the Python program to insert nodes in a B-Tree and print the structure has been successfully executed.
