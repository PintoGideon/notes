# Adding a file to your repo
```
git add index.html
```

### Repositories

A Git repository is simply a database containing all the information needed to retain and manage the revisions and history of a project. Within a repository, Git maintains two primary data structures, the object store and the index. All of this repository data is stored at the root of your working directory in a hidden subdirectory called git.


### Git Object Types

At the heart of Git's repository implmentation is the object store. In contains your original data files and all the log messages , author info,dates and other information required to rebuild any version or branch of the object.

Git places four types of objects in the object store

- blobs
- trees
- commits
- tags

Blob a contraction of the binary large object is a term that's commonly used in computing to refer to some variable or file that can contain any data and whose internal structure is ignored by the program. A blob is treated as being opaque. A blog holds a file's data but does not contain any metadata about the file or even it's name.

A tree object represents one level of directory info. It records blob identifiers , path names and bit of metadata for all the files in one directory. 

A commit object holds metadata for each change introduced into the repository including the author, committer , commit date and log messgae.


tags are object that give descriptive names to commits.


### Git tracks content


Git is a content tracking system. Git's object store is based on the hashed computation of the contents of its objects, not on the file or directory names from the user's original file layout.

If two files have exactly the same content, whether in the same or different directories, Git stores a single copy of that content as a blob within the object store. Git computes the hash code for each file according solely on it's content, determines that files have the same SHA1 values and thus the same content,  and places the blob object in the object store indexed by that SHA1 value.

If one of those file changes, Git computers a new SHA1 value for it.


Git uses a more efficient storage mechanism called pack file. To create a packed file, Git first locates files whose content is very similar and stores the complete content for one of them. It then computes the differences or deltas, between similar files and stores just the differences.

Git's object store is based on the hashed computation of the contents of its objects, not on the file or directory names from the user's orignal file layout.

WHen Git places a file in the object store, it does so based on the hash on the data and not on the name of the file. In fact, Git does not track file or directory names, which are assosciated with files in secondary ways.

***Git tracks content instead of files***


When Git needs to create a working directory, it says to the filesystem: "Hey I have this big blog of data that is supposed to be placed at pathname /path/to/directory/file, Does that make sense to you? The filesystem is responsible for saying "Ah, yes I recognize that string as a subset of directory names and I know where to place your blob of data! Thanks"

Git uses a efficient storage mechanism called a pack file. To create a packed file, Git first locates files whose content is very similary and stores the complete content for one of them. It then computes the differences or deltas between similar files and stores just the differences. For example, if you were to just change or add one line to a file, Git might store the complete, newer version and then takes note of the line change as a delta and store that in the pack too.

Storing a complete version of a file and the deltas needed to construct other versions of similar files is not a a new trick.



### Git Workflow


When you create a new file in a git repo , for examples hello.txt, Git doesnt' care that the filename is hello.txt. Git cares only about what's inside the file: The sequence of 12 bytes that represent "Hello world" and the terminating newline. Git performs a few opertions on this blob, calculates its SHA1 hash, and enters it into the object store as a file named after the hexadecimal representation of the hash.



Now that the "hello world" blob is safely ensconed in the object sore, git tracks the pathnaes of file through another kind of object called a tree. 

### It's all about the Index

Git's index doesn't contain any file content; it simply tracks what you want to commit. When you run git commit, Git checks the index rather than your working directory to discover what to commit.


```
git diff

```

git diff displays the changes that remain in your working directory and are not staged.



### Using git add

The command ```git add ``` stages a file. In terms of Git's file classifications, if a file is untracked, then git add converts that file's status to tracked. When git add is used on a directory name, all of the files and subdirectories beneath it are stagesd recursively.

In term's of Git's object model, the entirety of each file at the moment you issued git add was copied into the object store and indexed by it's resulting SHA1 name. 


Most of the day-to-day changes within your repo will likely be simple edits. After any edit or before you commit your changes, run git add to update the index with the absolute latest and greatest version of your file.


Using git rm

The command git rm is naturally the inverse of git add. It removes a file from both the repository and the working directory. However, because removing a file tends to be more problematic than adding a file, Git treats the removal of a file with a bit more care.

### A quick workflow


