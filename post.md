Using git (and GitHub) is an extremely common practice, and many of us never use more than the standard fare that git has to offer: fetch, pull, checkout, commit, push, merge. Its true power is often taken for granted, but is exposed in the most (or some not so) dire situations.

In this post, I would like to demonstrate some of Git's lesser known arsenal that have both enhanced my efficiency and protected my sanity. I am assuming the reader has a basic command of Git. If you need to brush up, [GitHub's Hello World](https://guides.github.com/activities/hello-world/) guide is a great refresher.

### Undo your last commit.

```
$ git commit ...                         (1)
$ git reset --soft HEAD~1                (2)
<< edit files as necessary >>            (3)
$ git add ....                           (4)
$ git commit --reedit-message ORIG_HEAD  (5)
```

#### Explanation

1. This is what you want to undo.
2. This is most often done when you remembered what you just committed is incomplete, or you misspelled your commit message1, or both. Leaves working tree as it was before "commit".
3. Make corrections to working tree files.
4. Stage changes for commit.
5. Commit the changes, editing the old commit message. `reset` copied the old head to `.git/ORIG_HEAD`; `commit` with `--reedit-message ORIG_HEAD` (or `-c ORIG_HEAD`) will open an editor, which initially contains the log message from the old commit and allows you to edit it. If you do not need to edit the message, you could use the `--reuse-message` (or `-C`) option instead.

[Originally answered on Stack Overflow.](http://stackoverflow.com/questions/927358/how-to-undo-the-last-commit)

### Squash a feature branch into a single commit.

I like to do this to have a nice, clean pull request as well as have the ability to easily roll back a larger change because it turns many commit objects into one. Imagine you have a feature branch named `chat-implementation` that is many commits ahead of master, many of them with useless commit messages (and often littered with swear words for some of us) that you want to turn into one nice commit object. Well, git will allow us to do just that. 

```
git checkout master
git pull
git checkout -b chat-implementation-pr
git merge --squash chat-implementation
git commit
```

#### Explanation

After creating a new branch from a fresh copy of master, you can merge all the forward changes in your original branch, `chat-implementation`, into the new branch, `chat-implementation-pr`. Once the squash occurs, the changes will be staged and ready to be committed.



### Use Git submodules for intelligent release management

Often times, when working on a larger project involving a suite of applications and/or services, there are known dependency issues that are version specific. After a couple releases, you end up with a giant, painful version matrix that represent what version(s) of which applications work with other version(s) of the other applications.

Well, now understanding and managing version interdependency is a task that can be done in code, using Git! Git submodules allow you to have context of separate Git repositories in a separate repo. Leveraging this functionality, we can create a "meta" repository that links to a specific tag (or release) of a child repository. Doing this, we can create a suite of tags that represents a known working set of applications.

Git Submodules deserve their own post, quite possibly their own book, so I'll just leave this link to an [Atlassian blog post that goes into great detail about Git Submodules](http://blogs.atlassian.com/2013/03/git-submodules-workflows-tips/).


## In Conclusion

Having more knowledge of Git's out of the box functionality can drastically improve your workflow, as well as save hours of headache and heartache. If there's any interest, I can turn this post into a series -- I am open to suggestion and inquiry for future topics! (Git rebase workflow, anyone?)