1. Don't use pointers for function arguments when references could work just as well.
2. const correctness
   - functions doesn't store parameters but only uses them -> `const &`
   - functions doesn't store parameters but only modifies them -> `&`
   - `const` members are frowned upon, because they break the regularity of the type. Enforce the const semantics by making use of const qualified member functions. Read about const correctness and const qualified member functions.
3. Prefer composition over inheritance. But if you do use implementation inheritance, consider inheriting privately.
4. avoid default parameters (constructors etc.)
5. if method is not inherited or used by users (only used internally), move it to the source file as a private implementation detail and because this is a private implementation detail, you can even limit linkage to inside the translation unit only so that the compiler can optimize more aggressively.
6. C++ type systems
   - By describing types and their semantics, you can prove your code is at least semantically correct, or it won't even compile. This means invalid code is unrepresentable. The compiler can also prove your code along those semantics, and then optimize more aggressively
   - Goals for Well-Typed code:
     	1. Make Illegal States unrepresentable
     	2. use `std::variant` and `std::optional` for formulations that are more natural and fit the business logic state better.
     	3. use phantom types for safety - make illegal behaviour a compile time error
     	4. write total functions - unsurprising behaviour + easy to use
   - Reading Material:
     	1. https://www.fluentcpp.com/2016/12/08/strong-types-for-strong-interfaces/
     	2. https://github.com/benroberts999/StrongType
7. Raw loops are imperative and error prone. Checkout standard algorithms and ranges
8. Standard C++ IOStreams and Locales
    - Your use of imperative parsing (`getline()`) can be replaced with stream iterators and algorithms. Once you make a type with a stream operator, you can use that stream iterator type.
