## git 操作

### Part 1
1. 安装: sudo apt-get install git
2. 设置全局: 
    
    git config --global user.name 'name'

    git config --global user.email 'email'

### Part 2

1. 选择一个文件夹，初始化空仓库: git init
2. 新建文件或者修改文件之后，将文件添加到版本库: git add 文件名(或多个文件名)
3. 提交并给出说明: git commit -m "简要说明"

### Part 3
1. 查看当前状态,告诉我们将要被提交的修改: git status
2. 想要查看文件具体被修改的内容的话: git diff filename

### Part 4
1. 查看版本的历史记录: git log (--pretty=online)
2. 回退到上一个版本: git reset --hard HEAD^ , 此时退出的那个版本在git log中会消失, git reset --hard 1094a(commit id 写前几位就可以了) 可以让你回到之前那个退出来的版本
3. 回到历史之后，不小心关掉的bash，又想回到未来，但此时commit id找不到了，可以使用git reflog，其记录了历史的操作
4. 如果工作区某个文件修改后想要放弃修改，使用 git checkout -- filename，会丢弃这次修改回到版本库最近的状态。git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

### Part 5
1. 删除某个文件：git rm filename; git commit -m 'del something'
2. 还原误删除: git checkout -- filename

### Part 6
1. 关联远程仓库：git remote add origin git@github.com:dtrimina/repository_name.git
2. 远程仓库为空时，将本地库的内容推送到远程加-u：git push -u origin master
3. 以后使用git push origin master就可以了

### Part 7
克隆远程仓库：git clone git@github.com:dtrimina/repository_name.git