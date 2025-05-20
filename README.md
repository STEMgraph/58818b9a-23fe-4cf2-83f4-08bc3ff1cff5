<!---
{
  "id": "58818b9a-23fe-4cf2-83f4-08bc3ff1cff5",
  "depends_on": ["functors"],
  "author": "Stephan Bökelmann",
  "first_used": "2025-05-20",
  "keywords": ["C++", "Lambda", "Functional Programming", "Closures"]
}
--->

# Exploring C++ Lambda Expressions

> In this exercise you will learn how to define and use lambda expressions in C++. Furthermore we will explore how to capture variables and use lambdas as function parameters.

### Introduction

Lambda expressions were introduced in C++11 and significantly enhance the expressiveness and conciseness of the language by enabling inline, anonymous functions. These functions are especially useful in situations involving standard algorithms, custom sorting, callbacks, or event-driven designs. A lambda expression allows the encapsulation of logic without naming a function explicitly and supports capturing variables from the surrounding scope.

A lambda expression in C++ has the basic syntax:

```cpp
[capture](parameters) -> return_type {
    // body
};
```

The `capture` clause determines how external variables are made available to the lambda. Parameters and return types can often be inferred. Captures can be by value (`=`), by reference (`&`), or explicitly listed, and mutable lambdas allow modification of captured variables if they are captured by value.

Before lambdas, C++ developers relied on functors—custom classes or structs with overloaded `operator()`—to achieve function-like behavior in objects. While powerful and flexible, functors introduced verbosity and extra boilerplate. If you're unfamiliar with functors, we recommend reviewing the [exercise sheet on functors](#) as a foundational prerequisite.

Understanding lambdas is crucial for modern C++ developers, as they are a backbone of idiomatic code in the STL and beyond. Mastery of lambda syntax, semantics, and common patterns will significantly reduce boilerplate and make your code more modular and expressive.

### Further Readings and Other Sources

* [cppreference on Lambdas](https://en.cppreference.com/w/cpp/language/lambda)
* [ISO C++ Lambda Expressions Explained](https://isocpp.org/wiki/faq/lambdas)
* Meyers, S. (2014). *Effective Modern C++*. ISBN: 9781491903995
* [https://www.youtube.com/watch?v=Y7fL6h3apKU](https://www.youtube.com/watch?v=Y7fL6h3apKU) (Lambda functions in C++)

---

### Tasks

#### Task 1: Basic Lambda Syntax

1. Open your `vim` editor and create a file named `lambda_basic.cpp`.
2. Write a lambda that prints "Hello, Lambda!" and immediately invoke it.

```cpp
#include <iostream>

int main() {
    []() { std::cout << "Hello, Lambda!" << std::endl; }();
    return 0;
}
```

3. Compile and run your program using:

```sh
g++ -std=c++11 lambda_basic.cpp -o lambda_basic && ./lambda_basic
```

#### Task 2: Capturing Variables

1. Create a file named `lambda_capture.cpp`.
2. Define an `int x = 10;`, then write two lambdas: one capturing `x` by value and one by reference.
3. Modify the referenced variable inside the lambda.

```cpp
#include <iostream>

int main() {
    int x = 10;

    auto by_val = [x]() { std::cout << "Captured by value: " << x << std::endl; };
    auto by_ref = [&x]() { x += 5; std::cout << "Captured by reference: " << x << std::endl; };

    by_val();
    by_ref();
    std::cout << "After lambda by_ref: " << x << std::endl;

    return 0;
}
```

#### Task 3: Lambdas with STL Algorithms

1. Use a lambda with `std::sort` to sort a vector of integers in descending order.
2. File: `lambda_sort.cpp`

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> nums = {5, 2, 9, 1, 3};

    std::sort(nums.begin(), nums.end(), [](int a, int b) {
        return a > b;
    });

    for (int n : nums) std::cout << n << " ";
    std::cout << std::endl;

    return 0;
}
```

#### Task 4: Mutable Lambdas

1. File: `lambda_mutable.cpp`
2. Show how `mutable` allows modification of captured-by-value variables.

```cpp
#include <iostream>

int main() {
    int x = 5;

    auto f = [x]() mutable {
        x += 10;
        std::cout << "Inside mutable lambda: " << x << std::endl;
    };

    f();
    std::cout << "Outside lambda: " << x << std::endl;

    return 0;
}
```

#### Task 5: Comparing Lambdas and Functors

If you’ve completed the [functor exercise sheet](#), use this task to draw a comparison.

1. Create a file `compare_lambda_functor.cpp`
2. Write a simple functor that checks if a number is even.
3. Then use a lambda to perform the same task.
4. Use both within `std::for_each` to filter and print even numbers.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

struct IsEven {
    bool operator()(int x) { return x % 2 == 0; }
};

int main() {
    std::vector<int> nums = {1, 2, 3, 4, 5, 6};

    std::cout << "Using functor: ";
    std::for_each(nums.begin(), nums.end(), [](int x) {
        if (IsEven()(x)) std::cout << x << " ";
    });

    std::cout << "\nUsing lambda: ";
    std::for_each(nums.begin(), nums.end(), [](int x) {
        if (x % 2 == 0) std::cout << x << " ";
    });

    std::cout << std::endl;
    return 0;
}
```

---

### Questions

1. What is the default capture mode in `[=]` and when should you use it?
2. How does capturing by reference differ in behavior compared to capturing by value?
3. What does the `mutable` keyword enable in lambda expressions?
4. Why are lambdas useful in combination with STL algorithms?
5. Can a lambda be assigned to a `std::function`? Provide an example.
6. In what situations would you still prefer functors over lambdas?

---

### Advice

Understanding lambda expressions may feel foreign if you're used to older C++ paradigms, but they are vital for writing expressive and modern code. Practice using them in small, incremental projects such as filtering or transforming collections. If you haven't already done so, study our [exercise sheet on functors](#) to better understand how lambdas relate to earlier C++ techniques and how to manage callable objects with state.
