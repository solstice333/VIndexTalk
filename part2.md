# A Convenient API To Achieve DB Index-like Behavior

```c++
auto vin = VIndex(make_extractor(Firefly, name)); // index Firefly::name
vin.emplace("01-K64", 50);
vin.emplace("Serenity", 100);
vin.emplace("02-K64", 100);
for (auto it = vin.cbegin(OrderType::INSERTION); it != vin.cend(); ++it) {
    // Firefly("01-K64", 50), 
    // Firefly("Serenity", 100),
    // Firefly("02-K64", 100),
}
for (auto it = vin.cbegin(OrderType::INORDER); it != vin.cend(); ++it) {
    // Firefly("01-K64", 50), 
    // Firefly("02-K64", 100),
    // Firefly("Serenity", 100),
}
```
[next](part3.md)
