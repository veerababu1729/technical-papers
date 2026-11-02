# GIT CHEAT SHEET:
1. What is git?  
Git is an open-source distributed version control system designed to track and manage changes in source code during software development. (runs locally)

2. What is github?
GitHub is an online hosting platform that backs up your Git projects code in the cloud. simply a google drive for your code.  
Analogy: Word is git whereas google doc is github.

3. What is version control?  
It is a system that tracks and manages every change to your project source code. use: collaboration, fallback.

4. Repository: Collection of source code (folder name).  
   
6. Commits: A commit is a saved snapshot of your project's file at a specific point of time. It is identified by commit msg and unique SHA1 hash code.  
   Golden rule of commit: Commit small, commit often, commit working code.  

7. Branches: A branch is an independent, parallel timeline for your project's code.  
Instead of modifying the working production code directly, you split off onto a branch to build a new feature or fix a bug in total isolation. Then merge into main.  
Usage: Work divide, Emergency fixes, Code review and permit, No merge conflicts.  

7.Every local repo undergoes working directory --> Staging area (git add .) --> Commit area (git commit -m).  

8. You can use ssh or Personal Access Tokens to connect or authenticate local system to github(cloud).  

## Imp Git commands for a beginner:  

1. ```git status```: Shows you exactly what files you changed, added, or deleted since your last save point. Run this constantly.
2. ```git log```: command displays a chronological record of all commits made in a repository.(conatins hashcode and commit msg)  
3. ```git init```: Initializing git repo.
4. ```git add a.txt```: add a file a.txt
5. ```git commit -m "first commit"```: make a commit.
6. ```git branch leaf```: create a new branch called leaf.  
7. ```git switch leaf```: Switch to leaf branch. 
8. ```git add b.txt```: add a file b.txt.  
9. ```git commit -m "second commit"```: create second commit.
10. ```git switch main``` and  ```git merge leaf```: Merge leaf into master.
11. HEAD: head is a pointer tells Git which branch and commit your terminal is currently looking at.(find with ```git log```)  
--------------

12. git remote add origin "url"
13. git pull origin main (pulls out all the latest changes from main branch and adds to your current code)
14. git push -u origin main  (later you can simply use git push)

15. git branch -m master main : -m stands for move so it renames master to main.
    
17. committed the wrong message?
    ```git commit --amend -m "Your new correct message"```  
18. skipped file in the last commit?
    ```git add skipped-file.txt  
       git commit --amend --no-edit```
19. accidentally committed on master?
    ```git branch feature-branch   # 1. Create a new branch with your changes
       git reset --hard HEAD~1     # 2. Roll back master by 1 commit
       git switch feature-branch   # 3. Switch to your feature branch to continue```
20. Accidentally committed to the wrong branch?  
    ```git switch right-branch  
       git cherry-pick <commit-hash>  # 1. Pull the commit over to the right branch
       git switch wrong-branch
       git reset --hard HEAD~1        # 2. Erase it from the wrong branch```  
21. want to undo the last 5 commits?  
    (i) Keep your code changes as unstaged files: ```git reset HEAD~5```  
    (ii) Completely erase all 5 commits and all code changes permanently: ```git reset --hard HEAD~5```  
22. want to undo changes to a file?  
    (i) Unstage a file (keep your edits but remove it from the next commit box):```git restore --staged filename.txt```  
    (ii)Completely discard edits and revert a file back to the last saved commit: ```git restore filename.txt```











   





