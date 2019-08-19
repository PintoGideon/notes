# Adding a file to your repo

git add index.html

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