![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)

# Git Team Workflows

## Learning Objectives

- Distinguish between git workflow models for collaborating on a project
- Explain and weigh the pros and cons different workflows
- Use branches and pull requests to isolate changes tied to specific features
- Efficiently and correctly resolve merge conflicts

## Framing

Although you've all been using Git and Github for a couple months, you've
largely been doing so individually. In your career, it's more likely that you'll be working as part of a team of
developers.

For the upcoming Project 3, we'll be working in small teams to gain a sense of
what collaborative development is like. Clear, repeatable version control
practices, combined with good communication make collaboration easier and more
efficient. In order to build up to that, we need to make sure we're building on
a solid foundation of Git basics.

## Set Up

1. Fork this repository to your own account and clone the forked repo to your machine.
1. Change directory into your cloned repo. 
1. Run `git checkout solution` to check out the _existing_ solution branch locally.
1. Run `git checkout main` to go back to the `main` branch.

## Review Git: Branching & Merging

### Why Git?

Apart from being free and open source, git is also a superior system to other
methods of version control in many ways. Git is a _"distributed" version control
tool_, meaning that there will be a 'redundant' copy of the repository held by
everyone working on the project.

It also means that there is no centralized approval structure for making changes
to a project; instead, every developer who clones the repository has their own
**complete copy**, which they can edit and change as they wish. This makes it
much easier to use when working in groups, since each member can have an
up-to-date and complete repository.

## Review Git Branching

Branches offer the following benefits:

1. **Experimentation**. By switching to a new branch, we can experiment with
   something. If the experiment fails, we can easily switch back to 
   another branch of our choice. If it succeeds, we can merge those changes
   into our working code.

2. **Isolate Specific Features**. We can have one branch for the production
   version of the app and another branch for the version currently under
   development. If a customer files a bug report, we can make a branch from the
   production version of the app to fix the bug without affecting the features
   that are still under active development and not ready to be deployed.

3. **Parallel work**. By creating a branch, we can work on a feature in
   isolation (without affecting the rest of the codebase). When the feature is
   complete, we can merge it into the main branch of our codebase.

