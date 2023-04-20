---
title: Makefile的函数与内部变量
date: 2023-04-20 18:57:55
tags: [Makefile]
---

### MAKE 

```makefile
$(MAKE) -C subdir
```

这个命令的作用是在指定的子目录 "subdir" 中执行 make 命令。具体解释如下：

- MAKE：make 命令的内置变量，指向当前正在执行的 make 命令。
- -C：指定要进入的子目录。
- subdir：需要进入的子目录。

因此，该命令将进入 "subdir" 子目录，并在该目录下执行 make 命令。在执行该命令之前，Makefile 文件会被自动递归执行以确保所有依赖关系已满足。

### SHELL 与 MAKEFLAGS

在Makefile中，`SHELL`变量指定了要使用的Shell解释器，例如：

```makefile
SHELL := /bin/bash
```

这里将使用bash作为Shell解释器。如果没有指定，则默认使用系统默认的Shell。

`MAKEFLAGS`是一个包含make命令行选项的字符串，由环境变量、命令行选项和makefile中的`.MFLAGS`变量构成。可以使用makefile中的`.MFLAGS`变量来设置初始的`MAKEFLAGS`值。在make过程中，MAKEFLAGS的值可能会被修改，例如在`-j`选项中指定并行度时，`MAKEFLAGS`会被修改以包含`-j`选项的值。

下面是一些常见的MAKEFLAGS选项：

- `-j N`: 指定并行编译的进程数为N。
- `-k`或`--keep-going`: 让make在编译过程中遇到错误时继续执行下去。
- `-s`或`--silent`或`--quiet`: 让make在执行命令时不输出命令信息。
- `-w`或`--print-directory`: 让make在执行命令时输出当前的工作目录信息。
- `-r`或`--no-builtin-rules`: 禁止make使用内置的规则和隐含规则。
- `-R`或`--no-builtin-variables`: 禁止make使用内置的变量。

除了MAKEFLAGS，还有其他一些相关的环境变量：

- `MAKELEVEL`: 表示当前make执行的嵌套层数。
- `MAKECMDGOALS`: 表示make当前需要构建的目标列表。
- `MAKEFILE_LIST`: 表示makefile的路径列表，按照它们被包含的顺序排列。

了解和掌握MAKEFLAGS和其他相关的环境变量的使用，可以更好地控制make的行为，并对于一些特殊情况下的调试和排错也会有帮助。

### CURDIR

`CURDIR` 是 Makefile 中的一个内置变量，代表当前工作目录的路径。

当 Makefile 中的规则需要执行一个命令时，它会先切换到该规则指定的工作目录下，然后再执行该命令。例如：

```makefile
all:
	cd foo && make
```

在执行 `make all` 时，Makefile 会先切换到 `foo` 目录下，然后执行 `make` 命令。此时，`CURDIR` 就是当前目录的路径，也就是 `foo` 目录的路径。

`CURDIR` 可以在 Makefile 中被显式地使用，例如：

```makefile
$(warning Current directory is $(CURDIR))
```

该语句会输出当前工作目录的路径。

### origin

在 Makefile 中，origin 函数可以用来判断一个变量的定义方式。它有如下语法：

```makefile
$(origin variable)
```

其中，`variable` 是一个变量名。`$(origin variable)` 将会返回 `variable` 的定义方式，可能的返回值如下：

- `undefined`：变量没有被定义。
- `default`：变量的值是默认值。
- `environment`：变量的值来自于环境变量。
- `file`：变量的值来自于文件或命令行。
- `command line`：变量的值来自于命令行。
- `override`：变量的值是通过 override 操作符设置的。
- `automatic`：变量的值是通过自动化规则计算得到的。

下面是一个使用 origin 函数的例子：

```makefile
FOO = bar

all:
	@echo $(origin FOO) # 输出 default

override FOO = baz

all:
	@echo $(origin FOO) # 输出 override
```

在这个例子中，我们先定义了变量 FOO 并赋值为 bar，然后在 all 目标中使用 origin 函数输出 FOO 的定义方式。此时，FOO 的定义方式为 default。然后，我们使用 override 操作符重新给 FOO 赋值为 baz，并再次输出 FOO 的定义方式。此时，FOO 的定义方式为 override。

