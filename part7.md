# Results

Designing the v-index with single ownership as the driving memory management scheme fit the problem very well. 

With a mutable pointer/reference to an `std::unique_ptr`, it was easy to leverage move semantics. When editing a tree on remove, subtrees can be moved out temporarily for update, then moved back into the `std::unique_ptr` that represents the left or right child of a parent node:
```c++
    template <typename T>
    std::unique_ptr<Node<T>>& _modify_proxy_tree(
        std::unique_ptr<Node<T>>* tree, 
        const std::function<
            std::unique_ptr<Node<T>>
            (std::unique_ptr<Node<T>>*)
        >& edit
    ) {
        auto working_tree = std::move(*tree);
        *tree = edit(&working_tree);
        return *tree;
    }
```

Additionally, it was important that C++ does not constrain itself to aliasability XOR mutability (AxM) because it allowed for both insert and remove operations to be optimally implemented in conjunction, and at convenience. An insert operation requires mutability from the root of the tree. As a node is inserted, it recurses through the root node, and so each node that is passed through needs to give update permissions. This is of course O(log(N)). A removal operation faces the challenge in that AVL trees with *non-index* comparator keys can have nodes with equivalent keys:

![v_index_dupl_cmp_key](v_index_dupl_cmp_key.png)

For this reason, mutable references, pointing into each AVL tree, were added to allow constant time access to the target node:

![v_index_dupl_cmp_key_sln](v_index_dupl_cmp_key_sln.png)

While this was still O(log(N)), it was convenient in that it avoided a large scale refactor, which otherwise, would have probably resulted in API churn/thrashing.

Memory management, as expected, was abstracted naturally through the single ownership model where a parent node owns its children. Again, this:
```c++
template <typename T>
struct Node {
    T data;
    std::unique_ptr<Node> left;
    std::unique_ptr<Node> right;
}
``` 
This was conducive for avoiding excessive refactoring and any kind of memory leak bug.

Transitioning into Rust was relatively less daunting, since the default assignment/binding semantics is single ownership.

[prev](part6.md)
