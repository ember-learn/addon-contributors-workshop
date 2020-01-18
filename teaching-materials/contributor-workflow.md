# Contributor workflow

This document will walk you through the steps that many contributros take when working on an open source addon.

## Fork, download, install

From the addon's page on GitHub, click the "Fork" button in the top corner.
This will create your own fork (your own copy) of the addon. This is where you will be doing your work. From the fork page, now you can copy the link to clone the repo, just like you normally would:

```sh
git clone YOUR_REPO_URL
cd addon-contributors-workshop
yarn install
```

## Choose an issue to work on, or open one

If the work you are doing is an issue that was not opened by the maintainer, stick to small changes/fixes.
If you want to work on something more significant, always open an issue first or comment to ask the maintainer if they would like help. Otherwise, your PR may be rejected.

## Branch

It's easiest to manage the fork if you do your work on a branch, and not on the main branch:

```sh
git checkout -b my-branch-name
```

## Do your work and commit

Review the project `README.md` and `CONTRIBUTING.md` before starting to see if there
are any special instructions to follow.

It is helpful to note the issue number in your PR.

```s
git add changed-file.js
git commit -m "Fix that one bug, closes #123"
```

## Run the tests, and add to them!

Usually, this is `ember test`, but check the addon's `README.md` and `CONTRIBUTING.md` for details.

If you added a new feature, write a test!
If you fixed a bug, write a test that fails before your work and passes after your work.

## See if you need to update the docs

Is there anything new the users of the addon need to know? If so, add to or change the docs.
Documentation is often in the `README.md`, `API.md`, or the `dummy` app files.

## Push your work up to your fork

```sh
git push origin my-branch-name
```

## Open a Pull Request

Go to the addon's page on GitHub. When you click the button to open a Pull Request, you will choose your fork from a dropdown menu.

In the pull request, it is helpful to link to the issue you worked on, describe the expected behavior of your code, and suggest the steps someone should do to test the work by hand. If your work is visual, screenshots are helpful!

## Make sure the tests pass

If they don't pass, keep working until they do. Many maintainers don't review PRs with failing tests unless the contributor asks for help.

## Handling merge conflicts

What if your work gets out of date, and you see "merge conflicts" in the Pull Request? You have two options. Sometimes you can resolve them right in GitHub if they are small. Look for a button next to the Merge Conflicts message in your Pull Request.

If that option isn't available on your Pull Request, you'll have to do some work with git. Assuming you did your work on a branch that is _not_ the same as the main branch for the addon (i.e. `master`):

```sh
git remote add upstream <git-url-for-the-main-addon-repo>
git pull upstream master # update your local `master` branch with the main addon code's `master`
git checkout <your-own-branch-name>
git merge master
# do the work to resolve the merge conflicts, and stage the files with git add
git commit
git push <origin your-own-branch-name>
```

Your open pull request will be automatically updated with your new work.

### Oh no, I did my work on the main branch! And I have merge conflicts!

If you did your work on the main branch for the addon (i.e. `master`), it's ok. You can do the steps above if you go through a couple other steps first.
If you aren't sure what is going on in the steps below, always ask another developer for help so that you don't lose your work!

```sh
git checkout master
git checkout -b <your-own-branch-name> # now your commits from master are copied onto this branch
git push origin <your-own-branch-name>
# check to make sure your work is all there on your fork, under the <your-own-branch-name> branch. The next step is destructive and if your work isn't safe on that branch on GitHub, it could be lost!
git branch -D master # permanently delete the master branch locally
git remote add upstream <git-url-for-the-main-addon-repo>
git fetch upstream # re-download the master branch for the addon
git checkout master
git pull upstream master
git checkout <your-own-branch-name>
git merge master
# do the work to resolve the merge conflicts, and stage the files with git add
git commit
git push <origin your-own-branch-name>
```
