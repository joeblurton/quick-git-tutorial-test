# A quick git useage tutorial for beginners

Git is an incredibly powerful and complex tool that can be intimidating for new users.  You will make mistakes as you learn, but don't worry - that's an essential part of the development journey. 

Remember that git is widely used, so the search engines are awash with useful information on how to resolve virtually any git-related issue.

By following the steps below I hope you will gain a simple working knowledge of the git workflow. 

_Please feel free to submit PRs to improve and expand on this guide._

------



## First steps

Please install git and ensure you are able to access it through the CLI/Command Prompt on your local machine. [This Atlassian tutorial](https://www.atlassian.com/git/tutorials/install-git) will guide you through the install process. 

Though these steps would be similar with BitBucket or any other hosted Git service, you will also need a GitHub account.



## Clone this repository

In order to work through the tutorial, we will want a basic structure. You have two main options here to clone:

### 1. via the command line (preferred)

Using your terminal, `cd` into a safe, user-controlled directory on your computer. Run the following command in your terminal:  `git clone https://github.com/joeblurton/quick-git-tutorial`

If you have configured git correctly, this will now proceed to clone, and after this is completed you will be able to enter the tutorial directory by typing `cd`. 

You can verify that you have cloned the correct files by using `ls` on Mac/Linux or `DIR` on Windows. The list of files and folders created by these commands should match the corresponding list for this repository on GitHub.

### 2. using a GUI client

Clone this repository using the GitHub clone button above and add it to any GUI git client you may have installed. The instructions for this will depend on your client of choice.



## Set up your own repository

Create a new repository on GitHub, naming it `quick-git-tutorial-test`.

In the cloned tutorial directory, change the remote repository using this pattern, **remembering to change your username in the URL**:

`git remote set-url origin https://github.com/your-username/quick-git-tutorial-test`



## Commit

Start by using a text editor to create a file in the tutorial directory called `counting.txt`, enter the text below, and then save.

```
Line 1
Line 2
Line 3
```

Git is used to track and manage changes in your files. It is already aware of this particular change, but you will need to *commit* this change in order for it to be added to version control.

You can do this by running the command `git add counting.txt`. This adds the file to a kind of queued collection. You can also use an interactive mode to add multiple files using `git add -i`.

Next we need to commit the change. For this, we need to include a "commit message". This allows other developers to understand what you intended, but it will also help you to understand your past decisions in the future. 

_To be at all useful, a commit message should be as short as possible, but also as descriptive as possible._

Commit `counting.txt` like so: `git commit -m 'Added line list: counting.txt'` 



## Making mistakes

In `counting.txt`, replace the original text with the following:

```
Mistake 1
Mistake 2
Mistake 3
```

Save the file, then run the following commands in order:

```
git add counting.txt
git commit -m "Added a mistake"
```

Git will tell you that 1 file changed, with 3 line insertions, and 3 deletions.

If you run `git log --oneline`, you will see that we now have two commits. **But wait! The last one was a mistake...**

You could undo the changes manually, but there is a simpler way:

```
git reset --hard HEAD~1
```

If you imagine the git log like a family tree, the HEAD was the last commit in the current working branch and represents you. We're simply discarding you and going back in time to your parent. You can use `HEAD~2` to reset to your grandparent, and so on. 

The `--hard` flag is important as it ensures the changes you made on the filesystem are removed. To keep the changes, use the `--soft` flag.



## Push

So we've made changes to the local git repository, but for the purposes of this tutorial we would quite like these to be pushed to the remote repository on GitHub too:

```
git push
```



## Branch

Branches act like copies of your code, which you can work on without disturbing anyone else's efforts or without making unwanted changes to your master branch, which should essentially be the best, most pristine version of your production code.

To make a new branch we can do this:

```
git branch french
```

You can check this worked by running:

```
git branch
```

You should now have two branches, `master` and `french`. The asterisk denotes which is the currently selected branch. You are still on `master`, and that's not ideal as we want to make some fresh changes in the new branch. 

Do this:

```
git checkout french
```

And hey presto you're now working in the `french` branch.

It's important to know what's going on here, so you went the long way around. You can achieve the same thing like so:

```
git checkout -b french
```

This simply creates the branch and switches to it in one command.

Change `counting.txt` to contain the following:

```
Line Un
Line Deux
Line Trois
```

Then add and commit this change like so:

```
git add counting.txt
git commit -m "Altered the count to French"
```

Now we have our text in French, head back to the master branch:

```
git checkout master
```



## Pull

Go to your GitHub repository using your browser. Click on `counting.txt` and then click the little edit button towards the top right of the source.

Change the text as follows:

```
Line One
Line Two
Line Three
```

Below the editor, add a commit message to say that you've used the English words for the numbers and then press the *"Commit changes"* button. 

You have now done a totally sensible thing and translated those pesky numerals to English in your master branch.

Go to the local repository in your terminal, where you should be on the master branch. You can confirm this by using `git branch`.

You can check if this change is available by using `git fetch`.

The following command will pull the changes down from the remote repository:

```
git pull
```



## Merge

Now what we ideally want is to merge everything that we have because the master branch maintainer is happy with their work, and you're happy with yours.

Run:

```
git merge french
```

**Zut alors!** Some pesky rosbif has arrogantly translated the text to English! Git doesn't like that at all. You're now trying to merge files where the same lines have been changed in different ways. What we have here is a **merge conflict**.

You should always try to ensure that you're working from a state that won't cause these types of issues, so *pull often*. Always assume someone is already working on an affected portion of the codebase, no matter how small the team.



## Resolving conflicts

Open `counting.txt` and you will see that git has inserted some funny symbols and you have double the text you had before.

The top section describes the current state of HEAD (what the current state of the `master` branch maintains is true). The bottom section contains the content from the `french` branch.

It's probably best to talk to the master branch maintainer at this point before you cause a diplomatic incident. You will need to agree on a sensible compromise. For the sake of this tutorial we're just going to remove the text that git inserted, and amend the file like so:

```
Lína eitt
Lína tvö
Lína þrjú
```

If this conflict affects multiple files, repeat the editing process in each file.

```
git add counting.txt
git commit -m "Translated counting.txt to Icelandic"
git push
```

Your local master branch and the remote master should now both agree that Icelandic is the best language.



## Deleting a branch

 We don't want our local repositories from getting too messy, so let's delete the `french` branch like so:

```
git branch -d french
```



## Wrapping up

There is so much more to git, and you may want to explore the documentation for the `git stash` and `git rebase` commands in particular. Most teams will require you to submit pull requests before making changes to remote repositories, and there is a [handy tutorial for that here](https://yangsu.github.io/pull-request-tutorial/). 

But if you follow the workflow outlined here, you will be up and running as a basic git user and your life just might get a little easier too!