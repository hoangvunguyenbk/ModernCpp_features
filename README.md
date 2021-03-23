# ModernCpp_features

C++11 language feature:

Up till C++11 the programmer needed to mention the data type of the variable being used in the code but now we can use the keyword decltype and auto the type will be idetified by the compiler itself. This property is called **type inference**

**auto**

*  Deduce type of variable by compiler acording to the type of their initializer.
  Together with **decltype**, **auto** is usefull primarily when writing code in template libraries.
  
* The auto keyword is a simple way to declare a variable that has a complicated type, handy when working with template and interator
  
  ```c++
  vector<int> v;
  auto iter = v.iterator(); // instead of vector<int>::iterator itr
  ```
    Return value of add function will be deduce from type of passed argument t and v.
    
**decltype**

* Act like and extractor, let you extract the type of passed variable/expression.

```cpp
int x;
int func();
decltype(x) y; // equal int y;
decltype(func()) z; // equal int z;
```

* Using auto and decltype together:

Without auto and decltype, need to call explictly when instanciate function: 
```cpp
template <typename T, typename U>
U add(T t, T v) {
	return t + v;
}
add<int, int>(1, 3)
```

With auto and decltype: 
```cpp
template <typename T, typename U>
auto add(T t, U u) -> decltype(t + u) {
  return t+u;
}

add(1, 3); //=4 return int 
add(1, 3.0); //=4.0 return double
add(1.0, 3.0); //=4.0 return double
```
Return type of function is deduced from the passed arguments.





