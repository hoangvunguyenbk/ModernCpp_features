# ModernCpp_features

Notes on major language and library features of the new standards  C++

[C++20](#C++20)

A. Language Feature

- [Concept](#concept)
- [Modules](#modules)

B. Library Feature


[C++17](#C++17)
A. Language Feature

- [constexpr if](#constexpr_if)
- [structured bindings](#structured_bindings)

B. Library Feature
- [std::variant](#std::variant)
- [std::optional](#std::optional)
- [std::any](#std::any)

[C++14](#C++14)

A. Language Feature


B. Library Feature


[C++11](#C++11)

A. Language Feature
- [Move sematic](#MoveSematic)
- [Concurency](#Concurency)
- [Lambda expression](#Lambda)


B. Library Feature


---

### Concept
**Definitions** 

Concept is set of conditions or constrains can be evaluated at compile-time to validate the template arguments.

Example:
```cpp
class Foo {
public:
	Foo(const int& x) {
		m_int = x;
	}

	int getInt() const {
		return m_int;
	}

private:
	int m_int = 0;
};

//Define a concept
template<typename T>
concept Printable = requires (std::ostream & os,  const T& obj) 
{
	std::is_class_v<T>;
	os << obj.getInt();
};

//Use concept as a parameter of funtion template
template<Printable T>
T print(const T& obj)
{
	std::cout << obj.getInt();
	return obj;
}
```
**Advantages**

- Requirements for templates are part of the interface, easy to understand and use interface effecition.
- Clearer error message
- Can used predefined or custom concept
- The overloading of functions or specialisation of class templates can be based on concepts.

****

Exp: Sortable concept
```cpp
// requires clause
template<typename Cont>
requires Sortable<Cont>
void sort(Cont& container);

// trailing requires clause
template<typename Cont>
void sort(Cont& container) requires Sortable<Cont>;

//Constrained template parameters
template<Sortable cont>
void sort(Cont& container)
```

### Modules
**Definitions** 

A module is a set of source code files that are compiled independently of the translation units that import them
```cpp
import std.core;
import std.regex;

```


### Move Sematic
The optimization technique introduced in C++11 makes it possible for compiler to replace expensive copying operations with moving operations, which are less expensive.

**Universal references vs Rvalue references** 

"T&&" has 2 meanings:
- Rvalue references: bind only to rvalue or temporary object, use to identify movable objects.
"T&&" is a rvalue reference when there is no type deduction take place.
```cpp
void f(Widget&& param);   // no type deduction;
Widget&& var1 = Widget(); // no type deduction, var1 is an rvalue reference

```
- Universal references: In contrast, "T&&" is either rvalue reference or lvalue reference, it can look like rvalue reference but behave as lvalue references. 
Universal references can bind either to lvalue, rvalue, const or non-const object...

Universal references correspond to rvalue references if they’re initialized with rvalues. They correspond to lvalue references if they’re initialized with lvalues.
```cpp
//param is a universal reference because of type deduction take place during compile time

template<typename T>
void f(T&& param); 

int t{10};
f(t); // bind to lvalue
f(std::move(t)); // bind to rvalue

```
An universal reference has to have exact form "T&&". Even
**const** can disqualify universal reference. 
```cpp
template<typename T>
void f(const T&& param); // param is an rvalue reference because of const
```

**std::move vs std::forward**

Actualy std::move and std::forwarding not move anything but cast to rvalue.

- **std::move** : perform unconditional cast to rvalue.

- **std::forward**: perform conditional cast to rvalue.

- Neither std::move nor std::forward do anything at runtime.

Sample implementation of std::move
```cpp
//C++14
//T&& param here is a universal reference

template<typename T>
typename remove_reference_t<T>&& move(T&& param)
{
	using ReturnType = typename remove_reference_t<T>&&;
	
	return static_cast<ReturnType>(param);
}
```

**std::forward** typical usecase is a function template taking a universial reference parameter that is pass to another function. So we want the second function is received exact param what we have passed.

```cpp
void process(const Widget& param); // lvalue
void process(Widget&& param);      // rvalue

template<typename T>
void logAndProcess(T&& param)
{
	auto _now = std::chrono::steady_clock::now();
	makeLogEntry("Call process");
	process(std::forward<T>(param));
}

Widget w;
logAndProcess(w); //call lvalue version of process fucntion
logAndProcess(std::move(w); // call rvalue version.

```



