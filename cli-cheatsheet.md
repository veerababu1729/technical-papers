# CLI Cheat Sheet:  
CLI: users interact with program using text instead of graphics.  
Terminal: Interface where users type those commands  
Shell: Command Interpreter to execute the commands.  

**shell examples: Bash, Powershell**

## Commands to remember:
1. chek disk status: ```df -h``` (h for human readabale)
2. absolute path always starts with / and rpath depends on the context.
3. cd
4. ~ -> home dir,   / -> root dir.
5. touch, nano, cat >, cat>> for new file operations.

6. ls -l : lists all the files or folders permissions breakdown in cur dir.
7. ls -l filename/folder (or) ls -lh file/foldername
8. 421 rule for rwx permissions (ugo)
9. change permissions: 
   * chmod 755 filename
   * chmod u+x, g=rw, o-x filename
   * for folder: include -R
   
10. firefox & (opens firefox background)
11. ps -aef : Lists all the processes.
12. pstree -p: Lists all the processes in a tree structure -p includes PIDs or simply pstree.

13. ps -aef | grep -i firefox (filters PIDs of firefox)
14. pgrep firefox
15. pidof firefox
16. pstree -p PID(of fire fox).
17. tree foldername(shows structure of folder in a tree structure)

18. kill -9 PID (root PID)
19. pkill firefox
20. ps -aef | grep firefox | awk '{print $2}' | xargs kill -9 (as per review question).

21. whoami(current username), id(current user ID, primary group, supplementary groups)
    
 **20. Print all the user names:** 
``` cut -d: -f1 /etc/passwd```  
21. ```groups``` : lists the groups current user belongs to  
22. ``` uname -m``` displays cpu architecture ex: x86_64  

23.``` lspci | grep -i audio ```  
    - PCI (Peripheral Component Interconnect simply hardware devices)  
    - lspci lists out PCI devices  
    - grep filters audio devices with case insensisitivity.  

24. Search file names or folder names:
    ``` sudo find / -iname '*java*' 2>/dev/null```  (at root level)  
    ``` find ~ -iname '*java*'```
    ``` find ~ -type f "*.txt"```
    Note that in CLI ' ' or " " is treated same by shell.

26. ```ls -lahtr ~``` : Lists All Home-Directory Entries by Modification Time
     - Flags exp:  
     -l Long listing format  
     -a Include hidden entries (names beginning with .)  
     -t Sort by modification time, newest first  
     -h Show file sizes in human-readable units  
     -r Reverse the sort order

27. Harry potter book:
   * curl -L "url" -o book.txt  (-L(opt): follows redirects, -o specifies what name shoube be given to saved one)
   * head -n 30 book.txt
   * tail -n 30 book.txt
   * grep -iow "Harry" book.txt | wc -l.
     - i for case insensitivity
     - o prints matching in seperate line (without o like grep "Harry" book.txt prints whole line)
     - w compares with whole word not its part(ex: harryish does not match)

27. ? matches exactly one charcater ex: ``` find ~ -iname '?.txt''```  (matches a.txt, b.txt).
