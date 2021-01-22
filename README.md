# CS503 XINU Projects Note

## XINU Lab
The XINU lab is located in *Haas257*. There are 21 Linux frontend machines *xinu01.cs.purdue.edu*, *xinu02.cs.purdue.edu*, *xinu03...*, ..., *xinu21.cs.purdue.edu* which are linked to 96 backend machines *galileo101.cs.purdue.edu*, *galileo102.cs.purdue.edu*, ..., *galileo196.cs.purdue.edu*. 


The front machines served as operating system *code development* (coding, compiling, etc) while the backend machines served as actual operating system *running platform* and it would be either accessed physicly or remotely via TLS/SSL applications such as **ssh** on Linux/MacOS/Windows or **PuTTY** for some early version Windows.  

The backend machines are **x86 Intel** Galileo boards equipped with *Quark X1000 processors* that are dedicated to running your implementation of XINU. 



## CS Account
Students registered in the course should have an account automatically set up. Please check by going to Haas257 or remote accessing one of the frontend lab machines. If you have registered but don't have an account, please contact [ScienceHelp@purdue.edu](ScienceHelp@purdue.edu).


## Project Assignments
The projects of this course will be publish through github. So, you need to: 
1. Have a github account.
2. Get familiar with some basic github operations. 

    The basic github commands such as `git clone`, `git status`, `git add`, `git commit` and `git push` would be good enough. 



## Walk through scheme for each project

1. Open the project link and accept the assignment.

    We would announce release the link through piazza or email. You just click the link and it might bring you to the following page.
    
   
    * 1.1. If it is the first time, you need to grant the access of *github classroom*.
        
        <kbd> <img src="https://github.com/ProbShin/CS503ProjectsNote/blob/main/img/img01.png" height="200"/> </kbd>
        <kbd> <img src="https://github.com/ProbShin/CS503ProjectsNote/blob/main/img/img02.png" height="200"/> </kbd>
   
    
    * 1.2. Accept the assignment.
    
        <kbd> <img src="https://github.com/ProbShin/CS503ProjectsNote/blob/main/img/img03.png"  height="200"/> </kbd>
        <kbd> <img src="https://github.com/ProbShin/CS503ProjectsNote/blob/main/img/img04.png"  height="200"/> </kbd>

    * 1.3. As the image indicate, your project repo is under `https://github.com/rssteaching/<projectName>-<yourId>/`


2. Work on the project. 
    
    In case forget the project repo. You can go to [github.com/rssteaching/](https://github.com/rssteaching/) to find your project repo.
    Let's assume your project is under 'github.com/rssteaching/project1-jonsnow' and your name is `jonsnow`.
    
    
    * 2.1. login one of the 21 front machines, either physically or remotely.  
        For example, open Terminal (of Linux/MacOS/Windows) or PuTTY and using the following command. `ssh jonsnow@xinu01.cs.purdue.edu`. Then fill in the password and login in the front machine.

    * 2.2. We need to do some profile setting for the backend machine
        ```
        export PATH=${PATH}:/p/xinu/bin
        ```
    
        If you don't want to manually type the above command each time. You can also put the above line within the `~/.bashrc` file so that the environment variable would be automatically set each time you login. 


    * 2.3. Clone the project repo under your directory.

        For example create a folder for repo.
        ```
        mkdir -p ~/my503/; cd ~/my503/;
        git clone https://github.com/rssteaching/project1-jonsnow.git
        cd project1-jonsnow   
        ```
        This project folder, `project1-jonsnow`,  would be your project1's **root directory**.


    * 2.4. Modify the source code as needed 



    * 2.5. Compile the project
    
        The *makefile* is under `./compile` folder.
        ```
        cd compile
        make
        # some times, you need to `make clean` before `make`
        ```
        If successfully compiled, a `xinu.xbin` file would be generated under the same folder.


    * 2.6 Connected to a backend machine
    
        ```
        cs-console
        ```
        The *console system* will automatically find one of the available machines. i.e. `connection 'galileo104', class 'quark', host 'xinuserver.cs.purdue.edu'`. (check `cs-concole -h` for more details.) 
        
        Then use <kbd>control</kbd>+<kbd>shift</kbd>+<kbd>2</kbd> to trigger the *command input mode*.  (`d` for upload. `p` for reboot.)
        ```
        (command-mode) d
        file: xinu.xbin
        ```
        And then 
        ```
        (command-mode) p
        file: xinu.xbin
        ```
        
        Then the *Galileo boards* would load your compiled operating system into its rom and reboot to execute it.
    
    
    
3. Commit your changes and push before the deadline.
    
    * 3.1. check repo modifications
        ```
        git status
        ```
    * 3.2. add all modifications so that git system collect the modification.
        ```
        git add <the modified files>
        ```
    * 3.2 commit modification so that git system will document your work.
        ```
        git commit -m "A brief sentence about the modification."
        ```
        (You can also use `git commit` to provide more detailed description.) 
        
    * 3.3 after `commit` all modification and `git status` shows all clear. Push the local modification sync with server. 
    
        ```
        git push
        ```
        TA could see your *push*es. Your project would be graded based on the **last push before the deadline**.
        
        
        **Reminder** Do not `push` after the deadline unless you would like to take the "late penalty" on purpous.


