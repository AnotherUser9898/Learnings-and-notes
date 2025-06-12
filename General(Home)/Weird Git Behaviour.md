#git

## Rebasing: `git pull --rebase`

When doing `git pull --rebase` git fetches the commits from the remote branch and applies your base branch on top of the fetched changes instead of doing the reverse.

So while doing conflict resolutions the incoming changes are from your base branch and the current changes are from the branch being merged in i.e. the remote branch.

> References: 
> - [Stack overflow post explaining the switch](https://stackoverflow.com/questions/2959443/why-is-the-meaning-of-ours-and-theirs-reversed-with-git-svn/2960751#2960751)
> - [Stack overflow post explaining conflict resolution in such scenarios](https://stackoverflow.com/questions/59607874/difference-between-accept-current-change-and-accept-incoming-change)
> - [Pro Git - Rebasing Section](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)


