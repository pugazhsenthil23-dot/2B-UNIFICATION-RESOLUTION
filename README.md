# Experiment 2(b) – Implementation of Unification and Resolution Algorithm

## Aim

To implement unification and resolution algorithm using Python.

## Scenario

In an AI-based expert system for automated reasoning, the system needs to resolve queries by unifying logical predicates and applying resolution inference.

Given:

* Rule 1: If John is a human, then John is a mortal.
* Fact 1: Human(John)
* Query: Is John mortal?

## Procedure

1. Define the unification function (`unify`).

   * If both terms are identical, return the current substitution.
   * If one term is a variable, unify it with the other term.
   * If both terms are compound expressions, unify their corresponding parts recursively.
   * Otherwise, return `None`.
2. Define the variable unification function (`unify_var`).
3. Define the resolution function (`resolution`).
4. Create a knowledge base containing facts and implications.
5. Define the query `Mortal(John)`.
6. Run the resolution function.
7. Print whether the query is resolved.

## Program

```python
def unify(x, y, theta=None):
    if theta is None:
        theta = {}

    if theta is None:
        return None
    elif x == y:
        return theta
    elif isinstance(x, str) and x.islower():
        return unify_var(x, y, theta)
    elif isinstance(y, str) and y.islower():
        return unify_var(y, x, theta)
    elif isinstance(x, list) and isinstance(y, list) and len(x) == len(y):
        result = unify(x[0], y[0], theta)
        if result is None:
            return None
        return unify(x[1:], y[1:], result)
    else:
        return None


def unify_var(var, x, theta):
    if var in theta:
        return unify(theta[var], x, theta)
    elif isinstance(x, str) and x in theta:
        return unify(var, theta[x], theta)
    else:
        theta[var] = x
        return theta


def apply_substitution(term, theta):
    if isinstance(term, list):
        return [apply_substitution(item, theta) for item in term]
    elif isinstance(term, str) and term in theta:
        return apply_substitution(theta[term], theta)
    return term


def resolution(kb, query, facts):
    for clause in kb:
        conclusion = clause[1]
        theta = unify(conclusion, query, {})

        if theta is not None:
            premise = apply_substitution(clause[0], theta)
            if premise in facts:
                return True

    return query in facts


knowledge_base = [
    [["Human", "John"], ["Mortal", "John"]]
]

facts = [
    ["Human", "John"]
]

query = ["Mortal", "John"]

if resolution(knowledge_base, query, facts):
    print("Query is resolved: John is Mortal")
else:
    print("Query could not be resolved")
```

## Output

```text
Query is resolved: John is Mortal
```

## Result

Thus, the unification and resolution algorithm was implemented successfully using Python. The query `Mortal(John)` was resolved using the given knowledge base and fact.
