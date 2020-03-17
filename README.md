# icecube-git-migration

[![Tests](https://github.com/fiedl/icecube-git-migration/workflows/Tests/badge.svg)](https://github.com/fiedl/icecube-git-migration/actions)

IceTrayCombo ("combo") is a project collection that holds a large part of the IceCube code base.

This repository serves as information hub for my experiments regarding the migration of IceTrayCombo and possibly other parts of the IceCube code base from svn to git.

[Initial experiments](https://github.com/fiedl/icecube-git-migration/issues?q=) resulted in the [following proposal](https://github.com/fiedl/icecube-git-migration/issues/9):

## Goal

[Idea for a minimal combo migration](https://github.com/fiedl/icecube-git-migration/issues/1#issuecomment-599725680): Have combo writable on git, but keep svn working, and also keep other meta projects on svn.

## Choices

- [ ] Use the existing https://github.com/IceCube-SPNO/IceTrayCombo repository
- [ ] Make https://github.com/IceCube-SPNO/IceTrayCombo git-writable
- [ ] Allow non-linear histories
- [ ] Allow pushes to master
- [ ] Encourage, but don't require pull-request workflow, including reviews
- [ ] Enable checks for pull requests:
  - [ ] Reject if the icecube password is committed
  - [ ] Reject if large (gigabyte) files are committed
  - [ ] Make sure the commit authors have valid email addresses
  - [ ] Make sure the tests are green (CI)
- [ ] Run same checks for pushes to master and send warnings (or even reset?)
- [ ] Make people collaborators and allow feature branches in origin
- [ ] Allow issues on github
- [ ] Allow private forks of combo
- [ ] Have `env-shell.sh` print combo's git commit id. Also log the current datetime and the commit id to log file on icetray execution.
- [ ] Welcome to IceCube! Add a `README.md` with getting-started and practical usage information. how to run the tests, links to documentation and further resources. (like [monopole-generator/README.md](https://github.com/IceCube-SPNO/IceTrayCombo/tree/master/monopole-generator))
- [ ] Test the build instructions with github actions
- [ ] Run the tests with github actions (CI) — for all pull requests and branches
- [ ] (almost) never rewrite history, i.e. never change commit hashes that have already been pushed
- [ ] Import histories of the individual projects, as well as histories of icerec and icesim.
- [ ] Allow, but discourage shadow repositories of individual projects
- [ ] Make a series of screencasts to help the adoption of git by showing how-tos and highlighting the advantages — I do have the necessary equipment and volunteer
- [ ] Define the `#git_migration` slack channel as central point of asking for help.

## Cons

- This is no silent change, but every icecuber has to do something: Get a github account, at least change the url of `svn checkout` or even learn how to `git clone`.
- There is a time delay of possibly several seconds after pushing to master, after which a push can be rejected and reset.
- People that love the beauty of linear commit histories may be disappointed at first (but we can still provide a beautified `git log` command)

## Pros

- We can have combo in git!
- We can use all the niceties of github (issues, actions, online code browser, reviews, ...)
- Automatic checks and CI for pull requests and commits
- all histories are preserved
- No redundant repos, i.e. no need to sort and migrate commits by hand, later
- Meta projects in svn still work
- As frustration-free as possible for the users
- People can still push to master or to trunk on the svn bridge; but we do encourage pull requests and code reviews; it has a nice feel to get code reviewed and approved


## Whys

- [x] Use the existing https://github.com/IceCube-SPNO/IceTrayCombo repository. Why?
  - People already know the link
  - People may have already based their work on this repo's commits
- [x] Allow non-linear histories
  - For the technical and workflow reasons, see [Discussion: Changing history](https://github.com/fiedl/icecube-git-migration#discussion-changing-history)
  - What would be the benefits of enforcing a linear history, which will produce work for us?
- [x] Allow pushes to master
  - This way, people don't get frustrated when the workflow they're already used to won't work anymore. We want the migration to be well-received by the IceCubers.
  - We still can run checks.
- [x] Encourage, but don't require pull-request workflow, including reviews
  - It's nice to have regular reviews
  - In projects I've worked on, newcomers liked the pull-request workflow and liked their code being approved by reviewers -- positive and constructive feedback, and a safety net
  - But don't block peoples workflow when committing to master
- [x] Make people collaborators and allow feature branches in origin
  - People already know what a feature branch is.
  - This way, people don't have to get used to the forking workflow and how to maintain an `origin` and an `upstream` remote.
  - I've had good experience with this model and newcomers in other projects.
- [x] Allow issues on github
  - Nice integration with github when mentioning issues
  - We could have it in parallel to trac
- [x] Allow private forks of combo
  - There are people that like the forking mechanism. What would be the benefit of not allowing it?
- [x] Have `env-shell.sh` print combo's git commit id. Also log the current datetime and the commit id to log file on icetray execution.
  - Then we can help people debugging, because we know the commit they were working with.
- [x] Welcome to IceCube! Add a `README.md` with getting-started and practical usage information. how to run the tests, links to documentation and further resources. (like [monopole-generator/README.md](https://github.com/IceCube-SPNO/IceTrayCombo/tree/master/monopole-generator))
  - a single entry point for newcomers
  - low barrier to achieve basic tasks (like running the tests)
  - easy to test in CI
- [x] Test the build instructions with github actions
  - easy to do
  - we know for each commit and each pull request if it builds
  - people can easily look at the build instructions in CI to figure out what they might miss when build fails locally
- [x] Run the tests with github actions (CI) — for all pull requests and branches
  - easy to do
  - for pull request we see in advance if it would break the tests
- [x] (almost) never rewrite history, i.e. never change commit hashes that have already been pushed
  - See [Discussion: Changing history](https://github.com/fiedl/icecube-git-migration#discussion-changing-history)
  - "almost": When removing big files from a recent commit, we need to rewrite this commit
- [x] Import histories of the individual projects, as well as histories of icerec and icesim.
  - preserve information of these commits
  - it's doing no harm
  - the git graph would not show when those projects were merged, and there would be duplicate commits; but this is better than losing the history altogether
- [x] Allow, but discourage shadow repositories of individual projects
  - What benefit would those shadow repos have?
  - They cause work
  - No single point of entry
  - We can do meta projects without individual project repos
- [x] Make a series of screencasts to help the adoption of git by showing how-tos and highlighting the advantages — I do have the necessary equipment and volunteer
  - The collaboration meeting has been cancelled and we still need to give people tutorials
  - People can watch this in the future whenever they join icecube and do not wait for the next bootcamp or collaboration meeting

## Tests, experiments, and checks

- [x] Merge a project (monopole-generator) into combo preserving both histories ➝ https://github.com/fiedl/monopole-generator/issues/3
- [x] Using svn externals for meta projects ➝ https://github.com/fiedl/icecube-git-migration/issues/2
- [x] Using gitman for meta projects ➝ https://github.com/fiedl/icecube-git-migration/issues/2
- [x] Use github actions to run build and tests ➝ https://github.com/fiedl/icecube-combo-install, https://github.com/fiedl/monopole-generator-install
- [ ] Import pre-combo history while preserving the existing combo git history
- [ ] Test how to change the repo remote locally: Automatically by making `meta-projects/combo` an svn external? Using `svn relocate`? Require a fresh `svn checkout` or `git clone`? ➝ https://github.com/fiedl/icecube-git-migration/issues/10

## Wishlists, questions, and discussions

### Nathan Whitehorn ([#5](https://github.com/fiedl/icecube-git-migration/issues/5))

https://drive.google.com/open?id=1RPuHWH_e5Z3oWH2MBRfp7vR9Y3QaHHbv

- [x] Commit authors need to be identifiable (have meaningful emails)
  - We can ensure this in pull requests on github with checks
  - We can run the same tests after pushes to master, and then send warnings, or even reset master, and move the changes into a feature branch
- [x]  Answer questions like “what was the repository state on May 25th?”
  - We can achieve this by logging date and commit id into a log file when executing `env-shell.sh` and icetray.
- [x] Is access control for sub-folders an issue?
  - We cannot restrict write access to sub folder when allowing pushes to `master` (this would require gitlab or github enterprise)
  - What would be the benefit of having project-wise access control? Would it be worth the effort?
  - We can run the same tests after pushes to master, and then send warnings, or even reset master, and move the changes into a feature branch
- [x] How do we prevent big-file commits?
  - We can do checks for pull requests, but not reject pushes to master based on these checks
  - We can run the same tests after pushes to master, and then send warnings, or even reset master, and move the changes into a feature branch
- [x] Git migration of combo drops svn commits before Oct 2018
  - We can still include other histories (which I am in favor for)
  - But from the git graph, it is not obvious then that the code has been merged in 2018
- [x] Migrate trac issues to github tickets? How could we keep the existing links intact?
  - We can open github issues in any case
  - We can move open trac issues to github if we want, and link the new github issues in the trac thread (but we could also keep track of currently open issues in trac, i.e. live with both systems in parallel for now)
- [x] What to do with metaprojects?
  - They can still live in svn and still use svn externals to include the code of projects living in combo.
- [x] What to do with sandboxes?
  - [Erik suggests](https://github.com/fiedl/icecube-git-migration/issues/7) to have them in (possibly private) repos in the users' namespace on github.
- [x] What to do with papers?
  - [Erik suggests](https://github.com/fiedl/icecube-git-migration/issues/7) solutions (slide 7)
- [x] How do we move pre-highlander history to git (important information)?
  - We can import those histories as well
  - But from the git graph it will not be clear when the merges actually have happened

### Erik Blaufuss, Don La Dieu ([#7](https://github.com/fiedl/icecube-git-migration/issues/7))

https://docs.google.com/presentation/d/1UQSj5j_flHoEbX9NpAc1hi55sSJGDKBSZNYWwJqL5HA/edit?usp=sharing

- [x] Sandboxes: Use private github account; also svn sandboxes will not disappear.
- [x] Also plan for analysis code and papers (slide 7)
- [x] Suggests shadow projects to allow release-based imports into meta projects
  - We can have svn meta projects without shadow projects using svn externals and github's svn bridge, or by using a manager like gitman
  - See Discussion "Single combo repo vs. separate project repos" below
- [x] Svn is not going away
  - We can keep our svn repo readonly
  - Better: We can use github's svn bridge
  - But we should *not* allow a separate svn repo for write access to save us from the work of sorting and cross-syncing

### Alex Olivas ([#8](https://github.com/fiedl/icecube-git-migration/issues/8))

- [x] the main sticking point is figuring how to make the git workflow work for us at scale. Or really coming up with some dev model (gitflow variant) that works.
  - encourage pull requests and code reviews
  - but allow commits to master
  - add people as collaborators to the combo repo, such that they don't need to fork, but can push feature branches
- [x] to answer questions like “what was the state of combo at 23 October” (because “it has worked then, why doesn’t it now”) is an issue, but a minor one. this is a natural problem with git that we'll just have to get used to.
  - See discussion below: "The code was running at 23 October"
  - We should log the commit id and the current date time to a log file when executing the `env-shell.sh` and icetray. This way, we could ask people that need support to grep that log file (`grep 2019-10-23 /var/log/icetray.log`) and give us the commit id from the output.
- [x] If we go with vanilla gitflow, where everyone forks and submits a PR, what's the process for accepting PRs that's sustainable? We have 100 projects, almost 40 developers on paper, and up to 100 collaborators committing in a year. Through the upgrade when we're expecting more activity, if we go with the Linux kernel model, then I'm probably the "Linus" and I approve all PRs going into the master branch. Or do we go with something closer to what we have now where people can commit directly to anything, with little oversight.
  - a good start would be a couple of automated requirements like checking for over-sized files, an email address being present, and CI tests
  - reviews, while not being mandatory, could increase the confidence in a pull request, and increase the speed it is going to be merged
  - if deny pushes to master, the frustration might be higher, because something people were used to do does not work anymore. But the migration should feel good, instead!
  - What we can do: Run checks also after pushes to master and have the bot send feedback to people. (Your code has been moved into this feature branch, because ...; check results; this is how you can add more commits, ...) -- People can still work as usual, but get constructive feedback.
  - Later on, we could still decide to deny pushes to master.
- [x] I have no interest in using or promoting github's svn interface. And I'll actively discourage collaborators from clinging to svn.
  - Github's svn interface is there
  - We can use it for svn externals of meta project
  - If people really want to still use svn, they'll probably figure out that the svn bridge is there (https://lmgtfy.com/?q=github+svn)
  - But we should tell people about the advantages of using git
- [x] Should we allow people to push to master? Or should we require pull requests? Or some hybrid?
  - I vote for allowing pushes to master, at least in the beginning, in order to avoid mega frustration
  - it's just one setting in the github repo
- [x] Should combo be one repo, or git submodules, or some kind of package manager? The main concern are separate meta projects (e.g. ehe or pnf) that require individual projects from combo.
  - I vote for one big repo, see discussion "Single combo repo vs. separate project repos" below
  - We can use github's svn bridge to add combo projects to svn meta projects
  - Later, we can use [gitman](https://github.com/jacebrowning/gitman) to manage dependencies
  - The other meta projects should not live in combo, because more maintenance would then be needed in preparation of a release, because we would add to the dependencies. For a combo release, the whole combo code needs to be in a good state.
- [x] Some projects want to stay with svn
  - Meta projects and other projects can stay in svn, as long as these projects are not in combo
  - For combo projects, it would be easiest to use the svn bridge if this is really necessary.
- [x] really we'll want to provide people with the option to compose meta-projects based on combo. This can be done with svn externals. The only downside is that it's not a clone, so people can't push back and not great for development. But we can live with that.
- [x] Before we start making decisions though we want to make sure we're choosing the optimal solution and have gamed out as many scenarios as we can.
  - [ ] Please challenge this proposal by posting scenarios, either in the `#git-migration` slack channel, or in [this repo's issues](https://github.com/fiedl/icecube-git-migration/issues).
- [x] We've already run into coordination issues with PROPOSAL, where we missed a critical (to some) bug fix. A bug was discovered about a week ago in PROPOSAL. Turns out the fix was committed to PROPOSAL in GitHub last October. We've since had two releases of combo where the bug fix wasn't synced to combo in svn. I would worry about doing something similar with all of combo if we can avoid it.
  - We move combo to github
  - Meta projects use github's svn bridge -- no redundancy there
  - People could use github's svn bridge to commit code -- no redundancy there
  - For all pull requests, one can see if they are already merged to master
  - With a one liner, one can check whether a certain commit is included in a release, branch, etc.
- [x] Add CI pipeline
  - We can use github actions to test the build, run the tests, and any other arbitrary code
  - But considering our build time, github actions will cost money if the repo is private
  - This can be circumvented by moving the actions into a public repo, but this will drop some niceties like having tests for pull requests

### Fiedl

- [x] Don't change history!
- [x] Preserve past histories, also from individual projects
- [x] Make combo git writable
- [x] Open issues on github

### Discussion: Single combo repo vs. separate project repos

- A single repo is easier to grasp for newcomers. One single repo, one central README as entry point. Get all necessary code with one single `git clone`.
- The github ui makes it easy to filter by project. Projects get their own rendered READMEs, their own histories, etc.
- Meta projects can pull releases of individual projects from single-project repos as well as from a "big" combo repo alike
- We can merge single-project git repos later into combo, preserving both histories
- During the 2019-10-31 software call, it was argued that the optimal solution would be to manage dependencies using a package-management tool. [Gitman](https://github.com/jacebrowning/gitman) is a simple python tool freeze dependencies based on release tags or commit ids. It supports both git and svn. But this cannot be used to manage inter-project dependencies if each project has its own repo (and its own gitman definition).

### Discussion: "The code was running at 23 October"

- This discussion arose in the 2019-10-31 software call from the scenario: "I checked out the code on 23 October. Then it worked. Now it doesn’t. Why?"
- In git, by design, there is no answer to the question: What was the code on 23 October. Because several people can have their own branches at this time.
- Several people were hoping that enforcing a linear history would enable us to answer the question. But if I create commits locally, push them together next week,  nobody has pushed in the meantime, i.e. fast-forward, then I will have changed the answer to the question: What was the code on 2 Nov.
- I’ve never seen something like `git log --show-push-date`, because I think, this is not well defined in git, because git is distributed, i.e. there could be several truths to the answer. I could push the code to several remotes at different times.
- One solution would be to rewrite history on push such that I have the commit date be the push date. But I really don’t like history rewrites, because they change my commit ids.
- We should log the commit id and the current date time to a log file when executing the `env-shell.sh` and icetray. This way, we could ask people that need support to grep that log file (`grep 2019-10-23 /var/log/icetray.log`) and give us the commit id from the output.

### Discussion: Changing history

- During the 2019-10-31 software call, it has been suggested that history rewrites might provide solutions to some of the open issues.
- Personally, I log my commit id in my lab journals. This way, I can reproduce results later.
- Also, when I work with somebody else on a feature branch, we need to agree on commit ids. When one of us changes commit ids, then our shared history is broken.
- When changing commit ids of commits already pushed, someone else could have fetched those (outdated) commits and based his own work on these. When merging his code later, this would re-introduce the outdated commits (or force to do a rebase, which then requires to change commit ids, again)
- Knowing a commit id of some point in development, gives us the information what commits the developer already during the development. When rebasing, this information is lost.
- I would vote for: Never change commit ids of commits that have already been pushed.
- This requires us to allow (non-linear) graph histories. But those reflect the truth of what actually happened during development.

### Discussion: Running checks for master

- Github allows checks for pull requests (author email, big files, ...)
- Github only allows checks for pull requests. For rejecting pushes to master based on these checks, we would need gitlab or github enterprise.
- We still can run these checks after pushes with github actions.
- We can use this to send warnings via email.
- We can even reset the master and move the commits into a feature branch using github actions. But there is a time delay of several seconds.

## What would every icecuber need to change locally?

To access the combo code, even through the github svn bridge, one would need to authenticate with github:

- Create a github account
- Become a member of the github icecube group
- Copy ssh key to github

Additionally, for committing to combo projects:

- Learn (README.md) how to commit to git combo
- Or: change the url of the svn / re-checkout

## Steps until goal is reached

- [x] Check with experiments and tests that this proposal would actually work
- [x] Import combo history to git
- [ ] Please challenge this proposal by posting scenarios, either in the `#git_migration` slack channel, or in [this repo's issues](https://github.com/fiedl/icecube-git-migration/issues).
- [ ] Approve this proposal
- [ ] Make an early announcement that everyone should get a github account and be added as collaborator to the combo git repo
- [ ] Import simulation and reconstruction history to git
- [ ] Provide a `README.md`: How to clone, build, run tests, commit, find further resources.
- [ ] Test these instructions, including the migration instructions, with a group of people and a copy of IceTrayCombo
- [ ] Enable github issues
- [ ] Enable private forking
- [ ] Publish announcement of the upcoming change, including how-to instructions
- [ ] Make svn read-only and refer to instructions on how to proceed
- [ ] Update the meta-project svn externals to point to read-only svn
- [ ] Add git as svn external for the combo meta project in the svn repo
- [ ] Make the repo git-writable

## Next steps after reaching the goal

- [ ] Add CI build using github actions
- [ ] Add CI tests using github actions
- [ ] Add commit id to `env-shell.sh` output
- [ ] Log commit id and datetime when running `env-shell.sh` and icetray
- [ ] Publish a series of screencasts to help with the migration
- [ ] Add checks (author email, file size, ...) to pull requests
- [ ] Add checks (author email, file size, ...) to pushes to master
- [ ] Import histories of individual projects into git
- [ ] Import tags for project versions to git that are referenced in meta projects
- [ ] Import histories of other individual projects, e.g. residing in other git repos
- [ ] Import pre-highlander history to git
- [ ] Possibly migrate other meta projects to git, possibly using [gitman](https://github.com/jacebrowning/gitman)

## Further resources

- Slides for this proposal, https://github.com/fiedl/icecube-git-migration-talk, 2020-03-18
- [Nathan Whitehorn's talk](https://drive.google.com/open?id=1RPuHWH_e5Z3oWH2MBRfp7vR9Y3QaHHbv), 2019-10-31
- [Erik Blaufuss' and Don La Dieu's talk](https://docs.google.com/presentation/d/1UQSj5j_flHoEbX9NpAc1hi55sSJGDKBSZNYWwJqL5HA/edit?usp=sharing), 2019-10-31
- Build instructions for combo with github actions: https://github.com/fiedl/icecube-combo-install

