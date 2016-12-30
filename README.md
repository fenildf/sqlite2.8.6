# sqlite2.8.6.tar.gz
下载网址:http://pkgs.fedoraproject.org/repo/pkgs/sqlite/sqlite-2.8.6.tar.gz/md5/3047e24370d7e49d7bfd9728addf76cf/sqlite-2.8.6.tar.gz

> 改建项目

把整个src拷贝过来,放在本项目的根路径下面

把tool下面的lemon.c跟lempar.c 也拷贝过来来

运行gcc -o lemon lemon.c来生成lemon可运行程序

再运行./lemon parse.y来生成parse.c跟parse.h文件

运行./configure 然后make,最后得到config.h跟opencodes.c跟opencodec.h还有 sqlite.h

然后统统拷贝过来。



> 用IDEA跑起来

 把所有代码扔到项目底下，增加一个cmake文件如下：

```
cmake_minimum_required(VERSION 3.5)
project(sqlite2)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -DSQLITE_CORE=1")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}) #like gcc: -I
file(GLOB sources "*.c")
file(GLOB headers "*.h")
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/lempar.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/lemon.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/test1.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/test2.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/test3.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/md5.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/tclsqlite.c)

add_executable(sqlite2 ${sources} ${headers})
```

编译的时候，遇到两个错误。
1）编译的时候，有TCL的错：
 
解决方法：把tclsqlite.c删除即可。tcl脚本功能就费掉了。。


2）编译的时候，有错如下：  

```
fts1 has a design flaw and has been deprecated.
```

解析：**FTS1**和FTS2都有设计的缺陷，现在已经被废弃，目前已经提供了FTS3或者FTS4，这些作为全文搜索的模块，弥补了以前的**FTS1**的不足。***_如果确定不会使用到全文搜索_***，可以直接使用SQLITE_CORE，禁用。将SQLITE_CORE添加到编译选项。

解决方法：在CMakeLists.txt加上这句话-DSQLITE_CORE=1即可：

```
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -DSQLITE_CORE=1")
```


最后成功跑起来。。
运行的时候,注意在Clion的Program Arguments 上加上 参数: /Users/baidu/test.db

* * *
ends;


parse.c 跟 parse.h的代码 是使用了LEMON程序根据parse.y生成的。


简单的运行测试:
sqlite>
create table film(title, length, year, starring);
sqlite>
insert into film values ('Silence of the Lambs, The', 118, 1991, 'Jodie Foster');
sqlite>
select * from film;


相比较3.7的版本的代码,【14w代码量】
sqlite2.8.6的代码量才5w不到。还是从这个sqlite2.8.6入手来学习吧。