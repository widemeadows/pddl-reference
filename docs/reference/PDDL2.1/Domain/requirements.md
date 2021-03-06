# Requirements (PDDL 2.1)

[Report a problem with this guide](https://github.com/nergmada/pddl-reference/issues/new/choose)

The following page offers details of requirements defined in PDDL 2.1.

## Numeric Fluents

Support: <span style="color:yellow">High</span>  
Usage: <span style="color:green">High</span>

`(:requirements :numeric-fluents)`

Allows the inclusion of a `:function` block which represent numeric variables in the domain. e.g.

```LISP
(:functions
    (battery-amount ?r - rover)
)
```

Note that this overrides the definition in PDDL1.2 in most planners.

## Durative Actions

Support: <span style="color:yellow">High</span>  
Usage: <span style="color:green">High</span>

`(:requirements :durative-actions)`

Allows the use of `durative-action` in the domain definition. Durative actions are actions which have a duration they take to complete.

```LISP
(:durative-action move
    :parameters (<arguments>)
    :duration (= ?duration 5)
    :condition (logical_expression)
    :effect (logical_expression)
)
```

Note that this does **not** imply `:fluents`

## Durative Inequalities

Support: <span style="color:yellow">High</span>  
Usage: <span style="color:green">High</span>

`(:requirements :durative-inequalities)`

Allows the use of inequalities to express a duration. Rather than expressing that an action has a fixed length of time we can express that an action has a duration range using an inequality.

## Continuous Effects

Support: <span style="color:orange">Medium</span>  
Usage: <span style="color:green">High</span>

`(:requirements :continuous-effects)`

Allows the use of continuous effects on numerics within durative actions. Because durative actions take a period of time, we could model the change in a numeric value as some function of time. In reality support for this shaky. Linear functions are relatively easy for planners but non-linear effects are still an area of research.

## Negative Preconditions

Support: <span style="color:red">Low</span>  
Usage: <span style="color:green">High</span>

`(:requirements :negative-preconditions)`

Allows the use of `not` in preconditions. The way some planners model actions mean they are not capable of handling negative preconditions. This is more an inconvenience that a serious design flaw as for every predicate their is an opposing predicate which is true when it's false. I.e.

`rover-charged` and `rover-not-charged` are mutually exclusive. In the event where negative preconditions are not support we can introduce a second predicate which represents the negation of the predicate we want to express a negative precondition on.

## References

- [PDDL - The Planning Domain Definition Language](http://www.cs.cmu.edu/~mmv/planning/readings/98aips-PDDL.pdf), [Ghallab, M. Howe, A. Knoblock, C. McDermott, D. Ram, A. Veloso, M. Weld, D. Wilkins, D.]
- [PDDL2.1: An Extension to PDDL for Expressing Temporal Planning Domains](https://jair.org/index.php/jair/article/view/10352/24759), [Fox, M. Long, D.]
- [PDDL Examples](https://github.com/yarox/pddl-examples)
- [OPTIC - Optimising Preferences and Time Dependent Costs](https://nms.kcl.ac.uk/planning/software/optic.html)