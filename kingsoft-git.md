# Git

- 一个分布式版本控制系统
- 最初由Linus Torvalds在2005年设计开发


- Centralized Version Control Systems

![Centralized Version Control Systems](huawei-basic-tools/centralized-version-control-systems.png)


- Distributed Version Control Systems

![Distributed Version Control Systems](huawei-basic-tools/distributed-version-control-systems.png)



## Why Git

- 可以本地操作
- 快速
- 小步提交的绝配
- 备份
- 轻量级的分支



## Basics

- `$ git init`
- `$ git clone`
- `$ git add`
- `$ git status`
- `$ git commit`
- `$ git log`
- `$ git log --oneline --decorate --color --graph`
- `$ git help add`


- 三种状态
 - 已提交（committed）
 - 已修改（modified）
 - 已暂存（staged）


![areas](huawei-basic-tools/areas.png)



## branch

![basic-branching-6](huawei-basic-tools/basic-branching-6.png)


- `$ git branch iss53`
- `$ git checkout iss53`
- `$ git branch`

Note: 如何创建一个分支



## merge

![basic-merging-2](huawei-basic-tools/basic-merging-2.png)


- `$ git checkout master`
- `$ git merge iss53`


- `$ git branch -d iss53`


### Exercise

#### at the beginning

```javascript
removeData: function( elem, name ) {
    dataUser.remove( elem, name );
},
```


#### on branch master

- rename removeData to deleteData

```javascript
- removeData: function( elem, name ) {
+ deleteData: function( elem, name ) {
      dataUser.remove( elem, name );
  },
```


#### on branch my-feature

- add try catch

```javascript
  removeData: function( elem, name ) {
-     dataUser.remove( elem, name );
+     try { dataUser.remove( elem, name ); }
+     catch ( e ) { console.log( e ); }
  },
```

- add parameter callback to removData

```javascript
- removeData: function( elem, name ) {
+ removeData: function( elem, name, callback ) {
      try { dataUser.remove( elem, name ); }
      catch ( e ) { console.log( e ); }
  },
```


#### finally

```javascript
deleteData: function( elem, name, callback ) {
    try { dataUser.remove( elem, name ); }
    catch ( e ) { console.log( e ); }
},

```


- `$ git clone git@github.com:macdao/training-kingsoft-git.git`
- `$ cd training-kingsoft-git`
- `$ git branch --all`
- `$ git merge origin/my-feature`
- fix conflicts
- `$ git add .`
- `$ git commit`



## rebase

![basic-rebase-1](huawei-testing-framework/basic-rebase-1.png)


![basic-rebase-2](huawei-testing-framework/basic-rebase-2.png)


![basic-rebase-3](huawei-testing-framework/basic-rebase-3.png)


![basic-rebase-4](huawei-testing-framework/basic-rebase-4.png)


### 好处

![interesting-rebase-1](huawei-testing-framework/interesting-rebase-1.png)


![interesting-rebase-5](huawei-testing-framework/interesting-rebase-5.png)


### 风险

> 一旦分支中的提交对象发布到公共仓库，就千万不要对该分支进行rebase操作。


### Exercise

- `$ git reset --hard origin/master`
- `$ git checkout my-feature`
- `$ git rebase master`
- fix conflicts
- `$ git add .`
- `$ git rebase --continue`
- `$ git checkout master`
- `$ git merge my-feature`



# 参考资料

- https://github.com/numbbbbb/progit-zh-pdf-epub-mobi

