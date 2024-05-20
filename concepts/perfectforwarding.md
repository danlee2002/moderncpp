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
## Perfect forwarding:
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
Consider the case in which an argument is passed where if T is a lvalue, then T is deduced to a lvalue reference, X&
- If the argument passed to T is a rvalue then T is deduced to lvalue X
In this case, consider the following snippet code:
```cpp
OuterFunction(6); //T is converted to lvalue T = int  
std::string var = "hello";
OuterFunction(hello); //T is converted to reference type string& or in other words T = string&

```
### Reference Collapsing:
The notion of reference collapsing is a set of rules in C++11 that are used to determine the value of T of a template function when taking the reference reference which is something illegal in cpp.
Consider the following snippet of code. 
```cpp
template<typname T>
void func(T t) {
    T& k = t;
}
string hello = "hello";
func(hello);
```
The following snippet of code is technically illegal but cpp has a procedure to handle this.
- Taking the reference of lvalue reference results in lvalue reference: (X&)& -> X& 
- Taking the rvalue reference of lvalue reference results in lvalue reference: (X&)&& -> X&
- Taking the lvalue reference of an rvalue reference results in lvalue reference:  (X&&)& -> X&
- Taking the rvalue reference of an rvalue reference results in rvalue refernece: (X&&)&& -> X&&
### Forwarding with forward:
The function std::forward is useful for solving the perfect forwaring problem 
Two primary objectives:
    - If passed an argument that isn't lvalue reference than the function returns a rvalue
    - If lvalue ref passed int then lvalue ref is passed
In other words, if we have a tempalte function that accepts a rvalue reference then the argument will be able to be forwarded as rvalue reference.
Whereas, if lvalue is passed then the function triggers an inner function assuming an overloaded function exists.
```cpp
template<typename T>
void func(T&& t){
    std::forward<T>(t);
}
```
An implementation of std::forward
```cpp
template <class T>
T&& forward(typename remove_reference<T>::type& arg) {
    return static_cast<T&&>(arg);
}
```

