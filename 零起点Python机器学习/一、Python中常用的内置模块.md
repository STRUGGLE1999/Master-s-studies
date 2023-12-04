| 模块名 | 描述 |
| --- | --- |
| sys | 与Python解释器及其环境操作相关的标准库 |
| time | 提供与时间相关的各种函数的标准库 |
| os | 提供了访问操作系统服务功能的标准库 |
| calendar | 提供与日期相关的各种函数的标准库 |
| urllib | 用于读取来自网上（服务器）的数据标准库 |
| json | 用于使用JSON序列化和反序列化对象 |
| re | 用于在字符串中执行正则表达式匹配和替换 |
| math | 提供标准算术运算函数的标准库 |
| decimal | 用于进行精确控制运算精度、有效位数和四舍五入操作的十进制运算 |
| logging | 提供了灵活的记录事件、错误、警告和调试信息等日志信息的功能 |

# 二、第三方模块的安装及使用

- 第三方模块的安装

pip install 模块名

- 第三方模块的使用

install  模块名

## 遇到的问题
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636188468120-23ace984-7a9f-4efa-beb2-f063f820d29a.png#clientId=ud2f1516f-221c-4&from=paste&height=632&id=u9960c0e4&originHeight=1264&originWidth=1816&originalType=binary&ratio=1&rotation=0&showTitle=false&size=190352&status=done&style=none&taskId=u335e8d6a-c361-412c-9959-7010e3fae2f&title=&width=908)

## 总结
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636188590265-5f15965d-7190-4c17-9544-42c0bcf138bb.png#clientId=ud2f1516f-221c-4&from=paste&height=768&id=uaec4769c&originHeight=1536&originWidth=2048&originalType=binary&ratio=1&rotation=0&showTitle=false&size=689846&status=done&style=none&taskId=u4ea613ac-7744-4716-814b-cf451feb45a&title=&width=1024)

# 三、编码格式
常见的字符编码格式

- Python的解释器使用的是Unicode(内存)
- .py文件在磁盘上使用UTF-8存储（外存）

os模块
可以调用系统文件或者可执行文件，用于对目录或文件进行操作。
os.path模块操作目录相关函数

| 函数 | 说明 |
| --- | --- |
| abspath(path) | 用于获取文件或目录的绝对路径 |
| exists(path) | 用于判断文件或目录是否存在，如果存在返回True，否则返回FALSE |
| join(path,name) | 将目录与目录或者文件名拼接起来 |
| splitext() | 分离文件名和扩展民 |
| basename(path) | 从一个目录中提取文件名 |
| dirname(path) | 从一个路径中提取文件路径，不包括文件名 |
| isdir(path) | 用于判断是否为路径 |
|  |  |

# 项目打包
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1639311728274-65b73350-bb6a-4ed2-81e9-dcbb60810b69.png#clientId=ud3c2f8ae-5849-4&from=paste&height=768&id=u05bee8e5&originHeight=1536&originWidth=2048&originalType=binary&ratio=1&rotation=0&showTitle=false&size=742333&status=done&style=none&taskId=u5a5701a2-216f-4b79-a57f-0ad232499b1&title=&width=1024)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1649774671326-c74b9dc7-2b05-4990-839f-5811e9cd87a3.png#clientId=u821b2695-303c-4&from=paste&height=800&id=u62c52216&originHeight=1600&originWidth=2560&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1231758&status=done&style=none&taskId=u71063dab-4089-4eb4-86ef-89f7ad311ef&title=&width=1280)
