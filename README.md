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
        2. use `std::variant` and `std::optional` for formulations that are more natural and fit the business logic state better
        3. use phantom types for safety - make illegal behaviour a compile time error
        4. write total functions - unsurprising behaviour + easy to use
   - Reading Material:
        1. https://www.fluentcpp.com/2016/12/08/strong-types-for-strong-interfaces/
        2. https://github.com/benroberts999/StrongType
        3. https://bulldogjob.pl/readme/taking-advantage-of-the-type-system-in-c
7. Raw loops are imperative and error prone. Checkout standard algorithms and ranges
   - Reading Materials:
        1. https://meetingcpp.com/blog/items/raw-loops-vs-stl-algorithms.html
        2. https://ericniebler.com/2018/12/05/standard-ranges
        3. https://belaycpp.com/2021/06/22/dont-use-raw-loops
9. Standard C++ IOStreams and Locales
    - Your use of imperative parsing (`getline()`) can be replaced with stream iterators and algorithms. Once you make a type with a stream operator, you can use that stream iterator type.  
10. Google C++ Style Guide
    - copyable/movable types https://google.github.io/styleguide/cppguide.html#Copyable_Movable_Types
    - streams usage https://google.github.io/styleguide/cppguide.html#Streams
    - const usage https://google.github.io/styleguide/cppguide.html#Use_of_const
    - constexpr, constinit, consteval https://google.github.io/styleguide/cppguide.html#Use_of_constexpr
11. General guideline to pass-by-value, pass-by-reference, pass-by-value-then-move
     - If your function wants to take ownership (whether it wants to modify the thing or not), accept the parameter by value. This allows for the code that's calling your function to either give you a copy, or to give you an rvalue reference. You're passing the buck up to the layer above your function. It's not your problem, you don't care, as long as you get the thing by value. Then, once you have the thing by value, you use std::move() to stick it in it's final destination, which will likely be as a member variable in the object that your function belongs to.
     - Passing by non-const-reference should be relatively rare, but happens. Use this for when the function won't own the data (e.g. won't make copies), but will only modify and then return.
     - If you want non-owning, non-mutating access, use const-ref if the object in question is larger than sizeof(void*) on your platform, or has a non-trivial constructor / destructor.
     - At some point or another, I heard that there was a macro, or type_trait thing in Boost somewhere that would automatically deduce this for you, but I never thought it was very elegant looking so I forgot about it until I saw your question.
