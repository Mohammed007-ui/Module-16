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
    def __init__(self, t, leaf=False):
        self.t = t  # degree (max keys = 2*t - 1 for B-tree, but B+ tree splits differently)
        self.leaf = leaf
        self.keys = []
        self.values = []  # Only used if leaf=True
        self.children = []
        self.next = None  # For leaf nodes linking

class BPlusTree:
    def __init__(self, t=2):
        self.root = BPlusTreeNode(t, leaf=True)
        self.t = t

    def find_leaf(self, node, key):
        if node.leaf:
            return node
        for i, item in enumerate(node.keys):
            if key < item:
                return self.find_leaf(node.children[i], key)
        return self.find_leaf(node.children[-1], key)

    def insert(self, key, value):
        root = self.root
        if len(root.keys) == (2 * self.t - 1):
            # Root is full, split
            new_root = BPlusTreeNode(self.t)
            new_root.children.append(root)
            self.split_child(new_root, 0)
            self.root = new_root
        self._insert_non_full(self.root, key, value)

    def _insert_non_full(self, node, key, value):
        if node.leaf:
            # Insert key in leaf node in sorted order
            i = 0
            while i < len(node.keys) and key > node.keys[i]:
                i += 1
            node.keys.insert(i, key)
            node.values.insert(i, value)
            # If overflow, split leaf node
            if len(node.keys) == 2 * self.t:
                self.split_leaf(node)
        else:
            # Internal node insertion
            i = 0
            while i < len(node.keys) and key > node.keys[i]:
                i += 1
            child = node.children[i]
            if len(child.keys) == 2 * self.t - 1:
                self.split_child(node, i)
                if key > node.keys[i]:
                    i += 1
            self._insert_non_full(node.children[i], key, value)

    def split_child(self, parent, i):
        t = self.t
        node = parent.children[i]
        new_node = BPlusTreeNode(t, leaf=node.leaf)

        # For internal node
        mid_key = node.keys[t - 1]
        parent.keys.insert(i, mid_key)

        # Split keys and children
        new_node.keys = node.keys[t:]
        node.keys = node.keys[:t - 1]

        if node.leaf:
            # For leaf nodes, split values and manage leaf links
            new_node.values = node.values[t:]
            node.values = node.values[:t]
            new_node.next = node.next
            node.next = new_node
            # Leaf nodes do not push mid_key to parent, override above:
            parent.keys[i] = new_node.keys[0]
        else:
            new_node.children = node.children[t:]
            node.children = node.children[:t]

        parent.children.insert(i + 1, new_node)

    def split_leaf(self, leaf):
        # Special split for leaf nodes
        parent = self.find_parent(self.root, leaf)
        if parent is None:
            # Leaf is root: create new root
            new_root = BPlusTreeNode(self.t)
            new_leaf = BPlusTreeNode(self.t, leaf=True)

            mid = self.t
            new_leaf.keys = leaf.keys[mid:]
            new_leaf.values = leaf.values[mid:]
            leaf.keys = leaf.keys[:mid]
            leaf.values = leaf.values[:mid]

            leaf.next = new_leaf

            new_root.keys = [new_leaf.keys[0]]
            new_root.children = [leaf, new_leaf]
            self.root = new_root
        else:
            # Split leaf and insert into parent
            new_leaf = BPlusTreeNode(self.t, leaf=True)
            mid = self.t
            new_leaf.keys = leaf.keys[mid:]
            new_leaf.values = leaf.values[mid:]
            leaf.keys = leaf.keys[:mid]
            leaf.values = leaf.values[:mid]

            new_leaf.next = leaf.next
            leaf.next = new_leaf

            # Insert key to parent
            i = parent.children.index(leaf)
            parent.keys.insert(i, new_leaf.keys[0])
            parent.children.insert(i + 1, new_leaf)

            # If parent overflows, split parent recursively
            if len(parent.keys) > 2 * self.t - 1:
                self.split_internal(parent)

    def split_internal(self, node):
        if node == self.root:
            new_root = BPlusTreeNode(self.t)
            new_root.children.append(node)
            self.split_child(new_root, 0)
            self.root = new_root
        else:
            parent = self.find_parent(self.root, node)
            idx = parent.children.index(node)
            self.split_child(parent, idx)
            if len(parent.keys) > 2 * self.t - 1:
                self.split_internal(parent)

    def find_parent(self, current, child):
        if current.leaf or current.children[0].leaf:
            return None
        for c in current.children:
            if c == child:
                return current
            else:
                parent = self.find_parent(c, child)
                if parent:
                    return parent
        return None

    def print_tree(self):
        levels = []
        self._get_levels(self.root, 0, levels)
        for i, level in enumerate(levels):
            print(f"Level {i}:", end=" ")
            for node in level:
                if node.leaf:
                    print(f"[{' '.join(str(k) for k in node.keys)}]", end=" ")
                else:
                    print(f"<{' '.join(str(k) for k in node.keys)}>", end=" ")
            print()

    def _get_levels(self, node, depth, levels):
        if len(levels) <= depth:
            levels.append([])
        levels[depth].append(node)
        if not node.leaf:
            for child in node.children:
                self._get_levels(child, depth + 1, levels)

# Example usage
bpt = BPlusTree(t=2)
elements = [(10, 'A'), (20, 'B'), (5, 'C'), (6, 'D'), (12, 'E'), (30, 'F'), (7, 'G'), (17, 'H')]

for key, val in elements:
    bpt.insert(key, val)

bpt.print_tree()

```

## OUTPUT
![image](https://github.com/user-attachments/assets/aacf3564-543b-43a8-b752-105f79d83b93)


## RESULT
The program inserts elements into the B+ Tree, splitting nodes as needed, maintains leaf node links, and prints the tree level-wise with keys. It correctly handles insertion and splitting for a degree t=2.

