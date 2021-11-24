---
title: "Ast"
weight: 1
draft: false
menu:
  docs:
    parent: data-structures
    weight: 1
---

# Abstract Syntax Trees
- Tree representation of code

## Lexical Analysis (Tokenization)
- Code is breaken up into a bunch of tokens that describe each part of the original code
  - Same approach behind syntax highlighting
- Tokens only represent the components of a file
  - Ie. they don't understand how the one piece of code fits together with another
  - Think of this as an array of different types of tokens
  - At this point, we know the identifiers, keywords, punctuation, but not how anything works together

## Syntax Analysis (Parsing)
- This step turns the list of tokens into an AST that represents the actual structure of the code
- This is where we start to understand whether the code is a function, variable, or something else
- No one AST format
  - It depends on the language you are parsing as well as the tool you are using to do the parsing

### AST Example
```json
{
 "type": "Program",
 "start": 0,
 "end": 14,
 "body": [
   {
     "type": "ExpressionStatement",
     "start": 0,
     "end": 14,
     "expression": {
       "type": "CallExpression",
       "start": 0,
       "end": 13,
       "callee": {
         "type": "Identifier",
         "start": 0,
         "end": 7,
         "name": "isPanda"
       },
       "arguments": [
         {
           "type": "Literal",
           "start": 8,
           "end": 12,
           "value": "🐼",
           "raw": "'🐼'"
         }
       ]
     }
   }
 ],
 "sourceType": "module"
}
```

## Code Generation
- Maniplutaing an AST is safer than doing so on the code text itself or on a list of tokens
  - Text doesn't provide enough context when performing manipulations
    - Ex: Manipulating text using string replacements or regex is really easy to get wrong
  - A list of tokens doesn't tell us how the code fits together
    - Ex: If we wanted to change the name of a variable, how would we know about resulting scoping issues or naming conflicts with just a list of tokens?
- From the AST we'd output whatever code is needed
  - Ex: The TypeScript compiler outputs JavaScript code
  - Ex: The Go compiler would output Assembly and eventually machine code

## What to do with ASTs?
- Three categories for using ASTs
  - Reading, modifying, and printing

### Read/Traversing ASTs
- First step is to parse text to create an AST
- Traversing an AST means that we look at every node found in it to understand how it fits in or actually perform an action on it
  - Ex: Code linting

### Modifying/Transforming ASTs
- Convert newer language version or features into older or more traditional features
  - Ex: Babel uses ASTs to compile React JSX code into JavaScript call functions
- Bundling code files into single programs
  - Ex: Webpack uses ASTs for this in JavaScript
- Test coverage tools use ASTs by adding a bunch of counters to your code and then checking the values of those counters after the code as run to see which pieces have been executed and which haven't

### Printing ASTs
- Printing and modifying generally go hand-in-hand
- A formating tool like Prettier in JavaScript (and maybe the go fmt tool) use ASTs to update your code to follow some style guidelines without chaning the code's meaning

# Sources
[ASTs - What are they and how to use them](https://www.twilio.com/blog/abstract-syntax-trees)