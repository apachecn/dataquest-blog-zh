# Python 子过程:简单的初学者教程(2022)

> 原文：<https://www.dataquest.io/blog/python-subprocess/>

September 6, 2022![Python Subprocesses](img/a21b3a26a30ea1fed2361eaab0bbd04d.png)

简单来说，计算机中发生的一切都是一个过程。当您打开应用程序或运行命令行命令或 Python 脚本时，您正在启动一个新的进程。从打开菜单栏到启动一个复杂的应用程序，一切都是在你的机器上运行的进程。

例如，当您从命令行调用 Python 脚本时，一个进程正在创建另一个进程。这两个进程现在有了相互关系:创建另一个进程的进程是父进程，而创建的进程是子进程。

同样，Python 脚本也可以启动一个新进程，然后它将成为这个新进程的父进程。在本文中，我们将看到如何使用 python 中的`subprocess`模块在常规 python 脚本的过程中运行不同的子流程。

虽然这不是一篇高级文章，但是为了理解示例和概念，Python 的基础知识可能是必要的。

## `subprocess`模块

`subprocess`是一个标准的 Python 模块，旨在从 Python 脚本中启动新的进程。它非常有用，事实上，当您需要并行运行多个进程或者从 Python 代码内部调用外部程序或外部命令时，它是推荐的选项。

`subprocess`模块的优点之一是它允许用户管理输入、输出，甚至是 Python 代码中的子进程产生的错误。这种可能性使得调用子流程更加强大和灵活——例如，它允许在 Python 脚本的其余部分将子流程的输出作为变量使用。

该模块最初是在 Python 2.4 中实现的，目标是作为其他函数的替代，比如`os.system`。另外，从 Python 3.5 开始，这个模块的推荐用法是通过`run()`函数，这将是本文的重点。

## 使用 subprocess.run()

Python 中的`subprocess`模块的`run`函数是在后台运行命令的好方法，不用担心打开新的终端或手动运行命令。该功能对于自动化任务或运行不想手动运行的命令也非常有用。

该函数接收的主参数是`*args`，它是一个 iterable，包含运行子流程的指令。为了执行另一个 Python 代码，`args`应该包含 Python 可执行文件和 Python 脚本的路径。

所以它看起来像这样:

```py
import subprocess

subprocess.run(["python", "my_script.py"])
```

也可以将 Python 代码直接写到函数中，而不是传递一个`.py`文件。下面是运行此类子流程的一个示例:

```py
result = subprocess.run(["/usr/local/bin/python", "-c", "print('This is a subprocess')"])
```

在`args`内部，我们有以下内容:

*   `"/usr/local/bin/python"`:本地 Python 可执行文件的路径。
*   `"-c"` :Python 标签，允许用户将 Python 代码作为文本写入命令行。
*   `"print('This is a subprocess')"`:实际要运行的代码。

其实这和把`/usr/local/bin/python -c print('This is a subprocess')`传到命令行是一样的。本文中的大部分代码将采用这种格式，因为这样更容易展示`run`函数的特性。但是，您总是可以调用包含相同代码的另一个 Python 脚本。

同样，在上面的例子中，我们两次都使用了`run`函数，`args`中的第一个字符串指的是 Python 可执行文件的路径。第一个例子使用了一个更通用的路径，我们将在整篇文章中保持这样。

然而，如果您在机器上找不到 Python 可执行文件的路径，您可以让`sys`模块帮您找到。这个模块与`subprocess`交互得很好，一个很好的用例是替换可执行文件的路径，如下所示:

```py
import sys

result = subprocess.run([sys.executable, "-c", "print('This is a subprocess')"])
```

run 函数然后返回一个`CompletedProcess`类的对象，它代表一个完成的流程。

如果我们打印`result`，我们会得到这样的结果:

```py
CompletedProcess(args=['/usr/bin/python3', '-c', "print('This is a subprocess')"], returncode=0)
```

正如我们所期望的，它是显示命令的`CompletedProcess`类的实例，而`returncode=0`表明它运行成功。

## 更多参数

既然我们已经了解了如何使用`run`函数运行子流程，我们应该探索能够更好地利用该函数的选项。

### 控制输出

注意，上面代码返回的对象只显示了命令和返回代码，但是没有关于子流程的其他信息。然而，如果我们将`capture_output`参数设置为`True`，它会返回更多的信息，让用户对他们的代码有更多的控制。

所以如果我们运行这个。。。

```py
result = subprocess.run(["python", "-c", "print('This is a subprocess')"], capture_output=True)
```

我们会得到这个。。。

```py
CompletedProcess(args=['/usr/bin/python3', '-c', "print('This is a subprocess')"], returncode=0, stdout=b'This is a subprocess\n', stderr=b'')
```

现在我们在返回的对象中也有了`stdout`和`stderr`。如果我们将它们都打印出来，我们会得到以下结果:

```py
print(result.stdout)
print(result.stderr)
```

```py
b'This is a subprocess\n'
b''
```

它们都是表示子流程输出的字节序列。但是，我们也可以将文本参数设置为`True`，然后将这些输出作为字符串。

但是，如果脚本中有错误，`stdout`将为空，`stderr`将包含错误消息，如下所示:

```py
result = subprocess.run(["python", "-c", "print(subprocess)"], capture_output=True, text=True)
print('output: ', result.stdout)
print('error: ', result.stderr)
```

