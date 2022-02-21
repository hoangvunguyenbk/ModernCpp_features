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