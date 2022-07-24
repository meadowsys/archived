# Repo Archive

Archived repositories from [Atom](https://github.com/atom) and [Pulsar](https://github.com/pulsar-edit)

## How to monorepo and keep history

This section describes how to take a previously seperate repository and add it into a monorepo, while keeping the commit history intact. It is what is being done in this repo, but can also be applied to any monorepo in general.

- Have a clean working directory (gitignored files should be alright, but if you're not sure it is best to move them out temporarily).
- Clone the repo that you want to merge into its desired location. For example, if you wanted to include a repo inside `packages/my-package`, you should clone it directly into there.
- Go into that directory and remove the `.git` directory.
- Add and commit all of these changes, using a command like `git add -A && git commit -m "prepare my-package for monorepoing"`.
- Add the original repo as a remote of this repo, using a command like `git remote add my-package-remote https://github.com/me/my-package.git`
- run `git fetch --all --no-tags`. `--no-tags` makes it not fetch tags, which are not needed.
- Merge in the repo using a command like `git merge --allow-unrelated-histories --no-commit my-package-remote/master`. Substitute `my-package-remote` and `master` for the remote name and branch to be merged.
  - `--allow-unrelated-histories` allows unrelated histories to be merged. Your old repository more than likely is not forked off of your monorepo, and has started off with a different initial commit.
  - `--no-commit` tells git to not commit immediately, and give us the option to inspect the results before committing. Git more than likely will give us a wrong result by spewing everything into the root of your monorepo, so we want to have the opportunity to inspect it and clean up.
- Notice how everything has indeed been put in the root of your repo. Go in and manually (without using git) revert all of its changes, staging the changes you just made using `git add -A`, and checking using git status (I need a better way to do this). You want these all gone because you do have the files themselves committed already, you just need the merge commit to bring in the history. The merge commit should be completely empty.
- When `git status` does not show any modified files, finalise the merge commit using something like `git commit -m "merge my-package"`.
- If you wish to, you can remove the remote using `git remote rm my-package-remote`. It probabl
- Done!
