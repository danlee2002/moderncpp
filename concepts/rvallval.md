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

