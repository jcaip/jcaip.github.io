---
images:

    - url: http://mathworld.wolfram.com/images/eps-gif/VertexColoring_750.gif 
      alt: graph coloring
      title: Constraint Satisfication Solver
---

This is a post about [constraint satisfaction](https://en.wikipedia.org/wiki/Constraint_satisfaction_problem).

If you're interested by this blog post, you should check out my AI notes [here](/posts/Artificial-Intelligence) for more information about constraint satisfaction and other similar concepts.

## The Problem
For a group of variables, $$x_1, x_2, \ldots , x_n$$, with domains $$D_1, D_2, \ldots , D_n$$, we want to find an assignment for each variable such that no constraints are violated.

From a problem, we can create a **constraint graph**, which is a visualization of the dependencies between variables. Each variables is represented by a vertex and an edge represents a constraint.

We can use a constraint solver to solve a graph coloring problem. The graph coloring problem is an assignment of labels to the vertices of a graph such that no two neighbors have the same labels. One use case of this algorithm is to determine colors of countries on a map - where countries are vertices and borders are edges. You can read more about graph coloring [here](https://en.wikipedia.org/wiki/Graph_coloring).

## Approach
We are using **Backtracking Search** in order to try and find a suitable set of values for the graph.

In order to speed up our search, we use several heurestics:
+ **Least Remaining Values** - when choosing a variable to assign, pick the variable that has the fewest states. 
+ **Degree Heurestic** - when choosing a variable to assign, pick the variable with the highest degree. The idea behind these two heurestics is to detect failure early, to avoid unnecessary expansion.
+ **Least Constraing Value** - when choosing a value to assign to a variable, pick the value that leaves the most remaining choices for the other unassigned variables.

To ensure that we do not break constraints, we employ **Local Consistency** - after each assignment, check that no constraints are violated. 
We run inference via **Forward Checking** - after each assignment, look at all the neighbors and remove all remaining values. 

The runtime to our algorithm is $$O(d^n)$$ where $$d$$ is the number of elements in the domain and $$n$$ is the number of variables.

## Code
You can see the full code on my github [here](https://github.com/jcaip/constraint_solver).

**Backtracking Search** - This is the core of our algorithm.

1. Assigns a vertex a value
2. See if that breaks any constraints
3. If yes, backtrack otherwise repeat

```python
#csp is a graph
@classmethod
def backtracking_search(cls, csp, assignment={}):
    if len(assignment.keys()) == len(csp.vs):
        return assignment

    var = cls.select_unassigned_var(assignment, csp)
    for value in cls.order_domain_values(var, assignment, csp):
        #add it into a possible assignment
        possible_assignment = assignment
        possible_assignment[var['name']] = value
        #check if assignment is possible
        if cls.consistency_checker(possible_assignment, csp):
            #if so commit, 
            assignment[var['name']] =  value
            new_csp = cls.inference(assignment, csp.copy())
            if new_csp:
                #add inferences to assignment
                result = cls.backtracking_search(new_csp, assignment)
                if result:
                    return result
        assignment.pop(var['name'])
    return False
```

**Choosing a vertex** - We use a combination of the mininum remaining values heurestic and the degree heurestic to pick a variable to assign.
```python
#use the mininum remaining values heurestic
#use the degree heurestic
@staticmethod
def select_unassigned_var(assignment, csp):
    current_best = None
    for vertex in csp.vs:
        if vertex['name'] not in assignment:
            if current_best:
                if len(vertex['domain']) > len(current_best['domain']):
                    current_best = vertex
                #if min remaining values are the same, then use degree heurestic
                elif len(vertex['domain']) == len(current_best['domain']):
                    if csp.degree(vertex) > csp.degree(current_best):
                        current_best = vertex
            else:
                current_best = vertex
    return current_best
```

**Choosing a value** - sets what value gets assigned to a specific variable.
```python
@classmethod
def order_domain_values(cls, var, assignment, csp):
    value_range = []
    for value in var['domain']:
        assignment[var['name']] = value
        new_csp = cls.inference(assignment, csp.copy())
        if new_csp:
            vals_remaining = sum([len(d) for d in new_csp.vs['domain']])
            value_range.append((value, vals_remaining))
        del assignment[var['name']]
    value_range.sort(key=lambda x:x[1])
    return [x[0] for x in value_range]
```


**Inference** - rules out future possible domain values that would otherwise break the constraint graph via forward checking.
```python
#inference using foward checking
@classmethod
def inference(cls, assignment, csp):
    neighboring_set = set()
    for set_val in assignment:
        neighboring_set.intersection_update(set(csp.neighbors(set_val)))

    for neighbor in neighboring_set:
        neighbor_name =  csp.vs[neighbor]['name']
        if neighbor_name not in assignment:
            for possible_value in csp.vs[neighbor]['domain']:
                assignment[neighbor_name] = possible_value
                if cls.consistency_checker(assignment, csp) == False:
                    csp.vs[neighbor]['domain'].remove(possible_value)
                del assignment[neighbor_name]
            if csp.vs[neighbor]['domain'] == None:
                return False
    return csp
```

## Result
Ran a quick test with US states.
```python
mysolver = ConstraintSolver(test)
mysolver.add_constraint(not_equal_constraint, 'california', 'oregon')
mysolver.add_constraint(not_equal_constraint, 'california', 'nevada')
mysolver.add_constraint(not_equal_constraint, 'oregon', 'washington')
mysolver.add_constraint(not_equal_constraint, 'nevada', 'arizona')
mysolver.add_constraint(not_equal_constraint, 'california', 'arizona')
result =(mysolver.backtracking_search(mysolver.csp))
```

And we get back a valid coloring - no two bordering states have the same color!
```python
{'california': 'r', 'oregon': 'g', 'nevada': 'g', 'arizona': 'b', 'washington': 'r', 'idaho': 'r', 'utah': 'r'}
```
