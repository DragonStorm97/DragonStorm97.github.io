+++
title = 'Attributes'
date = 2024-09-14T18:56:16+02:00
draft = false
math = true
tags = ['c++', 'attributes', 'nodiscard', 'fallthrough', 'maybe_unused']
authors = ["Johnathan Jacobs"]
+++

[cppreference](https://en.cppreference.com/w/cpp/language/attributes)

## `[[nodiscard]]`/`[[nodiscar("reason...")]]` (CPP17)

If a function declared `nodiscard` or returning an enumeration or class declared `nodiscard` is called without the return value being used,
other than a cast to void, the compiler is encouraged to issue a warning.

Appears in a function, enumeration, or class declaration.

## `[[maybe_unused]]` (CPP17)

Suppresses warnings on unused entities.

## `[[fallthrough]]` (CPP17)

Indicates that the fall-through from the previous case label is intentional and should not be diagnosed by a compiler that warns on fall-through.

## `[[likely]]` / `[[unlikely]]` (CPP20)

[video resource](https://www.youtube.com/watch?v=ew3wt0g99kg)

## `[[no_unique_address]]` (CPP20)

[no_unique_address](https://en.cppreference.com/w/cpp/language/attributes/no_unique_address)

## `[[assume(expression)]]` (CPP23)

[assume](https://en.cppreference.com/w/cpp/language/attributes/assume)

## `[[noreturn]]` (CPP11)

Indicates that the function will not return control flow to the calling function after it finishes
(e.g. functions that terminate the application, throw exceptions, loop indefinitely, etc.).

```cpp
[[noreturn]] void f()
{
    throw "error";
    // OK
}
 
void q [[noreturn]] (int i)
{
    // behavior is undefined if called with an argument <= 0
    if (i > 0)
        throw "positive";
}
 
// void h() [[noreturn]]; // error: attribute applied to function type of h, not h itself
 
int main()
{
    try { f(); } catch(...) {}
    try { q(42); } catch(...) {}
}
```

## `[[deprecated]]`/`[[deprecated("reason")]]` (CPP14)

Mark something as deprecated, and optionally provide a reason.

```cpp
[[deprecated]]
void TriassicPeriod()
{
    std::clog << "Triassic Period: [251.9 - 208.5] million years ago.\n";
}
 
[[deprecated("Use NeogenePeriod() instead.")]]
void JurassicPeriod()
{
    std::clog << "Jurassic Period: [201.3 - 152.1] million years ago.\n";
}
```