```py
output:  
error:  Traceback (most recent call last):   File "<string>", line 1, in <module> NameError: name 'subprocess' is not defined
```

因此，这使得您可以根据子流程的输出来调整代码的其余部分，在整个代码中使用该输出作为变量，甚至可以跟踪子流程并存储它们(如果它们都运行无误)及其输出。

## 引发错误

尽管我们能够在上面的代码中生成一条错误消息，但重要的是要注意它没有停止父进程，这意味着即使子进程中发生了错误，代码仍然会运行。

如果我们想让代码在子流程出错时继续执行，我们可以使用`run`函数的`check`参数。通过将该参数设置为`True`，子进程中的任何错误都将在父进程中引发，并导致整个代码停止。

下面，我们使用与上一节相同的例子，但是使用了`check=True`:

```py
result = subprocess.run(["pyhon", "-c", "print(subprocess)"], capture_output=True, text=True, check=True)
print('output: ', result.stdout)
print('error: ', result.stderr)
```

```py
---------------------------------------------------------------------------
CalledProcessError                        Traceback (most recent call last)
<ipython-input-7-5b9e69e8fef6> in <module>()
----> 1 result = subprocess.run(["python", "-c", "print(subprocess)"], capture_output=True, text=True, check=True)
      2 print('output: ', result.stdout)
      3 print('error: ', result.stderr)
/usr/lib/python3.7/subprocess.py in run(input, capture_output, timeout, check, *popenargs, **kwargs)
    510         if check and retcode:
    511             raise CalledProcessError(retcode, process.args,
--> 512                                      output=stdout, stderr=stderr)
    513     return CompletedProcess(process.args, retcode, stdout, stderr)
    514
CalledProcessError: Command '['python', '-c', 'print(subprocess)']' returned non-zero exit status 1.
```

代码引发了一个错误，指出子流程返回了状态 1。

引发错误的另一种方式是使用`timeout`参数。顾名思义，使用该参数将停止子进程，如果运行时间比预期的长，将引发错误。

例如，下面子流程中的代码需要五秒钟来运行:

```py
subprocess.run(["python", "-c", "import time; time.sleep(5)"], capture_output=True, text=True)
```

但是如果我们将`timeout`参数设置为小于 5，我们会有一个异常:

```py
subprocess.run(["python", "-c", "import time; time.sleep(5)"], capture_output=True, text=True, timeout=2)
```

```py
---------------------------------------------------------------------------
TimeoutExpired                            Traceback (most recent call last)
<ipython-input-9-93e2b7acaf10> in <module>()
----> 1 subprocess.run(["python", "-c", "import time; time.sleep(5)"], capture_output=True, text=True, timeout=2)
/usr/lib/python3.7/subprocess.py in _check_timeout(self, endtime, orig_timeout, stdout_seq, stderr_seq, skip_check_and_raise)
   1009                     self.args, orig_timeout,
   1010                     output=b''.join(stdout_seq) if stdout_seq else None,
-> 1011                     stderr=b''.join(stderr_seq) if stderr_seq else None)
   1012 
   1013
TimeoutExpired: Command '['python', '-c', 'import time; time.sleep(5)']' timed out after 2 seconds
```

## 在子流程中输入

我们前面看到，在父代码的其余部分使用子进程的输出是可能的，但是反过来也是正确的:我们可以使用`input`参数将父进程的值输入到子进程。

我们使用该参数将任何字节或字符串序列(如果是`text=True`)发送给子流程，子流程将通过`sys`模块接收该信息。`sys.stdin.read()`函数将读取子进程中的`input`参数，在子进程中，它可以被赋给一个变量，并像代码中的任何其他变量一样使用。

这里有一个例子:

```py
result = subprocess.run(["python", "-c", "import sys; my_input=sys.stdin.read(); print(my_input)"], capture_output=True, text=True, input='my_text')
print(result.stdout)
```

```py
my_text
```

上面子流程中的代码导入模块，并使用`sys.stdin.read()`将输入分配给一个变量，然后打印该变量。

但是，我们也可以通过`args`输入值，并使用`sys.argv`在子代码内部读取它们。例如，假设我们在一个名为`my_script.py`的脚本中有以下代码:

```py
# ../subprocess/my_script.py 

import sys

my_input = sys.argv

def sum_two_values(a=int(my_input[1]), b=int(my_input[2])):
    return a + b

if __name__=="__main__":
    print(sum_two_values())
```

下面的`run`函数中的`args`位于将成为上面的子进程的父进程的脚本中，它包含另外两个值，这两个值将由`sys.argv`访问，然后在`my_script.py`中累加。

```py
# ../subprocess/main.py 

result = subprocess.run(["python", "my_script.py", "2", "4"], capture_output=True, text=True)
print(result.stdout)
```

```py
6
```

## 结论

Python 子流程模块和 subprocess.run()函数是非常强大的工具，用于在运行 Python 脚本时与其他流程进行交互。

在本文中，我们介绍了以下内容:

*   流程和子流程

*   如何使用`run()`功能运行子流程

*   如何访问子流程生成的输出和错误

*   如果子进程中有错误，如何在父进程中引发错误

*   如何为子进程设置超时

*   如何向子流程发送输入