### Introduction:
In this section, we shall cover the notion of perfect forwarding. 
A significant concept introduced in C++11.
Consider the following snippet of code:
```cpp
int &&j = 42;
```
The following piece of code in essence takes an existing piece of code and binds into the memory location in which rvalue subsides. 
Why is this important?
Recall from the notion of lvalue and rvalues that the following snippet of code doesn't compile:
```cpp
int &a = 2; //compiler will be sad 
const int &j = 2; //have to use this work around 
//cpp builds a construct to get around this using the notion of rvalue
int scale(int &&j, int k) {
    return j * k;
}
```
### Perfect forwarding:
With the notion of rvalue reference covered, we can now cover the notion of perfect forwarding.
Perfect forwarding is the act of passing a function parameter while perserving it's reference category.
To consider the motivation, let's consider the following example.
Assume that we want to be able to pass a reference to a variable 
```cpp
template<typename T> 
void OuterFunction(T& param){
    transform(param);
}
```
It's clear that as long as we pass in a lvalue reference, the following piece of code should be legal.
Now consider the following function call `OuterFunction(5)`. It's clear that this wouldn't work as we are trying to pass a rvalue to a lvalue.
How can we solve this problem?
One consideration that may come to mind is to use the `const keyword`. However, this has its own bottlenecks as we cannot modify param.
Now one may have the thought to overload the function and define a function that takes a rvalue reference. This itself has it's own limitations.
Consider the following snippet:
```cpp
template<typename T> 
void OuterFunction(T&& param) {
    transform(param);
}

```
Although this function may be able to take a rvalue reference, it's clear that there are problems if in the inner function can only take lvalue references.
This is when perfect forwarding comes in. Before we delve into the monolith of perfect forwarding, we first need to cover some other concepts.
The first being the notion of Template Type deduction:
- Template type deduction covers how the compiler deduces the type T passed into a template function when the function parameter is of the type T&&.

