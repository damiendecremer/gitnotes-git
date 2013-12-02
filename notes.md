# GIT notes


## Creating a new github repository

    git init
    git add .
    git commit -m "initial commit"

On github.com, log in and click "Create a new repo" in upper right corner. Copy the link to the newly created repository and run

    git remote add origin git@github.com:USER/NEW-REPO-NAME
    git push -u origin master

You might get an error message saying *Permission denied (publickey).* To fix this you need to run 

    ssh-keygen

which creates the file `id_rsa.pub`. Choose *no passphrase* to be able to push changes without entering a password. On github.com go to *Edit your profile* > *SSH keys* > *Add SSH Key* and copy the output of

    cat ~/.ssh/id_rsa.pub

into the *Key* field. The error should disappear.



## A simple branching model

For my own workflow it has proven useful to work with a *master* branch, a *develop* branch, and to create a new branch for every new feature added to the code. To create a new branch from the current one, first stage and commit the current changes

    git add .
    git commit -m "last update of develop branch"

and create the *new_feature* branch with 

    git branch new_feature

To make it the current branch use

    git checkout new_feature

Then implement the feature, not forgetting to commit often and early. After successfully implementing the new feature, commit one last time and change back into the *develop* branch

    git checkout develop

Now merge the *new_feature* branch with the *develop* branch and delete the *new_feature* branch with the following commands

    git merge new_feature
    git branch -d new_feature

The *develop* branch is now ready to be pushed to the repository

    git push origin develop



## History modification

**Committed too early**

Say you have created a file and committed the change:

    touch newfile.txt
    git add newfile.txt
    git commit -m "added newfile.txt"

But you forgot to add content to the file

    echo "content" > newfile.txt

Instead of committing once again, the last commit can be amended:

    git commit -a --amend
    

**Committed too often**

    git init
    touch file1
    git add .
    git commit -m "initial commit"

    touch file2
    git add .
    git commit -m "added file2"
    
    echo "content" > file2
    git commit -am "added content to file2"
    git log
       > added content to file2
       > added file2
       > initial commit

Now we might want to *squash* the last two commits into a single one. This is done as follows:

    git rebase -i HEAD~2

In the editor we see:

    pick d656c5d file2
    pick c71f8db file2 content

which we change to

    pick d656c5d file2
    squash c71f8db file2 content

In the next window combine the two commit messages into a single one:

    added file2 with content

Now the new log looks as follows:

    git log
       > added file2 with content 
       > initial commit
        
    




## .gitignore

Some files need not be added to the repository. These include temporary files and binaries. The file *.gitignore* with the following content will ignore all `*.swp` files created by *vim*:

    .*.swp

    


