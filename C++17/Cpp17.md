Non movable and non-copyable type 

 

#include <iostream> 

#include <array> 

using namespace std; 

 
 

struct NonMoveable 

{ 

  NonMoveable(int x) : v(x) { } 

  NonMoveable(const NonMoveable&) = delete; 

  NonMoveable(NonMoveable&&) = delete; 

   

  std::array<int, 1024> arr; 

  int v; 

}; 

 
 

NonMoveable make(int val) 

{ 

  if (val > 0) 

    return NonMoveable(val); 

  return NonMoveable(-val); 

} 

 
 

int main() 

{ 

  auto largeNonMoveableObj = make(90); // construct the object 

  cout << "The v of largeNonMoveableObj is: " <<largeNonMoveableObj.v <<endl; 

  return largeNonMoveableObj.v; 

} 

The above code wouldn’t compile under C++14 as it lacks copy and move constructors. But with C++17 the constructors are not required - because the object largeNonMovableObj will be constructed in place. 

Moreover, it’s important to remember, that in C++17 copy elision works only for unnamed temporary objects, and Named RVO is not mandatory. 

 Value Categories: 

lvalue - an expression that can appear on the left-hand side of an assignment. An expression that has an identity, and which we can take the address of. 

rvalue - an expression that can appear only on the right-hand side of an assignment 

xvalue - an eXpiring lvalue, "eXpiring lvalue" - an object that we can move from, which we can reuse. Usually, its lifetime ends soon 

prvalue - a pure rvalue, an xvalue, a temporary object or sub-object, or a value that is not associated with an object, something without a name, which we cannot take the address of, we can move from such expression. A prvalue is an expression whose evaluation initializes an object, bit-field, or operand of an operator, as specified by the context in which it appears 

glvalue - a generalised lvalue, which is an lvalue or an xvalue, a glvalue is an expression whose evaluation computes the location of an object, bit-field, or function 

 <img title="Expression Category" alt="Alt text" src="/images/ExprCategory.png">
 

class X { int a; }; 

X{10}   // this expression is prvalue 

X x;    // x is lvalue 

x.a     // it's lvalue (location) 