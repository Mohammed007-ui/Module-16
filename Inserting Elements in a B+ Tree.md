# Ex. No: 16D - Inserting Elements in a B+ Tree in Python

## AIM:
To write a Python function `def insert(self, key, value):` to insert elements into a **B+ Tree**.

---

## ALGORITHM:

**Step 1**: Start the program.

**Step 2**: Define a `BPlusTreeNode` class to represent each node in the B+ Tree:
- Store keys and corresponding values.
- Maintain child pointers.
- Track if the node is a leaf.

**Step 3**: Define a `BPlusTree` class to manage the overall structure:
- Implement methods to insert new keys and values.
- Handle node splitting when the node exceeds the allowed degree.

**Step 4**: Implement `insert(self, key, value)`:
- Locate the correct leaf node for insertion.
- Insert key-value pair.
- If the node overflows, split the node and propagate the split up if necessary.

**Step 5**: Implement methods to handle:
- Finding the appropriate node for insertion.
- Splitting full nodes.
- Linking leaf nodes for fast range queries.

**Step 6**: Print the B+ Tree level-wise after insertion.

---

## PYTHON PROGRAM
```
class BPlusTreeNode:
    def __init__(self, is_leaf=False):
        self.is_leaf = is_leaf
        self.keys = []
        self.values = []
        self.children = []
        self.next = None  # Only for leaf nodes

class BPlusTree:
    def __init__(self, max_degree):
        self.root = BPlusTreeNode(True)
        self.max_degree = max_degree

    def insert(self, key, value):
        root = self.root
        if len(root.keys) == self.max_degree - 1:
            new_root = BPlusTreeNode()
            new_root.children.append(self.root)
            self._split_child(new_root, 0)
            self.root = new_root
        self._insert_non_full(self.root, key, value)

    def _insert_non_full(self, node, key, value):
        if node.is_leaf:
            pos = 0
            while pos < len(node.keys) and key > node.keys[pos]:
                pos += 1
            node.keys.insert(pos, key)
            node.values.insert(pos, value)
        else:
            pos = 0
            while pos < len(node.keys) and key > node.keys[pos]:
                pos += 1
            if len(node.children[pos].keys) == self.max_degree - 1:
                self._split_child(node, pos)
                if key > node.keys[pos]:
                    pos += 1
            self._insert_non_full(node.children[pos], key, value)

    def _split_child(self, parent, index):
        node = parent.children[index]
        mid = self.max_degree // 2

        if node.is_leaf:
            new_node = BPlusTreeNode(True)
            new_node.keys = node.keys[mid:]
            new_node.values = node.values[mid:]
            node.keys = node.keys[:mid]
            node.values = node.values[:mid]

            new_node.next = node.next
            node.next = new_node

            parent.keys.insert(index, new_node.keys[0])
            parent.children.insert(index + 1, new_node)
        else:
            new_node = BPlusTreeNode()
            mid_key = node.keys[mid]
            new_node.keys = node.keys[mid + 1:]
            node.keys = node.keys[:mid]
            new_node.children = node.children[mid + 1:]
            node.children = node.children[:mid + 1]

            parent.keys.insert(index, mid_key)
            parent.children.insert(index + 1, new_node)

    def print_tree(self, node=None, level=0):
        if node is None:
            node = self.root
        print("Level", level, ":", node.keys)
        if not node.is_leaf:
            for child in node.children:
                self.print_tree(child, level + 1)

    def print_leaves(self):
        print("\nLeaf Nodes:")
        node = self.root
        while not node.is_leaf:
            node = node.children[0]
        while node:
            print(node.keys, end=" -> ")
            node = node.next
        print("None")

# Example usage
bpt = BPlusTree(4)  # Degree 4 B+ Tree
data = [(10, 'A'), (20, 'B'), (5, 'C'), (6, 'D'), (12, 'E'), (30, 'F'), (7, 'G'), (17, 'H')]

for key, value in data:
    bpt.insert(key, value)

print("\nB+ Tree Structure After Insertions:")
bpt.print_tree()

bpt.print_leaves()
```

## OUTPUT

![image](https://github.com/user-attachments/assets/60811112-4796-47c2-87fa-501c9cb9da61)


## RESULT
Thus, the Python program to insert elements into a B+ Tree and print its structure was successfully implemented and verified.
