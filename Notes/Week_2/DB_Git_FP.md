# **Revision Notes — Simplicity, Git, Datomic & Functional Programming**

## **1. Simple Made Easy — Rich Hickey**

**Simple** means things are not unnecessarily intertwined. **Easy** means something is familiar, convenient or requires less effort.

- **Complecting:** Entangling independent responsibilities.
- **Composition:** Combining independent components.
- **Modularity:** Breaking a system into independently understandable and reusable parts.

**Example:**

```
# Entangled responsibilities
def process_order(order):
    validate(order)
    calculate_price(order)
    save_to_database(order)
    send_email(order)
```

Separating business calculations from database operations and notifications reduces unnecessary dependencies.

**Takeaway:** Easy to implement does not necessarily mean simple to maintain.

## **2. Identity, Value, State and Time**

| Concept  | Definition                                             | Example                     |
| -------- | ------------------------------------------------------ | --------------------------- |
| Identity | An entity that persists over time                      | Bank account A              |
| Value    | Immutable information                                  | ₹10,000                     |
| State    | Value associated with an identity at a particular time | Account A has ₹10,000 at T1 |
| Time     | Progression of states                                  | T1 → T2 → T3                |

The account changes its associated value. The values themselves do not change.

## **3. Mutability and Immutability**

**Mutability:** Modifying an existing object.

```
a = [1, 2, 3]
b = a

a.append(4)

print(b)  # [1, 2, 3, 4]
```

Both references observe the modification.

**Immutability:** Creating a new value instead of modifying an existing one.

```
a = (1, 2, 3)
b = a + (4,)

print(a)  # (1, 2, 3)
print(b)  # (1, 2, 3, 4)
```

Benefits include predictable behavior, safe sharing, easier debugging and reduced coordination.

**Takeaway:** Change does not necessarily require mutation.

## **4. Database as a Value — Datomic**

Traditional databases commonly expose the latest state. Datomic allows an entire database at a particular transaction boundary to be treated as an immutable value.

**Example: Customer address change**

| Transaction | Customer | City      | Operation |
| ----------- | -------- | --------- | --------- |
| T1          | 101      | Hyderabad | Assert    |
| T2          | 101      | Hyderabad | Retract   |
| T2          | 101      | Bangalore | Assert    |

The historical information is preserved.

### **Important concepts**

| Concept            | Meaning                                                      |
| ------------------ | ------------------------------------------------------------ |
| Basis              | A consistent database version                                |
| Datom              | Entity, Attribute, Value, Transaction, Assertion/Retraction  |
| Accretion          | Accumulating information over time                           |
| Structural sharing | Reusing unchanged data structures                            |
| Perception         | Observing a database value                                   |
| Transactor         | Coordinates and commits writes                               |
| Peer               | Reads data and executes queries in the original architecture |
| Time travel        | Querying historical database states                          |

**Example (pseudocode):**

```
db1 = get_database_value(connection)

# A new transaction occurs

db2 = get_database_value(connection)

query(db1)  # Hyderabad
query(db2)  # Bangalore
```

`db1` remains unchanged.

**Takeaway:** Separate information, observation and the process of changing information.

## **5. Git Internals**

Git is a specialized, distributed, content-addressable database.

### **Git objects**

| Object        | Purpose                                       |
| ------------- | --------------------------------------------- |
| Blob          | Stores file contents                          |
| Tree          | Stores filenames and directory structure      |
| Commit        | References a snapshot, parents and metadata   |
| Annotated tag | References an object with additional metadata |

### **Important internal concepts**

| Concept            | Meaning                                     |
| ------------------ | ------------------------------------------- |
| Hashing            | Identifying objects based on their content  |
| Snapshot           | Complete logical version of a project       |
| Reference          | Named pointer to an object                  |
| Deduplication      | Avoiding duplicate identical objects        |
| Structural sharing | Reusing unchanged blobs and trees           |
| Merkle DAG         | Graph of objects connected through hashes   |
| Packfiles          | Efficient storage of multiple objects       |
| Delta compression  | Storing differences between similar objects |
| Indexing           | Efficiently locating information            |
| Reachability       | Finding objects by following references     |

**Example:** Modify one file in a project containing 100 files. Git can reuse the 99 unchanged blobs instead of storing them again.

```
git cat-file -p HEAD
git ls-tree HEAD
git show HEAD~1:main.py
```

These commands inspect Git's stored information.

**Takeaway:** Git logically stores snapshots but physically optimizes their storage.

## **6. The Git Parable**

An educational story that explains why Git was designed the way it is by imagining building version control from scratch.

| Problem                           | Git's solution     |
| --------------------------------- | ------------------ |
| Losing previous work              | Snapshots          |
| Connecting snapshots              | Parent references  |
| Independent development           | Branches           |
| Remembering releases              | Tags               |
| Identifying objects independently | Hashing            |
| Combining histories               | Merging            |
| Preparing logical changes         | Staging area       |
| Reorganizing history              | Rebasing           |
| Wasting storage                   | Structural sharing |

### **Branches and immutability**

When a new commit is created, the branch pointer moves. Previous commits remain unchanged.

Rebase also creates new commits rather than modifying existing ones.

**Takeaway:** Understand the problem behind a Git command instead of simply memorizing it.

## **7. Functional Programming**

A programming paradigm that emphasizes transforming values using composable functions rather than modifying shared state.

### **Pure functions**

Same inputs produce the same output, without side effects.

```
def square(x):
    return x * x

square(5)  # 25
square(5)  # 25
```

### **Impure functions**

Depend on or modify external state.

```
counter = 0

def increment():
    global counter
    counter += 1
    return counter
```

Successive calls return different results.

### **Referential transparency**

