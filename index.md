# Git: Making Great History

* Revision history in general is for a future person; could be you, could be someone else
* git blame is important
* git history should look sensible
* A commit is a body of work
* Ideally a branch should squash to a single commit. In reality, we are not precise enough and/or have too much tech debt for this to be real
* Cover the case of formatting/whitespace/cleanup: make whitespace change as one commit in beginning of branch, then do your work as separate commit after
* Cover the case of adding records to a test data file
* Cover the case of changing a template and then applying the template: one commit for the template change, one commit for applying the template
* Cover the case of commenting out blocks unused code; remove them
* Cover the case of splitting a commit into multiple commits with different files
* Cover the case of splitting a commit into multiple commits with 1 file; interactive staging
* Cover the case of refactoring. Break stuff out into function in one commit, then alter the function in a follow up commit
* Code reviews should look at all commits, that's why it should be broken up
* A fast forward merge is guaranteed to work, make your work fast-forwardable