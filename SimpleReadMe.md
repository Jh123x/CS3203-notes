# CS3203 Simple Rules

- [CS3203 Simple Rules](#cs3203-simple-rules)
- [SIMPLE Language Rules](#simple-language-rules)
- [Procedures](#procedures)
  - [Variables](#variables)
  - [Statements](#statements)
  - [Conditions](#conditions)
  - [Operators](#operators)
- [Program Types](#program-types)
- [Statement numbers](#statement-numbers)
  - [The following do not have indexes](#the-following-do-not-have-indexes)
- [Simple Concrete Syntax Grammar](#simple-concrete-syntax-grammar)
  - [Things to note about the above](#things-to-note-about-the-above)
- [Symbal Abstrct Syntax Grammar](#symbal-abstrct-syntax-grammar)
  - [Meta Symbols](#meta-symbols)
  - [Lexical Tokens](#lexical-tokens)
  - [Grammar Rules](#grammar-rules)
  - [Attribute and attribute value types](#attribute-and-attribute-value-types)
- [AST](#ast)
  - [Things to note for AST](#things-to-note-for-ast)
  - [Code 4](#code-4)
    - [AST](#ast-1)
  - [While code fragment](#while-code-fragment)
    - [AST](#ast-2)
  - [Expressions](#expressions)
- [Control Flow Graph](#control-flow-graph)
  - [Things to note](#things-to-note)
  - [Code 5](#code-5)
    - [Control flow graph](#control-flow-graph-1)

# SIMPLE Language Rules
- Consist of 1 or more procedures
- There are no arrays
- There are no pointers

# Procedures
1. Non-empty list of statements
2. No parameters
3. No nesting
4. No recursion

## Variables
1. Unique name
2. Global scope
3. Integer type
4. No declarations

## Statements
1. Assignments
2. While Loops
3. If then else Statements
4. Call
5. Statement
6. Print
7. Read

## Conditions
1. Boolean Expressions consisting of Operators

## Operators
| Operator |
| -------- |
| `+`      |
| `-`      |
| `*`      |
| `\`      |
| `%`      |
| `<`      |
| `>`      |

# Program Types
| Type of Statement | Explanation                                                                                          | Example                            |
| ----------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Calls             | Procedures are called by name                                                                        | `call readPoint`                   |
| Assignments       | Left hand side of assignment is assigned with the expression on the right hand side                  | `x = 2; x = a + 2 * b`             |
| While             | Execute statements in the while body until the condition become `false`                              | `while (i > 0) {...}`              |
| If                | If condition is `true`, execute `then` branch otherwise execute `else` branch. The else is mandatory | `if (i < 0) then {...} else {...}` |
| Read              | Read input from stdin                                                                                | `read x`                           |
| Print             | Print outputs the variable to stdout                                                                 | `print x`                          |

# Statement numbers
- They are indexed for easy referencing
- The first statement in the first procedure is labelled statement 1
## The following do not have indexes
- Procedure definition
- Empty lines
- `else` keywords

# Simple Concrete Syntax Grammar

| Symbol / Token | Representation            | Explanation                                          |
| -------------- | ------------------------- | ---------------------------------------------------- |
| `a*`           | 0 or more times of a      | Similar to regex                                     |
| `a+`           | 1 or more times of a      | Similar to regex                                     |
| `a | b`        | a or b                    | Similar to regex                                     |
| brackets       | used for grouping         | Similar to regex                                     |
| Letter         | `A-Z | a-z `              | Capital or small letter                              |
| Digit          | `0-9`                     | Single digit numbers                                 |
| Name           | `Letter (Letter | Digit)` | String of letters and digits, starting with a letter |
| Integer        | `Digit+`                  | 1 or more digits **No leading Zeros**                |
| Program        | `procedure+`              | A program consist of 1 or more procedures            |

| Program Syntax | Syntax                                                                                                                                                                           |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Procedure      | `'procedure' proc_name '{' stmtLst '}'`                                                                                                                                          |
| stmtLst        | `stmt+`                                                                                                                                                                          |
| stmt           | `read \| print \| call \| while \| if \| assign`                                                                                                                                 |
| read           | `'read' var_name`                                                                                                                                                                |
| print          | `'print' var_name`                                                                                                                                                               |
| call           | `'call' proc_name`                                                                                                                                                               |
| while          | `'while' '(' cond_expr ')' '{' stmtLst '}'`                                                                                                                                      |
| if             | `'if' '(' cond_expr ')' 'then' '{' stmtLst '}' 'else' '{' stmtLst '}'`                                                                                                           |
| assign         | `varname '=' expr`                                                                                                                                                               |
| cond_expr      | `rel_expr \| '!' '(' rel_expr ')' \| ‘(’ cond_expr ‘)’ ‘&&’ ‘(’ cond_expr ‘)’ \| ‘(’ cond_expr ‘)’ ‘\|\|’ ‘(’ cond_expr ‘)’`                                                     |
| rel_expr       | `rel_factor ‘>’ rel_factor \| rel_factor ‘>=’ rel_factor \| rel_factor ‘<’ rel_factor \| rel_factor ‘<=’ rel_factor \| rel_factor ‘==’ rel_factor \| rel_factor ‘!=’ rel_factor` |
| rel_factor     | `var_name \| const_value \| expr`                                                                                                                                                |
| expr           | `expr '+' term \| expr '-' term \| factor`                                                                                                                                       |
| term           | `term '*' factor \| term '/' factor \| term '%' factor \| factor`                                                                                                                |
| factor         | `var_name \| const_value \| '(' expr ')'`                                                                                                                                        |
| var_name       | `NAME`                                                                                                                                                                           |
| proc_name      | `NAME`                                                                                                                                                                           |
| const_value    | `INTEGER`                                                                                                                                                                        |

## Things to note about the above
1. 2 procedures cannot have the same name
2. Calls to non-existing procedure produces an error
3. Recursive and cyclic calls are not allowed.
   1. EG: 
      1. A calls B, B calls C, C calls A.
4. Simple rules are case sensitive
5. Spaces can be used freely in Simple
   1. EG: The following are the same
      1. `x + y`
      2. `x+y`
      3. `x+ y`
6. The statements are indexed, program line are same as statement number
7. Procedure names can be teh same as variable names
8. Logical expressions may only appear as the conditions for `while`/`if`
9. Logical expressions are fully bracketed
10. Constants are sequences of digits
11. Constants cannot start with 0 if it consists of more than 1 digit
12. Print statements can only print a single variable
13. Read statements read in an integer for a single variable
14. There are no explicit boolean values (`true` / `false`)
15. Expressions are left-associative due to the following rules
    1.  expr: `expr '+' term | expr '-' term | term`
    2.  term `term '*' factor | term '/' factor | term '%' factor | factor`
    3.  factor `var_name | const_value | '(' expr ')'`

# Symbal Abstrct Syntax Grammar

## Meta Symbols
| Symbols | Meaning   | Remarks |
| ------- | --------- | ------- |
| `a+`    | 1 or more |         |
| `\|`    | or        |         |

## Lexical Tokens
| Lexical Token | Meaning                   | Remarks                                 |
| ------------- | ------------------------- | --------------------------------------- |
| Letter        | `A-Z \| a-z`              | Caps or small letter                    |
| Digit         | `0-9`                     | Single digit numbers                    |
| Name          | `Letter (Letter \| Digit) | Starts with a letter                    |
| Integer       | `Digit+`                  | More than 1 digit (Cannot start with 0) |

## Grammar Rules
| Abstraction | Rule                                          |
| ----------- | --------------------------------------------- |
| program     | `procedure+`                                  |
| procedure   | `stmtLst`                                     |
| stmtLst     | `stmt+`                                       |
| stmt        | `read \| print \| call \| while \| assign`    |
| read        | `variable`                                    |
| print       | `variable`                                    |
| while       | `cond_expr stmtLst`                           |
| if          | `cond_expr stmtLst stmtLst`                   |
| assign      | `variable expr`                               |
| cond_expr   | `rel_expr \| not \| and \| or `               |
| not         | `cond_expr`                                   |
| and         | `cond_expr cond_expr`                         |
| or          | `cond_expr cond_expr`                         |
| rel_expr    | `gt \| gte \| lt \| lte \| eq \| neq`         |
| gt          | `rel_factor rel_factor`                       |
| gte         | `rel_factor rel_factor`                       |
| lt          | `rel_factor rel_factor`                       |
| lte         | `rel_factor rel_factor`                       |
| eq          | `rel_factor rel_factor`                       |
| neq         | `rel_factor rel_factor`                       |
| rel_factor  | `variable \| constant \| expr`                |
| expr        | `plus \| minus \| times \| div \| mod \| ref` |
| plus        | `expr expr`                                   |
| minus       | `expr expr`                                   |
| times       | `expr expr`                                   |
| div         | `expr expr`                                   |
| mod         | `expr expr`                                   |
| ref         | `variable constant`                           |

## Attribute and attribute value types
| Attribute   | Attribute value types |
| ----------- | --------------------- |
| `procedure` | `.procName` `.stmt#`  |
| `call`      | `.procName` `.stmt#`  |
| `variable`  | `.varName`            |
| `read`      | `.varName`  `.stmt#`  |
| `print`     | `.varName`  `.stmt#`  |
| `while`     | `.stmt#`              |
| `if`        | `.stmt#`              |
| `assign`    | `.stmt#`              |
| `constant`  | `.value`              |

| Attr Type   | Output Value |
| ----------- | ------------ |
| `.stmt#`    | `Integer`    |
| `.procName` | `Name`       |
| `.varName`  | `Name`       |
| `.value`    | `Integer`    |


# AST
## Things to note for AST
1. Order of executable statements must be **explicitly** represented and well defined
2. Left and right components of binary operations must be stored and correctly identified
3. Identifiers and their assigned values must be stored for assignment statements
4. Nodes typically correspond with the non-terminals of the `syntax grammar`
5. The directed edges appear for each grammar rule.
6. Format of `value:type` for each block
   1. Values are empty except for terminal blocks (Refer to the example below)


## Code 4
```
     procedure main {
01      flag = 0;
02      call computeCentroid;
03      call printResults;
     }
     procedure readPoint {
04        read x;
05        read y;
    }
    procedure printResults {
06        print flag;
07        print cenX;
08        print cenY;
09        print normSq;
    }
    procedure computeCentroid {
10        count = 0;
11        cenX = 0;
12        cenY = 0;
13        call readPoint;
14        while ((x != 0) && (y != 0)) {
15            count = count + 1;
16            cenX = cenX + x;
17            cenY = cenY + y;
18            call readPoint;
        }
19        if (count == 0) then {
20            flag = 1;
        } else {
21            cenX = cenX / count;
22            cenY = cenY / count;
        }
23        normSq = cenX * cenX + cenY * cenY;
    }
```

### AST
![Code 4 AST](image/ast%20for%20code%204.png)


## While code fragment
```
while (i > 0) {

    x = x + z \* 5;

    z  = 2;

    if (x > 0) then {…} else {…}

}
```

### AST
![While code fragment AST](image/ast%20for%20while.png))


## Expressions
![Expressions AST](image/ast%20for%20exprs.png)

# Control Flow Graph

## Things to note
- 1 CFG per procedure
- If statements in CFG have **Diamond Shape**
- While loops have a **Loop Shape**
- Show Procedure **Exit Nodes**

## Code 5
```
      procedure First {
      read x;
      read z;
      call Second; }
  
      procedure Second {
01        x = 0;
02        i = 5;
03        while (i!=0) {
04            x = x + 2*y;
05            call Third;
06            i = i - 1; }
07        if (x==1) then {
08            x = x+1; }
          else {
09            z = 1; }
10        z = z + x + i;
11        y = z + 2;
12        x = x * y + z; }
  
      procedure Third {
          z = 5;
          v = z;
          print v; }

```

### Control flow graph
![CFG for code 5](image/cfg%20for%20code%205.png)