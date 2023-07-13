---
layout: post
title: some principles to write fancy codes
image: 
  path: /assets/img/blog/triangle.png
description: >
  There are some principles that we can refer to  when we write codes.
sitemap: false
---

# some principles to write fancy codes


## Avoid bad naming pattern

Choose descriptive and meaningful names for variables, functions, and classes. Avoid using vague or overly general names like "data" or "temp". Instead, use names that accurately convey the purpose and meaning of the code element.

## Prefer composition over inheritance

When it comes to designing a code architecture, one of the most important principles to keep in mind is to prefer composition over inheritance. **While both concepts are designed to reuse code, inheritance can often lead to rigid and inflexible code structures.**

Inheritance requires bundling all common elements into a parent class, which can be useful in some cases. However, as soon as you encounter an exception to the commonality, it can require significant changes and result in a huge expense to refactor. This can make the code difficult to maintain and extend in the long run.

On the other hand, composition allows you to build complex objects from smaller, more modular parts. **This approach is more flexible and allows for greater code reuse**. Instead of relying on a fixed parent-child relationship, you can combine many classes for a particular use case. T**his leads to more maintainable and extensible code in the long run.**

However, the composition is not perfect. It requires boilerplate to initialize internal types, which leads to a lot of code repetition. Furthermore, we need to write many wrapper methods to expose information from internal types.

Ultimately, composition reduces coupling to reused code and increases adaptability as new requirements arise.

## Tradeoff between abstraction and coupling

Not all abstraction is good, and not all code repetition is bad. There is a hidden tradeoff that is often overlooked: coupling. While most of us understand the concept of coupling, we only feel its impact when trying to modify an over-coupled system.

**Coupling is considered to be the equal and opposite reaction of abstraction. For every bit of abstraction you add, you also add more coupling**. In some cases, such as when sharing member variables that are not valuable for the abstraction user, it may not be worth it. We can use abstraction when dealing with many implementations with complex construction. It is okay to have a little bit of repeated code, as it causes less pain when changing code than over-coupling.

## Don’t nest codes over than three levels

Nesting codes is when you add more inner blocks to a function. Adding each open brace means one more depth to the function.

```jsx
function test(){
  for(i=0; i<5; i++){
		if(i===2){   
    }
  }
}
```

**There are two methods to denest: extraction and inversion.**

Extraction involves extracting one of the inner blocks into a separate function. This results in small and concise functions that have a single responsibility.

Inversion involves inverting the nested condition and putting the unhappy case first. If we encounter the unhappy case condition, we exit the function since we have already returned. The else block is not actually needed, so we can flatten our else code into the main level. As a result, the happy condition moves down the function and all the error paths are indented. The function ends up with a sort of validation gatekeeping section of the code which declares the requirements and remains the crux of the real functionality at the main level.

## Premature optimization is bad

Performance, velocity, and adaptability are part of a tradeoff triangle. **Velocity refers to how quickly a new feature can be added, while adaptability is how well the system can change to meet new requirements.** Velocity and adaptability are interconnected, but sometimes they conflict with each other. For example, focusing solely on velocity can result in hastily put together code that leads to technical debt, ultimately weighing you down to a halt.

**Adaptability involves writing code in a way that enables changes to be made easily as new requirements arise. This can be achieved through creating reusable and extensible components, well-crafted interfaces, and configurability.** With the right design, the size of changes needed to add new features can be reduced, increasing velocity. However, building a highly adaptable system that can handle every possible case can end up **being a waste of time.**

**Highly extensible systems can also hurt performance**. Adding adaptability naturally increases the abstraction and indirection in code, which typically has a negative impact on performance.

The tradeoff among these factors depends on the stage of the project. While performance is important, it shouldn't be the primary consideration in the early stages of a project. Instead, focus on velocity and designing the system to be adaptable.

It's also important not to focus too much on micro performance, such as ++i and i++, which may be faster but can hurt readability. **Instead, prioritize readability in functions until they have been identified as the leading cause of performance issues.**

![tradeoff triangle](/assets/img/blog/triangle.png)

### How to optimize

1. have a real performance problem
2. measure
3. make 80% moves by swapping data structure or a well-known faster algorithm
4. profile and fix hot spots
5. analyze the code could be working under the hood and find ways to simply stuff
6. Consider memory usage and add in-memory caching where appropriate.