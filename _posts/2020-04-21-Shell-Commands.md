---
title: Shell commands
date: 2020-04-21 15:57:00
tags: [Shell]
music-id: 1039895
youtube-video-ID: 
bilibili-video-ID: 
mathjax: true

---

# [Shell commands](https://en.wikipedia.org/wiki/Shell_(computing))

[EN](#EN)|[CN](#CN)

<span id="EN"> 

In computing, a shell is a user interface for access to an operating system's services. In general, operating system shells use either a command-line interface (CLI) or graphical user interface (GUI), depending on a computer's role and particular operation. It is named a shell because it is the outermost layer around the operating system.

Command-line shells require the user to be familiar with commands and their calling syntax, and to understand concepts about the shell-specific scripting language (for example bash).

Graphical shells place a low burden on beginning computer users, and are characterized as being easy to use. Since they also come with certain disadvantages, most GUI-enabled operating systems also provide CLI shells.

## [Leetcode 192th word-frequency](https://leetcode-cn.com/problems/word-frequency)

```
Write a bash script to calculate the frequency of each word in a text file words.txt.

For simplicity sake, you may assume:

words.txt contains only lowercase characters and space ' ' characters.
Each word must consist of lowercase characters only.
Words are separated by one or more whitespace characters.
Example:

Assume that words.txt has the following content:

the day is sunny the the
the sunny is is
Your script should output the following, sorted by descending frequency:

the 4
is 3
sunny 2
day 1
Note:

Don't worry about handling ties, it is guaranteed that each word's frequency count is unique.
Could you write it in one-line using Unix pipes?

```

**Answer**

```bash
cat words.txt | xargs -n 1 | sort | uniq -c | sort -nr | awk '{print $2" "$1}'
```

### cat

**NAME**
     cat -- concatenate and print files

**SYNOPSIS**
     cat [-benstuv] [file ...]

****DESCRIPTION**
     The cat utility reads files sequentially, writing them to the standard
     output.  The file operands are processed in command-line order.  If file
     is a single dash ('-') or absent, cat reads from the standard input.  If
     file is a UNIX domain socket, cat connects to it and then reads it until
     EOF.  This complements the UNIX domain binding capability available in
     inetd(8).

### xargs

**NAME**
     xargs -- construct argument list(s) and execute utility

**SYNOPSIS**
     xargs [-0opt] [-E eofstr] [-I replstr [-R replacements]] [-J replstr]
           [-L number] [-n number [-x]] [-P maxprocs] [-s size]
           [utility [argument ...]]

**DESCRIPTION** 
     The xargs utility reads space, tab, newline and end-of-file delimited
     strings from the standard input and executes utility with the strings as
     arguments.  
     Any arguments specified on the command line are given to utility upon
     each invocation, followed by some number of the arguments read from the
     standard input of xargs.  The utility is repeatedly executed until stan-
     dard input is exhausted.  
     Spaces, tabs and newlines may be embedded in arguments using single
     (\`\` ' '') or double (\`\`"'') quotes or backslashes (\`\`\'').  Single quotes escape all non-single quote characters, excluding newlines, up to the
     matching single quote.  Double quotes escape all non-double quote charac-
     ters, excluding newlines, up to the matching double quote.  Any single
     character, including newlines, may be escaped by a backslash.

### uniq

**NAME**
     uniq -- report or filter out repeated lines in a file

**SYNOPSIS**
     uniq [-c | -d | -u] [-i] [-f num] [-s chars] [input_file [output_file]]

**DESCRIPTION**
     The uniq utility reads the specified input_file comparing adjacent lines,
     and writes a copy of each unique input line to the output_file.  If
     input_file is a single dash (\`-') or absent, the standard input is read.
     If output_file is absent, standard output is used for output.  The second
     and succeeding copies of identical adjacent input lines are not written.
     Repeated lines in the input will not be detected if they are not adja-
     cent, so it may be necessary to sort the files first.

     The following options are available:

     -c      Precede each output line with the count of the number of times
             the line occurred in the input, followed by a single space.

### sort

**NAME**
     sort -- sort or merge records (lines) of text and binary files

**SYNOPSIS**
     sort [-bcCdfghiRMmnrsuVz] [-k field1[,field2]] [-S memsize] [-T dir]
          [-t char] [-o output] [file ...]
     sort --help
     sort --version

**DESCRIPTION**
     The sort utility sorts text and binary files by lines.  A line is a
     record separated from the subsequent record by a newline (default) or NUL
     '\0' character (-z option).  A record can contain any printable or
     unprintable characters.  Comparisons are based on one or more sort keys
     extracted from each line of input, and are performed lexicographically,
     according to the current locale's collating rules and the specified com-
     mand-line options that can tune the actual sorting behavior.  By default,
     if keys are not given, sort uses entire lines for comparison.
     -n, --numeric-sort, --sort=numeric
             Sort fields numerically by arithmetic value.  Fields are supposed
             to have optional blanks in the beginning, an optional minus sign,
             zero or more digits (including decimal point and possible thou-
             sand separators).

### awk

**NAME**
       awk - pattern-directed scanning and processing language

**SYNOPSIS**
       awk [ -F fs ] [ -v var=value ] [ 'prog' | -f progfile ] [ file ...  ]

**DESCRIPTION**
       Awk scans each input file for lines that match any of a set of patterns
       specified literally in prog or in one or more  files  specified  as  -f
       progfile.   With  each  pattern  there can be an associated action that
       will be performed when a line of a file matches the pattern.  Each line
       is  matched  against the pattern portion of every pattern-action state-
       ment; the associated action is performed for each matched pattern.  The
       file  name  - means the standard input.  Any file of the form var=value
       is treated as an assignment, not a filename, and  is  executed  at  the
       time  it  would  have been opened if it were a filename.  The option -v
       followed by var=value is an assignment to be done before prog  is  exe-
       cuted;  any  number  of  -v  options  may be present.  The -F fs option
       defines the input field separator to be the regular expression fs.
       An input line is normally made up of fields separated by  white  space,
       or by regular expression FS.  The fields are denoted $1, $2, ..., while
       $0 refers to the entire line.  If FS is null, the input line  is  split
       into one field per character.

       A pattern-action statement has the form

              pattern { action }

       A  missing  {  action  } means print the line; a missing pattern always
       matches.  Pattern-action statements are separated by newlines or  semi-
       colons.

<span id="CN"> 

# [Shell](https://baike.baidu.com/item/shell/99702?fr=aladdin)

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。
Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。
例如: bash / sh / ksh / csh / zsh（Unix/linux 系统）

## [Leetcode 192 统计词频](https://leetcode-cn.com/problems/word-frequency)

```
写一个 bash 脚本以统计一个文本文件 words.txt 中每个单词出现的频率。

为了简单起见，你可以假设：

words.txt只包括小写字母和 ' ' 。
每个单词只由小写字母组成。
单词间由一个或多个空格字符分隔。
示例:

假设 words.txt 内容如下：

the day is sunny the the
the sunny is is
你的脚本应当输出（以词频降序排列）：

the 4
is 3
sunny 2
day 1
说明:

不要担心词频相同的单词的排序问题，每个单词出现的频率都是唯一的。
你可以使用一行 Unix pipes 实现吗？

```

解:

```bash
cat words.txt | xargs -n 1 | sort | uniq -c | sort -nr | awk '{print $2" "$1}'
```

1. 把文件words.txt输入给标准输出流,
2. 然后以读取命令行的方式从标准输出流里把里面的单词分开安一定格式输出
3. 排序
4. 获取不重复的单词,并在每行前加上它出现的次数
5. 根据数字大小进行逆序排序(从大到小排)
6. 把输出流里的字符串按照先输出单词再输出出现次数的格式打印出来




