---
title: "Using GitHub Actions is like programming in the '60s"
date: 2020-11-26T00:15:00+01:00
draft: false
categories: ["blog"]
tags: ["github"]
---

If you were lucky enough to have access to a computer in the 1960s you would have had to go through a convoluted process in order to run programs on it.\
At first you would have had to punch your program onto cards, which can store 80 characters each (yes, the 80 char line width of ttys and programming came from punch cards). Then, while making sure to keep the cards in order, you would have had to go to the computer room and give the card stack to the computer operator, who would have put them in an execution queue and eventually would have loaded them into the machine to run your program.\
As we all know, programs have bugs, and so they did in the '60s. If a problem occurred during execution the computer would simply halt and print an error. The operator would then hand the printout back to you and the cycle will repeat until you got the wanted result.

The workflow I described above strongly resembles the one needed in order to debug GitHub Actions, albeit with a lot less downtime and handwork involved.

The steps are the following:

1. Write the code
2. Commit and push the code to GitHub
3. Wait for the action to run, an hopefully succeed
4. If errors, goto 1

The issue with this workflow is the need to push the code upstream and wait for GH Actions to run, which can take tens of minutes at times, other than messing up your git history with tons of `fix action` commits. The most obvious solution is to run actions locally, without committing changes.\
While it might seem simple, I only found [this](https://github.com/nektos/act) project that attempts to fix the problem and it did not work in my case.

A better fix is to avoid proprietary features (like community actions) and use tools that you can already run locally, like containers or, if you're careful enough, shell scripts.\
The added benefit of this approach is avoidance of ecosystem lock-in.\
Still, this is only a workaround since you would have to run the commands manually.

The frustrating thing to me is that the built-in CI/CD offering on the biggest software hosting platform has this glaring limitation while [other](https://about.gitlab.com/stages-devops-lifecycle/continuous-integration/), [smaller](https://sourcehut.org/) platforms don't since they opted to develop open source workflows.
