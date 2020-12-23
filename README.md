# iOS Interview Questions and Answers for Senior Developers
I recently got into the position of leading the technical interview when my client was searching for a new senior iOS developer.
It's a challenging task to evaluate the skills and knowledge of another developer. So in this series, I want to share my results of the most useful iOS and Swift questions and answers with you. I categorised the questions into different topics:

1- Swift Programming Language
2- Networking
3- Persistence & Databases 
4- Concurrency
5- Architecture & Design Patterns

# 1- Swift Programming Language
# 1. What do you like about Swift?
Examples of positive aspects the interviewed person could name are type safety, type inference, short and adept syntax, functional programming patterns, generics, protocol oriented programming, extensions, built-in error handling etc.

# 2. What is the difference between value and reference types?
Value types are usually defined as struct or enum and reference types as a class. When copying a value type, the variable will create a copy of its data. The reference type variable will just point to the original data in the memory.

# 3. What are optionals? How can you unwrap an optional in a safe way? What is forced unwrapping?
Optional types are used to handle the abscence of a value. You can use if let or guard statements to unwrap the optional and get the value safely.
Forced unwrapping provides direct access to the value by using the exclamation mark. When the optional has no value (is nil), forced anwrapping leads to a runtime error.

# 4. What are higher order functions? Give some examples of higher order functions in Swift. Explain one of them.
A higher order function is a function that takes one or more functions as arguments or returns a function as its result. Examples are map, filter and reduce. Higher order functions improve readability and decrease the length of code. E.g. map iterates through the array and changes each element of that array based on the closure passed to the method.

# 5. What benefits do generics have? How can you define a generic function in Swift?
Generic code enables you to avoid code duplication by writing flexible and reusable components. You can define a generic function by putting the placeholder type name inside angle brackets right after the function name.

func methodName<T>(parameterName: T)
You can then use this generic type for parameters or as return type. An indepth explanation with examples is available in the official Swift documentation.

# 6. What is a protocol associated type in Swift?
Associated types are a powerful way of making protocols generic. You can declare one or more associated types as part of the protocol’s definition. An associated type gives a placeholder name to a type that is used as part of the protocol. The actual type to use for that associated type is specified when the protocol is adopted.

# 7. What is protocol composition?
Protocol compositions can be created by combining two or more protocols with a & sign. This is really useful when you want a type to conform to multiple protocols at the same time.
func methodName(paramterName: ProtocolOne & ProtocolTwo)

# 8. What are opaque types in Swift? Where can opaque types be useful?
To define a type as opaque, you can use the some keyword.
func methodName() -> some ProtocolName
Opaque types give developers the capability to return a concrete type without having to expose it. Returning an opaque type is almost like returning a protocol. In both cases, the caller cannot see the concrete type. The difference is that unlike protocols, an opaque type still refers to a specific type.
An indepth explanation with examples is available in the official Swift documentation.

# 9. What is Automatic Reference Counting in Swift? Explain how it works.
Swift uses Automatic Reference Counting (ARC) to manage the memory usage of the app. Every time a new class instance is created, ARC allocates memory to store it. To free up the allocated memory, it needs to determine when an instance is no longer needed. To do that, ARC counts the strong references of each instance. It will deallocate the instance when it's count of strong references drops to zero.

# 10. Explain the difference between a weak, unowned and a strong reference?
Creating a strong reference to an instance will increase it's reference count.
A weak or an unowned reference only points to an object and does not increase it's reference count.
The difference between a weak and an unowned reference is that a weak reference allows the object to become nil. An unowned reference presumes that it will never become nil during it's lifetime.

# 11. What is a memory leak? How can you avoid them? Explain your process for tracing and fixing a memory leak.
A memory leak is created when memory cannot be deallocated due to retain cycles. A retain cycle is created when two instances keep a strong reference to each other so ARC cannot release any of them. Memory leaks can be avoided by using a weak or unowned reference when needed.
For detecting memory leaks, Xcode offers a built-in debug memory graph. An alternative way is the Instruments tool. Xcode's debug memory graph is easier to use, Instruments offers a more in-depth look into memory leaks.



