### Introductions:
- In modern CPP, the notion of rval vs lval can be a confusing concepts
- From a primitive standpoint, one can simply assume that a lvalue is some object that has an identifiable location in memory
- rvalue on the other hand can be thought of as the complement as the set of lvalues
### Examples:
```cpp
//lvalue examples
int a; //a is a lval
int *p = &(a); //lvalue
//rvalue examples
dog d1;
d1 = dog(); //rvalue 
int sum(x,y){returm x + y }; //rvalue 
int i = sum(3,4); //sum(3,4) is a rvalue 
```
### Lvalue references:
Now with an intuition of how lvalues and rvalues work 
- We can now consider the notion of lvalue reference, or in other words a reference to a lvalue reference or in other words a reference to a lvalue
- Consider the following snippet of code:
```cpp
int i; 
int &r = i; //note that we can only a lvalue reference to a lvalue 
int &r = 2; //Throws an error
//exception to the rule, can assign reference to a const because it ensures that value cannot be changed 
const int &r = 2; //legal
```
Now to see why the const keyword is useful, let's consider the notion of a simple square root function:
```cpp
int square(int &x) {
    return x * x;
}
square(i); //ok 
square(40); //:(
//work around
int square(const int& x) {
return x *x;
}

square(40); //:)
square(i); //:)

```
### Creating Rvalue from Lvalue
- Consider the following case:
```cpp
int i = 1;
int x = i + 2; //it's clear that i is a lvalue but in this context i is a rvalue
int x = i; //same logic as above
```
### Creating a LValue from Rvalue
```cpp
int v[3]; 
*(v+2) = 4; //temporary pointer v+2 is used to create a lvalue
```
### Misconceptions about Lvalue and Rvalues:
- Misconception 1: a function or operator always yields r values
```cpp
int x;
int& returnGlobal() {

    return myglobal; //becomes lvalue due to return type 
} 
array[3] =2; //array[3] is a lvalue since it is memory location that can be assigned a new value
```
### Misconception 2: lvalues are modifiable
```
const int c = 1; //c is a lvalue
```
### Misconception 3: rvalues are not modifiable
```
class dog;
dog().bark(); //could change state of dogs 
```
