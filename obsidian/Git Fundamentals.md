- Version Control Systems can help you with tracking changes done to code, documents or other collections of information.

- without a VCS (Version Control System) you would have to keep track of the versions of the file/files manually.

- advantages of a VCS:
	- **team collaboration**: multiple people can work on the same project simultaneously.
	- **traceability**: changes can be associated with a message (commit message)
	- **track history**: if mistakes are made, developers can easily go back to a previous version of the files.

- types of VCS:
	- Distributed: 
		- each developer has a local copy of the project.  
		- changes can be made locally.
		- changes can be **pushed** and **pulled** from the central repository.
	- Centralized:
		- each developer can only make changes to the project **directly** to the central repository (no local copy)
		- each contributor synchronizes with the central repository
		- you won't be able to make changes to the projects if you don't have internet access.

- Key terminology:
	- **repository** or just **repo**: it is just the database that contains all the code, and changes of the project.
	- **clone**: refers to the process of creating a local copy of the project.
	- **origin**: represents the **remote** repository. Generally the remote server where we push/pull our changes to/from.
	- **staging**: refers to adding our changes to the **staging area** where we can do a final check before committing them to the local repo.
	- **commit**: apply the changes to the local repository. a commit represents a snapshot(version) of the code.
	- **push**: pushing is the process through which we can send our changes from the local repository to origin.
	- **pull**: pulling is the process of updating our local repository with new changes from the remote repository (origin).
	- **branch**: branches allow us to keep track of multiple different timelines of the code. this is what enables good workflows in a team.
	- **merge**: merging is the process through which Git allows for one branch to combine their history into another.

- Basic Git Workflow:
	- Contributor changes files to the working directory -> stages changes (staging area) -> commit changes to local repo -> push changes to the origin (remote) repo.

- Basic git commands:
	- `git init`
	- `git add`
	- `git commit`
	- `git status`
	- `git push`
	- `git clone`
	- `git pull`
	- `git fetch`
	- `git checkout`
	- `git merge`
	- `git stash` - can be used to save the current changes locally, without creating a commit. you can then "pop" them (get them back), after done dealing with more important issues like production issues.
	- `git reset` - used to discard previous commits. Caution: can also be used to rewrite history.

- Initial Setup
	- `Add-WindowsCapability -Online -Name OpenSSH.Client----0.0.1.0` (run as admin)
	- `winget --version`, you can install winget from the github's repo releases
	- `winget install Microsoft.VisualStudioCode --silent`
	- `winget install Git.Git --silent`
	
	- or you can use `chocolatey` if `winget` doesn't work
	- `choco install vscode`
	- `choco install git`
	
	- configure GitLab SSH keys:
		- `ssh-keygen`
		- `cat ~/.ssh/id_rsa.pub`
---
	- `Write-Output "some text" | Out-File -FilePath ./file2.txt`\
	- `git status -s` - shorter output of the `git status` command
	- `git diff --staged`
	- `Move-Item .\file2.txt -Destination .\Subfolder1\`
	- `Remove-Item -Path file3.txt`
	- `git config --global --edit`
	- `git commit -am "message"` : automatically adds files to the staging area and commit
		- though, the recommeded way is: first `git add` and then `git commit -m "message"`
	- `git branch --show-current`: display your current branch
	- `git branch` : display all your local branches
	- `git branch -a`: display all your local **and** remote branches.
	- `git checkout <branch>`: move to a different branch
	- `git checkout -b <branch>`: create a new branch and move to the new branch
	- `Write-Output "Javascript code" | Out-File -FilePath ./script.js; Write-Output "Style Code" | Out-File -FilePath ./style.css; Write-Output "Appended code" | Out-File -FilePath ./file1New.txt -Append`

---
	- when you checkout to an old commit you go into a **detached HEAD** state.  you're no longer on a branch, you're just viewing that specific old commit.
	- `git reflog` to check where your "**HEAD**" has been.
	- `git switch` and `git restore` replace most of `git checkout`'s use cases
	- `git switch -c`,  same as `git checkout -b`
	- `git checkout <hash>` same as `git switch --detach <hash>`, `--detach` makes the intent explicit
	- the advantage of using `git merge --squash` is: cleaner history on the destination branch, but you'll lose the details of the commits of the source branch.
	- 
