

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

### Learnings from Git Commands

#### revert
When using revert to revert a specific commit, git can possibly identify a conflict in that attempt. How?

Git's diff engine looks at lines in chunks called hunks, usually using 3 lines of unchanged context above and below a change to locate where a patch should be applied.

When changes happen on consecutive lines, Git merges them into a single hunk:
```
Joke 2
Joke 3   <-- commit1
Joke 4   <-- commit2
```
Because `commit1` and `commit2` touch adjacent lines, Git treats them as a single overlapping region. Reverting `commit1` means modifying a line (`Joke 3`) that directly touches a line modified later (`Joke 4`). Git flags this as a collision.

##### How Blank Lines Change the Outcome
If you put blank lines between each joke:

`commit1`

```
Joke 2

Joke 3
```

`commit2`

```
Joke 2

Joke 3

Joke 4
```

Here, the blank line creates a buffer. Git sees `Joke 3` and `Joke 4` as **two separate, independent hunks**.

When you run `git revert commit1`:

Git looks for the hunk containing `Joke 3` surrounded by empty lines.

It clean-deletes `Joke 3` without touching `Joke 4`.

No merge conflict is raised!

##### Key Takeaway
Git handles non-adjacent changes automatically. A conflict only triggers when two commits touch the exact same line or directly adjacent lines without enough stable context in between for Git to safely apply the change.