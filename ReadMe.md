

### Terminal Commands
```
pwd     print working directory
ls      short listing
ls -l   long listing
ls -a   list ALL files (includes hidden files)
cd      change directory
cd ..   change directory to parent directory
cd ~    change directory to home directory
cd /    change directory to root directory
mkdir   make directory
touch   make a file
```

In Windows PowerShell, `ls` is not the actual Unix binary—it is a built-in alias for the PowerShell cmdlet `Get-ChildItem`.
When you run `ls -l`, PowerShell interprets `-l` as the parameter abbreviation for `-LiteralPath`, which requires a target path directly after it. Because you provided nothing after `-l`, PowerShell throws the `Missing an argument for parameter 'LiteralPath'` error.

### What to Use Instead
Depending on what you want to achieve, use one of the following alternatives:
- Standard PowerShell list (detailed table view):
```
ls | Format-Table
# or simply:
dir
# or full list view:
ls | Format-List
```

- Include hidden/system files (equivalent to Unix ls -la):
```
Get-ChildItem -Force
```

- Call the actual Git Bash / Unix ls binary directly:
If you have Git for Windows installed, run:
```
& "C:\Program Files\Git\usr\bin\ls.exe" -l
```

- Switch your VS Code default terminal to Git Bash:

Press Ctrl + Shift + P, type `Terminal: Select Default Profile`, and choose **Git Bash**. This will give you standard Linux flags like `ls -la`, `grep`, and `touch` natively.


### Git Commands

```
git init    Initializes an empty Git repository in the current directory
git status  prints status of the repository
git add     adds the file to be tracked by Git
git commit  commits the file to the repository
            git commit -m "Initial commit."
git log     shows the log for the repository

git revert -n   [commit hash or first 7 chars of the commit hash] 
            -n does not commit the revert.
git revert [commit hash or first 7 chars of the commit hash] 
            omitting the -n flag commits as well
git revert HEAD
            reverts the latest commit (:q to save the proposed comments)

git reset [commit hash upto which we want to reset]  
            resets to that version and commit history also goes away till that version
git reset [hash] --hard     
            resets to hash and syncs the working directory as well.
git reset HEAD~1 --hard
            resets to head-1 and syncs the working directory as well.
```
