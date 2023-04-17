---
layout: single
title: 리눅스 프로세스 명령어(linux process command) 2. filtering
categories: Linux
---

# 1. I/O redirection

## 1-1 `>` and `>>`

* `>`: overwrite

```console
command > filename
```

* `>>`: append

```console
command >> filename
```

\* 0: stdin, 1: stdout, 2: stderr

\* if make to dispose output, redirect to /dev/null

* `<`: using the contents of file, run command.

```console
command < filename
```

# 2. pipeline

```console
command1 | command2
```

`command1` output goes into `command2` input(filename)

# 3. **sort**

```console
sort filename
```

<center>
    <table>
        <thead><tr><td>option</td><td>example</td><td>explain</td></tr></thead>
        <tbody>
            <tr><td>-k</td><td>sort -k 1,2 data.txt</td><td>sorting contents in data.txt following key 1.<br>if key 1 value same, follow key 2 </td></tr>
            <tr><td>-n</td><td>sort -n data.txt</td><td>sorting contents in data.txt by its numeric value</td></tr>
            <tr><td>-r</td><td>sort -r data.txt</td><td>sorting contents in data.txt in reverse</td></tr>
        </tbody>
    </table>
</center>

# 4. **find**

```console
find start_dir search_option command
```

* search_option

<center>
    <table>
        <thead><tr><td>option</td><td>example</td><td>explain</td></tr></thead>
        <tbody>
            <tr><td>-name</td><td>find -name *.out command</td><td>find file named *.out</td></tr>
            <tr><td>-size</td><td>find -size 0 command</td><td>find 0 byte size file</td></tr>
        </tbody>
    </table>
</center>

* command

<center>
    <table>
        <thead><tr><td>option</td><td>example</td><td>explain</td></tr></thead>
        <tbody>
            <tr><td>-exec</td><td>find -name *.out -exec rm -i {} ';'</td><td>find file named *.out and execute rm -i searched_file</td></tr>
            <tr><td>-print</td><td>find -size 0 -print</td><td>find 0 byte size file and print</td></tr>
        </tbody>
    </table>
</center>

&emsp;&emsp; \* `-exec` using found file, {}

&emsp;&emsp; \* `-exec` must end with char `;` (escape sequence `\;`, single char `';'`)