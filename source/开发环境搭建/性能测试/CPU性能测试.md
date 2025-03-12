以下是针对AM62x平台进行CPU性能测试的详细方法：

| **测试类型** | **工具列表**                      |
| -------- | ----------------------------- |
| 硬件信息工具   | lscpu, hwloc/lstopo           |
| 系统监控工具   | top, mpstat, vmstat, sar      |
| 压力测试工具   | stress-ng, cpuburn, mprime    |
| 整数性能基准测试 | Dhrystone, sysbench, Super PI |
| 浮点性能基准测试 | Whetstone, Linpack            |
| 实时系统测试   | cyclictest                    |
| 性能分析工具   | perf                          |
| 综合性能基准测试 | Geekbench, UnixBench          |


# **一、硬件信息工具**
## `lscpu`
- **用途**：显示CPU架构、核心数、线程数等硬件信息。
- **使用**：
```bash
lscpu
```
- 示例输出：
```bash
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3
Thread(s) per core:    2
Core(s) per socket:    2
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 142
```

## `hwloc`/`lstopo`
- **用途**：可视化CPU拓扑结构，辅助性能调优。
- **安装与使用**：
```bash
sudo apt-get install hwloc
lstopo-no-graphics
```


# **二、系统监控工具**

## htop

### **1. `htop`简介**
- **功能**：`htop`是一个交互式进程查看工具，提供实时系统监控和进程管理功能，支持鼠标操作与颜色高亮显示，相比传统`top`更直观易用
- **特点**：
  - 动态刷新进程列表，支持横向/纵向滚动
  - 可自定义视图（如按CPU、内存排序）
  - 支持树形视图展示进程层级关系

### **2. 安装与启动**
- **安装**（以Ubuntu为例）：
```bash
apt install htop
```
- **启动**：
```bash
htop
```

### **3. 界面布局**
- **顶部状态栏**：
  - 显示CPU核心使用率（颜色区分各核心）、内存/交换分区使用情况
  - 示例：`CPU0: 10%`表示第一个核心负载10%。
- **进程列表**：
  - 列信息包括：PID（进程ID）、USER（所属用户）、PRI（优先级）、NI（nice值）、VIRT（虚拟内存）、RES（物理内存）、CPU%、MEM%、TIME+（累计运行时间）、COMMAND（进程命令）


### **4. 常用操作与快捷键**
| **操作**   | **快捷键/命令**  | **功能描述**             | **来源**     |
| -------- | ----------- | -------------------- | ---------- |
| **排序**   | `F6` 或点击列标题 | 按CPU、内存、PID等列排序      | [[4]][[5]] |
| **终止进程** | `F9`        | 选择信号（如`SIGKILL`）终止进程 | [[5]][[8]] |
| **树形视图** | `F5`        | 以层级结构显示进程父子关系        | [[7]][[8]] |
| **搜索进程** | `F3`        | 输入关键词过滤进程            | [[7]][[9]] |
| **设置选项** | `F2`        | 自定义界面颜色、列显示、刷新频率等    | [[3]][[6]] |
| **水平滚动** | 左右箭头键       | 查看超出屏幕的列信息           | [[4]]      |
| **帮助**   | `F1`        | 查看快捷键与功能说明           | [[4]]      |
| **退出**   | `F10` 或 `q` | 退出`htop`（可保存配置）      | [[6]][[9]] |

### **5. 高级功能**
- **资源监控**：
  - 顶部的CPU和内存条形图实时显示负载，颜色区分不同核心或内存类型（如缓存、已用）
- **进程过滤**：
  - 按`F4`输入正则表达式过滤进程（如`/python`显示所有Python进程）
- **优先级调整**：
  - 选中进程后按`F7`/`F8`调整`nice`值（需root权限）


### **6. 命令行参数**
- **常用选项**：
```bash
htop -d 5       # 设置刷新间隔为5秒
htop -s CPU     # 启动时按CPU使用率排序
htop -C         # 无颜色模式（简化终端显示）
```

## `top`
- **用途**：实时显示系统的整体性能，包括CPU、内存使用情况及各进程资源占用。
- **常用命令**：
- 查看所有CPU核心的利用率：按`1`键。
- 示例输出：
```bash
top - 14:05:23 up 1 day,  3:22,  1 user,  load average: 0.15, 0.10, 0.05
Tasks: 265 total,   1 running, 264 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.7 us,  0.3 sy,  0.0 ni, 98.9 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  16036.8 total,   1234.5 free,   8765.4 used,   6036.9 buff/cache
MiB Swap:   2048.0 total,   1024.0 free,   1024.0 used.   5678.9 avail Mem
```

