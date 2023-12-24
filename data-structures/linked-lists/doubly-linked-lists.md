---
description: Doubly linked lists hold references to both the previous and next elements
---

# Doubly Linked Lists

{% hint style="info" %}
Doubly linked lists are particularly useful when paired with other data structures like hash tables since it allows for $$O(1)$$ retrieval and deletions
{% endhint %}

It is easiest to use a dummy head and tail pointer for the doubly linked list to avoid weird edge cases

```python
class DoublyLinkedList:
    def __init__(self):
        self.head = Node(0, 'HEAD')
        self.tail = Node(-1, 'TAIL')
	# Setup the dummy pointers
        self.head.next = self.tail
        self.tail.prev = self.head
        self.size = 0

    def insert_end(self, node):
        last = self.tail.prev
	# Fix the last element pointers
        last.next = node
        node.prev = last
	# Set the current element pointers
        node.next = self.tail
        self.tail.prev = node
        self.size += 1

        return node

    def delete(self, node):
        node_prev, node_next = node.prev, node.next
	# All we need to do is re-point the next and prev
	# Don't need to care too much about head/tail 
	# because we're using dummy pointers
        node.prev.next = node_next
        node.next.prev = node_prev
        node.next = None
        node.prev = None
        self.size -= 1

    def front(self):
        if self.head.next == self.tail: return None
        return self.head.next

    def length(self):
        return self.size

class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None
        self.prev = None
```
