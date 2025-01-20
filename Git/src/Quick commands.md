

### Instantiate a new repository
-	Creates an empty git repository [git init](https://git-scm.com/docs/git-init)
-	Don't do it inside an already existing git repo.
	`git init`


### Cloning the repository
-	Clone a repository into a new directory [git-clone](https://git-scm.com/docs/git-clone)
	`git clone <URL of git repository>`

### Current state of working tree
- Show the working tree status [git-status](https://git-scm.com/docs/git-status)
	`git status`

### Staging Changes
-  Add file contents to the index [git-add](https://git-scm.com/docs/git-add)
	`git add <path>`
- Will stage all changes
	`git add .`

### Committing changes
-	Make commit feature specific and atomic
-	Record changes to the repository [git-commit](https://git-scm.com/docs/git-commit)
	`git commit -m "descriptive message"`

![gitcommit.png](gitcommit.png)


### Check commit log
- Lists all commits in reverse chronological order [git-log](https://git-scm.com/docs/git-log)
	`git log`

- diffs for specific path
	 `git log -p <path>`

### Creating branch
	`git branch <branch-name>`

### List all branches
`git branch`
	-` **indicates the current working branch`

### Switching branch
`git checkout <branch-name-to-checkout>`

### Making and staging changes
`git add <file-name>`
	- `add -> Tells git to add, stage the changes in the file`


### Pushing changes
`git push --set-upstream origin <branch-name-you-want-to-push>

![gitpush.png](gitpush.png)



## Difference
```
git diff
```
*Shows what has changed between two files*
*Compares working tree to the staging area*
`git diff --staged`

## .keep files
*Creating empty directory by using placeholder file* 
`touch .keep`


## Undoing staged changes
```
git reset HEAD <file to remove from staging>
```
*reset -> restores the environment to a particular state*
*HEAD -> lable that references the most recent commit i.e. keeps track of latest commit*
so when reset is put; use HEAD as a reference point, restore the staging area to that point, but only restore any changes related to the file.

## Moving files
```git mv <src> <tgt>
```

## deleting files
```
git rm
```

## Restoring deleted files
```
git reset HEAD <deleted filename>
```
```
git checkout HEAD <deleted filename>
```

## .gitignore
