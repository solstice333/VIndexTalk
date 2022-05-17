# Some Important Design Considerations 

1. Of course, if convenience was a priority, maybe the AVL tree could have been swapped with, perhaps, an `std::map` (red-black balanced BST), but again, the purpose of this exercise was to explore how ergonomic `std::unique_ptr` is when faced with a more complex structural challenge. Also, v-index nodes may contain equivalent comparator keys for any AVL tree whose key is a *non-index* data member of `T`. An `std::map` property is that keys must be unique. And so, worst case scenario, if `std::map` is to be used, then nodes would need to be clustered under the same key.
2. When implementing a dynamically sized type (DST), the question of how to handle data on the heap eventually arises. The initial consideration was to use an `std::vector<std::unique_ptr<T>>` to hold an *arena/pool* of nodes that can be referenced from an AVL tree. And if a node is removed from the AVL tree, the index in the vector is marked as unused, allowing for potential reuse, as opposed to mindlessly allocating more space on the heap, or even worse, dropping the `std::unique_ptr<T>` element, and causing elements after it to shift down at linear time. When these free indices are linked together, this approach of reusing previously allocated space is commonly known as a *freelist*.
3. Ok, but (2) can result in a lot of tedium since it results in effectively writing a memory management abstraction layer, though it's not hard to do so. There is a better option that can automate memory management - have a node take ownership of its children:
```c++
template <typename T>
struct Node {
    T data;
    std::unique_ptr<Node> left;
    std::unique_ptr<Node> right;
}
``` 

[prev](part5.md)|[next](part7.md)
