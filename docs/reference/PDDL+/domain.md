# Domain
[return to homepage](../../readme.md) | [return to PDDL+ main page](./main.md) | [Report a problem with this guide](https://github.com/nergmada/pddl-reference/issues/new/choose)

PDDL+ introduces two new constructs to domains in PDDL, the first is `Processes` and the second is `Events`, both can essentially be thought of as uncontrollable durative actions, and uncontrollable instantaneous actions respectively.

```LISP
(define
    (domain ballphysics)
    (:requirements :time :typing)
    (:types
        ball - object
    )
    (:predicates
        (held ?b - ball)
    )
    (:functions
        (velocity ?b - ball)
        (distance-to-floor ?b - ball)
    )
    (:process FALLING
        :parameters (?b - ball)
        :precondition (and
            (not (held ?b))
            (< (velocity ?b) 100)
        )
        :effect (and
            (increase (velocity ?b) (* #t 9.8))
            (decrease (distance-to-floor ?b) (* #t (velocity ?b)))
        )
    )
    (:event HIT-GROUND
        :parameters (?b - ball)
        :precondition (and
            (not (held ?b))
            (<= (distance-to-floor ?b) 0)
            (> (velocity ?b) 0)
        )
        :effect (
            (assign (velocity ?b) (* -0.8 (velocity ?b)))
        )
    )

    ; Actions omitted
)
```

## Contents

- [Requirements](#requirements)
- [Processes](#processes)
- [Events](#events)

## Requirements

[back to contents](#contents)

Support: <span style="color:green">Universal</span>
Usage: <span style="color:green">High</span>

`(:requirements <requirement_name>)`

Requirements are similar to import/include statements in programming languages, however as PDDL is a kind of declarative language, it is a `:requirement` as a given planner is "required" to facilitate some aspect of the language.

Multiple `:requirements` can be specified through a space separated list e.g.

`(:requirements :strips :adl :typing)`

Only one additional requirement is necessary in order to include **both** processes and events, this requirement is `:time`, as shown below

`(:requirements :time)`

## Processes

[back to contents](#contents)

Support: <span style="color:orange">Medium</span>
Usage: <span style="color:orange">Low</span>

```LISP
(:process FALLING
    :parameters (?b - ball)
    :precondition (and
        (not (held ?b))
        (< (velocity ?b) 100)
    )
    :effect (and
        (increase (velocity ?b) (* #t 9.8))
        (decrease (distance-to-floor ?b) (* #t (velocity ?b)))
    )
)
```

A process is defined in three sections, `:parameters`, `:precondition` and `:effect` which define what a process is acting upon, when a process is acting upon it and what effect that process has when it acts. These directly correspond with the definition of an [action](../PDDL/domain.md#actions) found in PDDL 1.2, but can include the expressivity of numerics as seen in PDDL 2.1.

Confusingly despite bearing similar behaviour to a durative action, a process conditions over it's duration, therefore the precondition can be thought of more like an `over all` condition in PDDL 2.1

`#t` represents the time of the process, it is used to calculate continuous numeric effects, therefore, if about halfway through the process some action might wish to run which conditions on one of the numeric fluents affected by the process, `#t` can be used to calculate the intermediate value of such a numeric fluent.

## Events

[back to contents](#contents)

Support: <span style="color:orange">Medium</span>
Usage: <span style="color:orange">Low</span>

```LISP
(:event HIT-GROUND
    :parameters (?b - ball)
    :precondition (and
        (not (held ?b))
        (<= (distance-to-floor ?b) 0)
        (> (velocity ?b) 0)
    )
    :effect (
        (assign (velocity ?b) (* -0.8 (velocity ?b)))
    )
)
```

An event is defined the same as a process in three sections, `:parameters` which are the objects upon which the effect acts `:precondition` which is the condition which must be satisfiend in order for the event to run and `:effect` which is the effect the event has.

Note that the distinctive nature of an event is that whilst it can occur more than once it generally does not happen back to back. Essentially, an event is a discrete moment in time, where as a process is an interval of time.

In the case above we see that the condition of hitting the ground is that velocity is greater than 0, and the effect is that velocity is negated, which ensures that - at least for a period - this event cannot run again.

## Reference

- [PDDL+: Modelling Continuous Time Dependent Effects](https://pdfs.semanticscholar.org/d391/59cb5dfcc21aafd3049002d854ec341037a7.pdf) [Fox, M. Long, D.]