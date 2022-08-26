## OSX
安装`无法打开，因为 Apple 无法检查它是否包含恶意软件`：
```
  sudo spctl --master-disable
打开系统偏好设置，安全，通用，任何来源
	sudo spctl --master-enable

ld: not found -lstdc++
xcrun --show-sdk-path

export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/usr/local/opt/llvm/include/c++/v1:
```

```bash
/Library/Developer/CommandLineTools/SDKs/MacOSX10.15.sdk/usr/include

export LIBRARY_PATH=$LIBRARY_PATH:```bash
/Library/Developer/CommandLineTools/SDKs/MacOSX10.15.sdk/usr/lib
```

```

```

iterm2:
| 功能             | 快捷键                 |
| ---------------- | ---------------------- |
| 新建窗口         | CMD + N                |
| 关闭窗口         | CMD+W                  |
| 全屏             | CMD + ENTER            |
| 新建TAB          | CMD + T                |
| 关闭TAB页        | CMD+W                  |
| 切换TAB          | CMD+数字或者方向键     |
| 垂直分PANE       | CMD+D                  |
| 水平分PANE       | CMD + SHIFT + D        |
| 切换PANE         | CMD+OPT+方向键         |
| 历史剪贴板       | CMD+SHIFT+H            |
| 历史命令提示     | CMD + ;                |
| 上一条命令       | CTRL+P     或UP        |
| 下一条命令       | CTRL + N       或 DOWN |
| 搜索历史命令     | CTRL + R               |
| 即时回放         | CMD+OPT+B              |
| 清屏             | CMD + R                |
| 清行             | CTRL + U               |
| 删除当前光标字   | CTRL + D               |
| 删除光标前字     | CTRL + H               |
| 删除光标前的WORD | CTRL + W               |
| 删除到文本末尾   | CTRL + K               |
| 交换光标处文本   | CTRL + T               |
| 光标移到行首     | CTRL + A               |
| 光标移到行尾     | CTRL + E                       |


## Shell中的特殊字符
```
* : 匹配任意字符串
? : 匹配单个字符
[a|b|c]: 匹配任一字符
!: 不包含
``: 命令

; ：顺序执行
| ：管道操作
&&：前一命令执行成功再执行下一命令
|| ：前一命令执行不成功，则执行下一命令；如果前一命令执行成功，则不执行下一命令

&：后台操作


$0 : 当前脚本的文件名
$1,$2,$n ： 第几个参数
$# ： 传入脚本的参数的个数
$* ：所有位置的参数组成的单字符串
$@ ：所有位置参数组成的字符串组
$? ：上一命令的返回值。0-成功
$$ ：当前shell进程的pid
$! ：后台运行的最后一个进程的pid
$- ：shell使用的当前选项
$_ ：之前命令的最后一个参数

myvar = str
set myvar a
unset myvar
readonly myvar=xx

取子字符串：${myvar:0:5}
字符串长度：${#myvar}
替换：${myvar/abc/def} # abc替换成def

输入输出重定向：
	cmd < input-file
	cmd > output-file
	cmd >> output-file 附加
	cmd 2> error-file 错误消息重定向

read 读取
echo 显示

控制：
	if .. then ..
	elif .. then ..
	else ..
	fi
	
	测试条件可以写为：[ expression ] 或 test expression
	
	case .. in 
	a) cmd-list;;
	b) cmd-list;;
	...
	n) cmd-list;;
	esac
	
	for .. in ..
	do ..
	done
	
	while ..
	do ..
	done
	
	until ..
	do ..
	done
	
	myfunc()
	{
		..
		[return] [n]
	}
	
	脚本跟踪执行： sh -x 或者 set -x|+x -x表示启用，+x表示禁用
	
	包含脚本：. ./subscript.sh 或 source ./subscript.sh
```


查看编译器默认include地址：
`echo | clang -v -x c++ -E --`

Yahaha.app已损坏，建议移除到垃圾篓：
`sudo xattr -rc Yahaha.app`
`sudo xattr -rd compapple.quarantine Yahaha.app`


linux下文件的权限：
`-rwxrwxrwx` 第1位是文件类型，后三位代表User，再3位代表Group，继3位代表其他用户。7(0b111)=rwx,6(0b110)=rw,