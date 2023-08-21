
## Instructions

1. Read Chapters 2 & 3 of [Pro Git][ProGit]. The chapters are short.
2. Answer these questions using [Markdown format][markdown-cheatsheet] (also [Github Markdown][github-markdown]). 
3. Place your answers between lines beginning with 3 backquotes, which tells Markdown it should be unformatted text, and write only the commands you would type (**no** shell prompt).  E.g.:
   ```
   git status         CORRECT
   $ git status       WRONG  - you do not type "$"
   ```
4. Indent the 3 backquotes so they line up with the question text (3 leading spaces) so Markdown formats you answer as part of the numbered item.
   Example:
   ```
   git init
   ```  
5. **Test that your answers are correct!** There is **no excuse** for incorrect answers since you can test your answers by experimentation.      
6. Verify that your Markdown formatting is correct -- points deducted for bad formatting. VS Code and IntelliJ have markdown previewers. You should also preview it on Github, since Github Markdown is a bit non-standard.


## Using Git

[Basics](#basics)    
[Adding and Changing Things](#adding-and-changing-things)    
[Undo Changes and Recover Files](#undo-changes-and-recover-files)     
[Viewing Commits](#viewing-commits)    
[Branch and Merge](#branch-and-merge)    
[Commands for Remotes](remote-commands.md)   
[Favorites](#favorites)     
[Resources](#resources)

#### Note on Paths

In this file, directory paths are written with a forward slash as on MacOS, Linux, and the Windows-Bash shell: `/dir1/dir2/somefile`.    


## Basics

1. When using Git locally, what are these?  Define each one in a sentence
   * Staging area -  Area where all the file are added and waiting for commit.
   * Working copy - It's an pack full of your files in local repositories that you've been working on.
   * master - Main branch of your project but recently its name has been changed to 'main'
   * HEAD - The pointer that point to your latest commit in that branch.

2. When you install git on a new machine (or in a new user account) you should perform these 2 git commands to tell git your name and email.  These values are used in commits that you make:
   ```
   git config --global user.name "Your Username"
   git config --global user.email  "Your Email Address"
   ```

3. There are 2 ways to create a local Git repository.  Briefly descibe each one:
   1. ```git init```
   Create .git sub-folder in your local repository. 
   
   2. ```git clone <URL of github>```
   Clone existing repository from github to your local directory.
   


## Adding and Changing Things

Suppose your working copy of a repository contains these files and directories:
```
README.md
out/
    a.exe
src/a.py
    b.py
    c.py
test/
    test_a.py
    ...
```     


1. Add README.md and *everything* in the `src` directory to the git staging area.
   ```
   git add README.md
   cd src
   git add .
   ```

2. Add `test/test_a.py` to the staging area (but not any other files).
   ```
   cd test
   git add test_a.py
   ```

3. List the names of files in the staging area.
   ```
   git status
   ```

4. Remove `README.md` from the staging area. This is **very useful** if you accidentally add something you don't want to commit.
   ```
   git rm --cached README.md
   ```

5. Commit everything in the staging area to the repository.
   ```
   git commit -m "Initial Commit."
   ```

6. In any project, there are some files and directories that you **should not** commit to git.    
   For a Python project, name *at least* files or directories that you should not commit to git:
   - pycache
   - venv
   - .log

7. Command to move all the .py files from the `src` dir to the top-level directory of this repository. This command moves them in your working copy *and* in the git repo (when you commit the change):
   ```
      mv src/*.py .
      git add .
      git commit -m "Move to top-level directory."

   ```


8. In this repository, create your own `.gitignore` file that you can reuse in other Python projects.  Add everything that you think is relevant.    
   *Hint:* A good place to start is to create a new repo on Github and during the creation dialog, ask Github to make a .gitignore for Python projects. Then edit it.  Don't forget to include pytest output and MacOS junk.



## Undo Changes and Recover Files


1.  Display the differences between your *working copy* of `a.py` and the `a.py` in the *local repository* (HEAD revision):
```
   git diff a.py
```


2. Display the differences between your *working copy* of `a.py` and the version in the *staging area*. (But, if a.py is not in the staging area this will compare working copy to HEAD revision):
```
   git diff --staged a.py
```

3. **View changes to be committed:** Display the differences between files in the staging area and the versions in the repository. (You can also specify a file name to compare just one file.) 

```
   git diff --staged
```


4. **Undo "git add":** If `main.py` has been added to the staging area (`git add main.py`), remove it from the staging area:
```
   git reset  main.py
```


5. **Recover a file:** Command to replace your working copy of `a.py` with the most recent (HEAD) version in the repository.  This also works if you have deleted your working copy of this file.
```
   git checkout HEAD -- a.py
```

6. **Undo a commit:** Suppose you want to discard some commit(s) and move both HEAD and "master" to an earlier revision (an earlier commit)  Suppose the git commit graph looks like this (`aaaa`, etc, are the commit ids)
```
aaaa ---> bbbb ---> cccc ---> dddd [HEAD -> master]
``` 
   The command to reset HEAD and master to the commit id `bbbb`:
```
   git reset --hard bbbb
```


7. **Checkout old code:** Using the above example, the command to replace your working copy with the files from commit with id `aaaa`:
```
   git checkout aaaa -- .
```
    Note:
    - Git won't let you do this if you have uncommitted changes to any "tracked" files.
    - Untracked files are ignored, so after doing this command they will still be in your working copy.  
 

## Viewing Commits

1. Show the history of commits, using one line per commit:
   ```
   git log --oneline
   ```
   Some versions of git have an *alias* "log1" for this (`git log1`).

2. Show the history (as above) including *all* branches in the repository and include a graph connecting the commits:
   ```
   git log --oneline --graph --all
   ```


3. List all the files in the current branch of the repository:
   ```
   git ls-tree --name-only HEAD
   ```
   Example output:
   ```
   .gitignore
   README.md
   a.py
   b.py
   test/test_a.py
   test/test_b.py
   ```


## Branch and Merge



1. Create new branch named "bugfix/issue-152"
   ```
   git branch bugfix/issue-152
   ```
2. Switch branch to the new one that we just created.
   ```
   git checkout bugfix/issue-152
   ```
3. Merge branch after resolve the issues.
   ```
   git checkout master
   git merge bugfix/issue-152
   ```
4. Delete old branch after merge.
   ```
   git branch -d bugfix/issue-152
   ```


## Favorites

### Revert some specific commit.
Sometimes I accidentally commit something that shouldn't commit or commit it by accident there's a safer way to revert than using `git reset ` is to use `git revert <commit hash>`


1. Identify git hash by using 
```
git log
```
2. Use git revert to revert the changes.
```
git revert <commit-hash>
```
3. Edit the commit message.
An editor will open to let type-in commit message to describe what you've reverting and why.
4. Push reverted changes.
```
git push origin/master branch-name
```



---
## Resources

* [ChatGPT](https://chat.openai.com/) An incredible source of information based on AI to answer your question.
* [Oh My Git!](https://ohmygit.org/) A fun game that has interactive interface tha will show internal structure of github.
* [Pro Git Online Book][ProGit] Chapters 2 & 3 contain the essentials. Downloadable e-book is available, too. 
* [Visual Git Reference](https://marklodato.github.io/visual-git-guide) one page with illustrations of git commands.
* [Markdown Cheatsheet][markdown-cheatsheet] summary of Markdown commands.
* [Github Markdown][github-markdown] some differences in the way Github handles markdown and special Markdown for repos.

Learn Git Visually:

* [Learn Git Interactive Tutorial][LearnGitInteractive] great visual tutorial
* [Git Visualizer][VisualizeGit] execute Git commands in a web browser and see the results as a graph.

[ProGit]: https://www.git-scm.com/book/en/v2 "Pro Git online book on Git-scm.com"
[ProGitPdf]: https://progit2.s3.amazonaws.com/en/2016-03-22-f3531/progit-en.1084.pdf "Pro Git v.2 PDF on AWS. Longer, book format."
[LearnGitInteractive]: https://learngitbranching.js.org "Interactive graphical git tutorial"
[VisualizeGit]: http://git-school.github.io/visualizing-git/ "Online tools draws a graph of commits in a repo as you type"
[markdown-cheatsheet]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[github-markdown]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
