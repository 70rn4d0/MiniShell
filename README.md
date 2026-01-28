<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=gradient&customColorList=1,2,30&height=150&section=header&text=Minishell&fontSize=80&fontColor=39FF14&animation=fadeIn&fontAlignY=45&desc=A%20Custom%20Bash%20Implementation%20in%20C&descAlignY=75&descAlign=50&descSize=20&stroke=39FF14&strokeWidth=2"/>
  
  <br />
  
  <img src="https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c&logoColor=white" />
  <img src="https://img.shields.io/badge/System-Unix-FCC624?style=for-the-badge&logo=linux&logoColor=black" />
  <img src="https://img.shields.io/badge/Focus-Process_Management-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/School-1337-000000?style=for-the-badge&logo=42&logoColor=white" />
</div>

---

## 🐚 About The Project

**Minishell** is a simplified but fully functional recreation of the Bash shell. This project involves deep interaction with the **Linux Kernel API**, managing processes, memory, and file descriptors manually.

It is not just a command runner; it is a full **interpreter** that parses user input, handles variable expansion, manages a command history, and controls execution flow using pipes and redirections.



---

## ⚡ Key Features

| Category | Features Implemented |
| :--- | :--- |
| **Parsing** | Handles single quotes `'`, double quotes `"`, and environment variable expansion (`$USER`, `$?`). |
| **Redirections** | Input `<` output `>`, append `>>`, and **Here-Doc** `<<` support. |
| **Pipes** | Connects multiple commands (`cmd1 | cmd2 | cmd3`) via file descriptors and forks. |
| **Signals** | Custom handlers for `Ctrl-C`, `Ctrl-D`, and `Ctrl-\` using `sigaction`. |
| **Execution** | Uses `execve`, `fork`, `waitpid` to manage child processes without zombies. |
| **History** | Persistent command history (up arrow navigation). |

---

## 🛠️ Built-in Commands

My shell implements the following built-ins from scratch, mirroring Bash behavior:

- `echo` with option `-n`
- `cd` with relative or absolute paths
- `pwd` to print working directory
- `export` to manage environment variables
- `unset` to remove environment variables
- `env` to print current environment
- `exit` to terminate the shell cleanly

---

## 🧠 Architecture & Logic

The shell operates in a continuous **Read-Eval-Print Loop (REPL)**:

1.  **Lexer/Tokenizer:** Splits the raw input string into tokens (Words, Operators, Pipes).
2.  **Parser:** Organizes tokens into a structured command table (AST or Linked List).
3.  **Expander:** Replaces `$VAR` with values from the environment array.
4.  **Executor:**
    * Creates pipes if needed.
    * `fork()` processes.
    * Redirects Standard I/O using `dup2()`.
    * Executes binary using `execve()`.

```c
// Simplified Execution Logic
if (fork() == 0) {
    dup2(fd[1], STDOUT_FILENO); // Connect Pipe
    close(fd[0]);
    execve(cmd, args, env);     // Replace process
} else {
    waitpid(-1, &status, 0);    // Parent waits
}
```


🚀 Installation & Usage
Prerequisites
GCC Compiler

GNU Readline Library

Building
```Bash

git clone https://github.com/70rn4d0/42-common-core.git
cd MiniShell_tty
make
```
Running
```Bash
./minishell
```
Example Usage
```Bash

minishell$ ls -l | grep ".c" | wc -l
minishell$ export 1337="Best School"
minishell$ echo $1337
Best School
```
