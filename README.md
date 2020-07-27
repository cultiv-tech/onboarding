![cultiv.llc](https://i.imgur.com/XIN25KK.png)

Cultiv Onboarding Guide
==

Hey there! You're probably reading this because you've been invited to take one of our code challenges or are working on a **Cultiv** engineering project. Welcome!

We're looking for talented developers to help us shape the future of our organization.

Code quality is very important to us, and we hope it is to you too. In this guide we'll share our engineering process that we use to ensure that we can build quickly while keeping our projects stable and maintainable. We'll cover topics like code style, commit guidelines, project workflow, and team communication.

## Code Style & Code Principles
- [Javascript Developers](javascript.md)
- [PHP Developers](php.md)

## Git & Gitlab

Version control is a project's best source of documentation when done correctly. When trying to understand code it's extremely useful to use `git blame` to find both the PR and the issue associated with that change.

PRs should be small and focused. Each commit should solve a single problem and be covered by a test that exemplifies that particular feature or fix.

All PRs must be reviewed by a teammate before they are eligible to be merged into  the `master` branch. Large PRs are difficult to review. Be sure to break large PRs into smaller ones so that they can be reviewed quickly and deployed to production.

* Never commit passwords, access tokens, or other credentials into version control. If you think you absolutely have to, ask first. If you do this by accident, tell someone immediately.
* Each commit should be as small and as simple as possible.
* Write the commit message in the [imperative mood](https://chris.beams.io/posts/git-commit/#imperative).
* The project must be operational and have all tests passing after every commit.
* Use [Conventional Commits](https://www.conventionalcommits.org)
  * See the "Commit Types" section below
  * Valid types are `chore:`, `docs:`, `style:`, `refactor:`, `perf:`, and `test:`
* Do not mix feature changes (added functionality) with fixes (restored functionality), refactors (no change in functionality), or style changes (only whitespace or other cosmetic changes).
* Before a PR is ready for review, make sure that it is a single commit. If the combined commit is too large or disparate, consider multiple PRs.
* The exception to the above single commit rule is when a PR introduces new packages. Create one extra commit in the same PR for each new package your PR needs.
* Do not modify a project's `.gitignore` to add files related to your editor or environment. Use your own [global .gitignore](https://stackoverflow.com/questions/7335420/global-git-ignore/22885996#22885996) for that instead.
* Be sure that your PRs have descriptive titles that explain what has been changed. Typically the commit message is sufficient. "Fixes #66" is not.
* When submitting a PR with UI or visual changes, please add before and after screenshots to the PR. This makes it easy for the reviewer to quickly see what has been done.

## Parent branches

With larger features, it can be tempting to create a parent branch, allowing for us to work on a feature in a "rough draft" state over a period of time. The challenge there is that keeping the parent branch up-to-date with master can be tedious, resulting in work not being merged into the parent branch. To get around this, we have a number options available, depending on the work being done.

To get around the need for a parent branch, put the new work behind a feature flag. This allows for making small incremental changes to a feature, hiding the fact that it is in a "rough draft" state from the users. This can also work if we are updating an existing feature that requires several days of work: create a new component by copying the old component. Once the new component is ready, remove the flag and delete the old component. If you are working on an entirely new view, you may not need the feature flag. The user will only see the view when we link to it from existing UI. Until that happens, we can merge work into master without having to worry about making sure the feature is "complete".

## Workflow

We use Gitlab Issues to manage our workflow. Each task is represented by an issue. Be sure to connect any PR you are working on to the appropriate card.

As tasks move from **Backlog** to **In Progress** to **Needs Review** to **Needs QA** to **Ready to Deploy** and finally to **Closed**.

* Backlog: This is where you will choose from issues to work on. Once you have selected one, move it to **In Progress**.
* In Progress: While you are working on an issue, it should stay in this column. Once you are finished and satisfied with your work, move it to **Needs Review**.
* Needs Review: **Each day you should be looking at this column for PRs to review.** This column will list all issues that are complete and are waiting on review before they can be ready for deploy. When you review an item, make sure that it follows all of our coding principles and will not cause problems when we deploy it to production. Once a PR has been reviewed, either move it back to **In Progress** or forward to **Needs QA** or **Ready to Deploy**. Do not allow items to sit in **Needs Review**.
* Needs QA: If the change affects the front-end and can be tested with a deploy preview it should be tested by QA. After successful testing it should be moved to **Ready to Deploy**, but if it is unsatisfactory, it should be moved back to **In Progress**.
* Ready To Deploy: After an issue is finished and has been reviewed and approved by a teammate, it will be moved to this column. After it has been deployed, it will be moved to the **Closed** column.

## Slack & Pull Requests

We have a dedicated Slack channel for posting PRs: #engineering-prs. Every PR should be posted in this channel, allowing for others to know that you have work that needs review. Communicating updates about the PR will happen in a thread attached to the original Slack message for the PR.

The process for posting a PR in Slack is:

- Post your PR in #engineering-prs
- If you are the developer reviewing the PR, start a thread, saying: `@<developer_who_made_PR> reviewing`
- If the PR needs to be pushed back, the developer reviewing responds in the thread with: `pushing back to In Progress @<developer_who_made_PR>`.
- If all comments have been addressed, the developer responds in the thread with: `@<developer_who_made_the_comment> all comments have been addressed. Pushing back to Needs Review`
- If you have approved the PR, respond in the thread with: `@<developer_who_made_PR> approved`

**Please note that all discussion about the PR should stay in GitHub.** Threads are not intended a place to discuss the code itself. Slack is only used to provide more immediate feedback.

## Commit Types

Commit types (e.g. `feat`, `fix`, `refactor`, `style`) are important because they are a signal to the reviewer for what they should be looking for and how much scrutiny the review needs. Reference from [Angular](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines):

- feat: A new feature
- fix: A bug fix
- refactor: A code change that neither fixes a bug nor adds a feature
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- test: Adding missing tests or correcting existing tests
- docs: Documentation only changes
- perf: A code change that improves performance
- build: Changes that affect the build system or external dependencies (example scopes: gulp, composer, broccoli, npm)
- ci: Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)

### Descending Levels of Scrutiny and Test Requirements

**Feature** commits require the most scrutiny. They add or change functionality, require additional tests, and have the greatest chance of introducing a bug.

**Fix** commits are smaller and have a more bounded scope than feature commits. Features usually introduce multiple new behaviors, but fix commits will only modify a single behavior. Fix commits need to provide additional test coverage because the existing tests were insufficient to catch the error. Feature commits should have multiple tests for these behaviors, but fix commits typically introduce a single test to prove that the error has been fixed (test should fail without the fix inactive and pass with it active). These commits can introduce additional bugs, but it’s less common than feature commits.

**Refactors** need less scrutiny than feature or fix commits because little about the app changes. All consumers of the refactored part of the app should be 100% unaware of the change. If module A is refactored, and module B depends on A, module B does not need to be tested because A hasn’t changed anything from their perspective. Similarly, anything that depends on B doesn’t need to be tested either, no change should bubble up to cause issues. Since nothing is changing, any existing tests/QA should already be sufficient to catch any issues created by the refactor. If there are no tests for the code being refactored, there should be new tests included in the PR along with the refactor. If there are existing tests, the refactor should not generate additional tests.

**Style** commits need the least amount of scrutiny. They tend to be very repetive (converting from snake_case to camelCase, standardizing indentation, or changing newlines). These changes will have no functional impact on the code and any existing tests should be sufficient.

## Slack and Communication

All communication about projects should be in #engineering. Do not use direct messages unless you are discussing something private. It is important that all conversations and questions (no matter how small they seem) should be in #engineering. We work as a team and that requires sharing, being transparent, and allowing the whole team to have a chance to answer your questions and to learn from the dialogue.
