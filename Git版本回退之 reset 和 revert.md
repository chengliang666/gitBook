# Git版本回退之 reset 和 revert

在开发过程中，可能会遇到过错误提交的情况。这种情况下，先不要着急，可以通过以下两个命令来帮助你优雅的实现版本回退。

## git reset

假如现在有如下几个提交：

![image-20210909145924975](https://img-blog.csdnimg.cn/9c9149f5772f484885dc3818b7331ddf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAY2hlbmdsaWFuZzY2Ng==,size_20,color_FFFFFF,t_70,g_se,x_16)

其中：A 和 B 是正常提交，而 C 和 D 是错误提交。现在想把 C 和 D 回退掉。而此时HEAD 指针指向 D 提交（5lk4er）。我们只需将 HEAD 指针移动到 B 提交（a0fvf8），即可。

这个时候就可以使用`git reset` 命令：

```
git reset --hard a0fvf8 // 将HEAD指针移动到B提交点
git push origin HEAD --force // 将提交强制推到远程仓库
```

此时HEAD指针就会移动到 B 提交下

![image-20210909151048438](https://img-blog.csdnimg.cn/7f2cee54276642e39eee05154a889201.png)

> 采用这种方式回退代码会使 HEAD 指针往回移动，从而会失去之后的提交信息且不可恢复，所以要慎重使用。

## git revert

git revert会创建一个新的版本，且HEAD指针会指向这个新生成的版本，原来错误提交信息也可以保留。

可以通过用`git revert` 命令逐个回退：

```
git revert 5lk4er
git revert 76sdeb
```

![image-20210909151934936](https://img-blog.csdnimg.cn/32a7d73f4ea7461e87faa5ed91ff6c71.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAY2hlbmdsaWFuZzY2Ng==,size_20,color_FFFFFF,t_70,g_se,x_16)

回退版本少的可以逐个回退，多的话就要批量回退了：

```
git revert OLDER_COMMIT^..NEWER_COMMIT
```

> 通过对比发现，git reset 会失去后面的提交，而 git revert 是通过反做的方式重新创建一个新的提交，而保留原有的提交。所以应尽量使用 git revert 命令来回退版本，慎重使用 git reset 命令。

## 补充

假如现在有三个提交，不巧的是那个错误的提交刚好位于中间

![image-20210909153952226](https://img-blog.csdnimg.cn/f7de9cd03e6f499cb3d08d5991917e47.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAY2hlbmdsaWFuZzY2Ng==,size_10,color_FFFFFF,t_70,g_se,x_16)

此时直接使用 git reset 命令将 HEAD 指针重置到 A 提交显然是不行的，因为 C 提交是正确的，需要保留的。

> 正确的做法：先把 C 和 B 提交全部回退，再使用 `cherry-pick` 命令将 C 提交重新再生成一个新的提交 C''，这样就实现了将 B提交回退的需求。

![image-20210909154254684](https://img-blog.csdnimg.cn/47ca0c473fe84004884a24d9ab71bb9d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAY2hlbmdsaWFuZzY2Ng==,size_20,color_FFFFFF,t_70,g_se,x_16)

