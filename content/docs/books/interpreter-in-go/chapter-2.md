---
title: "C2: Parsing"
weight: 2
draft: false
menu:
  docs:
    parent: interpreter-in-go
    weight: 2
---

# Chapter 2: Parsers

## 2.1 - Parsers
- Turns its input into a tree-like structure that repsents that input
    - This input is commonly tokens from a lexer
- Ex: JavaScript's `json.parse()` method provides the functionality of a JSON parser so we can easily take a string and turn it into json
    - Conceptualy, the parser here works the same way as a parser for a programming language would work
    - The amin difference between the json parser and that of a programming language lies in the input
        - With the json parser you can also see what the structure of the output will look like because the input is simply json in surrounded by quotes. Whereas with a programming language the input in some source code, and we probably can't immediately tell what the data structure will look like after parsing it
- Most interpreters and compilers use an `abstract syntax tree (aka. syntax tree or AST)`
    - The `abstract` part comes in because some details that are included in the source code are not included in the parsed representation
        - Ex: Semicolons, newlines, whitespace, comments, braces, brackets, and parentheses are not included, but they do help guide the parser
    - AST formats vary from parser to parser depending on the programming language

Example of what parsing might look like using JavaScript
```javascript
if (3 * 5 > 10) {
    return "hello";
} else {
    return "goodbye";
}
```

The code above might result in an AST that looks like this:
```json
{
    type: "if-statement",
    condition: {
        type: "operator-expression",
        operator: ">",
        left: {
            type: "operator-expression",
            operator: "*",
            left: { type: "integer-literal", value: 3 },
            right: { type: "integer-literal", value: 5 }
        },
        right: { type: "integer-literal", value: 10 }
    },
    consequence: {
        type: "return-statement",
        returnValue: { type: "string-literal", value: "hello" }
    },
        alternative: {
        type: "return-statement",
        returnValue: { type: "string-literal", value: "goodbye" }
    }
}
```

- Even though the data structure we produced no longer has the `"`, `(`, etc. from the source code (hence the abstract part), it still represents it pretty well
- Parsing is also called `syntactic analysis` because it is during this stepp that the input is check for correctness

## 2.2 - Why Not a Parser Generator?
- Generate parsers based on the description of a language
- Usually they take in `Context Free Grammars (CFGs)`
    - Set of rules that describe how to form valid sentences in a language
    - Usually use Backus-Naur Form (BNF) or Extended Backus-Naur Form (EBNF)
- People suggest using parser generators because parses are well-suited for being automatically generated
- Overall, it most likely makes sense to use a parser generator
    - The reason for writing your own is to understand how parsers work and what the generator is doing under the hood

## 2.3 - Writing a Parser for the Monkey Programming language
- Two main approaches to parsing
    - Top-down
    - Bottom-up
- This book covers a `top down operator precedence` parser
    - Based on recursive descent
    - Also called a `Pratt parser` after its inventor Vaughan Pratt
- Difference between bottom-up and top-down is the direction in which they construct the AST
    - Top-down starts with the root node, while bottom-up goes the other way around

## 2.4 - Parser's First Steps: Parsing Let Statements
- `let`, `var`, and `const` are common ways to declare a variable
    - Ex: `let name = "Garrett"`
    - These keywords essentially bind the value on the right of the `=` to the identifier on the left
- Expressions vs. Statements
    - Expressions produce values, while statements do not
    - Whether something produces a value is different across languages
        - Ex: In some language `let add = func(...) {...}` is valid and thus the function literal is an expression. In other languages that isn't valid and functions have to be declared on their own as top level entities, thus they are not expressions
- A program is broken down into statements in the AST

```go
package ast

type Node interface {
    TokenLiteral() string
}

type Statement interface {
    Node statementNode()
}

type Expression interface {
    Node expressionNode()
}

type Program struct {
    Statements []Statement // Just a series of nodes that implement the statement interface
}

func (p *Program) TokenLiteral() string {
    if len(p.Statements) > 0 {
        return p.Statements[0].TokenLiteral()
    } else {
        return ""
    }
}
```

- The program node will be the root node of every program
- A struct is created for each type of statement
    - Note for my DSL builder I won't be able to do this because the mechanics of the language being built are unkown at compile time

```go
type LetStatement struct {
    Token token.Token // the token.LET token Name *Identifier
    Value Expression
}
```

- The parser gets its own package

```go
type Parser struct {
    l *lexer.Lexer

    curToken  token.Token
    peekToken token.Token
}
```

- Note that a Parser actually has pointer to a specific lexer
- Recursive decent parsing is when the root level AST program node is created and the parser goes token by token, calling the appropriate functions that know how to break down the token into sets based on the context they provide and parse them accordingly
- Method `expectPeek` seemed to be unnecessary, however it is an assertion method that ensure the next token is of the correct type given the type of statement being parsed and most parsers have them

```go

func (p *Parser) expectPeek(t token.TokenType) bool {
    if p.peekTokenIs(t) {
        p.nextToken()
        return true }
    else {
        return false
    }
}
```

- 