> 在 Makefile 中，以 `$` 开头的变量引用表示该变量的值。

### firstword

在 makefile 中，`firstword` 是一个函数，它会返回一个以空格为分隔符的字符串中的第一个单词。这个函数的语法是：

```makefile
$(firstword <text>)
```

其中 `<text>` 是一个以空格为分隔符的字符串。`firstword` 会返回 `<text>` 中的第一个单词。

例如，假设我们有一个变量 `FOO` 的值为 `"hello world"`，那么 `$(firstword $(FOO))` 的值就是 `"hello"`。

`firstword` 函数可以在需要提取一个字符串中的第一个单词的场合下使用。

### shell

在 makefile 中，shell 函数用于执行 shell 命令并返回其输出结果。它的语法如下：

```makefile
$(shell command)
```

其中，`command` 是要执行的 shell 命令，可以包含任意的 shell 语句和变量引用。

`shell` 函数的作用是将 `command` 命令的输出作为字符串返回。这个字符串可以被用于定义变量、进行比较、作为命令的参数等。

例如，下面的代码使用 shell 函数来获取当前的日期，并将其保存在一个名为 `DATE` 的变量中：

```makefile
DATE := $(shell date +%Y-%m-%d)
```

这个命令将执行 `date +%Y-%m-%d`，获取当前的日期，并将其保存在变量 `DATE` 中。

还有一种语法是使用反斜杠（\）将 shell 命令包装在一个单行文本中：

```makefile
VAR := $(shell \
	echo "some command" \
	&& echo "another command" \
)
```

这种语法可以使代码更易读，尤其是在需要执行多个命令时。

### PHONY

在 Makefile 中，PHONY 是一个特殊的目标，它表示一个伪目标，通常用来声明一些不是文件的目标，比如“clean”、“all”等。PHONY 也可以理解为“虚假的”或者“假的”。

PHONY 的作用是告诉 make，不管是否存在同名的文件，都应该执行该目标所定义的命令。因为 Makefile 是通过文件的时间戳来判断是否需要重新生成目标文件的，如果目标是一个伪目标，Makefile 无法通过文件时间戳来判断是否需要重新生成目标文件，所以需要通过 PHONY 来告诉 Makefile 该目标是一个伪目标，需要执行该目标所定义的命令。

一个 PHONY 的例子：

```makefile
.PHONY: clean
clean:
	rm -rf *.o
```

上面的例子中，“clean” 是一个 PHONY 目标，它的命令是删除所有的“.o”文件。当我们执行 “make clean” 命令时，不管是否存在“clean”文件，都会执行该目标所定义的命令。如果没有声明“clean”为 PHONY 目标，当存在“clean”文件时，make 将会判断该文件的时间戳是否比“*.o”文件的时间戳更新，如果没有更新，make 将不会执行任何命令，从而导致 clean 的目的无法达到。

### dir

在 makefile 中，`dir` 函数用于提取文件路径，其语法为 `$(dir names...)`，其中 `names` 表示要提取路径的文件名。如果 `names` 中的文件名包含路径，则 `dir` 函数将返回该路径；如果 `names` 中的文件名没有包含路径，则 `dir` 函数将返回当前目录的路径 `./`。

例如，在以下代码中：

```makefile
SRC := src/foo.c src/bar.c
DIR := $(dir $(SRC))
```

`$(dir $(SRC))` 将返回 `src/ src/`，即 `foo.c` 和 `bar.c` 所在的路径。注意，返回的路径最后带有斜杠 `/`。

### include

在 Makefile 中，include 命令用于在当前 Makefile 中引入其他的 Makefile。使用 include 命令可以将其他 Makefile 中的定义、规则和变量添加到当前 Makefile 中，从而避免将所有的规则和定义都写在一个文件中。

语法格式为：

```makefile
include <filename>
```

其中，`<filename>` 指定要引入的 Makefile 文件名。Make 会先在当前目录下查找该文件，如果找不到，则会按照环境变量 `VPATH` 中指定的路径查找。

在引入 Makefile 时，include 命令会读取指定的 Makefile 并将其中的规则和变量定义插入到当前 Makefile 中。这些规则和变量会覆盖当前 Makefile 中已有的同名规则和变量。在 Makefile 中，include 命令通常用于引入系统定义的变量和规则，或者将一些通用的规则和变量定义在一个文件中，以便在多个 Makefile 中共享使用。

