---
layout: post
title: Work Breakdown for the Agilist
---

This post is based on my <a href="http://programmers.stackexchange.com/a/263596/151189">Answer</a> to the programmers stackexchange question <a href="http://programmers.stackexchange.com/q/263589/151189">"How to break up a programming project into tasks for other developers?"</a>.

How to breakdown work?
So you have this big project coming up.
No matter whether you're requirements engineer, project manager, product owner, scrum master, lead developer, architect, team lead, line manager or whatever, many of us are in a role or situation where we're confronted with the question "how do we split this?".
Last but not least to also answer a bunch of other questions: How much effort is this? How much time will it take? Can we do it?

So here's my hitlist of how to *breakdown a programming project*.

## Basics
- *Don't go alone.* Try to involve your team-mates as much as possible.
- *Travel lightweight.*
- *Democracy, but not too much.* Often it's not about what satisfies the biggest number of people, but what hurts the least number of people.
- Keep *what* (needs to be done) and *how* (it is done) *separate*.
- Learn about *Scrum* ("what"), *<abbr title="Extreme Programming">XP</abbr>* ("how"), *Kanban* ("how much"), *Lean* ("what not") and DevOps ("with whom").
- Learn about *Software Craftsmanship*, *Clean Code* and *Pragmatic Programming*.
- Good architecture is about *maximizing the number of decisions not taken*.
- Scrum / XP / Lean / Agile is about *maximizing the amount of work not done*. *<abbr title="You Ain't Gonna Need It">YAGNI</abbr>*.
- The *Primary Value of Softtware* is that you can *easily change* it. That it does what it should do is important but that's only its *Secondary Value*.
- Prefer an *iterative* approach, use *time boxes* for almost everything, especially meetings, make *Parkinson's Law* your friend.
- Balance team structure with an understanding of *Conway's Law* and *Tuckman's Stages of Team Development*.
- Programming is a quaternity, it is *science*, *engineering*, *art* and *craft* all at the same time, and those need to be in balance.
- Just because *Scrum* / *XP* / XYZ is good for someone (including me) doesn't necessarily mean that it's also good for you / suits your environment. *Don't believe the hype*, don't blindly follow it, understand it first.
- *Inspect and Adapt!* (Scrum Mantra)
- *Avoid Duplication - Once and only Once!* (XP Mantra) aka *<abbr title="Don't Repeat Yourself">DRY</abbr>*.

## "What world" work breakdown process
- Collect Requirements as *User Stories / Job Stories* into a *Product Backlog*.
- *User* (of User Story) similar to *Actor* (in UML) similar to *<a href="http://www.romanpichler.com/tools/persona-template/">Persona</a>* similar to *Role*.
- *Refine User Stories* until the meet your team's *Definition of Ready* based on *INVEST* (Scrum Meeting: *Backlog Refinement*).
- Sort the *Product Backlog* by *Business Value*.
- Don't start work on a Story before it's *Ready Ready* (ready according to the Definition of Ready).
- Use *Planning Poker* to estimate the effort of Stories in *Story Points*. Use *Triangular Comparison* to ensure consistency of the estimates.
- *Yesterday's Weather* is the best estimate, hope the worst.
- *Split Stories* if they are too big.
- Improve delivery culture with a *Definition of Done*.
- Don't accept the implementation of a User Story before it's *Done Done* (done according to the Definition of Done).
- Check your progress with *Burndown Charts*.
- Regulary check with your Stakeholders whether what the team delivers is what's really needed. (Scrum Meeting: *Sprint Review*)

## Story Breakdown
- List and Describe *Users* / *Personas* / *Actors* / *Roles* (Product Owner)
- Epic -> Stories (User Story or Job Story) (Product Owner)
- Story -> Acceptance Criteria (Product Owner)
- Story -> Subtasks (Dev Team)
- Acceptance Criteria -> Acceptance Tests (Spec: Product Owner, Impl: Dev Team)

## "How world" realization
- Use *OOA/D*, *UML* and *CRC Cards*, but *avoid the big design upfront*.
- Implement *object-oriented*, *structured* and *functional* at the same time as much as possible, regardless of the programming language.
- Use *Version Control* (preferably *distributed*).
- Start with *Acceptance Tests*.
- Apply *TDD*, leeting the *Three Laws of TDD* drive you through the *Red-Green-Refactor-Cycle*, with *Single-Assert-Rule*, *4 A's*, *GWT (Given When Then) from *BDD*.
- Apply the *SOLID* and the *package principles* to manage *Coupling and Cohesion*. Example: S in SOLID is SRP = Single Responsibility Principle, significantly reduces the number of edit- resp. merge-conflicts in teams.
- Know *Law of Demeter / Tell, Don't Ask*.
- Use *Continuous Integration*, if applicable even *Continuous Delivery* (DevOps).
- Use *Collective Code Ownership* based on an agreed common Coding Standard.
- Apply *Continuous Design Improvement* (fka Continuous Refactoring).
- *The Source Code is the Design.* Still upfront thinking is indispensable, and nobody will object a few good clarifying UML diagrams.
- XP doesn't mean no day is architecture day, it means every day is architecture day. It's a focus on architecture, not a defocus, and the focus is in the code.
- Keep your *Technical Debt* low, avoid the four design smells *Fragility*, *Rigidity*, *Immobility* and *Viscosity*.
- Architecture is about business logic, not about persistence and delivery mechanisms.
- Architecture is a team sport (*there is no 'I' in Architecture*).
- *Design Patterns*, *Refactoring* and the *Transformation Priority Premise*.
- Project Code is the *ATP-Trinity* with priorities: 1. *Automation Code*, 2. *Test Code*, 3. *Production Code*.
- Regulary check with your team peers whether how the team delivers can be improved. (Scrum Meeting: *Sprint Retrospective*)

Above list is certainly incomplete, and some parts might even be disputable!

Found something that's missing? Disagree with something? Please join the discussion below!
