# 9.3.5 核心转储 Core dump

核心转储（Core dump）历史悠久，很在就在各类 Unix 系统中出现。

在任何安装了《Linux man 手册》的 Linux 发行版上，都可以通过运行 man core 命名查阅相关信息。

```bash
$ man core
...
A small number of signals which cause abnormal termination of a process
     also cause a record of the process's in-core state to be written to disk
     for later examination by one of the available debuggers.  (See
     sigaction(2).)
...
```

核心转储（Core dump）中的 “core” 代表程序的关键运行状态，“dump” 意为导出或快照。**当程序异常终止时，Linux 系统会将程序的关键运行状态（如程序计数器、内存映像、堆栈跟踪等）导出到一个核心文件（core file）中。使用调试器（如 gdb）打开核心文件，开发者可以查看程序崩溃时的内存状态、变量值和函数调用堆栈情况，从而更容易地定位问题**。

:::tip  注意

由于核心文件会占用大量磁盘空间，复杂应用程序崩溃时甚至能生成几十 GB 的文件。默认情况下，核心文件的大小可能会受到限制。如果你想要为特定的程序生成一个无限制大小的核心文件，须通过命令 ulimit -c unlimited，告诉系统不要对 核心文件大小进行限制。

:::

此外，CNCF 发布的可观测性白皮书中仅提及了 core dump。实际上，dumps 范围应该扩展到包括 Heap dump（Java 堆栈在特定时刻的快照）和 Thread dump（特定时刻的 Java 线程快照）、Memory dump（内存快照）等等。

最后，虽然 CNCF 将 dumps 纳入可观测性体系，但由于容器应用与系统全局配置的冲突、数据持久化的挑战（如在 Pod 重启前需要将 core dump 文件写入持久卷）等众多技术挑战，导致处理及分析 dumps 数据仍然得用传统的手段。