## `mpstat`
- **用途**：提供详细的CPU使用率统计，包括用户态、系统态、空闲等时间百分比。
- **安装与使用**：
```bash
sudo apt-get install sysstat
mpstat -P ALL 1 5 # 每秒更新一次，共5次
```
- 示例输出：
```bash
14:05:23     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
14:05:24     all    0.75    0.00    0.25    0.00    0.00    0.00    0.00    0.00    0.00   98.00
14:05:24       0    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00
```

## `vmstat`/`sar`
- **用途**：监控CPU使用率、上下文切换等系统级指标。
- **使用**：
```bash
vmstat 1 5 # 每秒更新一次，共5次
sar -u 1 5 # 每秒更新一次，共5次
```


#  三、压力测试工具

## 1. **`stress-ng`**
- **用途**：对 CPU 进行压力测试，评估其在高负载下的性能和稳定性。
- **使用方法**：
    - 使用 `--cpu N` 选项启动 N 个 CPU 压力测试工作线程。例如，`stress-ng --cpu 4` 将启动 4 个 CPU 压力测试线程，使 CPU 负载增加。
    - 可以结合 `--timeout T` 选项设置测试的持续时间，例如 `stress-ng --cpu 4 --timeout 60s` 表示进行 60 秒的 CPU 压力测试。
    - 使用 `--metrics` 或 `--metrics-brief` 选项在测试结束后显示性能指标，帮助分析 CPU 的性能表现。
- **示例**：
```bash
stress-ng --cpu 4 --timeout 60s --metrics
```

## 2. **`cpuburn`**
- **用途**：最大化 CPU 负载，测试系统的散热能力和稳定性。
- **使用方法**：
- 安装：通过 Git 克隆仓库并编译。
```bash
git clone https://github.com/ssvb/cpuburn.git
cd cpuburn
make
```
- 运行测试：根据核心数量启动多个实例，烧录所有核心。
```bash
./cpuburn-a53 &
```

