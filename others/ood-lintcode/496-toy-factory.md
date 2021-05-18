# 496 · Toy Factory

## Question

Description

Factory is a design pattern in common usage. Please implement a `ToyFactory` which can generate proper toy based on the given type.Example

Example 1：

```text
Input：
ToyFactory tf = ToyFactory();
Toy toy = tf.getToy('Dog');
toy.talk(); 
Output:
Wow
```

Example 2:

```text
Input:
ToyFactory tf = ToyFactory();
toy = tf.getToy('Cat');
toy.talk();
Output：
Meow
```

## Solution

```cpp
/**
 * Your object will be instantiated and called as such:
 * ToyFactory* tf = new ToyFactory();
 * Toy* toy = tf->getToy(type);
 * toy->talk();
 */
class Toy {
public:
    virtual void talk() const=0;
};

class Dog: public Toy {
    // Write your code here
    void talk() const {
        cout << "Wow" << endl;
    }
};

class Cat: public Toy {
    // Write your code here
    void talk() const {
        cout << "Meow" << endl;
    }
};

class ToyFactory {
public:
    /**
     * @param type a string
     * @return Get object of the type
     */
    Toy* getToy(string& type) {
        // Write your code here
        if (type == "Dog") {
            return new Dog();
        } else if (type == "Cat") {
            return new Cat();
        }
        return NULL;
    }
};
```

