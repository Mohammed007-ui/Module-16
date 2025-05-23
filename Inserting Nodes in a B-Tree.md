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
        self.t = t  # Minimum degree (defines the range for number of keys)
        self.leaf = leaf  # True if node is leaf
        self.keys = []  # List of keys
        self.children = []  # List of child pointers

    def __str__(self):
        return f'Keys: {self.keys}, Leaf: {self.leaf}'

class BTree:
    def __init__(self, t):
        self.root = BTreeNode(t, leaf=True)
        self.t = t

    def insert(self, k):
        root = self.root
        # If root is full, tree grows in height
        if len(root.keys) == (2 * self.t - 1):
            new_root = BTreeNode(self.t, leaf=False)
            new_root.children.append(root)
            self.split_child(new_root, 0)
            self.root = new_root
            self.insert_non_full(new_root, k)
        else:
            self.insert_non_full(root, k)

    def insert_non_full(self, node, k):
        i = len(node.keys) - 1
        if node.leaf:
            # Insert new key at correct position in leaf node
            node.keys.append(None)
            while i >= 0 and k < node.keys[i]:
                node.keys[i + 1] = node.keys[i]
                i -= 1
            node.keys[i + 1] = k
        else:
            # Move down to the child node that will have the new key
            while i >= 0 and k < node.keys[i]:
                i -= 1
            i += 1
            if len(node.children[i].keys) == (2 * self.t - 1):
                self.split_child(node, i)
                if k > node.keys[i]:
                    i += 1
            self.insert_non_full(node.children[i], k)

    def split_child(self, parent, i):
        t = self.t
        node = parent.children[i]
        new_node = BTreeNode(t, leaf=node.leaf)

        # New node will get last t-1 keys of node
        new_node.keys = node.keys[t:]
        node.keys = node.keys[:t - 1]

        # If node is not leaf, split its children as well
        if not node.leaf:
            new_node.children = node.children[t:]
            node.children = node.children[:t]

        # Insert new node as child of parent
        parent.children.insert(i + 1, new_node)
        # Move middle key up to parent
        parent.keys.insert(i, node.keys.pop(-1))

    def print_tree(self, node=None, level=0):
        if node is None:
            node = self.root
        print("Level", level, ":", node.keys)
        if not node.leaf:
            for child in node.children:
                self.print_tree(child, level + 1)

# Example usage
btree = BTree(t=3)  # B-Tree of minimum degree 3

values = [10, 20, 5, 6, 12, 30, 7, 17]

for val in values:
    btree.insert(val)

btree.print_tree()

```

## OUTPUT
![image](https://github.com/user-attachments/assets/53464c3a-3e91-4f74-be16-90071ac5f821)


## RESULT
The program correctly inserts nodes into a B-Tree of degree 3, splitting nodes when full and maintaining B-Tree properties. The tree structure is printed level-wise showing keys at each node.