![Git Branch
Diagram](https://wac-cdn.atlassian.com/dam/jcr:389059a7-214c-46a3-bc52-7781b4730301/hero.svg)

> From
> [Atlassian - Git Branching Tutorial](https://www.atlassian.com/git/tutorials/using-branches)


### Branch Commands Reference

| Command                        | Description                                                                                  |
| ------------------------------ | -------------------------------------------------------------------------------------------- |
| `git branch`                   | List branches on your local repo                                                             |
| `git branch -a`                | List branches on on your local repo and all remotes                                          |
| `git branch <branchName>`      | Create a new branch but don't switch to it                                                   |
| `git checkout <branchName>`    | Switch to a branch that already exists                                                       |
| `git checkout -b <branchName>` | Create a new branch and switch to it                                                         |
| `git branch -D <branchName>`   | Hard delete a branch (works if commits haven't be pushed to remote, `-d` is the soft delete) |

> :rotating_light: **NOTE:** When creating a new branch, it will create a copy of whatever branch you're currently on. So make sure you're in the correct branch before making a new one! 

## Merging and Merge Conflicts

As we've learned, we can create branches to create new versions of our project
from within a repository. We can do this to build out a new feature in isolation
and then merge those features into the rest of our codebase when we're ready.

### Merge via a Pull Request 

You can merge both at the command line and inside of Github using a Pull Request (or PR for short).  Using a PR is considered a best practice when working on teams, because it provide complete visibility to the team and creates a record of the operation in the project's history.  

Steps to merge a branch via a PR:

1. Add/commit/push your branch up to the repo 
1. Open a pull request
1. You should see a bar like this: 
![](https://i.imgur.com/7YgEVlH.png) 
   * Make sure the **base** is the branch you want to merge _into_ 
   * Make sure the **compare** is the branch with changes you want to _merge_ 
1. If Github tells you it's able to merge, create the pull request.  If it's unable to merge, make sure to go back to your local copy and pull down any changes and `rebase` your local branch first.  
1. When working in teams, typically someone else on the team should double check everything and then merge your pull request 
1. Whenever a pull request has been merged, everyone on the team must pull the changes down to their local repository!


### Rebasing After Merge

After you merge a pull request into the main branch of your project, all developers will want to update their own local repositories using `git pull orgin <branchname>`. 

Most times we work from a common baseline across the team that is based off of the main production branch.  If your team is using `main` for example, all team members will always want to start their own work in a new branch that uses the latest version of code in `main` as the starting point!

What happens when `main` changes after we've started a new branch for our own work but before we've finished and created our own PR?  Well, things will be out of sync for us and we won't be able to cleanly merge our work into `main`.  Does that mean we need to start all over again from scratch? 

Thankfully, there's no need to start anew.  We just need to **_rebase_** our branch off of the new version of `main`.  When we `rebase`, all of our new work is removed from the old version of the `main` branch and then reapplied to the new `main` branch code base.  You can see this in the two diagrams below.  In the first diagram, you can see that the point at which the feature branch was created from the main branch is now two commits behind the current main branch!  Rebase lets us remove all of the commits in our feature branch and reapply them to a new starting baseline.  We'll still be working on our feature branch, but it will have a new starting point and therefore a brand new history.

![Oh no main has moved beyond where we branched our work from](https://wac-cdn.atlassian.com/dam/jcr:01b0b04e-64f3-4659-af21-c4d86bc7cb0b/01.svg?cdnVersion=1520)

![Good thing we have rebase to move all of our new feature commits to the new end of main](https://wac-cdn.atlassian.com/dam/jcr:5b153a22-38be-40d0-aec8-5f2fffc771e5/03.svg?cdnVersion=1520)

## Merge Conflicts 

When we go to merge our work, our coworkers or teammates have likely continued
working off of master and may have already merged their work. So when we go to
merge our work we may find that we've changed a file or files that one of our
teammates also changed. When this happens, we get merge conflicts.

Merge conflicts look like this:

```
<<<<<<< HEAD
var x = 1,
    y = 2;
=======
var x;
>>>>>>> other_branch
```

The first section is the version that exists on the current branch; the second
section is the version that exists on the branch you're trying to merge in.
Figure out which version of the code makes the most sense moving forward, delete
the version that doesn't and all the extra stuff that Git adds (`<<<<<<<`,
`=======`, etc.) and run `git commit` to finalize the merge.

An easy way to do this inside of VS Code is to use the links provided  when you open the conflicting file in the editor:

![image](https://media.git.generalassemb.ly/user/17300/files/25801b80-8e07-11eb-91d2-50964d42a917)

For example, if we decided we only needed `var x`, we could simply click on the link for _Accept Incoming Change_.

Now, we have only the code we need and can commit the changes we made to resolve
the merge conflict.

### You do: Merging and Merge Conflicts 

Merge conflicts happen, they sound scary but aren't the end of the world. In
fact they have never been easier to manage. Let's take a look at one together.

1. Make your changes locally
   - Create a file called `conflict.md` and add something to it.
   - Now add and commit the file.
1. Rebase on to another branch
   - We will attempt to rebase master off of the solution branch with
     `git rebase solution` solution
   - Uh-oh, looks like there was already a file with that name on the solution
     branch and git doesn't know which file to use. Let's take a look at the
     file in VS Code.
1. Review the merge conflict
   - Notice the file shows you what text is different, which version of the
     file the text comes from, and also provides you with an easy interface to
     choose which text you want.
   - Git places merge markers in the file to define where one version of a file
     starts and ends, and where the other conflicting version starts and ends.
     Luckily, VS Code abstracts away the complexity of dealing with merge markers.
     We just need to choose using a nice GUI button which version to use.
1. Complete the merge
   - Let's pick the text we want in VS Code.
   - Now head back to the terminal.
   - Notice the terminal is giving us some tips on what we should do next:
   - We need to add the change (`git add conflict.md`) and then following the
     instructions, type `git rebase --continue`
1. Ensure you are where you want to be
   - Type `git status`. Also verify the file you merged looks how you want it.
   - Does everything look good? If you are still in the state of rebasing, your
     terminal will tell you that you are in the middle of a rebase. You have
     the choice of a few different options on how to proceed:
     `git rebase --continue | --skip | --abort | --quit | --edit-todo`
     (view more info on these using `git rebase --help`)

## Git Workflows

The process described above is a simplified version of a **Feature Branch Workflow** that is commonly used in the industry. This workflow process will form the basis of the **Project Workflow** we'll use for project 3. 

Some workflows you should know are:

### Centralized Workflow

The remote repo has only a single
branch. All collaborators have separate clones of the repository. They
can each work independently on separate things. However, before anyone runs a
`git push`, they need to run `git pull` to make sure that their **local** copy
of the main branch isn't out of sync with the **remote** main branch and they then merge their changes locally in the main branch.

-![Centralized Workflow
Diagram](https://wac-cdn.atlassian.com/dam/jcr:0869c664-5bc1-4bf2-bef0-12f3814b3187/01.svg)

> From
> [Atlassian - Centralized Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#centralized-workflow)

**(+)** Very simple

**(-)** Collaboration is clunky and error-prone

_Use this model when working alone on a project or with only one other
collaborator and the project is small/insignificant._

### Feature Branch Workflow

The remote repo has a main branch and a branch
for each feature that is under active development. All collaborators have a
clone of the repository with the main branch and any feature branches that
they are currently working on.  In the Feature Branch Workflow, pull requests are submitted when a new feature is ready to be merged into the main branch.

![Feature Branch
Workflow](https://wac-cdn.atlassian.com/dam/jcr:80d671b1-8a4b-4378-914c-e25fe3d2dcce/07.svg?cdnVersion=dj)

> From
> [Atlassian - Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)

**(+)** Better isolation than the Centralized model, but sharing is still easy.
Very flexible.

**(-)** Sometimes it's too flexible - it doesn't distinguish in any meaningful
way between different branches, and that lack of structure can be problematic
for larger projects.

_Use this model when working on a small to medium sized project with others that
doesn't require strict collaboration_

### GitFlow

The Gitflow workflow builds on the Feature Branch model by assigning very
specific roles to different branches and providing strict guidelines for how
these branches interact. Under Gitflow, a repository will have a `master`
branch, a branch for each feature under development, branches for each
environment (i.e. staging and production) and each release.

![Feature Branch
Workflow](<https://wac-cdn.atlassian.com/dam/jcr:61ccc620-5249-4338-be66-94d563f2843c/05%20(2).svg>)

> From
> [Atlassian - Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

**(+)** Features are most isolated from each other and sharing work is still
easy. It's easy to work on multiple, separate features simultaneously and merge
them in a single release. It's designed to fit in nicely with Agile.

**(-)** The structure is very rigid - that can be complex and difficult to
maintain and require more on-boarding for new team members.

_Use this model when working on medium to large sized project with others,
especially if working on a team of 5+ developers._

### Fork and PR Workflow 

The Fork and PR approach is the model we're all most familiar with: it's how we
submit our homework. Under this model, everyone maintains
their own remote repository (their fork). Changes are submitted via pull
request.

**(+)** One team member integrates all changes, so there's consistency. It ensures
the integrity of the original remote repository (the main branch of the primary repository).

**(-)** Could get overwhelming for large projects: one person will spend most of
their time reviewing and merging pull requests.

_Use this model when working on an open source project or when working with or
as an outside contractor or freelancer._

![Fork and PR Workflow](https://wac-cdn.atlassian.com/dam/jcr:642c56e3-ddc6-43ff-ab86-c5cd845afd05/03.svg?cdnVersion=805)

> From
> [Atlassian - Forking Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)


## Project Workflow

For project 3 the process we'll be following is a version of the feature branch workflow.

### Initial Set Up

1. Create a GitHub Organization for your repos, and add collaborators as members
   of the organization. Their role must be set to **Owner**. To confirm that
   they have joined as owners, go to the "People" tab on your organization. If
   you need to change someone's role, you can do so by clicking the gear icon.
   Any repos that you create as part of the project will go inside this
   organization. Make sure you create the organization on GitHub and not GitHub
   Enterprise.

1. Create an empty starting repo within the new GitHub organization. (If you were
   working on an existing repo such as for homework, you'd fork it).

1. Using `git remote add origin <your-ssh-git-url>` attach your two GitHub
   repos to the corresponding ones on your local computer (one for React containing your
   front end app and another for Express containing your API server).

1. Create a `dev` branch in each repo and push them up to the remotes
   on GitHub.

1. Have each member of the team clone, **NOT FORK**, both repos, so that they
   have their own copies of each.

### Regular Workflow

On a day-to-day basis, your team will follow a feature branching workflow. Each
time you want to create a new feature for your app, you'll go through the
following stages.

#### Creating a New Feature Branch

1. Check out your `dev` branch (`git checkout dev`)

1. Ensure that `dev` is up to date with the `dev` branch on
   GitHub by running `git pull origin dev`.

1. Create and check out a new feature branch using `git checkout -b my-feature-branch`

#### Integrating a Feature

1. After you're done working on the branch, check in with your team and let them
   know that you're ready to integrate your feature.

1. Because `dev` may have been updated in the time since the feature
   branch was created, it's important to make sure that the new feature doesn't
   conflict with anything. Run `git checkout dev` and `git pull origin dev` to make sure that your `dev` branch incorporates any
   updates that were made on the repo on GitHub. Then, run `git checkout my-feature-branch` and `git rebase dev` to rebase your new feature on
   top of the (updated) `dev` branch.

1. If any conflicts were introduced in the previous step,
   work through the code **with your team** and resolve each one;
   when you finish, make a commit.

1. Now that your branch has been rebased, and you're ready to integrate it,
   push your branch up to GitHub with `git push origin my-feature-branch`
   and then create a pull request (within your GitHub repo)
   from your feature branch to the `dev` branch.

1. As a team, review the pull request, confirm whether or not
   it can be merged in automatically, and decide whether or not
   to approve the pull request.

   If there are merge conflicts preventing an automatic merge,
   a member of your team will need to resolve those conflicts manually
   on their machine, and then push the newly updated `dev` branch
   back up to GitHub.

Once `dev` has been updated, other members of the team
will need to rebase their own feature branches on it (as described in Step 2)
before they push up those feature branches up to GitHub.

What if you want to know about remote branches, such as a feature branch that
someone else is working on? You might want to pull down a feature branch to
test it locally, for example.

Each team member can learn about what exists on the remote. This can be done
with `git fetch origin`. Then, your local git knows about remote branches that
may not have existed when you first cloned the repo.
`git checkout <some-new-branch>` will now be set up as a new branch that tracks
the remote feature branch. Without the fetch, the local git will not know
anything about origin's branches.

#### Deploying a Working App

Work through the following steps as a team.

1. Have one member of the team check out `dev`
   and pull down the latest version from GitHub.

1. For this version, check and make sure that the application is working.
   If you have tests, run them.

1. When you're satisfied that the app is ready to deploy,
   check out the `master` branch and run `git merge dev`.

1. Push the finished version of your code up to GitHub
   (`git push origin master`).

1. Deploy!

## Resources

### Cheat Sheets

- [Github Official Cheat Sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet/)

- [Atlassian Cheat Sheet](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)

- [Dash App](https://kapeli.com/dash) – "Dash gives your Mac instant offline
  access to 200+ API documentation sets."

### Further Reading

- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
- [How to use Git efficiently](https://medium.freecodecamp.org/how-to-use-git-efficiently-54320a236369)
- [GitHub docs on resolving a merge conflict](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)
- [Git Workflows Overview](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [Git Teams](http://gitforteams.com/)
- [Git Alias](https://githowto.com/aliases)

## Bonus

<details><summary><strong>Advance Git Tips & Tricks</strong></summary><p>
   
   ## Tips & Tricks

**Using `git fetch` and `git diff`**

What if you are a little out of sync with your teammates and are worried that a
`git pull` will result in hours of working through merge conflicts?

Use `git fetch` and `git diff` to see the changes instead!

```bash
git fetch <remote> <branch>
git diff <remote>/<branch>
```

One of the common undos takes place when you commit too early and possibly
forget to add some files, or you mess up your commit message. If you want to try
that commit again, you can run `git commit --amend` with the `--amend` option...

```sh
git commit --amend
```

### Deleting Branches: Locally VS Remote

Locally...

`git branch -d <branch>` Deletes local branch

`git branch -D <branch>` Forces delete for un-merged branches

Remote...

`git push origin :<branch>` Deletes Remote Branch

### Git Blame

`git blame <file_name>`

Git blame allows you to see who has made changes to a file, or when the file was
last changed by someone. This can be used to find out what feature(s) were
added.

To find out who changed a file, you can run git blame against a single file, and
you get a breakdown of the file, line-by-line, with the change that last
affected that line.

Additionally, this feature is available to view on Github!

To use git blame on GitHub:

- Navigate to a repository, and click on a file that you're interested in.
- Click on the Blame button in the upper-right tab list.
- Browse the list of changes in a file.

</details>

   
   </p>
   </details>
