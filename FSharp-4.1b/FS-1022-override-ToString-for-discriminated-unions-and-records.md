# F# RFC FS-1022 - Override `ToString` for discriminated unions and records

The design suggestion [Override `ToString` for discriminated unions and records](https://fslang.uservoice.com/forums/245727-f-language/suggestions/7574961-override-tostring-for-discriminated-unions-and-r) has been marked "approved in principle".
This RFC covers the detailed proposal for this suggestion.

* [x] Approved in principle
* [x] [User Voice Request](https://fslang.uservoice.com/forums/245727-f-language/suggestions/7574961-override-tostring-for-discriminated-unions-and-r)
* [x] Details: [under discussion](https://github.com/fsharp/FSharpLangDesign/issues/123)
* [x] Implementation: completed for [unions](https://github.com/Microsoft/visualfsharp/pull/1589) and [records](https://github.com/Microsoft/visualfsharp/pull/2430)

# Summary
[summary]: #summary

Generate an override of `ToString` automatically for discriminated unions and records at compile time, which gives the same result as `sprintf "%A"`.

# Motivation
[motivation]: #motivation

Adding a `ToString` override manually for each type is tedious and repetitive, and it is often necessary to add it to make F# types produce meaningful output with `String.Format`, or in the debugger visualizer.

# Detailed design
[design]: #detailed-design

As part of compiling a record type or a discriminated union, the compiler generates an `object.ToString()` override which is semantically equivalent to:

```fsharp
override t.ToString() = sprintf "%A" t
```
This automatic override is only generated by the compiler if an explicit override was not given in the source code.

# Drawbacks
[drawbacks]: #drawbacks

No drawbacks have been identified so far.

# Alternatives
[alternatives]: #alternatives

No other designs have been considered so far.

# Unresolved questions
[unresolved]: #unresolved-questions

* The most straightforward code to generate for `ToString` is just  `sprintf "%A"`, but this is slow. Since we statically know the type, and even the specific union case, can we avoid relying on reflection and generate more performant code at compile time instead?