---
layout: single
title: 리눅스 프로세스 명령어(linux process command) 1. Environment
categories: Linux
---

# 1. startup file

## 1-1 PATH

* setting CWD(current working directory)

```console
PATH=$PATH:.
```

\* `.`: current working directory

* print path

```console
echo $PATH
```

## 1-2 startup file details

* when bash starts, it reads configuration scripts 'startup file'

* Login shell session startup file

<center>
    <table>
        <tr><td>/etc/profile</td><td>global configuration</td></tr>
        <tr><td>~/.bash_profile</td><td>user's configuration</td></tr>
        <tr><td>~/.bash_login</td><td>when ~/.bash_profile not found</td></tr>
        <tr><td>~/.profile</td><td>when ~/.bash_profile, ~/.bash_login not found</td></tr>
    </table>
</center>

* Non-login shell session startup file

<center>
    <table>
        <tr><td>/etc/bash.bashrc</td><td>global configuration</td></tr>
        <tr><td>~/.bashrc</td><td>user's configuration</td></tr>
    </table>
</center>

## 1-3 **alias**

* command for nickname(**alias**)

* invalid when logout -> set in login configuration file

* both command, pathname can be set

```console
alias nickname="target"
```

## 1-4 **source**

* changing config file cannot affect right now until restart shell

* forcing to affect right now

```console
source configfile
```

# 2. process command

## 2-1 terminologies

<center>
    <table>
        <tr><td>process</td><td>active entity</td></tr>
        <tr><td>task</td><td>single or multiple process</td></tr>
        <tr><td>job</td><td>collections of job</td></tr>
    </table>
</center>

therefore, process $\in$ task $\in$ job

* foreground

&emsp;&emsp; current task
  
* background

&emsp;&emsp; running behind the scene

&emsp;&emsp; if write & in end of command, running in background process

&emsp;&emsp; sub-shell: no stdin(keyboard)

&emsp;&emsp; to prevent print on the screen: redirect to file

## 2-2 keystroke with ctrl

<center>
    <table>
        <tr><td>ctrl + c</td><td>interrupting</td></tr>
        <tr><td>ctrl + d</td><td>EOF</td></tr>
        <tr><td>ctrl + h</td><td>backspace</td></tr>
        <tr><td>ctrl + z</td><td>stop foreground process and place in background</td></tr>
    </table>
</center>

## 2-3 **p**roce**s**s

![](/assets/images/Linux_process_1.png)

```console
ps
```

usually pipelined with `|`

\* option

<center>
    <table>
        <tr><td>-a</td><td><b>a</b>ll user process</td></tr>
        <tr><td>-u</td><td><b>u</b>ser info</td></tr>
        <tr><td>-x</td><td>all process whether not attached in shell</td></tr>
        <tr><td>-e</td><td>include all running process</td></tr>
        <tr><td>-f</td><td><b>f</b>ull string</td></tr>
        <tr><td>-l</td><td><b>l</b>ong list</td></tr>
    </table>
</center>

## 2-4 **top**

![](/assets/images/Linux_process_2.png)

```console
top
```

viewing process dynamically

press `q` to **q**uit

## 2-5 **job**

![](/assets/images/Linux_process_3.png)

```console
job
```

showing job ID

## 2-6 **kill**

* to kill process

```console
kill PID
```

* to kill job

```console
kill %jobID
```