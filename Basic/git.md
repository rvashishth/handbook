- [how to merge history from different remote origin](#how-to-merge-history-from-different-remote-origin)
- [Use vscode as default git diff tool](#use-vscode-as-default-git-diff-tool)

### how to merge history from different remote origin

1. add new remote origin for </br>
   `$ git remote add link-origin https://github.optum.com/rvashish/apache-pulsar-chart.git`

2. change upstream for your branch </br>
   `$ git branch master -u link-origin/master`

3. Git pull from pulsar remote origin </br>
   `$ git pull --allow-unrelated-histories`

4. Resolve merge conflict if any

5. Change upstream of your branch to previous origin

6. Push merged changes back in link branch

7. Move a repo between organization with history
   1. clone original repo
      `https://github.com/openmessaging/openmessaging-benchmark.git`
   2. create new repo 
      `https://github.optum.com/link/openmessaging-benchmark.git`
   3. add origin  of new repo  in original repo's clone
      `git remote add linkorigin https://github.optum.com/link/openmessaging-benchmark.git`
   4. push master branch on new origin
      `git push -u linkorigin master`


### Use vscode as default git diff tool

I needed to compare files across two entirely separate git repositories and in the process found a tip for using VS Code as your default git compare tool.

Find your .gitconfig file and add these lines to it:
```
[diff]
    tool = vscode
[difftool "vscode"]
    cmd = code --wait --diff $LOCAL $REMOTE
```

Then, you can use git diff from the command line and it will launch the comparison in vscode.
 
In my case I wanted to compare across repos, so I added a remote and then did the comparison.

```
cd \path\to\repo
git remote add other_repo https://github.optum.com/whatever.git
git fetch other_repo
git diff other_repo/branch_on_other_repo // compares entire branch
git difftool other_repo/branch_on_other_repo -- path/file.txt // compares single file that exists in both
git difftool other_repo/branch_on_other_repo -- path/file.txt -- path2/file2.txt // compares specific files from both branches.
// and of course, other_repo/branch can just be any old branch in the current repo.
```