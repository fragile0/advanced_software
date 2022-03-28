## makefile

- gcc hello.c -o hello	生成hello.exe文件

- 微观c/c++编译执行过程

  - *源代码* 经过**编译**得到*汇编代码*，*汇编代码*经过**汇编**得到*机器代码*。

    通常，我们会把“预处理+编译+汇编+连接”简称为“编译+连接”，甚至直接称为“编译”。

  - -E**预处理**   .cpp和.h结合              

  - -S**编译**       生成.s文件   -S

  - -C**汇编**       生成.o文件  .obj文件

  - -O**链接**       生成.exe文件   .exe / .elf

    ```
    gcc -E hello.c -o hello.i	// 预处理
    gcc -S hello.i -o hello.s	// 编译
    gcc -C hello.s -o hello.o	// 汇编
    gcc - hello.o -o hello		// 生成可执行文件
    
    gcc a.c b.c c.c -o hello  	// 编译a.c b.c c.c生成hello 
    ```

  - g++ -g

    ```
    g++编译单文件并调试
    ```

- makefile脚本语言

  - 第一层

    - 创建 文本文档，取名Makefile

    - '#'是注释

    - 显示规则

      ```
      hello:hello.o		// 第一个是最终目标  递归
      	gcc hello.o -o hello
      hello.o:hello.S
      	gcc -c hello.s -o hello.o
      hello.s:helloi		// 目标文件：依赖文件
      	gcc -S hello.i -o hello.S	// [TAB]指令
      hello.i:helloc.c
      	gcc -E hello.c -o hello.i
      .PHONY
      clear:
      	rm -rf a  // 调用make clear使用该指令
      clearall:
      	rm -rf a b c d	// make clearall
      ```

  - 第二层

    ```
    = 替换 	+= 追加	:= 常量 恒等于
    $(变量名)	替换
    
    TAR = test
    OBJ = circle.o cube.o main.o
    CC := gcc
    
    $(TAR):$(OBJ)
    	$(CC) $(OBJ)   -o $(TAR)
    ```

  - 第三层

    ```
    隐含规则 
    	%.c,%.o任意的.c或者.o
    	*.c,*.o所有的.c或者.o
    %.o:%.c
    	$(CC) -c %.c -o %.o
    ```

  - 第四层

    ```
    $^ 所有的依赖文件
    $@ 所有的目标文件
    $< 所有的依赖文件的第一个文件
    
    $ (TAR): $(OBJ)
    	$(CC) $^ -O $@
    %.o:%.c
    	$(CC) -c $@ -o $^ 
    ```

- 基于cmake

  - 编写cmake文件

    ```(
    project(项目名字)
    add_executable(生成exe main.cpp swap.cpp)
    ```

  - cmake命令

    ```
    mkdir build
    cd build
    cmake ..	// 生成makefile
    make		// 构建项目
    ```

    





