# Git exercise

> Practice with commits, merges, and pushes

------------------------------------------------------------------------

## Initial code description

`code/01_make_output1.R`
- generates random numbers
- saves numbers as a `.rds` object in `output/` folder

`code/02_render_report.R`
- renders `report.Rmd`

`report.Rmd`
- reads random numbers generated by `code/01_make_output1.R`
- makes three histograms

------------------------------------------------------------------------

## git exercise description

### Make a stable code base

1. Confirm that you can build the report by executing `make`.
	- If the report does not build, correct code until it builds correctly

### Initialize git repository 

1. Use `git init` to initialize a git repository in the `project4` folder
2. Create a `.gitignore` file that ignores:
	- all `.rds` files in the `output` directory
	- all `.html` files in the project directory
3. Appropriately use `git status`, `git add`, and `git commit` to create your first commit. The commit should include: 
	- all contents of the `code` directory
	- `report.Rmd`
	- `README.md`
	- `Makefile`
	- `.gitignore`
	- the `output` directory but none of its contents (other than `.gitkeep` file)

At this point the `main` branch should have 1 commit.

### Create a GitHub repository

1. Log in to GitHub and create an empty GitHub repository.
	- choose any name you like
2. Use `git remote add origin git@github.com:<your_github_name>/<your_repo_name>` to add your GitHub repository as a remote of your local repository named `origin`.
	- replace `<your_github_name>` and `<your_repo_name>` with your GitHub user name and GitHub repository name, repsectively
3. Use `git push origin <your_branch_name>` to push your local repository to GitHub.
	- `<your_branch_name>` is probably `main`, but it may be `master` for some of you
4. Refresh your GitHub repository's web page to confirm that the push was successful.

### Create a new branch

Now, we want to add a fourth set of numbers to our report. However, because we have a stable code base already, we want to use branches to work on this code.

1. In your local repo, create and checkout a new branch called `devel` by executing `git checkout -b devel`.
	- confirm that you have switched to the new branch by executing `git branch`
2. Add the following lines to `code/01_make_output.R` (ignore the lines with back ticks):

```r
set.seed(4)
random_numbers4 <- rbinom(100, 1, 0.25)
```

3. Add additional lines of code to save `random_numbers4` object into the `output` folder.
4. Confirm that `random_numbers4` gets created and saved properly (e.g., by running `make random_numbers`).
5. Once you are confident that `code/01_make_output.R` is stable, appropriately use `git add` and `git commit` to make a new commit along the `devel` branch.
	- include a meaningful commit message
6. Add a new section to `report.Rmd` called "Random numbers 4"
	- the contents of the section should be exactly the same as the other sections
	- e.g., you can copy/paste the "Random numbers 3" section and appropriately modify its contents
7. Confirm that you can build the report (e.g., by executing `make report.html`)
8. Once you are confident that `report.html` is building properly, appropriately use `git add` and `git commit` to make a new commit along the `devel` branch.
	- include a meaningful commit message
9. Push the `devel` branch to GitHub.
	- `git push origin devel`

At this point: 
	- the `main` branch should have 1 commit.
	- the `devel` branch should have 3 commits.

### Merge `devel` branch

1. Complete a final check of stability of the `devel` branch.
	- E.g., run `make clean` and `make` to confirm that `report.html` builds properly from scratch
2. Once you are satisfied that the report builds correct, merge `devel` into `main`.
	- `git checkout main` (or replace `main` with `master` if needed)
	- `git merge devel`
3. Once you are satisfied that the merge executed successfully, push the updated `main` branch to GitHub
	- `git push origin main` (or replace `main` with `master` if needed)

At this point: 
	- the `main` branch should have 3 commits.
	- the `devel` branch should have 3 commits.
	- the `main` and `devel` branch should point to the same commit

------------------------------------------------------------------------

## Bonus exercise description

If you finish the above exercise with time remaining, you can complete the following.

### `revert` to first commit

For the purposes of this exercise, we want to go back to the status of the repository after the first commit. __HOWEVER__, we have already pushed our repository to GitHub. Thus, we do not want to use `git reset` to do this, because that would [rewrite the history of our GitHub repository](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History). Instead, we will use `git revert` to create a new commit that takes us back to a previous version of the repository.

1. Execute `git log --oneline` and retrieve the commit number of the first commit in your repostiory.
2. Execute `git revert <commit_number>` to create a new commit that reverts the repository back to its state at the first commit.
	- replace `<commit_number>` by the actual commit number that you identified in `git log`.
3. Push the new commit to Github

### Create a `big-data` branch

1. Create and checkout a new branch called `big-data`.
2. Modify `code/01_make_output.R` to sample `1000` observations from each distribution instead of `100`.
	- I.e., modify lines 6, 14, and 22 replacing `100` by `1000`
3. Confirm that your changes work (e.g., by running `make` and examining contents of the report).
4. `add` and `commit` your changes on the `big-data` branch
5. Modify the title of `report.Rmd` to read "Three large sets of random numbers"
6. Confirm that your changes work (e.g., by running `make` and examining contents of the report).
7. `add` and `commit` your changes on the `big-data` branch

### Create conflicts on the `main` branch

We are now going to checkout the `main` branch, and make modifications that will result in conflicts when we try to merge in `big-data`.

1. Checkout the `main` branch.
2. Modify `code/01_make_output.R` to sample `10` observations from each distribution instead of `100`.
	- I.e., modify lines 6, 14, and 22 replacing `100` by `10`
3. Confirm that your changes work (e.g., by running `make` and examining contents of the report).
4. `add` and `commit` your changes on the `main` branch
5. Modify the title of `report.Rmd` to read "Three small sets of random numbers"
6. Confirm that your changes work (e.g., by running `make` and examining contents of the report).
7. `add` and `commit` your changes on the `main` branch

### `merge` and resolve conflicts

1. Attempt to merge the `big-data` branch into `main`
2. Resolve conflicts in `code/01_make_output.R` and `report.Rmd`.
	- Open files and find where conflicts occurred (e.g., search for `<<<<` symbol)
	- Delete the extra symbols and the code from the `main` branch (i.e., leave in the contents of the big-data branch)
	- Save files after you have made changes
3. Confirm that your conflict resolution results in a stable report build (e.g., by running `make` and examining contents of the report).
4. Once you are satisfied that the report is correct, appropriately use `git status`, `git add`, and `git commit` to make a merge commit.
5. Delete the `big-data` branch locally.
6. Push the `main` branch to Github.

