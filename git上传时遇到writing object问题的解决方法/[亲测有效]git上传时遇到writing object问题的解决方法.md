### git上传时遇到writing object问题的解决方法

在使用git的过程中，上传代码的时候很可能会遇到卡，push时会提示writing object的异常。那么这到底是怎么解决呢？

我在遇到这个问题之后，查阅资料以及搜索百度各大神的解决方法，结合使用之后将解决方法记录下来：

**出现这个问题的原因是传送了太大的文件。**

**1.找到git的.gitconfig文件，打开手动输入改变postBuffer的数值，一般百度上搜索到的都会让添加一行代码$ git config --global http.postBuffer 524288000。其实就是改变postBuffer的数值即可，当postBuffer=524288000时表示可上传500M的文件，但假如还不可以，直接在后面添加一个0，亲测有效。**

那么git的.gitconfig文件在哪里呢？

我安装git时一路下一步，如没改变路径就是在C盘。

![1](C:\Users\14665\source\JAVA学习\git上传时遇到writing object问题的解决方法\1.png)

点击进去修改

![2](C:\Users\14665\source\JAVA学习\git上传时遇到writing object问题的解决方法\2.png)

修改完之后push又出现了**git did not exit cleanly(exit code 1)**的异常提醒。

![3](C:\Users\14665\source\JAVA学习\git上传时遇到writing object问题的解决方法\3.png)

**此时我们可在git中新建立一个仓库，将其clone到本地，然后再将你要上传的文件按照git上传方式上传即可。**

![6](C:\Users\14665\source\JAVA学习\git上传时遇到writing object问题的解决方法\6.png)

然后上传即可成功。

![4](C:\Users\14665\source\JAVA学习\git上传时遇到writing object问题的解决方法\4.png)

此时查看git中新建立的仓库中，就已存在你要上传的文件。

![5](C:\Users\14665\source\JAVA学习\git上传时遇到writing object问题的解决方法\5.png)

#### 注意：若在上传过程中依旧出现writing object异常提醒，多试着push一两次即可。