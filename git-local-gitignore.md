### .gitignore 文件
没有被git纳入管理的文件/文件夹，并且不期望其出现在未追踪文件列表中，可以通过.gitignore 文件，配置可以忽略的文件名称模式，git将根据配置的模式，忽略匹配的文件。

#### glob模式
```
"*"：星号匹配零个或多个任意字符
[]：匹配任何一个列在方括号中的字符，如[ab]匹配a或者匹配b
"?"：问号匹配一个任意字符
[n-m]：匹配所有在这两个字符范围内的字符，如[0-9]表示匹配所有0到9的数字
```
####  格式规范
- 行注释符号 #, 以#符号开头的行 或者 空行， git不解析
- 可以使用标准的 glob 模式匹配
- 匹配模式最后跟斜杠(/)说明要忽略的是目录
- 要忽略指定模式以外的文件或目录，可以在模式前加上感叹号(!)进行取反

#### 示例
```
logs/：忽略当前路径下的logs目录，包含logs下的所有子目录和文件

/logs.txt：忽略根目录下的logs.txt文件

*.class：忽略所有后缀为.class的文件

!/classes/a.class：不忽略classes目录下的a.class文件

tmp/*.txt：只忽略tmp目录下的.txt文件

**/foo：可以忽略/foo, a/foo, a/b/foo等

```
 #### 全局 .gitignore 文件配置
 ```
$ git config --global core.excludesfile ~/.gitignore
 ```
#### .gitignore 文件失效
<B> .gitignore只能忽略没有被track的文件，如果已经被纳入了版本管理中，则修改.gitignore是无效的</B>