```

// Stage a newly created file named "oops"
git add oops

git status
// Changes to be committed
// new file: oops

// To convert a file from stage to unstaged, use git rm --cached

git rm --cached oops
git ls-files --stage


```

Whereas git rm --cached removes the file from the index and leaves it in the working directory, git rm removes the file from both the index and the working directory

### Important pieces of removing files

If you want to remove the file once it's been committed, just stage the request through a simple git rm filename

```

$ git commit -m "Add some files"

/*
2 files changed
.gitignore
data
*/

$ git rm data
// rm 'data'

```


Before Git removes a file, it checks to make sure the version of the file in the working directory matches the latest version in the current branch. 

And incase you really meant to keep a file you accidentally removed, simply add it back

```
$ git add data

```

Darn ! Git removes the working copy too!!

Here is how you recover it

```
git checkout HEAD --data
$ cat data

//New data
```


### .gitignore

- A simple, literal filename matches a file in any directory with that name

- A directory name is marked by a trailing slash characer (/). This matches the named directory and any subdirectory but does not match a file or a symbolic link



### A detailed view of Git's object Model and Files


Let's note down the progress of a single file named ***file1*** as it is edited, staged in the index and finally committed.

The working directory contains two files 


- file1
   -foot.html

- file 2
   -bar.html

Now, the master branch has a commit that records a tree with exactly the same "foo" and "bar" contents for file1 and file2. Furthermore the index records SHA1 values a23bf and 9d3a2 for exactly those same file contents. The working directory, the index and the object store are all synchronized and in agreement. Nothing is dirty.

Now, if you edit file1 in the working directory so that its contents now consist of an html template, nothing in the index nor in the object store has changed, but the working directory is now considered dirty.

Here is what happends when you use the command ```git add ile1``` to stage the edit for file1.

Git first takes the version of file1 from the working directory, computes SHA1 hash ID for its contents and places the ID in the object store. Git records in the index that the pathname file1 has been updated to new SHA1.


Because the contents of the file2 hasn't changed and no git add staged file2, the index continues to reference the original blob object for it.


At this point, you have staged file1 in the index and the working directory and the index agree. However the index is considered dirty with respect to HEAD because it differs from the tree recorded in the object store for the HEAD commit of the master branch.

After all the changes have been staged in the index, a commit applies them to the repository. 

The commit initiates three steps.

1. The virtual tree object that is the index gets converted into a real tree object and placed into the object store under the SHA1 name. Second a new commit object is created with your log message. 

2. The new commit points to the newly created tree object and also to the previous or parent commit. 

3. Third, the master branch ref is moved from the most recent commit to the newly created commit object , becoming the new master HEAD.


### Commits

In git, a commit is used to record changes in a repo.

Git maintains several special symrefs automatically for particular purposes. They can be used anywhere a commit is used.


HEAD
HEAD always refers to the most recent commit on the current branch. When you change branches, HEAD is updated to refer to the new branch's latest commit


Each commit introduces a tree object that represents the entire repository.


In the field of computer science, a graph is a collection of nodes and a set of edges between the nodes. There are several types of graphs with different properties. Git makes use of a special graph called directed acyclic graph. A DAG has two important properties. First , the edges within the graph are all directed from one node to another. Second, starting at any node in the graph, there is no path along the directed edge that leads back to the starting node.


Git implements the history of commits within a repo called as a DAG. In the commit graph, each node is a single commit and all edges are directed from one descendant node to another parent node, forming an ancestor relationship. 


### Branches

A branch is the fundamental means of launching a seperate line of development within a software project. A branch is a split from a kind of unified, primal state, allowing development to continue on multiple directions simultaneously and potentially to produce different versions of the project. 


### Creating Branches

A new branch is based upon an existing commit within the repository. It is entirely upto you to determine and specify which commit to use as a part of the new branch. 


### Listing Branch Names

The ```git branch``` command lists branch names found in the repository.

```
$ git branch

```

### Viewing Branches

The ```git show-branch``` command provides more detailed ouput than git branch.


### Checking out brances

To start working on a different branch, issue the ```git checkout``` command. Given a branch name, git checkout makes the branch the new, current working branch. It changes your working tree file and directory structure to match the state of the given branch.


### A basic example of Checking out a Branch


```
git checkout gideon
Switched to branch "gideon"

```


### Merging Changes into a different Branch

