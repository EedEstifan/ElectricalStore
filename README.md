# ElectricalStore

A C++ object-oriented inventory management system for a multi-branch electrical store chain. Built as a university project to demonstrate core OOP principles including inheritance, polymorphism, templates, design patterns, and exception handling.

## Overview

The system models a real-world retail chain where a **MainOffice** oversees multiple **Branches**, each holding an inventory catalog of **Items** — computers, tablets, keyboards, and mice. Items can be added, removed, and queried across branches.

## Class Hierarchy

```
Item  (base class)
├── Computer
│   └── Tablet
└── PeripheralDevice  (virtual inheritance from Item)
    ├── Keyboard
    └── Mouse

Branch          — holds a catalog of Item*
MainOffice      — Singleton, manages all branches via std::map
```

## Key Design Patterns & Concepts

- **Inheritance & Polymorphism** — `Item` is a polymorphic base with a virtual destructor and `operator std::string()` overridden in every subclass
- **Virtual Inheritance** — `PeripheralDevice` uses `virtual public Item` to resolve the diamond problem (since `Mouse`/`Keyboard` extend both `PeripheralDevice` and transitively `Item`)
- **Singleton Pattern** — `MainOffice` is a globally unique instance, using a nested `Cleaner` class for safe deterministic destruction
- **Template Method** — `giveMeFinest<T>()` in `Branch` uses `dynamic_cast` to find the highest-priced item of any given type at runtime
- **Strategy Pattern** — `printBranch()` and `printBranchesByLocation()` accept a function pointer, allowing caller-defined print behavior
- **Custom Exception Hierarchy** — 8 typed exceptions (`FullCatalogError`, `NonExistingItemError`, `NoneExistingItemTypeError`, etc.) for expressive error handling

## Project Structure

```
├── Item.h / Item.cpp                   — Base item class with static ID counter
├── Computer.h / Computer.cpp           — Desktop computer, holds connected peripherals
├── Tablet.h / Tablet.cpp               — Extends Computer
├── PeripheralDevice.h / .cpp           — Abstract peripheral, connects to a Computer
├── Keyboard.h / Keyboard.cpp           — Extends PeripheralDevice (type, layout)
├── Mouse.h / Mouse.cpp                 — Extends PeripheralDevice (DPI, buttons)
├── Branch.h / Branch.cpp               — Inventory catalog with capacity enforcement
├── MainOffice.h / MainOffice.cpp       — Singleton managing all branches
├── HWExceptions.h                      — Custom exception types
└── main.cpp                            — Demo and usage examples
```

## Building & Running

This project was developed with Visual Studio on Windows. Open `HW4.sln` in Visual Studio and build the solution, or compile manually with any C++11-compatible compiler:

```bash
g++ -std=c++11 *.cpp -o electrical_store
./electrical_store
```

## Sample Usage

```cpp
MainOffice& office = MainOffice::getInstance();
office.addBranch("Tel Aviv", 50);
office.addBranch("Haifa", 30);

Branch& tlv = office.getBranch("Tel Aviv");
tlv.addItem(new Computer(2500, "Apple"));
tlv.addItem(new Keyboard(150, "Logitech", "Black", true));

// Get the most expensive keyboard in this branch
Keyboard* best = tlv.giveMeFinest<Keyboard>(new Keyboard(0, "", "", false));
```

## Authors

- Eed Estifan
- Adam Masalha