### patsubst

`patsubst` 是 `make` 内置函数，用于将符合某个模式的字符串替换为另一个字符串，其语法如下：

```makefile
$(patsubst pattern,replacement,text)
```

其中 `pattern` 是匹配的模式，可以包含通配符 `%`，`replacement` 是替换的字符串，`text` 是待处理的字符串。如果 `text` 中存在符合 `pattern` 的子串，则将这些子串替换为 `replacement`，返回替换后的字符串。

例如，假设有一个变量 `SOURCES` 包含多个源文件的路径，如下：

```makefile
SOURCES := src/foo.c src/bar.c src/baz.c
```

可以使用 `patsubst` 函数将所有源文件的扩展名 `.c` 替换为 `.o`，并得到目标文件的路径，如下：

```makefile
OBJS := $(patsubst %.c,%.o,$(SOURCES))
```

这样得到的 `OBJS` 将包含以下内容：

```makefile
OBJS := src/foo.o src/bar.o src/baz.o
```

### wildcard

`wildcard` 是一个 GNU Make 内置的函数，用于获取指定目录下所有匹配某个模式的文件名列表。其语法格式如下：

```makefile
$(wildcard pattern)
```

其中 `pattern` 是一个模式字符串，支持通配符 `*` 和 `?`。调用该函数时，GNU Make 将会在当前工作目录下搜索所有匹配 `pattern` 的文件，并将它们的文件名返回为一个以空格分隔的列表。

例如，要获取当前目录下所有的 `.c` 和 `.h` 文件，可以这样写：

```makefile
SRCS := $(wildcard *.c) $(wildcard *.h)
```

这将会把所有匹配 `*.c` 和 `*.h` 的文件名存入变量 `SRCS` 中。

###  strip

在Makefile中，`strip`是一个函数，用于删除字符串两端的空格，以及将字符串中间的多个空格缩减为一个。它的语法为：

```makefile
$(strip string)
```

其中 `string` 表示要进行处理的字符串。`strip` 函数将会返回一个新的字符串，该字符串删除了 `string` 字符串中的前导空格、后续空格以及将多个空格压缩为一个空格。

### Makefile 自定义函数

自定义函数是在 Makefile 中定义的可以重复使用的一段代码。通常情况下，自定义函数使用 Makefile 中的变量来执行一些特定任务。

在 Makefile 中定义自定义函数的语法如下：

```makefile
define <function_name>
    <commands>
endef
```

其中，`<function_name>` 是函数名称，`<commands>` 是函数体，包含了一组操作命令。

例如，下面是一个简单的自定义函数 `hello`，该函数将接收一个参数 `name`，并在控制台输出 `"Hello, <name>!"` 的字符串。

```makefile
define hello
    echo "Hello, $(1)!"
endef
```

可以通过在 Makefile 中使用 `$()` 和 `$(call)` 函数调用自定义函数。

例如，下面的 Makefile 文件中，我们定义了 `hello` 函数，并在 `build` 目标中调用该函数，传入了一个参数 `world`。

```makefile
define hello
	echo "Hello, $(1)!"
endef

.PHONY: build
build:
	$(call hello,world)
```

执行 `make build` 命令将会输出 `"Hello, world!"` 的字符串。

### sort

在 Makefile 中，sort 函数用于对一组字符串进行排序。它的语法如下：

```makefile
$(sort <list>)
```

其中，list 是一组空格或制表符分隔的字符串。sort 函数将按照字典序对这些字符串进行排序，并返回排好序的字符串列表。重复的字符串将被删除。

下面是一个例子：

```makefile
unsorted := d e b c a a c b e d
sorted := $(sort $(unsorted))
```

这里，unsorted 是一个未排序的字符串列表，包含 10 个元素。sort 函数对 unsorted 进行排序，并返回排好序的列表 sorted，其内容为：

```makefile
a b c d e
```

需要注意的是，sort 函数返回的是一个列表，而不是一个字符串。因此，如果要将排序后的字符串列表作为一个参数传递给另一个函数，需要使用类似于 $(foreach) 函数的方式来处理。
