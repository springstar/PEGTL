# Contrib and Examples

The PEGTL includes several parts that go beyond the core library functionality.

They are included both for convenience and to show how certain things can be done with the PEGTL.

## Notice

All feedback is highly welcome, in particular whether more sub-rules to serve as attachment points for actions are required.

Similarly, if you have written a grammar with the PEGTL that might be generally useful, you are welcome to contribute it for inclusion in future versions.

For all questions and remarks contact us at **taocpp(at)icemx.net**.

## Contents

* [Contrib](#contrib)
* [Examples](#examples)

## Contrib

###### `<tao/pegtl/contrib/abnf.hpp>`

* Core ABNF rules according to [RFC 5234, Appendix B](https://tools.ietf.org/html/rfc5234).
* Ready for production use.

###### `<tao/pegtl/contrib/alphabet.hpp>`

* Constants for ASCII letters.
* Shortens `string<'f','o','o'>` to `string<f,o,o>`.
* Ready for production use.

###### `<tao/pegtl/contrib/changes.hpp>`

* Control class components for [external switching style](Actions-and-States.md#external-switching).
* Ready for production use but might be changed in the future.

###### `<tao/pegtl/contrib/counter.hpp>`

* Control class for obtaining basic statistics from a parsing run, namely how often each rule
  1. was attempted to match,
  2. succeeded to match,
  3. failed to match.

###### `<tao/pegtl/contrib/http.hpp>`

* HTTP 1.1 grammar according to [RFC 7230](https://tools.ietf.org/html/rfc7230).
* Has been used successfully but is still considered experimental.

###### `<tao/pegtl/contrib/json.hpp>`

* JSON grammar according to [RFC 7159](https://tools.ietf.org/html/rfc7159) (for UTF-8 encoded JSON only).
* Ready for production use.

###### `<tao/pegtl/contrib/raw_string.hpp>`

* Grammar rules to parse Lua-style long (or raw) string literals.
* Ready for production use.

###### `<tao/pegtl/contrib/to_string.hpp>`

Utility function `to_string<>()` that converts template classes with arbitrary sequences of characters as template arguments into a `std::string` that contains these characters.

###### `<tao/pegtl/contrib/tracer.hpp>`

* Control class that prints a line of information to `std::cerr`
  1. when and where a rule is attempted to match,
  2. when and where a rule succeeded to match,
  3. when and where a rule failed to match.

###### `<tao/pegtl/contrib/unescape.hpp>`

This file contains helpers to unescape JSON and C and similar escape sequences.

* Utility functions frequently needed to unescape escape-sequences.
* Action classes that perform unescaping of escape-sequences.

###### `<tao/pegtl/contrib/uri.hpp>`

* URI grammar according to [RFC 3986](https://tools.ietf.org/html/rfc3986).
* This is still experimental.

## Examples

###### `src/example/pegtl/abnf2pegtl.cpp`

Reads a file with an [ABNF (RFC 5234)](https://tools.ietf.org/html/rfc5234)-style grammar and converts it into corresponding PEGTL rules in C++. Some extensions and restrictions compared to RFC 5234:

* As we are defining PEGs, the alternations are now ordered (`sor<>`).
* The *and*- and *not*-predicates from PEGs have been added as `&` and `!`, respectively.
* A single LF is also accepted as line ending.
* C++ identifiers are formed by replacing the dashes in rulenames with underscores.
* Reserved identifiers (keywords, ...) are rejected.
* Numerical values must fit into the corresponding C++ data type.

###### `src/example/pegtl/analyze.cpp`

A small example that provokes the [grammar analysis](Grammar-Analisys.md) to find problems.

###### `src/example/pegtl/calculator.cpp`

A calculator with all binary operators from the C language that shows

* how to use stack-based actions to perform a calculation on-the-fly during the parsing run, and
* how to build a grammar with a run-time data structure for arbitrary binary operators with arbitrary precedence and associativity.

In addition to the binary operators, round brackets can be used to change the evaluation order. The implementation uses `long` integers as data type for all calculations.

```sh
$ build/src/example/pegtl/calculator  "2 + 3 * -7"  "(2 + 3) * 7"
-19
35
```

In this example the grammar takes a bit of a second place behind the infrastructure for the actions required to actually evaluate the arithmetic expressions.
The basic approach is "shift-reduce", which is very close to a stack machine, which is a model often well suited to PEGTL grammar actions:
Some actions merely push something onto a stack, while other actions apply some functions to the objects on the stack, usually reducing its size.

###### `src/example/pegtl/csv1.cpp` and `src/example/pegtl/csv2.cpp`

Two simple examples for grammars that parse different kinds of CSV-style file formats.

###### `src/example/pegtl/hello_world.cpp`

Minimal parser-style "hello world" example from the [Getting Started)(Getting-Started.md) page.

###### `src/example/pegtl/json_parse.cpp`

Shows how to use the custom error messages defined in `json_errors.hpp` with the `<tao/pegtl/contrib/json.hpp>` grammar to parse command line arguments as JSON data.

###### `src/example/pegtl/json_build_one.cpp`

Extends on `json_parse.cpp` by parsing JSON files into generic JSON data structures using the "[switching style](Actions-and-States.md#changing-state)" of managing actions and states.

###### `src/example/pegtl/json_build_two.cpp`

Extends on `json_parse.cpp` by parsing JSON files into generic JSON data structures using a more monolithic approach to managing the states and actions.

###### `src/example/pegtl/json_count.cpp`

Shows how to use the included [counter control](#taopegtlcontribcounterhpp), here together with the JSON grammar from `<tao/pegtl/contrib/json.hpp>`.
Invoked with one or more JSON files as argument, will attempt to parse the files and print the statistics counters to `std::cout`.

###### `src/example/pegtl/lua53_parse.cpp`

Parses all files passed on the command line with a slightly experimental grammar that should correspond to the [Lua](http://www.lua.org/) 5.3 lexer and parser.

###### `src/example/pegtl/modulus_match.cpp`

Shows how to implement a custom parsing rule with the simplified calling convention.

###### `src/example/pegtl/recover.cpp`

See [PEGTL issue 55](https://github.com/taocpp/PEGTL/issues/55) and the source code for a description.

###### `src/example/pegtl/s_expression.cpp`

Grammar for a toy-version of S-expressions that shows how to include other files during a parsing run.

###### `src/example/pegtl/sum.cpp`

Simple example that adds a list of comma-separated `double`s read from `std::cin`.

###### `src/example/pegtl/unescape.cpp`

Uses the building blocks from `<tao/pegtl/contrib/unescape.hpp>` to show how to actually unescape a string literal with various typical escape sequences.

###### `src/example/pegtl/uri_trace.cpp`

Shows how to use the included [tracer control](#taopegtlcontribtracerhpp), here together with the URI grammar from `<tao/pegtl/contrib/uri.hpp>`.
Invoked with one or more URIs as command line arguments will attempt to parse the URIs while printing trace information to `std::cerr`.

Copyright (c) 2014-2017 Dr. Colin Hirsch and Daniel Frey
