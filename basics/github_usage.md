# Using github
Presenting a detailed example of using git via github.

I've found many useful articles online talking about using github, the various git commands, and how branches work. However, I never found a detailed step-by-step example of the commands I could actually use to do some work and have it become part of the main "trunk" (e.g. master branch). This writeup is intended to share the specific set of steps/commands that I typically follow in my data science work.

This is NOT intended to be a thorough introduction to git (and github). I highly recommend you read up on that first. This document is geared towards using git in a specific way.

# Preliminaries
I branch off master. I believe it's straight-forward to do this off release branches too, but I follow the KISS principle, and the vast majority of my work doesn't require release branches.

I highly recommend using an augmented bash prompt to show your current branch (and virtualenv too). I'm not a developer by training, and this has saved my multiple times from causing a mess in git to cleanup. See the bottom of this article for an example of what you can add to your .bash_profile settings.

# Steps
I assume you have a new set of work to do on an already cloned/setup repo. This might be fixing errors in some previous work (e.g. bug fixes). It could be adding more functionality to existing code/models. Or it could be the initial work on a new project. Regardless, these are the steps I follow.

All `commands` are run on my local machine. Steps 8&9 involve using github's features, and are done through a web-browser.

1. Ensure you're locally up-to-date on master
  `git checkout master && git pull`

2. Create a branch for your work
  `git checkout -b BuildRandomForest_Kaggle123`

3. Do your work. Create files, add directories, ...

Once you've reached the point of having some unit(s) of work done -- there may be more to do still -- then you should commit your changes.

4. Check what new additions you've got via
  `git status`

5. Add the ones you want for the commit
  `git add file1 file2`

6. Commit the changes locally
  `git commit -m "I create file1 to prep data and file2 to build the RF model."`

At this point, you can keep working till you feel you've got all the work under this branch done. Or, if you want to make it available to others in the meantime, you can push your local changes to the origin, so they can check them out too.

Your call. I tend to work individually for my DS work, so I commit locally till the work is done, then push the entire branch to origin

7. Push to origin
  `git push origin BuildRandomForest_Kaggle123`

When all the work on this branch is done (i.e. you've finished the work set out for this branch you created in step 1) then

8. Create a Pull Request (PR). The first time you push, you'll have the option to create a PR appear in the github page for this repo in your browser. If you've done multiple pushes, you'll have to explicitly create a PR through the repos github page.

Procedure is
  - Click the Create Pull Request for the branch you just pushed.
  - Assign yourself, and notify anyone else that should review your work.
  - Address all their comments and concerns.
  - Address any conflicst.
  - Ensure all your tests are passing.
  then...

9. Merge the PR into master.
This is done through the same page as above (there's a big button on the PR page).
  
It's good practice to delete the branch too, and github gives you a handy button in the UI to do that after a PR is merged.

10. Update your local master to get those changes you just merged into master.
  `git checkout master && git pull`

And you're done!

# Summary
To summarize, my most common workflow involves:
- `git checkout master && git pull` # To update local
- `git checkout -b RandomForest_Kaggle123`
-  Create my new notebooks, code, scripts.
- `git add file1 file2` # Add the files I've changed/created
- `git commit -m "I create file1 to prep data and file2 to build the RF model."`
- `git push origin RandomForest_Kaggle123`
- # create the PR in the github UI, assign myself, merge it.
- # delete the branch in the github UI.
- `git checkout master && git pull` # To update local
and done.

Things get more complicated when there's conflicts, or changes made locally you want to discard or ignore (or undo). More on those situations later, but the above should get you using git in a consistent and useful way!

# Bash prompt config
```
# Set the terminal title to the current working directory.
PS1="\[\033]0;\w\007\]";
PS1+="\[${bold}\]\n"; # newline
PS1+="\[${userStyle}\]\u"; # username
PS1+="\[${white}\] at ";
PS1+="\[${hostStyle}\]\h"; # host
PS1+="\[${white}\] in ";
PS1+="\[${green}\]\w"; # working directory
PS1+="\$(prompt_git \"${white} on ${violet}\")"; # Git repository details
PS1+="\n";
PS1+="\[${white}\]\$ \[${reset}\]"; # `$` (and reset color)
export PS1;

PS2="\[${yellow}\]→ \[${reset}\]";
export PS2;
```
