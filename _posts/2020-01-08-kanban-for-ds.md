---
title: "Applying Kanban for Data Science"
categories: Data Science
date: 2020-01-08
tags:
  - data science
  - kanban
  - agile
---

I've been experimenting with how to manage the Data Science part of a larger project. One of the biggest issues is that Agile sprints are a poor fit to data science tasks, mainly because:

- data science tasks can be variable
- there is a task dependency between tasks, but the order can be rearranged
- the priorities of the data science pipeline changes a lot and from different sources
- the value delivered by a data team can change depending on the result of the task itself, the stakeholders requirements and other external influences

So this post is my attempt on understanding and adapting the kanban approach to manage data science teams.

## What is kanban?

Kanban is considered as one of the Agile project management tools. Created in the 1940s at Toyota, the idea is to visualize a task board. The goal is to deliver the product to the client in the shortest time possible.

## Kanban components

The first component is the task. The tasks should have similar sizes consistently.

### Work cycle

Other important component is the *work cycle*. It means the time required for one task to pass all stages of the kanban, from *to do* to *done*. 

### User stories

User stories in kanban are organized in the product backlog by the product owner. Only the product owner, being the representative of the customer, can prioritize the backlog, so the PO should work constantly with the team. Each user story should be a vertical slice of a business value, and it should have a beginning, middle and end, and each one should deliver value to the customer.

### Backlog

The backlog is the repository of the user stories. The team is only focused on work that is actively in progress. Once a task is done, another one is moved from the top of the backlog.

Sometimes another level after the backlog, the TODO section, with tasks that are moved from the backlog, prioritized by the PO and ready to be executed by the team. 

### Work in progress (WIP)

There is a limit in the number of tasks that can be under development (WIP). This is set to make the team focus on getting a task done instead of starting a new task. 

## Differences between Scrum and kanban

There are main differences between Scrum and kanban:

- Scrum has fixed sprints, while kanban the flow is continuous;
- Versioning is changed at the end of each sprint, while in kanban the delivery is continuous;
- The roles in Scrum are fixed: Scrum master, PO, development team. Kanban has a much more fluid structure, without very rigid and predefined roles;
- Scrum is based in measuring velocity, while kanban focus in cycle time;
- Changes in the middle of a sprint in Scrum must be avoided, while these changes can occur at any time in kanban.


## References
- https://en.wikipedia.org/wiki/Kanban_(development)
- https://pt.slideshare.net/bradswanson/kanban-statik
- https://br.atlassian.com/agile/kanban
- https://www.atlassian.com/agile/kanban/wip-limits
- https://agileramblings.com/2013/03/10/the-difference-between-the-kanban-method-and-scrum/
