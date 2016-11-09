# Git - 关于git使用中的一些技巧和心得

## ```git tag```
简单来说就是对commit的重命名,本地创建的tag不会直接推送至远端,需要显示指明.创建的时候最好使用带有注释的tag,方便管理

## ```git rebase <branch_name>```
当你切出新分支修复bug或写新功能时,主分支又更新了,你想基于主分支的更新继续修改或创建,类似如下图的变化
```
A -> B -> D          A -> B -> D
    \         ===>              \
     C -> E                      C -> E
```

## ```git push origin commit-id:master```
push指定的commit到master分支