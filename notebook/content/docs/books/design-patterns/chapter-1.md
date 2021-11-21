---
title: "Chapter 1"
weight: 1
menu:
  docs:
    parent: design-patterns
    weight: 1
---

# Chapter 1. Introduction
- Object oriented design is hard as it is and even harder, if not impossible, to get right the first time designing something for a new problem
    - You must find the right granularity, interfaces and inheritance hierarchies
- Design patterns allow devs to not have to reinvent the wheel when designing software
    - On a personal level this is ideal when you've already solved a problem at least once before, why resolve the problem over and over again when you could use the same pattern you had previously used
- Design patterns describe how to reuse designs and architectures
- Design patterns can improve maintainability and documentation of systems
    - This is because they create a specification for object and class interations as well as their underlying intent

## 1.1 What is a Design Pattern?
- Describes a problem that will come up repeatedly and an approach to its solution that can be easily be replicated over and over
- A pattern contains four elements
    1. The **pattern name** describes the pattern in one or two words. Essential for letting us design at a higher level of abstraction
    2. The **problem** covers when to use the pattern. It also explains the problem along with its context
        - Might describe conditions that must be met in order for the pattern to make sense
    3. The **solution** covers the pieces that make up the design
    4. The **consequences** are the results and trade-offs of using a pattern
        - Often space and time trade-offs, as well as language and implementation issues
        - It is important to explicitly list the consequences so we can understand and evaluate them
- Design patterns can be different depending on the perspective and level of granularity
    - This book focuses on *descriptions of communicating objects and classes that are customized to solve a general design problem in a particular context*
- Identifies the classes and instances, their roles and how they work together, as well as what each of their responsibilities are

## 1.3 Describing Design Patterns
- Patterns are described with a standardized template

### Pattern Name and Classification
- Important because it will become part of your design vocabulary

### Intent
- Addresses what the pattern does, what the rationale is, and what problem it is used to solve

### Also Known As
- Other well-known name for the pattern

### Motivation
- A scenario that shows a problem and how the pattern solves it

### Applicability
- Covers situations in which the pattern can be used
- Examples of bad designs that the pattern can help with
- How to recognize these situations

### Structure
- Visually shows the classes in the pattern work togetherr using Object Modeling Technique (OMT) and interaction diagrams

### Participants
- Classes and/or objects in the pattern and their responsibilities

### Collaborations
- How the participants work together to accomplish their responsibilities

### Consequences
- Covers the trade-offs and considerations that must be understood when using a pattern

### Implementation
- Language-specific issues
- Pitfalls, hints, or techniques to be aware of when using a pattern

### Sample Code
- Code snippets that illustrate how to implement a pattern

### Known Uses
- Examples of the pattern being used in real-world situations

### Related Patterns
- Closely related patterns, important difference between them, and other patterns that should be used with the one being discussed

## 1.4 The Catalog of Design Patterns

