---
created: 2024-09-13T12:32:15 (UTC -03:00)
tags: []
source: https://www.c-sharpcorner.com/article/discard-variable-in-c-sharp-net/?ref=dailydev
author: Ajay Kumar
---

# Discard Variable in C# .NET

> ## Excerpt
> The discard variable (_), introduced in C# 7.0, allows developers to ignore values intentionally. It enhances code clarity by signaling unused data in tuples, pattern matching, out parameters, and LINQ queries, promoting cleaner, more efficient, and maintainable programming in .NET applications.

---
## Introduction

In the ever-evolving C# and .NET world, developers continually seek ways to write cleaner, more efficient, and readable code. One such feature introduced in C# 7.0 is the discard variable, represented by an underscore (\_). This seemingly simple addition can significantly impact how we handle unnecessary values in our code. In this article, we will explore the discard variable, how it works, and where it can be effectively used in C# .NET applications.

## What is the Discard Variable?

The discard variable is a special, read-only variable that can be used to indicate that a value is intentionally being ignored. Instead of creating a variable to store a value that will not be used, you can use a discard to tell the compiler that the value can be safely ignored. This leads to cleaner and more maintainable code.

### Basic Usage

The discard variable is used by assigning the underscore (\_) to a value that you want to ignore. Here’s a simple example.

In this example, we are calling a Divide method that returns a tuple containing the quotient and remainder of a division operation. By using the discard variable, we indicate that we are only interested in the quotient and can ignore the remainder.

## Common Scenarios for Using Discards  
 

### Deconstructing Tuples

When working with tuples, there are often values that are not needed. The discard variable can be used to ignore these unwanted values.

Here, we are only interested in the last name and age, so we use the discard variable for the first name.

### Pattern Matching

Discards can be used in pattern matching to ignore specific values.

In this example, we are only interested in the last name property of the Person object.

### Out Parameters

Discards can be used without parameters when you only need some of the values returned by a method.

Here, the TryParse method returns a boolean indicating success and an integer result. The discard variable is used to ignore the boolean value.

### LINQ Queries

In LINQ queries, the discard variable can be used to ignore unwanted values when projecting results.

In this example, we are projecting the even numbers from the collection while ignoring their original indices.

### Benefits of Using Discards

-   **Readability:** Discards make your intent clear, indicating that certain values are not needed, which enhances code readability.
-   **Maintainability:** By not creating unnecessary variables, your code becomes easier to maintain and less prone to errors.
-   **Performance:** While the performance benefits may be negligible in many cases, using discards can reduce memory usage by avoiding the allocation of unnecessary variables.

## Conclusion

The discard variable in C# .NET is a powerful feature that allows developers to write more concise and expressive code. By using discards, you can communicate which values are irrelevant, making your code cleaner and easier to understand. Whether you are deconstructing tuples, performing pattern matching, or working without parameters, the discard variable is a valuable tool in your C# programming arsenal. Embrace this feature to improve the readability and maintainability of your code in modern C# applications.