Suppose you wanted to shift gears from the dev branch in the previous section's example but instead devote your attention to fixing the problem assosciated with the bug/pr-1 branch.

Selecting a new current branch might have dramatic changes on your working tree files and directory structure. Naturally, the extent of that change depends on the differences between your current branch and the new target branch you would like to check out.

If the current state of your working directory conflicts with that of the branch you wanted to switch to, a merge is needed. The changes in your working directory must be merged with the files being checked out.

If specifically requested with the -m option, Git attempts to carry your local change into the new working directory by performing a merge operation between your local modifications and the target branch.



```
git checkout -m dev

```


The merge operation does not introduce a merge commit on any branch.

### Deleting Branches

```
git branch -d branchname

```

Git prevents you from removing the current branch. Removing the current branch would leave Git unable to determine what the resulting working directory tree would look like. Instead, you must always name a noncurrent branch.

Git won't allow you to delete a branch that contains commits that are not also present on the current branch. That is Git prevents you from accidentally removing the development in commits that will be lost if the branch were to be deleted.

If the branch you want to delete has a commit that can only be foind in that particular branch. If that branch were to be deleed, there would no longer be a way to access that commit.


Git is keeping you from accidentally losing content from the branc to be deleted that is not merged into your current branch.


An approach is to merge the content from the branch you want to delete into your current branch.

```
git merge branchname

```

Once you are sure you don't want the extra content in that branch, you can override Git's safety check by using -D instead of -d.
 
### Diffs

A diff is a compact summary of the differences between two items. Git has it's own diff facility and can likewise produce a digest of differences. The command `git diff` can compare two files much akin to Unix's diff command. Moreever, like diff -r, Git can traverse two free object and generate a representation of the variances.

But git diff also has it's own nuances and powerful features tailored to particular needs of Git users.

### Merges

Git is a distirbuted version control system. It allows , for example, a developer in Japan and another in NJ to make and record changes independently and it permits the two developers to combine their changes at any time , all without a central repo.

### Merge Examples

To merge ```other_branch``` into ```branch```, you should check out the target branch and merge other brances into it.

```
$ git checkout branch
$ git merge other_branch

```


### Merge with a conflict

A merge operation is inherently problematic because it necessarily brings together potentially varying and conflicting changes from different lines of development. The changes on one branch may be similar to or radically different from the changes on a different branch. 

When a merge conflict like this occurs, you should almost invariably investigate the extend of the conflict using the git diff command. 


If you are happy with the conflict resolution, you should git add the file to the index and stage it for the merge commit

```
$ git add file
$ git commit

```


### Working with Merge Conflicts

As demonstrated by the previous example, there are instances when conflicting changes can't be merged automatically.

```
$ git init

mkdir test
touch index.html
echo "hey">index.html
git add test
git commit -m "Testing Merge Conflict"


git checkout -b alternate

// Switched to branch "alternate"

mkdir test
touch index.html
echo "heys">index.html
git add test
git commit -m "Added content to the alternate branch"

git checkout master
git merge alt

// Automatic merge failed, fix conflicts
```

### Locating Conflicted Files

You can use ```git status``` or ```git ls-files -u``` command to show the set of files that remain unmerged in your working tree.

### Merge Strategies

Imagine there are three developers (Alice, Bob and Cal) contributing to a single repository.

Because the developers are all contributing to seperate branches , let's leave it upto one person Alice to manage the integration of the various contributions. In the meantime, each developer is allowed to leverage the development of the others by directly incorporating or merging a co-workers branch as needed.

### Degenerate Merges

There are two common degenerate scenarios that lead to merges and are called already up-to-date and fast forward. 

