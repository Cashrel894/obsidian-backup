[Lab 10: Interpreters | CS 61A Fall 2024](https://insideempire.github.io/CS61A-Website-Archive/lab/lab10/index.html)

An interpreter is a program that allows you to interact with the computer using a specific language. It takes the code you write, interprets it, and then executes the corresponding actions, often using a more fundamental language to communicate with the computer hardware.

In Project 4, you'll develop an interpreter for the Scheme programming language using Python. Interestingly, the Python interpreter you've been using throughout this course is primarily written in the C programming language. At the lowest level, computers operate by interpreting machine code, which is a series of ones and zeros that instructs the computer on performing basic tasks such as arithmetic operations and data retrieval.

When we talk about an interpreter, there are two languages at work:

1. **The language being interpreted:** For Project 4, this is the Scheme language.
2. **The implementation language:** This is the language used to create the interpreter itself, which, for Project 4, will be Python.

  

**REPL**

A common feature of interpreters is the Read-Eval-Print Loop (REPL), which processes user inputs in a cyclic fashion through three stages:

- **Read:** The interpreter first reads the input string provided by the user. This input goes through a parsing process that involves two key steps:
    
    - The _lexical analysis_ step breaks down the input string into tokens, which are the basic elements or "words" of the language you're interpreting. These tokens represent the smallest units of meaning within the input.
    - The _syntactic analysis_ step takes the tokens from the previous step and organizes them into a data structure that the underlying language can understand. For our Scheme interpreter, we assemble the tokens into a `Pair` object (similar to a `Link`), to represent the structure of the original call expression.
        
        - The first item in the `Pair` represents the operator of the call expression, while the subsequent elements are the operands or arguments upon which the operation will act. Note that these operands can also be call expressions themselves (nested expressions).  
            

Below is a summary of the read process for a Scheme expression input:  

![](https://insideempire.github.io/CS61A-Website-Archive/lab/lab10/assets/parser.png)  

- **Eval:** This step evaluates the expressions you've written in that programming language to obtain a value. It involves the following two functions:
    
    - `eval` takes an expression and evaluates it based on the language's rules. When the expression is a call expression, `eval` uses the `apply` function to obtain the result. It will evaluate the operator and its operands in order. For example, in `(add 1 2)`, `eval` would identify `add` as the operator and `1` and `2` as the operands. It evaluates `add` to ensure it's a valid function and then evaluates `1` and `2` to ensure they're valid arguments.
    - `apply` takes the evaluated operator (the function) and applies it to the evaluated operands (the arguments). Note that it's possible that, during this process, `apply` needs to evaluate more expressions (like those found within the function body). This is where `apply` may call back to `eval`, and thus these two stages are _mutually recursive_.
- **Print:** Display the result of evaluating the user input.

  
Here's how all the pieces fit together:  

![](https://insideempire.github.io/CS61A-Website-Archive/lab/lab10/assets/repl.png)