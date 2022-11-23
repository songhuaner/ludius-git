
# git 底层命令

## git hash-object

### 作用:
<b>使用指定文件的内容（可以在工作树之外）计算具有指定类型的对象的对象ID值，并可选择将结果对象写入对象数据库。将其对象ID报告给其标准输出</b>

### 使用:
```
git hash-object [-t <type>] [-w] [--path=<file>|--no-filters] [--stdin [--literally]] [--] <file>…​
git hash-object [-t <type>] [-w] --stdin-paths [--no-filters]

```






