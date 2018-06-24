# 第一次互评作业

何国林 20180624

## 第一题：用系统功能调用实现简单输入输出

### 问题

利用系统功能调用从键盘输入，转换后在屏幕上显示，具体要求如下：

(1) 如果输入的是字母（A~Z，区分大小写）或数字（0~9），则将其转换成对应的英文单词后在屏幕上显示，对应关系见下表

(2) 若输入的不是字母或数字，则在屏幕上输出字符“*”，

(3) 每输入一个字符，即时转换并在屏幕上显示，

(4) 支持反复输入，直到按“?”键结束程序。

| **A** | Alpha   | **N** | November | **1** | First   | **a** | alpha   | **n** | november |
| ----- | ------- | ----- | -------- | ----- | ------- | ----- | ------- | ----- | -------- |
| **B** | Bravo   | **O** | Oscar    | **2** | Second  | **b** | bravo   | **o** | oscar    |
| **C** | China   | **P** | Paper    | **3** | Third   | **c** | china   | **p** | paper    |
| **D** | Delta   | **Q** | Quebec   | **4** | Fourth  | **d** | delta   | **q** | quebec   |
| **E** | Echo    | **R** | Research | **5** | Fifth   | **e** | echo    | **r** | research |
| **F** | Foxtrot | **S** | Sierra   | **6** | Sixth   | **f** | foxtrot | **s** | sierra   |
| **G** | Golf    | **T** | Tango    | **7** | Seventh | **g** | golf    | **t** | tango    |
| **H** | Hotel   | **U** | Uniform  | **8** | Eighth  | **h** | hotel   | **u** | uniform  |
| **I** | India   | **V** | Victor   | **9** | Ninth   | **i** | india   | **v** | victor   |
| **J** | Juliet  | **W** | Whisky   | **0** | zero    | **j** | juliet  | **w** | whisky   |
| **K** | Kilo    | **X** | X-ray    |       |         | **k** | kilo    | **x** | x-ray    |
| **L** | Lima    | **Y** | Yankee   |       |         | **l** | lima    | **y** | yankee   |
| **M** | Mary    | **Z** | Zulu     |       |         | **m** | mary    | **z** | zulu     |

### 题解

```assembly
.data
	cap_word: .ascii
            "Alpha \0\0\0\0",
            "Bravo \0\0\0\0",
            "China \0\0\0\0",
            "Delta \0\0\0\0",
            "Echo \0\0\0\0\0",
            "Foxtrot \0\0",
            "Golf \0\0\0\0\0",
            "Hotel \0\0\0\0",
            "India \0\0\0\0",
            "Juliet \0\0\0",
            "Kilo \0\0\0\0\0",
            "Lima \0\0\0\0\0",
            "Mary \0\0\0\0\0",
            "November \0",
            "Oscar \0\0\0\0",
            "Paper \0\0\0\0",
            "Quebec \0\0\0",
            "Research \0",
            "Sierra \0\0\0",
            "Tango \0\0\0\0",
            "Uniform \0\0",
            "Victor \0\0\0",
            "Whisky \0\0\0",
            "X-ray \0\0\0\0",
            "Yankee \0\0\0",
            "Zulu \0\0\0\0\0"

	number: .ascii
            "zero \0\0\0\0\0", 
            "First \0\0\0\0", 
            "Second \0\0\0", 
            "Third \0\0\0\0", 
            "Fourth \0\0\0",
            "Fifth \0\0\0\0", 
            "Sixth \0\0\0\0", 
            "Seventh \0\0",
            "Eighth \0\0\0",
            "Ninth \0\0\0\0"
            
	small_word: .ascii
            "alpha \0\0\0\0",
            "bravo \0\0\0\0",
            "china \0\0\0\0",
            "delta \0\0\0\0",
            "echo \0\0\0\0\0",
            "foxtrot \0\0",
            "golf \0\0\0\0\0",
            "hotel \0\0\0\0",
            "india \0\0\0\0",
            "juliet \0\0\0",
            "kilo \0\0\0\0\0",
            "lima \0\0\0\0\0",
            "mary \0\0\0\0\0",
            "november \0",
            "oscar \0\0\0\0",
            "paper \0\0\0\0",
            "quebec \0\0\0",
            "research \0",
            "sierra \0\0\0",
            "tango \0\0\0\0",
            "uniform \0\0",
            "victor \0\0\0",
            "whisky \0\0\0",
            "x-ray \0\0\0\0",
            "yankee \0\0\0",
            "zulu \0\0\0\0\0"

.text
	
	main:
		# read char
		li $v0, 12
		syscall
		move $t0, $v0 		# copy result to t0
		
		beq $t0, '?', exit 	# if '?' exit
		
		blt $t0, '0', isStar
		ble $t0, '9', isDigit
		blt $t0, 'A', isStar
		ble $t0, 'Z', isCapital
		blt $t0, 'a', isStar
		ble $t0, 'z', isSmall
		bgt $t0, 'z', isStar
		
	isStar:
		li  $t1, '*'
		
		# display char
		li $v0, 11
		move $a0, $t1
		syscall

		j main
	
	isDigit:
		li $t1, '0'
		sub $t1, $t0, $t1
		mul $t1, $t1, 10	# t1 is relative address

		la $a0, number
		add $a0, $t1, $a0
		
		# display digit string
		li $v0, 4
		syscall
	
		j main

	isCapital:
		li $t1, 'A'
		sub $t1, $t0, $t1
		mul $t1, $t1, 10	# t1 is relative address

		la $a0, cap_word
		add $a0, $t1, $a0
		
		# display Capital ascii string
		li $v0, 4
		syscall
		
		j main
		
	isSmall:
		li $t1, 'a'
		sub $t1, $t0, $t1
		mul $t1, $t1, 10	# t1 is relative address

		la $a0, small_word
		add $a0, $t1, $a0
		
		# display Small ascii string
		li $v0, 4
		syscall
		
		j main

	exit:
		li $v0, 10
		syscall
```

