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


## .gitignore

Some files need not be added to the repository. These include temporary files and binaries. The file *.gitignore* with the following content will ignore all `*.swp` files created by *vim*:

    .*.swp

    


