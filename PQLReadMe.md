# CS3203 PQL Notes

- [CS3203 PQL Notes](#cs3203-pql-notes)
- [Basic Relations](#basic-relations)
  - [Code 6](#code-6)
  - [`Follows / Follows*`](#follows--follows)
    - [Example with reference to Code 6](#example-with-reference-to-code-6)
  - [`Parent / Parent *`](#parent--parent-)
    - [Examples with reference to Code 6](#examples-with-reference-to-code-6)
  - [`Uses`](#uses)
    - [Examples with reference to Code-6](#examples-with-reference-to-code-6-1)
  - [`Modifies`](#modifies)
    - [Examples with reference to Code-6](#examples-with-reference-to-code-6-2)
  - [Code 5](#code-5)
  - [`Calls/Calls*`](#callscalls)
    - [Examples with reference to Code-5](#examples-with-reference-to-code-5)
  - [`Next/Next*`](#nextnext)
    - [Examples with reference to Code 5](#examples-with-reference-to-code-5-1)
  - [`Affects/Affects*`](#affectsaffects)
    - [Examples with reference to Code 5](#examples-with-reference-to-code-5-2)


# Basic Relations
| Design Abstraction | Relationship between                 |
| ------------------ | ------------------------------------ |
| `Follows/Follows*` | Statements                           |
| `Parent/Parent*`   | Statements                           |
| `Uses`             | (Statement / Procedure) and Variable |
| `Modifies`         | (Statement / Procedure) and Variable |


## Code 6
```
    procedure main {
      flag = 0;
      call computeCentroid;
      call printResults;
    }
    procedure readPoint {
        read x;
        read y;
    }
    procedure printResults {
        print flag;
        print cenX;
        print cenY;
        print normSq;
    }
    procedure computeCentroid {
01        count = 0;
02        cenX = 0;
03        cenY = 0;
04        call readPoint;
05        while ((x != 0) && (y != 0)) {
06            count = count + 1;
07            cenX = cenX + x;
08            cenY = cenY + y;
09            call readPoint;
        }
10        if (count == 0) then {
11            flag = 1;
        } else {
12            cenX = cenX / count;
13            cenY = cenY / count;
        }
14        normSq = cenX * cenX + cenY * cenY;
    }

```

## `Follows / Follows*`
- Definition:
  - For any 2 statements `s1`, `s2`
    - `Follows(s1, s2)` holds if they fulfill the following
      - Same nesting level
      - Same statement List
      - `s2` appears directly after `s1`
  - `s1` -> Leader
  - `s2` -> Follower
- `Follows*` is the transitive closure of `Follows`
  - IE: `Follows(1,2)` and `Follows(2,3)` $\to$ `Follows*(1,3)`
- Tips to check
  - Check which statement list directly contains the statement mentioned in a relationship
  - If `s1` comes directly before `s2` in the statement list `Follows(s1,s2)` holds.
  - If `s1` comes before `s2` in the statement list `Follows*(s1, s2)` holds.

### Example with reference to [Code 6](#code-6)

| Relation           | Holds? | Remarks                                              |
| ------------------ | ------ | ---------------------------------------------------- |
| `Follows(1,2)`     | Yes    | As 1 comes after 2 without nesting in the same block |
| `Follows(4,5)`     | Yes    | As 4 comes after 5 without nesting in the same block |
| `Follows(5, 10)`   | Yes    | As the statement at the same nesting after 5 is 10   |
| `Follows*(3, 10)`  | Yes    | As 3 $\to$ 4 $\to$ 5 $\to$ 10                        |
| `Follows*(1, 14)`  | Yes    | As 1 $\to$ 2 $\to$ 3 $\to$ 10 (From above) $\to$ 14  |
| `Follows(5, 6)`    | No     | Different nesting levels of 5 and 6                  |
| `Follows(9, 10)`   | No     | Different Nesting levels                             |
| `Follows(11, 12)`  | No     | Different blocks                                     |
| `Follows*(12, 14)` | No     | They are of different nesting and blocks             |

## `Parent / Parent *`
- Definition
  - For any 2 statements `s1`, `s2`
  - `Parent(s1, s2)` holds if they fulfill the following
    - `s2` is directly nesting in `s1`
  - `s1` -> parent
  - `s2` -> child
  - `Parent*` is the transitive closure of `Parent`
    - IE: `Parent(1, 2)` and `Parent(2, 3)` $\to$ `Parent*(1, 3)`
  - Tips to check   
    - If `s2` is directly in a child statment list of `s1` then `Parent(s1, s2)` holds
    - If `s2` is in any child statement list of `s1` then `Parent*(s1, s2)` holds

### Examples with reference to [Code 6](#code-6)

| Relation          | Holds? | Remarks                                      |
| ----------------- | ------ | -------------------------------------------- |
| `Parent(5, 7)`    | Yes    | 7 is directly within the statement list of 5 |
| `Parent(5, 8)`    | Yes    | 8 is directly within the statement list of 5 |
| `Parent(10, 11)`  | Yes    | 11 is within the then block of 10            |
| `Parent(10, 13)`  | Yes    | 13 is within the else block of 10            |
| `Parent*(5, 7)`   | Yes    | `Parent(5, 7)` $\to$ `Parent*(5, 7)`         |
| `Parent*(10, 13)` | Yes    | `Parent(10, 13)` $\to$ `Parent*(10, 13)`     |

Note*: The EG in [Code-6](#code-6) cannot clearly show the `Parent*` relation

## `Uses`
- `Uses` have different meaning depending on the arguments
- The right hand side (RHS) of `Uses` must always be a variable

| LHS Design Entity | Definition                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------- |
| Assign a          | Holds if `v` appears of RHS of `a`                                                          |
| Print P           | Holds if `v` appears in `P`                                                                 |
| If I              | Holds if `v` appear in conditional stmt or `Uses(s1, v)` holds for any statement within `I` |
| While w           | Holds if `v` appear in conditional stmt or `Uses(s1, v)` holds for any statement within `w` |
| Procedure p       | Holds if there is a statement or a called procedure `s` in `p` such that `Uses(s, v)`       |
| Call C            | Same as `Uses(p, v)`                                                                        |

### Examples with reference to [Code-6](#code-6)

| Relation                       | Holds? | Remarks                                                      |
| ------------------------------ | ------ | ------------------------------------------------------------ |
| `Uses(7, "x")`                 | Yes    | `x` appears on the RHS of assign stmt 7                      |
| `Uses(12, "count")`            | Yes    | `count` appears on RHS of assign stmt 12                     |
| `Uses(12, "cenX")`             | Yes    | `cenX` appears on RHS of assign stmt 12                      |
| `Uses(10, "count")`            | Yes    | 12 within else block and `Uses(12, "count")`                 |
| `Uses(10, "cenX")`             | Yes    | 12 within else block and `Uses(12, "cenX")`                  |
| `Uses("main", "cenX")`         | Yes    | 12 within procedure `main` and `Uses(12,"cenX")`             |
| `Uses("main", "flag")`         | Yes    | `main` calls `printResults` which `print flag`               |
| `Uses("computeCentroid", "x")` | Yes    | `computeCentroid` has stmt 7 which is `cenX = cenX + x;`     |
| `Uses(3, "count")`             | No     | assign stmt 3 does not have `count` on RHS                   |
| `Uses(10, "flag")`             | No     | if stmt 10 does not have `flag` on RHS of all its statements |
| `Uses(9, "y")`                 | No     | procedure `readPoint` does not use `y`                       |


## `Modifies`
- `Modifies` have different meaning depending on the arguments
- The Right hand side (RHS) of `Modifies` must always be a variable

| LHS Design Entity | Definition                                                                                                                               |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Assign a          | Holds if `v` appears on LHS of `a`                                                                                                       |
| Read r            | Holds if `v` appears in `r`                                                                                                              |
| If I              | Holds if there is a stmt `s` within `I` such that `Modifies(s, v)`                                                                       |
| While w           | Holds if there is a stmt `s` within `w` such that `Modifies(s, v)`                                                                       |
| Procedure p       | Holds if there is a stmt `s` within `p` such or `s` within a procedure called directly or indirectly from `p` such that `Modifies(s, v)` |
| Call c            | Holds if for procedure `p` called in `c`, `Modifies(p, v)` holds                                                                         |

### Examples with reference to [Code-6](#code-6)
| Relation                             | Holds? | Remarks                                                              |
| ------------------------------------ | ------ | -------------------------------------------------------------------- |
| `Modifies(1, "count")`               | Yes    | `count` is on the LHS of assign 1                                    |
| `Modifies(7, "cenX")`                | Yes    | `cenX` is on the LHS of assign 7                                     |
| `Modifies(9, "x")`                   | Yes    | `x` is modified in procedure `readPoint`                             |
| `Modifies(10, "flag")`               | Yes    | `flag` is modified by stmt 11 within stmt 10                         |
| `Modifies(5, "x")`                   | Yes    | `x` is modified in call 9 within while 5                             |
| `Modifies("main", "y")`              | Yes    | `y` is modified within `call printResult` within `main`              |
| `Modifies(5, "flag")`                | No     | `flag` is not modified by any stmt within 5                          |
| `Modifies("printResults", "normSq")` | No     | `normSq` is not modified by any stmt within procedure `printResults` |



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


## `Calls/Calls*`
- Definition
  - For any procedure `p` and `q`
  - `Calls(p, q)` is true if procedure `p` directly calls `q`
  - `p` Caller procedure
  - `q` Procedure called
  - `Calls*` is the transitive closure of `Calls`.
    - IE: `Calls(a, b) and Calls(b, c)` -> `Calls*(a,c)` 
    - IE: `Calls(a, b)` -> `Calls*(a,b)`
  - Some properties
    - As there can be no recursive statements, `Calls(a, b)` -> `Calls(b, a)` is False

### Examples with reference to [Code-5](#code-5)
| Relation                    | Holds? | Remarks                                                                           |
| --------------------------- | ------ | --------------------------------------------------------------------------------- |
| `Calls("First", "Second")`  | Yes    | On the third line of procedure `First` it calls `Second`.                         |
| `Calls("Second", "Third")`  | Yes    | At line 5 of procedure `Second` is calls `Third`.                                 |
| `Calls*("First", "Second")` | Yes    | As `Calls("First", "Second")` is true, -> this is also true.                      |
| `Calls*("First", "Third")`  | Yes    | As `Calls("First", "Second")` and `Calls("Second", "Third")`, thus its true.      |
| `Calls("First", "Thrid")`   | No     | There are no statements in procedure `First` that calls `Third`                   |
| `Calls("Second", "First")`  | No     | There are no statements in procedure `Second` that calls `First`.                 |
| `Calls*("Second", "First")` | No     | There are no statements from `Second` that directly not indirectly calls `First`. |


## `Next/Next*`
- Definition
  - For any 2 program lines `p1` and `p2` within the same procedure.
  - `Next(p1, p2)` is true if `p2` can be executed directly after `p1` in some execution sequence.
  - `p1` -> First execution statement.
  - `p2` -> Second execution statement.
  - `Next*(p1, p2)` is true if `p2` is executed somewhere after `p1`. (Transitive closure of `Next`)
    - IE: `Next(p1, p2)` and `Next(p2, p3)` -> `Next*(p1, p3)`
  - Some Properties
    - `Next(p1, p2)` -/-> not `Next(p2, p1)` due to the existence of while loops

### Examples with reference to [Code 5](#code-5)
| Relation       | Holds? | Remarks                                                                |
| -------------- | ------ | ---------------------------------------------------------------------- |
| `Next(2, 3)`   | Yes    | `3` will execute after `2`.                                            |
| `Next(3, 4)`   | Yes    | `4` will execute after `3` if it enters the while loop.                |
| `Next(3, 7)`   | Yes    | `7` can execute after `3` if it does not enter the while loop.         |
| `Next(5, 6)`   | Yes    | `6` will execute after `5`.                                            |
| `Next(7, 9)`   | Yes    | `9` can execute after `7` if the statement enters the `else` block.    |
| `Next(8 ,10)`  | Yes    | `10` will happed after `8` if the execution enters the `if` statement  |
| `Next*(1, 2)`  | Yes    | As `Next(1, 2)` -> `Next*(1, 2)` is true                               |
| `Next*(1, 3)`  | Yes    | As `Next(1, 2)` and `Next(2, 3)` -> `Next*(1, 3)`                      |
| `Next*(2, 5)`  | Yes    | This will be true if the execution enters the while loop               |
| `Next*(4, 3)`  | Yes    | This will be true if the while loop repeats itself                     |
| `Next*(5, 5)`  | Yes    | This will be true if the while loop repeats itself                     |
| `Next*(5, 8)`  | Yes    | This will be true if it enters the while loop and the if statement.    |
| `Next*(5, 12)` | Yes    | This will be true if it enters the while loop and finished execution.  |
| `Next(6, 4)`   | No     | They do not occur directly after each other                            |
| `Next(7, 10)`  | No     | Either the if block or the else block will be executed first.          |
| `Next(8, 9)`   | No     | They are in different blocks and cannot execute after one another.     |
| `Next*(8, 9)`  | No     | They are in different blocks and cannot execute after one another.     |
| `Next*(5, 2)`  | No     | `2` is not part of the loop and thus cannot execute after statement 5. |

## `Affects/Affects*`
- Definition
  - It is a relation between assign statements.
  - `Affects(a1, a2)` holds if
    - `a1` and `a2` are in the same procedure.
    - `a1` modifies a variable `v` which `a2` uses.
    - There is a control flow path from `a1` to `a2` such that v is not modified in any assign, read, procedure or call statements on that path
- `Affects*` is the transitive closure of `Affects`
  - IE: `Affects(a1, a2)` and `Affects(a2, a3)` -> `Affects(a1, a3)`

### Examples with reference to [Code 5](#code-5)
| Relation         | Holds? | Remarks                                      |
| ---------------- | ------ | -------------------------------------------- |
| `Affects(2, 6)`  | Yes    | `2` modifies `i` and `6` uses `i`            |
| `Affects(4, 8)`  | Yes    | `4` modifies `x` and `8` uses `x`            |
| `Affects(4, 10)` | Yes    | `4` modifies `x` and `10` uses `x`           |
| `Affects(6, 6)`  | Yes    | `6` both modifies and uses `i`               |
| `Affects(1, 4)`  | Yes    | `1` modifies `x` and `4` uses `x`            |
| `Affects(1, 8)`  | Yes    | `1` modifies `x` and `8` uses `x`            |
| `Affects(1, 10)` | Yes    | `1` modifies `x` and `10` uses `x`           |
| `Affects(1, 12)` | Yes    | `1` modifies `x` and `12` uses `x`           |
| `Affects(2, 10)` | Yes    | `2` modifies `i` and `10` uses `i`           |
| `Affects(9, 10)` | Yes    | `9` modifies `z` and `10` uses `z`           |
| `Affects(9, 11)` | No     | `9` does not modify anything that `11` uses. |
| `Affects(9, 12)` | No     | `9` does not modify anything that `12` uses. |
| `Affects(2, 3)`  | No     | `2` does not modify anything that `3` uses.  |
| `Affects(9, 6)`  | No     | `9` happens after `6`                        |