## 3. **`mprime`（Prime95）**
- **用途**：分布式计算项目工具，可进行极端负载测试。
- **使用方法**：
- 访问[Prime95官网](https://www.mersenne.org/download/)下载并解压适合你系统的版本。
- 运行程序，可选择 "Just Stress Testing" 模式进行压力测试。
- **示例**：
```bash
./mprime
```



# 四、基准测试工具

## 1. **整数性能测试**

### 1.1 **`Dhrystone`**
- **用途**：评估处理器的整数性能。
- **使用方法**：    
	- Dhrystone是一个经典的基准测试程序，主要测量处理器执行简单整数运算和控制流操作的速度。
    - 它在现代处理器中从温暖的L1缓存中运行，其结果以Dhrystone每秒（DhrystoneP）为单位，与处理器的时钟速度呈线性关系。
    - 由于不同运行可能会产生略有不同的结果，建议多次运行该测试以获得最大性能数值。
- **示例**：
```bash
# 进入 Dhrystone 测试目录
cd /path/to/dhrystone
# 编译并运行测试
make
./dhrystone
```

### 1.2 **`sysbench`**
- **用途**：多线程系统基准测试工具，支持 CPU、内存、文件 I/O 和数据库等方面的测试。
- **使用方法**：
  - 单线程CPU基准测试:
```bash
sysbench cpu --threads=1 run
```
  - 多线程CPU基准测试（例如4个线程）:
```bash
sysbench cpu --threads=4 run
```

### 1.3 **Super PI**
```bash
./super_pi  # 计算圆周率耗时，时间越短性能越好
```


## 2. **浮点性能测试**
### 2.1 **`Whetstone`**
- **用途**：测量浮点运算性能。
- **使用方法**：
    - Whetstone基准测试主要用于测量浮点运算性能，通过执行大量的浮点运算操作，如加法、乘法、除法等，来评估处理器的浮点处理能力。
    - 结果以百万次浮点运算每秒（MIPS）为单位。
- **示例**：
```bash
# 进入 Whetstone 测试目录
cd /path/to/whetstone
# 编译并运行测试
make
./whetstone
```

### 2.2 **`Linpack`**
- **目的**：测量处理器解决密集线性方程组的峰值双精度（64位）浮点性能。
- **方法**：
    - Linpack基准测试用于评估处理器在进行大规模科学计算时的性能表现。
    - 它通过求解密集线性方程组来测试处理器的浮点运算能力，结果以千次浮点运算（Kflops）为单位。
- **示例**：
```bash
# 进入 Linpack 测试目录
cd /path/to/linpack
# 编译并运行测试
make
./linpack
```

# **五、实时系统测试**
##  `cyclictest`
- **目的**：用于评估实时系统的相对性能，测量系统的最大、最小和平均延迟，以及抖动等指标。
- **方法**：
	- `cyclictest`是实时系统性能测试的常用工具，可以与`stress-ng`结合使用。
	- 使用`-m`选项启用mlockall，`-Sp80`设置优先级为80，`-D6h`设置测试持续时间为6小时，`-h400`设置每次测试的间隔为400微秒，`-i200`设置测试的间隔为200微秒，`-M`启用mmap，`-q`启用quiet模式。
- **示例**：
```
cyclictest -m -Sp80 -D6h -h400 -i200 -M -q
```


# **六、性能分析工具**

## `perf`

### **1. `perf` 简介**
- **功能**：`perf` 是 Linux 内核自带的性能分析工具，用于监控 CPU、内存、I/O 等系统资源，支持硬件事件（如缓存未命中、分支预测错误）和软件事件（如上下文切换）的统计与分析[[1]][[2]][[5]]。
- **核心能力**：
  - **事件采样**：通过定时中断捕获程序运行时的性能数据[[7]]。
  - **性能计数器管理**：利用 CPU 硬件性能监控单元（PMU）统计事件[[5]][[10]]。
  - **分析报告生成**：提供可视化报告，定位性能瓶颈[[1]][[8]]。

### **2. 安装与准备**
- **安装**（以 Ubuntu 为例）：
  ```bash
  sudo apt install linux-tools-common linux-tools-generic
  ```
- **编译要求**：  
  程序需使用 `-g` 参数编译以保留符号信息，便于 `perf` 关联函数名[[3]][[5]]。

### **3. 常用子命令详解**
#### **3.1 `perf stat`**
- **功能**：统计程序运行期间的性能事件（如 CPU 周期、指令数、缓存未命中等）[[4]][[5]]。
- **示例**：
  ```bash
  perf stat ./your_program    # 统计程序执行期间的性能事件
  perf stat -e cpu-cycles sleep 1  # 统计1秒内的CPU周期数
  ```

#### **3.2 `perf record` + `perf report`**
- **功能**：记录性能数据并生成报告，用于分析热点函数[[3]][[4]]。
- **步骤**：
  ```bash
  perf record -g ./your_program  # 记录性能数据到perf.data
  perf report                   # 交互式查看报告（支持符号解析）
  ```

#### **3.3 `perf top`**
- **功能**：实时显示当前系统的性能热点函数，按 CPU 使用率排序[[8]][[9]]。
- **示例**：
  ```bash
  perf top -g  # 显示函数调用链
  ```

#### **3.4 `perf list`**
- **功能**：列出所有支持的性能事件（硬件/软件/Tracepoint）[[4]][[9]]。
- **示例**：
  ```bash
  perf list | grep cycles  # 查看与CPU周期相关的事件
  ```

#### **3.5 `perf diff`**
- **功能**：比较两次 `perf record` 生成的数据差异，分析性能优化效果[[6]]。
- **示例**：
  ```bash
  perf diff perf.data.old perf.data.new
  ```

### **4. 使用场景**
1. **定位性能瓶颈**：  
   通过 `perf top` 或 `perf report` 快速找到耗时最多的函数[[8]][[10]]。
2. **分析缓存效率**：  
   统计 `cache-misses`、`cache-references` 事件，优化内存访问模式[[1]][[5]]。
3. **内核与用户空间分析**：  
   支持对内核函数和用户程序的混合分析，如 `perf record -a` 监控全系统事件[[2]][[5]]。

### **5. 注意事项**
- **权限**：部分事件需 `root` 权限（如 `perf record -a`）[[5]]。
- **采样开销**：高频采样可能影响性能，建议在测试环境中使用[[7]]。
- **符号解析**：若报告中显示地址而非函数名，需确保程序编译时包含调试信息（`-g`）[[3]][[5]]。


### **6. 原理概述**
`perf` 通过 **硬件性能计数器（PMU）** 和 **软件事件监控** 实现数据采集：  
1. 定时中断（如每毫秒一次）触发事件统计[[7]]。  
2. 结合调用栈信息（`-g` 选项）生成火焰图，直观展示函数调用链[[10]]。

通过 `perf`，开发者可以高效诊断代码性能问题、优化系统资源利用率，是 Linux 性能分析的核心工具之一。

# **七、综合性能测试**

## 1. `geekbench`
`Geekbench` 是一个多平台基准测试工具，提供详细的CPU和GPU性能评估。
- **下载并安装**:
  - 访问 [Geekbench官网](https://www.geekbench.com/) 下载适合你系统的版本，并按照说明安装。
- **运行测试**:
```bash
geekbench
```


## 2.`UnixBench`
- **用途**：综合测试工具，包含CPU性能、进程调度等多个维度评分。
- **安装与使用**：
```bash
git clone https://github.com/kdlucas/byte-unixbench.git
cd byte-unixbench/UnixBench
make
./Run
```

