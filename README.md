# The Innovation flow

This is an abstract idea of the Git workflow we're following in the Innovation
team, it covers from the structure of our commit messages to the branching model
we're using.

## Commit messages

In order to give more context to our pull requests and to have a beautiful and
useful `git tree` we should add useful `commit` messages to our `pull requests`.

The commit template we're using is this:

```
Some awesome title

# This question tells reviewers of your pull request what to expect in the
# commit, allowing them to more easily identify and point out unrelated changes.
1. Why is this change necessary?

# Describe, at a high level, what was done to affect change.
2. How does it address the issue?

# This is the most important question to answer, as it can point out problems
# where you are making too many changes in one commit or branch.
3. What side effects does this change have?
```

The commit title must be 50 characters or less, the body must be 80 characters
or less and the title must start with the type of auxiliary branch used for the
integration, for example:

    feature: Implement GPG key ring to sing requests

Consider making including a link to the issue/story/card in the commit message
as the first paragraph after the summary a standard for your project. Full urls
are more useful than issue numbers, as they are more permanent and avoid
confusion over which issue tracker it references.

Starting with this structure forces you to answer why the change is necessary
before outlining the changes that you have made towards this goal.

![Commit - Issues](img/commit-issue.png)

###### Automate it!

Save the commit template inside a file named `~/.gitmessage`.
Then add the following lines to your `~/.gitconfig`:

```
[commit]
  template = ~/.gitmessage
```

## Branches

Our branching model is based on the efficient branching model
[git-flow](https://danielkummer.github.io/git-flow-cheatsheet/) but has some
minor changes to simplify things.

### Master and Develop branches

Our worflow has two main branches, `develop` and `master`.

The `master` branch stores the official release history.
The `develop` branch is used to integrate new features.

The constraints for using these branches are the following.:

1. Only Tony can merge into to `master`.
2. No one can push directly to `master` unless it's a `hotfix`.

Once `develop` has enough features and is ready to be integrated into
`master` a release should be created by using a `hard tag` and it must be named
after a [star](https://www.naic.edu/~gibson/starnames/starnames.html).

### Feature branches

Each new feautre must have its own branch which can be pushed to the central
repository for collaboration or backup without modifying `develop` or
`master`.

All feature branches are created from `develop` and when a `feature` is
finished it must be `rebased + merged` into `development. Features should
**never** interact with master directly.

The steps for creating a new `feature` branch are the following:

1. `git checkout develop`
2. `git pull origin develop`
3. `git checkout -b "feature/my-feature"`

Once a feature is completed and ready to be integrated into `develo` its commits
must be `rebased` (in case there's more than one) and `amended` so the branch
has only 1 `commit` and its message follows the template provided above.

Then, we follow this process:

1. `git fetch`
2. `git rebase origin/develop`
3. `git push origin feature/my-feature`