An expression can be replaced by its value without changing the program's meaning.

```
square(5) + square(5)

# Equivalent to
25 + 25
```

### **Side effects**

Interactions with the external world, such as writing files, updating databases, sending emails and modifying shared state.

**Takeaway:** Separate pure computations from necessary side effects.

## **8. Higher-Order Functions**

Functions that accept or return other functions.

```
def apply(operation, value):
    return operation(value)

apply(square, 5)  # 25
```

They separate reusable computational patterns from specific behavior.

### **Map, Filter and Reduce**

**Map — Transform elements**

1

2

3

4

2

4

6

8

**Filter — Select elements**

1

2

3

4

2

4

**Reduce — Combine elements**

1

2

3

4

10

```
numbers = [1, 2, 3, 4]

list(map(lambda x: x * 2, numbers))
# [2, 4, 6, 8]

list(filter(lambda x: x % 2 == 0, numbers))
# [2, 4]

from functools import reduce

reduce(lambda a, b: a + b, numbers, 0)
# 10
```

## **9. Function Composition, Recursion and Folds**

**Composition:** Combining smaller functions into a larger computation.

```
def double(x):
    return x * 2

def square(x):
    return x * x

square(double(5))  # 100
```

**Recursion:** Solving a problem through smaller instances of itself.

```
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)
```

**Fold:** A reusable abstraction for combining elements of a recursive data structure.

```
Addition:       1 + (2 + (3 + 0)) = 6

Multiplication: 1 × (2 × (3 × 1)) = 6
```

Same traversal pattern, different combining functions.

**Currying and partial application:** Constructing specialized functions by supplying some arguments in advance.

```
from functools import partial

def multiply(a, b):
    return a * b

double = partial(multiply, 2)

double(5)  # 10
```

## **10. Lazy Evaluation**

Compute information only when it is required.

**Eager evaluation:** Calculate everything immediately.

**Lazy evaluation:** Defer calculations until demanded.

```
def numbers():
    n = 0
    while True:
        yield n
        n += 1

from itertools import islice

list(islice(numbers(), 5))
# [0, 1, 2, 3, 4]
```

The producer describes an infinite sequence, but the consumer requests only five values.

Python generators illustrate demand-driven computation, although they are not identical to persistent lazy lists.

**Takeaway:** Separate information generation from information consumption.

## **11. Why Functional Programming Matters — John Hughes**

The paper's central argument is **better modularity**, achieved through two forms of glue.

**Higher-order functions**

Separate an algorithm's structure from its specific behavior.

Example: map, fold, composition

**Lazy evaluation**

Separate producers from consumers and avoid unnecessary computation.

Example: infinite sequences

### **Important examples from the paper**

| Example            | Concept demonstrated                                         |
| ------------------ | ------------------------------------------------------------ |
| List reduction     | Reusable traversal using folds                               |
| Tree reduction     | Generalizing folds to recursive structures                   |
| Newton-Raphson     | Separating approximation generation from stopping conditions |
| Differentiation    | Reusing numerical computation modules                        |
| Integration        | Composing approximation sequences                            |
| Minimax            | Evaluating possible game states                              |
| Alpha-beta pruning | Avoiding unnecessary evaluation of game-tree branches        |

**Newton-Raphson example:**

```
Generate approximations:
10 → 6.25 → 5.125 → 5.0015 → 5.0000

Consumer:
Stop when consecutive values are close enough.

Result:
√25 ≈ 5
```

The approximation generator and stopping condition can be reused independently.

**Takeaway:** Functional programming matters because it provides powerful ways to combine independent, reusable computations.

## **12. Functional State Management**

Represent state changes as transformations.

```
def deposit(balance, amount):
    return balance + amount

s1 = 10000
s2 = deposit(s1, 5000)
s3 = deposit(s2, 2000)
```

\\[ NewState=f(OldState,Action) \\]

Existing values remain unchanged.

**Functional core, imperative shell:** Keep business calculations pure and manage external effects separately.

**Read database / Receive request**

Side effects

**Validate → Calculate → Return result**

Pure functional core

**Write database / Send response**

Side effects

Immutability reduces coordination requirements, but concurrent writes still require appropriate synchronization.

# **Final Revision — Connecting Everything**

| Concept            | Git              | Datomic             | Functional programming |
| ------------------ | ---------------- | ------------------- | ---------------------- |
| Identity           | Branch reference | Entity ID           | Domain identity        |
| Value              | Immutable object | Database value      | Immutable data         |
| State              | Commit snapshot  | Database at a basis | State value            |
| Time               | Commit ancestry  | Transaction history | State transitions      |
| Change             | New commit       | New transaction     | New value              |
| Structural sharing | Blobs and trees  | Index segments      | Persistent collections |
| Composition        | Object graph     | Facts               | Functions              |

## **One-line revision of all five resources**

| Resource                           | Main message                                                               |
| ---------------------------------- | -------------------------------------------------------------------------- |
| Simple Made Easy                   | Avoid unnecessary entanglement.                                            |
| Database as a Value                | Separate identity, values, state and time.                                 |
| Git Internals                      | Store and share immutable information efficiently.                         |
| The Git Parable                    | Understand the reasoning behind system design.                             |
| Why Functional Programming Matters | Compose reusable computations through higher-order functions and laziness. |

## **Golden principles to remember**

- Simple ≠ Easy.
- Identity ≠ Value.
- Change ≠ Mutation.
- Modularity ≠ Merely splitting code into files.
- Immutability ≠ Copying everything.
- Composition ≠ Entanglement.
- Functional programming ≠ Elimination of all side effects.

**The common objective is to design software that is predictable, understandable, reusable, maintainable and easier to change.**

Version 1

Return to current versionBranch in new chat