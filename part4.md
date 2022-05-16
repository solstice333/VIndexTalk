# Instantiation of V-Index and Add Data

```c++
auto vin = VIndex(make_extractor(Firefly, name)); // index Firefly::name
vin.emplace("01-K64", 50);
vin.emplace("Serenity", 100);
vin.emplace("02-K64", 100);
```

Underneath all of this, the V-Index generates this:

![v_index_internals](v_index_internals.png)
