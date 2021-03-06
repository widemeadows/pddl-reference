# PDDL 2.2

[return to homepage](../../readme.md) | [Report a problem with this guide](https://github.com/nergmada/pddl-reference/issues/new/choose)

## Introduction

PDDL 2.2 introduces a key new feature not previously considered in PDDL, Timed Initial Literals. In previous versions, we assumed that a predicate was either true or false at the start.

This is not a realistic way of modelling because it fails to consider facts which may become true later. For example, in dynamic planning, where we are planning and re-planning as the world changes, we may wish to represent resources or states which are in the course of changing which we have no control over.

Imagine for instance, we are planning a rail schedule. We've generated some schedule from a plan, which the trains at this point in time are currently running to. Something goes wrong, for example a train breaks down. We now have less resources, but what we also have is a state that is currently in motion.

So for example we may have another dozen trains currently in use that won't become available until some later stage than the point we're currently planning at. We can't represent these realistically with actions because actions are optional, instead we need some mechanism to represent the uncontrollable change of state at a later point in time.

This is what timed initial literals allow us to do, by indicating at what point in time a fact which was previously false, becomes true.

Now we could arguably jury rig this behaviour with cleverly created durative actions and predicates, that are forced to run at a certain point and then subsequently release the resource after the time has elapsed, but these approaches will almost certainly have a significant overhead to them, because creating a durative action which the planners has to figure out it has to run in order to do everything else, will increase the state space necessary to explore.

Bottom line, Timed Initial Literals add a simple yet powerful new aspect to planning.

PDDL 2.2 also reintroduces Axioms as derived predicates, with a different simpler syntax to them. To be clear, these features are the same but consist of different syntax, typically planners support the newer syntax.

*This is speculative.

## Contents

- [Domain](./domain.md)
    - [Requirements](./domain.md#requirements)
        - [List of PDDL 2.2 Requirements](./domain.md#list-of-requirements)
    - [Derived Predicates](./domain.md#derived-predicates)
- [Problem](./problem.md)
    - [Timed initial literals](./problem.md#timed-initial-literals)

## References

- [PDDL - The Planning Domain Definition Language](http://www.cs.cmu.edu/~mmv/planning/readings/98aips-PDDL.pdf), [Ghallab, M. Howe, A. Knoblock, C. McDermott, D. Ram, A. Veloso, M. Weld, D. Wilkins, D.]
- [PDDL2.1: An Extension to PDDL for Expressing Temporal Planning Domains](https://jair.org/index.php/jair/article/view/10352/24759), [Fox, M. Long, D.]
- [PDDL2.2: The Language for the Classical Part of the 4th International Planning Competition](https://pdfs.semanticscholar.org/4b3c/0706d2673d817cc7c33e580858e65b134ba2.pdf) [Edelkamp, S. Hoffmann, J.]
- [PDDL Examples](https://github.com/yarox/pddl-examples)
- [OPTIC - Optimising Preferences and Time Dependent Costs](https://nms.kcl.ac.uk/planning/software/optic.html)