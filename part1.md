# A Simple POD (Plain Old Data) Type

```c++
struct Firefly {
    std::string name;
    int hp;

    Firefly(const std::string& name, int hp):
        name(name),
        hp(hp)
        {}

    bool operator<(const Firefly& a, const Firefly& b) {
        return a.name < b.name;
    }
};
```

[prev](part0.md)|[next](part2.md)