说明：

> - 开始想用.align指令对齐`ascii`，发现没有用，后来就手工敲上了'\0'，这一部分工作对高级语言来说应该是由编译器提供的特性；
> - 参考了youtube上的一个教程，讲的很有趣:[MIPS Assembly Programming Simplified](https://www.youtube.com/watch?v=u5Foo6mmW0I&list=PL5b07qlmA3P6zUdDf-o97ddfpvPFuNa5A)。

## 第二题：字符串查找比较

### 问题

利用系统功能调用从键盘输入一个字符串，然后输入单个字符，查找该字符串中是否有该字符（区分大小写）。具体要求如下：

(1) 如果找到，则在屏幕上显示：

Success! Location: X

其中，X为该字符在字符串中第一次出现的位置

(2) 如果没找到，则在屏幕上显示：

Fail!

(3) 输入一个字符串后，可以反复输入希望查询的字符，直到按“?”键结束程序

(4) 每个输入字符独占一行，输出查找结果独占一行，位置编码从1开始。

提示：为避免歧义，字符串内不包含"?"符号

格式示例如下：

abcdefgh

a

Success! Location: 1

x

Fail!

### 题解

```assembly
.data
	start_msg: .asciiz "Input a String no longer than 256:\n"
	search_for: .asciiz "Search for(one char, '?' excluded.): "
	success_msg: .asciiz "Success! Location: "
	fail_msg: .asciiz "Fail!\n"
	user_input_msg: .space 256
	search_msg: .space 32

.text

	# show start message
	li $v0, 4
	la $a0, start_msg
	syscall
		
	main:
		# get user's input message
		li $v0, 8
		la $a0, user_input_msg
		li $a1, 256
		syscall
		
		while:
			la $t0, user_input_msg
			lb $s0, ($t0)	# move input message start addr to $s0
		
			# show search message
			li $v0, 4
			la $a0, search_for
			syscall
		
			# get user's serch message
			li $v0, 8
			la $a0, search_msg
			li $a1, 32
			syscall
			
			lb $s1, ($a0)			# move first search char to $s1
			
			beq $s1, '?', exit 		# if '?' exit
			add $t2, $zero, 1
			
			loop:
				beq $s0, '\0', search_fail
				beq $s0, $s1, search_success
				addi $t0, $t0, 1
				addi $t2, $t2, 1
				lb $s0, ($t0)
				j loop
			
			search_success:
				# show search message
				li $v0, 4
				la $a0, success_msg
				syscall
				
				li $v0, 1
				add $a0, $zero, $t2
				syscall
				
				# '\n'
				li $v0, 11
				addi $a0, $zero,'\n'
				syscall
				
				j while
			
			search_fail:
				# show search message
				li $v0, 4
				la $a0, fail_msg
				syscall
		
				j while
	
	exit:
		li $v0, 10
		syscall
```