- Already up-to-date: When all the commits from the other branch (it's HEAD) are already present in your target branch, even if it has advanced on it's own, the target branch is said to be already up to date. As a result, no new commits are added to your branch

- Fast-forward: A fast forward merge happens when your branch HEAD is already fully present and represented in the other Because your HEAD is already present in the other branch , Git simply tacks on to your HEAD the new commits from the other branch. Git then moves your branch HEAD to point to the final new commit. 


### Altering Commits

A commit records the history of your work and keeps your changes sacrosanct, but the commit iteself isn't cast in stone. 

### Using git reset

```
git reset --soft commit
git reset --mixed commit
git reset --hard commit

```


git reset adjusts the HEAD ref to a given commit and, by default update the index to match that commit. If desired, git reset can also modify your working directory to mirror the revision of your project represented by the given commit.

One interesting use case of git reset is to simply redo or eliminate the topmost commit on a branch. 

```

git init
mkdir website
touch index.html

git add website
git commit

echo "Hey there">>index.html
git commit -m "Add content"

```

Now suppose we realize that the second commit is wrong and you want to go back and do it differently. This is a classic application of git reset --mixed HEAD. 

***The HEAD references the commit parent of the current master HEAD and represents the state immediately prior to completing the second, faulty commit***

After git reset HEAD, Git has left the new state of the master_file and the entire working directory just as it was immediately prior to making the "Add content" commit.


### Illustrating the use of git reset with other branches

```
git checkout master
Already on "master"

git checkout -b dev
echo bar >> dev_file
git add dev_file
git commit

git checkout master
git rev-parse HEAD^

```

There are several ways of determining the commit to which the master branch should infact be reset

```
$ git log
```
### reset, revert and checkout

If you want to change to a different branch, use git checkout. Your current branch and HEAD ref change to match the tip of the given branch.

The ```git reset``` command does not change your branch. However if you supply the name of a branch, it will change the state of your current working directory to look like the tip of the named branch. In other words, ```git reset``` is intended to reset the current branch's HEAD reference.

### Rebasing commits


The ``` git rebase``` command is used to alter where a sequence of commits is based. This command requires at least the name of the other branch onto which your commits will be relocated. By default, the commits from the current branch that are not already on the other branch are rebased.

A common use for ```git rebase ``` is to keep a series of commits that you are developing up-to-date with respect to another branch, usually a master branch or a tracking branch from another repository.

You can keep your commit series upto date with respect to the master branch by writing the commits so that they are based on commit E rather than B. 

``` 
git checkout topic
git rebase master

```


### Remote Repositories

A clone is a copy of a repository. A clone contains all the objects from the original; as a result, each clone is an independent and autonomous repository and a true, symmetric peer of the original. A clone allows each developer to work locally and independently without centralization , polls or locks. Ultimately , it's cloning that allows Git to easily scale and permit many geographically seperated contributors.


You must also relate on repo to another to establish paths for data exchange. Git establishes these repository connections through remotes.


A remote is a reference, or handle to another repository through a filesystem or network path. You use a remote as a shorthand name for an otherwise lengthy and complicated git url.

### Tracking changes

Once you clone a repo, you can keep up with changes in the orginal source repository even as you make local changes and create local changes.

- Remote tracking branches are assosciated with a remote and have the specific purpose of following the changes of each branch in that remote repository.

- A local-tracking branch is paired with a remote-tracking branch. 


### Example using remote repositories

The workflow is pretty simple. Place an initial repo in the depot, clone development repositories out of the depot, do development work within them, and then sync them with the depot.

Individual work should be done in the local clone.

The authoritative upstream repository would likely already be hosted on some server.  


```
git clone http://url.git
```

If you initialize a repo using git init and not via a clone, it lacks an origin. Infact it has no remote configured at all.

It's needed if the goal is to perform more development in your initial repository and then push that development to the newly established , authoritative repository in the depot. A developer who  clones a repository from the depot will have an origin remote created automatically. In fact, you were to turn around now and clone off the depot, you would set it setup for you as well.

```
git remote add origin url

```

git remote added a new remote section called origin to your config. The name origin isn' magical or special. You could have used any other name, but the remote that points back to the basis repository is named origin by convention.

The remote establishes a link from your current repository to the remote repository found. 

Here are the comomands for setting up the origin remote by establishing new remote-tracking branches in the original repository to represent the branches from the remote repository.

```
git remote update
git branch -a
git push origin master

```



All the output means that Git has taken your master branch changes, bundled them up and sent them to the remote repository named origin. 

In the fetch step, git locates the remote repository. Because the command line did not specify a direct repository URL, or a direct remote name, it assumes the default remote name origin. 

Should you merge or rebase?

```
git pull --rebase

```

By using merge, you will potentially incur an additional merge commit at each pull to record the updated changes simultaneously present in each branch. 


### Creating Tracking Branches

```

git push branchname

```














 


























