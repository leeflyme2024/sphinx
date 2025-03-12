# 压力测试
## free
```bash
root@AM62x:~# free -h
              total        used        free      shared  buff/cache   available
Mem:         930.7M      113.9M      776.1M      416.0K       40.7M      801.3M
Swap:             0           0           0
```
在Linux系统中，`free`命令输出的 **free** 和 **available** 字段虽然都表示内存的“可用性”，但它们的含义和用途有本质区别。以下是详细解释：

### **1. `free`（空闲内存）**
- **定义**：完全未被任何进程或内核占用的**物理内存**。
- **计算方式**：`free = Total - used - buff/cache`（但实际包含在`buff/cache`中未被主动释放的部分）。
- **特点**：
  - 表示当前未被分配的“原始”空闲内存。
  - **不包含**缓存（cache）和缓冲区（buffer）中可回收的内存。
- **示例**：你的输出中 `free = 776.1M`，表示系统中有约776MB内存未被使用。


### **2. `available`（可用内存）**
- **定义**：系统**实际可分配给新进程的内存**，包括空闲内存（`free`）和可回收的缓存/缓冲区（`buff/cache`）。
- **计算方式**：`available ≈ free + 可立即回收的缓存和缓冲区`。
- **特点**：
  - 更贴近实际使用场景，反映系统**真实的可用内存容量**。
  - 当启动新程序时，系统会优先使用`free`内存，不足时自动回收缓存/缓冲区。
- **示例**：你的输出中 `available = 801.3M`，表示在不影响现有进程的情况下，系统可提供约801MB内存给新程序。


### **3. 对比总结**
| 字段         | 含义                          | 包含可回收缓存？ | 实际用途                     |
|--------------|-------------------------------|------------------|------------------------------|
| **free**     | 未被分配的物理内存            | ❌ 否            | 反映“闲置”内存               |
| **available**| 系统实际可用的内存（含回收）  | ✅ 是            | 评估新进程的可用内存上限     |


### **4. 示例场景**
假设你的系统需要运行一个占用 **500MB** 的新程序：
- 如果只看 `free=776.1M`，表面看内存足够。
- 但实际系统可能已分配了部分缓存（如文件缓存），此时：
  - 系统会优先使用 `free` 的776.1MB。
  - 若程序需要更多内存，内核会**自动回收缓存**（如 `buff/cache` 中的40.7MB），最终可用的总内存接近 `available=801.3M`。


### **5. 为什么 `available` 大于 `free`？**
在你的输出中，`available (801.3M) > free (776.1M)`，这是因为：
1. `available` 包含了 `free` 的776.1MB。
2. 额外加上了内核可快速回收的缓存（如未修改的文件缓存），即使这些缓存当前计入 `buff/cache`。

### **6. 操作建议**
- **监控内存压力**：优先关注 `available`，若其值接近 **0**，系统可能频繁触发OOM（内存不足）。
- **优化内存分配**：若需运行大型程序（如`memtester`），可参考 `available` 值设置测试内存上限（但需预留部分内存给系统进程）。


### **附：你的内存状态分析**
根据你的输出：
```bash
              total        used        free      shared  buff/cache   available
Mem:         930.7M      113.9M      776.1M      416.0K       40.7M      801.3M
```
- **实际可用内存**：约 **801.3MB**（足够运行 `sudo memtester 500M 3`）。
- **内存健康度**：`available` 占总内存的 **86%**，系统内存资源充足。


## htop
![[Pasted image 20250310133656.png]]

## stress-ng


### 虚拟内存方法(vm)测试
```bash
-m N, --vm N                start N workers spinning on anonymous mmap
      --vm-bytes N          allocate N bytes per vm worker (default 256MB)
      --vm-hang N           sleep N seconds before freeing memory
      --vm-keep             redirty memory instead of reallocating
      --vm-ops N            stop after N vm bogo operations
      --vm-locked           lock the pages of the mapped region into memory
      --vm-madvise M        specify mmap'd vm buffer madvise advice
      --vm-method M         specify stress vm method M, default is all
      --vm-populate         populate (prefault) page tables for a mapping
      --vm-addr N           start N vm address exercising workers
      --vm-addr-ops N       stop after N vm address bogo operations
      --vm-rw N             start N vm read/write process_vm* copy workers
      --vm-rw-bytes N       transfer N bytes of memory per bogo operation
      --vm-rw-ops N         stop after N vm process_vm* copy bogo operations
      --vm-segv N           start N workers that unmap their address space
      --vm-segv-ops N       stop after N vm-segv unmap'd SEGV faults
      --vm-splice N         start N workers reading/writing using vmsplice
      --vm-splice-ops N     stop after N bogo splice operations
      --vm-splice-bytes N   number of bytes to transfer per vmsplice call
```

| 参数                   | 作用                             |
| -------------------- | ------------------------------ |
| `--verbose`          | 显示详细操作日志                       |
| `--metrics`          | 输出性能指标（速度、吞吐量）                 |
| `--vm-method <mode>` | 指定内存压力模式（如 `memset`, `memcpy`） |
| `--vm-locked`        | 锁定内存页，防止交换到磁盘                  |
| `--timeout <time>`   | 限制测试时长（如 `1h`, `30m`）          |


- `--vm N`  
  启动 `N` 个虚拟内存压测工作线程（默认分配 256MB 内存/线程）  
  ```bash
  stress-ng --vm 4  # 启动4个线程，每个分配256MB内存
  ```

**内存超量分配**  
   - 系统总内存为 **930.9MB**，你通过 `--vm 2 --vm-bytes 827M` 尝试分配 **2×827MB = 1654MB**，远超物理内存容量。
   - 即使有 `--vm-keep` 参数复用内存，初始分配时仍会触发 OOM。

**调整内存分配参数**
**公式：**  
安全内存分配量 ≈ (可用内存 - 系统预留) / 线程数  
根据你的 `free -h` 输出：  
- 可用内存（`available`）为 **848.2MB**  
- 建议单线程分配不超过 `848.2M / 2 ≈ 400M`

- `--vm-bytes N`  
  指定每个线程分配的内存大小（支持 `K/M/G` 单位）  
  ```bash
  stress-ng --vm 2 --vm-bytes 1G  # 每个线程分配1GB内存
  ```

- `--vm-method M`  
  指定内存访问模式（如 `prime`, `walk`, `rowhammer` 等）  
  ```bash
  stress-ng --vm 1 --vm-method rowhammer  # 模拟Rowhammer攻击
  ```

- `--vm-locked`  
  锁定内存页（防止被换出到交换分区）  
  ```bash
  stress-ng --vm 2 --vm-locked  # 锁定内存，避免交换
  ```

- `--vm-keep`  
  重用已分配内存（不释放内存，持续重写）  
```bash
stress-ng --vm 1 --vm-keep --timeout 60s  # 持续重写内存60秒
```

- `--vm-rw N`  
  使用 `process_vm_readv`/`process_vm_writev` 跨进程读写内存  
  ```bash
  stress-ng --vm-rw 2 --vm-rw-bytes 4K  # 每次读写4KB
  ```

**`--vm-method <method>` (指定内存访问模式)**  
   支持以下具体方法：
   - **`all`**：遍历所有内存测试方法
   - **`prime`**：通过质数跳跃模式访问内存（模拟随机访问）
   - **`walk`**：顺序遍历内存（模拟线性访问）
   - **`incdec`**：对内存进行递增/递减操作
   - **`galpat`**：Galloping Pattern测试（交替读写不同内存区域）
   - **`gray`**：使用格雷码模式访问内存
   - **`rowhammer`**：模拟Rowhammer攻击（高频访问相邻内存行）
   - **`prime-icache`**：结合指令缓存刷新的质数跳跃
   - **`flush`**：强制刷新缓存行
   - **`lock`**：通过内存锁定操作测试
   
`--vm-method all` 会遍历所有内存测试方法（包括 `rowhammer` 等高强度模式），加剧内存压力。

```bash 
# 启动2个worker进程,每个分配1GB内存
stress-ng --vm 2 --vm-bytes 1G

# 指定运行时间60秒
stress-ng --vm 2 --vm-bytes 1G --timeout 60s

# 使用不同的vm方法进行测试
stress-ng --vm 2 --vm-bytes 1G --vm-method all   # 使用所有方法
stress-ng --vm 2 --vm-bytes 1G --vm-method flip  # 位翻转方法
stress-ng --vm 2 --vm-bytes 1G --vm-method galpat-rand  # 随机访问
stress-ng --vm 2 --vm-bytes 1G --vm-method gray  # 格雷码
stress-ng --vm 2 --vm-bytes 1G --vm-method prime # 素数
```
`--vm-method rowhammer` 可能引发硬件级错误，建议在测试环境执行。

**优化 OOM Killer 行为**
降低 `stress-ng` 进程的 OOM 优先级（值越小越不容易被杀）：
```bash
# 启动前调整 OOM 评分
stress-ng --vm 2 --vm-bytes 400M --vm-keep --timeout 60s &
sudo echo -1000 > /proc/$!/oom_score_adj  # $! 为最后一个后台进程的 PID
```


1. **基础内存分配与释放**  
   ```bash
   stress-ng --vm 4 --vm-bytes 1G --timeout 60s
   ```  
   启动4个进程，每个分配1GB内存，持续60秒。

2. **内存保持（不释放）**  
   ```bash
   stress-ng --vm 2 --vm-bytes 50% --vm-keep --timeout 10m
   ```  
   分配50%系统内存并保持占用，持续10分钟。

3. **内存访问模式**  
   ```bash
   stress-ng --vm 1 --vm-method memset --vm-bytes 2G
   ```  
   使用`memset`方法填充内存，模拟高带宽压力。

4. **大页内存测试**  
   ```bash
   stress-ng --vm 1 --vm-huge-pages --vm-bytes 1G
   ```  
   测试透明大页（THP）性能。


  ```bash
  stress-ng --vm 4 --vm-bytes 1G
  ```
  启动 `4`个进程，每个进程都会分配 `1GB` 字节的匿名内存映射，并不断分配和释放内存。

  ```bash
  stress-ng --vm 2 --vm-hang 30 --timeout 60s
  ```
  这会启动 2 个进程，在分配内存后暂停 30 秒，整个测试持续 60 秒。

  ```bash
  stress-ng --vm 2 --vm-locked --vm-bytes 512M
  ```
锁定已分配的内存页面到物理内存中，防止它们被交换出去。

#### **(1) 启用详细输出 (`--verbose`)**
```bash
stress-ng --vm 2 --vm-bytes 785M --verbose --timeout 1h
```
- **作用**：显示内存分配、释放等操作的详细日志。

#### **(2) 启用性能指标 (`--metrics`)**
```bash
stress-ng --vm 2 --vm-bytes 785M --metrics --timeout 1h
```
- **输出示例**：
  ```text
  stress-ng: info:  [1234] stressor       bogo ops real time  usr time  sys time   bogo ops/s   byte/sec
  stress-ng: info:  [1234] vm              1234567  3600.00s  7200.00s    0.00s      343.21      123.45MB/s
  ```

#### **(3) 结合 `dmesg` 检查内核日志**
```bash
# 运行 stress-ng 后检查内核日志
dmesg | grep -i "memory\|oom\|error"
```

#### **(1) 选择内存压力模式 (`--vm-method`)**
```bash
# 示例：使用不同的内存访问模式
stress-ng --vm 2 --vm-bytes 785M --vm-method all --timeout 1h
```
- **常用方法**：
  - `all`：轮询所有内存操作（默认）。
  - `malloc`：仅测试内存分配/释放。
  - `memset`：测试内存写入。
  - `memcpy`：测试内存拷贝。

#### **(2) 锁定内存页 (`--vm-locked`)**
```bash
stress-ng --vm 2 --vm-bytes 785M --vm-locked --timeout 1h
```
- **作用**：防止内存被交换到磁盘（Swap），模拟物理内存压力。

#### **(3) 限制测试时间 (`--timeout`)**
```bash
stress-ng --vm 2 --vm-bytes 785M --timeout 30m  # 30 分钟
```


#### 基本虚拟内存测试
```bash
stress-ng --vm 4 --vm-bytes 1G --timeout 60
```
此命令启动4个虚拟内存工作进程，每个分配1GB内存，测试持续60秒。

#### 内存锁定测试
```bash
stress-ng --vm 2 --vm-bytes 512M --vm-locked --timeout 60
```
此命令启动2个进程，每个分配512MB内存并锁定在物理内存中，防止被交换出去。
`--vm-locked` 可能导致系统触发 OOM Killer，需谨慎使用。

#### 内存保持测试
```bash
stress-ng --vm 2 --vm-bytes 512M --vm-keep --timeout 60
```
此命令测试内存的重复使用而非重新分配，更接近实际应用场景。
- 内存测试可能导致 **OOM Killer** 终止进程，建议通过`--vm-keep`减少内存释放

#### 内存映射建议测试
```bash
stress-ng --vm 2 --vm-bytes 512M --vm-madvise random --timeout 60
```
此命令使用random访问模式的madvise提示，测试内存映射的性能。

#### 进程间内存读写测试
```bash
stress-ng --vm-rw 4 --vm-rw-bytes 64M --timeout 60
```
此命令测试使用process_vm_readv/writev进程间内存传输，每次传输64MB。

#### 内存分段错误测试
```bash
stress-ng --vm-segv 2 --timeout 60
```
此命令测试取消映射内存并处理由此产生的SEGV错误的能力。

#### 内存拼接测试
```bash
stress-ng --vm-splice 2 --vm-splice-bytes 4M --timeout 60
```
此命令测试使用vmsplice进行内存传输，每次传输4MB。




### 组合测试方案
```bash
stress-ng --vm 4 --vm-bytes 85% --vm-method all --vm-keep --vm-locked --vm-ops 0 --vm-hang 2 --vm-populate --vm-addr 2 --vm-rw 2 --vm-segv 1 --vm-splice 1 --backoff 10000 --timeout 120s --metrics-brief --verify --verbose
```

#### ‌**一、核心内存测试参数**‌

1. ‌**`--vm 4`**‌
    - 启动 ‌**4 个虚拟内存（VM）工作线程**‌，模拟多任务内存操作‌12。
    - _作用_：通过多线程增加内存访问竞争，测试内存并发性能。
2. ‌**`--vm-bytes 85%`**‌
    - 每个 VM 线程分配 ‌**系统可用内存的 85%**‌‌16。
    - _注意_：总分配量可能超过物理内存（需结合 `--vm-keep` 复用内存以避免 OOM）‌56。
3. ‌**`--vm-method all`**‌
    - 使用 ‌**所有内存压力方法**‌（如 `malloc`, `mmap`, `mlock`, `madvise` 等）‌14。
    - _覆盖场景_：混合内存操作模式，模拟真实负载复杂性。
4. ‌**`--vm-keep`**‌
    - ‌**保持已分配内存不被释放**‌，避免重复分配/释放操作‌56。
    - _用途_：测试内存长期占用时的稳定性。
5. ‌**`--vm-locked`**‌
    - ‌**锁定内存页**‌，防止被换出到交换空间（即使系统无 Swap）‌45。
    - _适用场景_：实时系统或内存关键型应用测试。

#### ‌**二、内存操作控制参数**‌

6. ‌**`--vm-ops 0`**‌
    - 每个线程执行 ‌**无限次内存操作**‌，直到超时（`--timeout`）或手动终止‌34。
7. ‌**`--vm-hang 2`**‌
    - 每次内存操作后 ‌**挂起 2 秒**‌，模拟间歇性内存访问‌56。
    - _用途_：测试内存释放与重新分配的延迟影响。
8. ‌**`--vm-populate`**‌
    - ‌**预分配所有内存页**‌（通过 `MAP_POPULATE` 标志），减少缺页中断对测试的干扰‌47。
    - _效果_：直接施加内存带宽压力，而非页表管理开销。

#### ‌**三、扩展内存测试功能**‌

9. ‌**`--vm-addr 2`**‌
    - 启动 ‌**2 个地址操作线程**‌，测试内存地址空间的随机访问性能‌24。
10. ‌**`--vm-rw 2`**‌
    - 启动 ‌**2 个读写线程**‌，使用 `process_vm_readv`/`process_vm_writev` 跨进程内存复制‌24。
    - _模拟场景_：进程间通信（IPC）的内存传输压力。
11. ‌**`--vm-segv 1`**‌
    - 启动 ‌**1 个段错误（SEGV）触发线程**‌，测试系统对非法内存访问的容错能力‌24。
    - _风险提示_：可能触发内核崩溃（需确保系统具备恢复机制）。
12. ‌**`--vm-splice 1`**‌
    - 启动 ‌**1 个 vmsplice 线程**‌，通过管道传输内存数据，测试内存与 I/O 混合负载‌24。

#### ‌**四、测试控制与输出参数**‌

13. ‌**`--backoff 10000`**‌
    - 线程启动后 ‌**延迟 10 毫秒（10,000 微秒）**‌，避免初始资源争用‌57。
14. ‌**`--timeout 120s`**‌
    - 测试总时长为 ‌**120 秒**‌，超时后自动终止‌12。
15. ‌**`--metrics-brief`**‌
    - 输出 ‌**简化的性能指标**‌（如 BogoOps/s），便于快速分析‌12。
16. ‌**`--verify`**‌
    - ‌**验证内存数据完整性**‌，检测内存损坏或位翻转错误‌14。
17. ‌**`--verbose`**‌
    - 显示 ‌**详细日志**‌，包括线程状态和操作进度‌12。

#### ‌**五、潜在风险与优化建议**‌

- ‌**内存超量风险**‌：`--vm-bytes 85%` 可能导致总分配量超过物理内存（尤其多线程叠加时），需监控 `dmesg` 防止 OOM‌56。
- ‌**实时性影响**‌：`--vm-locked` 在实时内核（如 `PREEMPT_RT`）中可能加剧调度延迟，建议结合 `chrt` 调整线程优先级‌4。
- ‌**混合负载优化**‌：可添加 `--cpu 4` 和 `--io 2` 参数，模拟 CPU 与 I/O 混合压力场景‌12。


```bash
# 同时测试多种内存压力
stress-ng --vm 2 --vm-bytes 1G \
          --mmap 2 --mmap-bytes 512M \
          --malloc 2 --malloc-bytes 512M \
          --timeout 5m
```

```bash
# 高详细度内存压力测试（1 小时）
stress-ng --vm 2 --vm-bytes 785M --verbose --metrics --vm-method all --timeout 1h | tee -a stress-ng.log

# 检查内核日志
dmesg | grep -i "memory" | tee -a dmesg.log
```

```bash
stress-ng --all 4 --timeout 5m
```  
同时启动4种不同类型的压力测试（CPU、内存、磁盘、网络等），全面压测系统

#### 全面内存压力测试
```bash
stress-ng --vm 2 --vm-bytes 512M --mmap 2 --mremap 2 --memcpy 2 --memthrash 2 --timeout 120
```
此命令组合多种内存测试方法，提供全方位的内存子系统压力测试。

#### 内存分配和同步测试
```bash
stress-ng --malloc 2 --malloc-bytes 256M --msync 2 --msync-bytes 128M --timeout 60
```
此命令测试内存分配和同步操作的结合。

#### 虚拟内存和页面错误测试
```bash
stress-ng --vm 2 --vm-bytes 512M --fault 2 --mincore 2 --timeout 60
```
此命令测试虚拟内存分配和页面错误处理的组合。


### 缓存测试(Cache)
```bash
-C N, --cache N             start N CPU cache thrashing workers
      --cache-ops N         stop after N cache bogo operations
      --cache-no-affinity   do not change CPU affinity
      --cache-fence         serialize stores
      --cache-flush         flush cache after every memory write (x86 only)
      --cache-level N       only exercise specified cache
      --cache-prefetch      prefetch on memory reads/writes
      --cache-ways N        only fill specified number of cache ways
      --cap N               start N workers exercising capget
```

```bash
# 基础缓存测试
stress-ng --cache 4  # 启动4个缓存压力测试进程

# L1/L2缓存压力测试
stress-ng --cache 2 --cache-ways 4 --cache-size 4M

# 缓存一致性测试
stress-ng --cache 2 --cache-ops 100000

# 高级缓存测试选项
stress-ng --cache 4 \
    --cache-ops 1000000 \      # 执行100万次操作后停止
    --cache-level 2 \          # 只测试L2缓存
    --cache-ways 8 \           # 只填充8路缓存
    --cache-fence \            # 序列化存储操作
    --cache-flush \            # 每次写入后刷新缓存(仅x86)
    --cache-prefetch          # 内存读写时预取

# 禁用CPU亲和性的缓存测试
stress-ng --cache 4 --cache-no-affinity
```

1. **基本缓存测试**：
   ```bash
   stress-ng --cache 4 --timeout 60
   ```
   启动4个缓存压力测试进程，运行60秒。

2. **指定缓存级别测试**：
   ```bash
   stress-ng --cache 2 --cache-level 1 --timeout 60
   ```
   仅测试L1缓存，启动2个进程，运行60秒。

3. **控制缓存操作数量**：
   ```bash
   stress-ng --cache 2 --cache-ops 1000000
   ```
   启动2个进程，执行1,000,000次缓存操作后停止。

4. **测试缓存刷新**：
   ```bash
   stress-ng --cache 2 --cache-flush --timeout 60
   ```
   测试每次内存写入后刷新缓存的性能（仅限x86架构）。

5. **完整缓存测试**：
   ```bash
   stress-ng --cache 4 --cache-level 2 --cache-ways 8 --cache-fence --timeout 120
   ```
   测试L2缓存，限制8路缓存，启用存储序列化，运行120秒。

### TLB清除测试选项
```bash
      --tlb-shootdown N     start N workers that force TLB shootdowns
      --tlb-shootdown-ops N stop after N TLB shootdown bogo ops
```
- `--tlb-shootdown N`  
  强制触发TLB刷新（测试多核TLB同步）  
  ```bash
  stress-ng --tlb-shootdown 2 --tlb-shootdown-ops 1000
  ```

- **TLB刷新（`--tlb-shootdown`）**  
  ```bash
  stress-ng --tlb-shootdown 1 --tlb-shootdown-ops 5000
  ```  
  通过频繁修改页表强制TLB失效，测试上下文切换开销。

#### TLB清除测试
```bash
stress-ng --tlb-shootdown 2 --timeout 60
```
此命令测试转换后备缓冲区(TLB)清除操作，这在多核系统上很重要。

### 内存分配测试(Malloc)

```bash
      --malloc N            start N workers exercising malloc/realloc/free
      --malloc-bytes N      allocate up to N bytes per allocation
      --malloc-max N        keep up to N allocations at a time
      --malloc-ops N        stop after N malloc bogo operations
      --malloc-thresh N     threshold where malloc uses mmap instead of sbrk
      --malloc-pthreads N   number of pthreads to run concurrently
      --malloc-touch        touch pages force pages to be populated
```

```bash
# 基础内存分配测试
stress-ng --malloc 4 --malloc-bytes 1G

# 高级内存分配测试
stress-ng --malloc 4 \
    --malloc-bytes 1G \        # 每次分配最大1G
    --malloc-max 100 \         # 同时保持100个分配
    --malloc-ops 10000 \       # 10000次操作后停止
    --malloc-thresh 1M \       # 设置malloc使用mmap而非sbrk的阈值
    --malloc-pthreads 4 \      # 使用4个并发线程
    --malloc-touch            # 强制页面加载到内存

# 测试内存分配触及
stress-ng --malloc 4 --malloc-bytes 1G --malloc-touch
```

1. **基本内存分配测试**：
   ```bash
   stress-ng --malloc 4 --malloc-bytes 1G --timeout 60
   ```
   启动4个进程，每个分配1GB内存，运行60秒。

2. **控制分配数量**：
   ```bash
   stress-ng --malloc 2 --malloc-bytes 512M --malloc-max 100 --timeout 60
   ```
   启动2个进程，每个最多分配512MB，同时保持100个分配，运行60秒。

3. **触碰分配内存**：
   ```bash
   stress-ng --malloc 2 --malloc-bytes 1G --malloc-touch --timeout 60
   ```
   分配内存并触碰页面，强制页面被填充，更有效地测试物理内存压力。

4. **多线程内存分配测试**：
   ```bash
   stress-ng --malloc 2 --malloc-bytes 512M --malloc-pthreads 4 --timeout 60
   ```
   启动2个进程，每个使用4个线程进行内存分配，总共512MB，运行60秒。

5. **完整内存分配测试**：
   ```bash
   stress-ng --malloc 4 --malloc-bytes 1G --malloc-max 50 --malloc-thresh 64K --malloc-touch --timeout 120
   ```
   启动4个进程，每个分配1GB内存，最多同时50个分配，设置64KB的mmap阈值，触碰页面，运行120秒。

### 内存拷贝测试(Memcpy)
```bash
      --memcpy N            start N workers performing memory copies
      --memcpy-ops N        stop after N memcpy bogo operations
      --memcpy-method M     set memcpy method (M = all, libc, builtin, naive)
```
```bash
# 基础内存拷贝测试
stress-ng --memcpy 4

# 测试内存带宽
stress-ng --memcpy 2 --memcpy-ops 100000

# 指定操作方法
stress-ng --memcpy 4 \
    --memcpy-ops 10000 \      # 10000次操作后停止
    --memcpy-method all      # 使用所有方法(libc,builtin,naive)

# 使用特定拷贝方法
stress-ng --memcpy 4 --memcpy-method libc
stress-ng --memcpy 4 --memcpy-method builtin
stress-ng --memcpy 4 --memcpy-method naive
```

1. **基本内存复制测试**：
   ```bash
   stress-ng --memcpy 4 --timeout 60
   ```
   启动4个内存复制测试进程，运行60秒。

2. **指定操作数量**：
   ```bash
   stress-ng --memcpy 2 --memcpy-ops 100000
   ```
   启动2个进程，执行100,000次内存复制操作后停止。

3. **测试特定内存复制实现**：
   ```bash
   stress-ng --memcpy 2 --memcpy-method libc --timeout 60
   ```
   测试libc实现的memcpy性能，启动2个进程，运行60秒。

4. **测试所有内存复制实现**：
   ```bash
   stress-ng --memcpy 2 --memcpy-method all --timeout 120
   ```
   测试所有可用的memcpy实现，启动2个进程，运行120秒。

### 内存映射测试(mmap)

```bash
      --mmap N              start N workers stressing mmap and munmap
      --mmap-ops N          stop after N mmap bogo operations
      --mmap-async          using asynchronous msyncs for file based mmap
      --mmap-bytes N        mmap and munmap N bytes for each stress iteration
      --mmap-file           mmap onto a file using synchronous msyncs
      --mmap-mprotect       enable mmap mprotect stressing
      --mmap-osync          enable O_SYNC on file
      --mmap-odirect        enable O_DIRECT on file
      --mmapaddr N          start N workers stressing mmap with random addresses
      --mmapaddr-ops N      stop after N mmapaddr bogo operations
      --mmapfixed N         start N workers stressing mmap with fixed mappings
      --mmapfixed-ops N     stop after N mmapfixed bogo operations
      --mmapfork N          start N workers stressing many forked mmaps/munmaps
      --mmapfork-ops N      stop after N mmapfork bogo operations
      --mmaphuge N          start N workers stressing mmap with huge mappings
      --mmaphuge-ops N      stop after N mmaphuge bogo operations
      --mmaphuge-mmaps N    select number of memory mappings per iteration
      --mmapmany N          start N workers stressing many mmaps and munmaps
      --mmapmany-ops N      stop after N mmapmany bogo operations
```

```bash
# 基础mmap测试
stress-ng --mmap 4 --mmap-bytes 1G

# 高级mmap选项
stress-ng --mmap 4 \
    --mmap-bytes 1G \         # 每次映射1G
    --mmap-ops 1000 \         # 1000次操作后停止
    --mmap-async \            # 使用异步msync
    --mmap-file \             # 基于文件的映射
    --mmap-mprotect \         # 启用mprotect压力测试
    --mmap-osync \            # 启用O_SYNC
    --mmap-odirect           # 启用O_DIRECT

# 特殊mmap测试
stress-ng --mmapfixed 4       # 固定地址映射测试
stress-ng --mmaphuge 4        # 大页面映射测试
stress-ng --mmapmany 4        # 多重映射测试

stress-ng --mmap 2 --mmap-bytes 1G --mmap-async  # 异步mmap
```

1. **基本内存映射测试**：
   ```bash
   stress-ng --mmap 4 --mmap-bytes 1G --timeout 60
   ```
   启动4个进程，每个映射1GB内存，运行60秒。

2. **基于文件的内存映射**：
   ```bash
   stress-ng --mmap 2 --mmap-bytes 512M --mmap-file --timeout 60
   ```
   启动2个进程，测试基于文件的内存映射，每个512MB，运行60秒。

3. **测试内存保护**：
   ```bash
   stress-ng --mmap 2 --mmap-bytes 512M --mmap-mprotect --timeout 60
   ```
   测试内存映射的保护属性变更，启动2个进程，运行60秒。

4. **大页内存映射测试**：
   ```bash
   stress-ng --mmaphuge 2 --mmaphuge-mmaps 10 --timeout 60
   ```
   测试大页内存映射，启动2个进程，每次迭代10个映射，运行60秒。

5. **多映射测试**：
   ```bash
   stress-ng --mmapmany 2 --timeout 60
   ```
   测试多个内存映射和取消映射，启动2个进程，运行60秒。

### 内存重映射测试选项(mremap)
```bash
      --mremap N            start N workers stressing mremap
      --mremap-ops N        stop after N mremap bogo operations
      --mremap-bytes N      mremap N bytes maximum for each stress iteration
      --mremap-lock         mlock remap pages, force pages to be unswappable
```
- `--mremap N`  
  高频调用 `mremap` 重映射内存区域  
  ```bash
  stress-ng --mremap 4 --mremap-bytes 64K  # 重映射64KB内存块
  ```

  ```bash
  stress-ng --mremap N --mremap-bytes M --mremap-ops O
  ```
- **说明**: 使用 `mremap` 系统调用来调整内存区域的大小，最大为 `M` 字节，执行 `O` 次操作。
  ```bash
  stress-ng --mremap 3 --mremap-bytes 1G --mremap-ops 100
  ```

#### 基本内存重映射测试
```bash
stress-ng --mremap 2 --mremap-bytes 128M --timeout 60
```
此命令测试mremap系统调用，最大重映射128MB内存。

#### 锁定重映射测试
```bash
stress-ng --mremap 2 --mremap-bytes 64M --mremap-lock --timeout 60
```
此命令测试重映射并锁定内存页面，防止被交换出去。

### 取消内存映射测试选项(munmap)
```bash
      --munmap N            start N workers stressing munmap
      --munmap-ops N        stop after N munmap bogo operations
```
- `--munmap N`  
  高频执行内存取消映射操作  
  ```bash
  stress-ng --munmap 3 --munmap-ops 10000  # 执行1万次munmap
  ```

#### 取消内存映射测试
```bash
stress-ng --munmap 2 --timeout 60
```
此命令测试munmap系统调用，该调用取消内存映射。

### 共享内存测试(shm)

```bash
      --shm N               start N workers that exercise POSIX shared memory
      --shm-ops N           stop after N POSIX shared memory bogo operations
      --shm-bytes N         allocate/free N bytes of POSIX shared memory
      --shm-segs N          allocate N POSIX shared memory segments per iteration
      --shm-sysv N          start N workers that exercise System V shared memory
      --shm-sysv-ops N      stop after N shared memory bogo operations
      --shm-sysv-bytes N    allocate and free N bytes of shared memory per loop
      --shm-sysv-segs N     allocate N shared memory segments per iteration
```

```bash
# 测试共享内存
stress-ng --shm 2 --shm-bytes 1G  

# POSIX共享内存测试
stress-ng --shm 4 \
    --shm-bytes 1G \          # 每次分配1G
    --shm-ops 1000 \          # 1000次操作后停止
    --shm-segs 10            # 每次迭代分配10个段

# System V共享内存测试
stress-ng --shm-sysv 4 \
    --shm-sysv-bytes 1G \     # 每次分配1G
    --shm-sysv-ops 1000 \     # 1000次操作后停止
    --shm-sysv-segs 10       # 每次迭代分配10个段
```

1. **POSIX共享内存测试**：
   ```bash
   stress-ng --shm 4 --shm-bytes 512M --timeout 60
   ```
   启动4个进程，每个分配512MB的POSIX共享内存，运行60秒。

2. **多段POSIX共享内存测试**：
   ```bash
   stress-ng --shm 2 --shm-bytes 256M --shm-segs 8 --timeout 60
   ```
   启动2个进程，每个分配8个共享内存段，每段256MB，运行60秒。

3. **System V共享内存测试**：
   ```bash
   stress-ng --shm-sysv 4 --shm-sysv-bytes 512M --timeout 60
   ```
   启动4个进程，每个分配512MB的System V共享内存，运行60秒。

4. **多段System V共享内存测试**：
   ```bash
   stress-ng --shm-sysv 2 --shm-sysv-bytes 256M --shm-sysv-segs 8 --timeout 60
   ```
   启动2个进程，每个分配8个System V共享内存段，每段256MB，运行60秒。

### 内存锁定测试(mlock)
```bash
      --mlock N             start N workers exercising mlock/munlock
      --mlock-ops N         stop after N mlock bogo operations
      --mlockmany N         start N workers exercising many mlock/munlock processes
      --mlockmany-ops N     stop after N mlockmany bogo operations
      --mlockmany-procs N   use N child processes to mlock regions
```
```bash
# 测试内存锁定
stress-ng --mlock 2 --mlock-bytes 1G

# 基础mlock测试
stress-ng --mlock 4 --mlock-ops 1000

# 多进程mlock测试
stress-ng --mlockmany 4 \
    --mlockmany-ops 1000 \    # 1000次操作后停止
    --mlockmany-procs 10     # 使用10个子进程
```

1. **基本内存锁定测试**：
   ```bash
   stress-ng --mlock 4 --timeout 60
   ```
   启动4个测试内存锁定/解锁的进程，运行60秒。

2. **指定操作数量**：
   ```bash
   stress-ng --mlock 2 --mlock-ops 10000
   ```
   启动2个进程，执行10,000次内存锁定操作后停止。

3. **多进程内存锁定测试**：
   ```bash
   stress-ng --mlockmany 2 --mlockmany-procs 8 --timeout 60
   ```
   启动2个工作进程，每个使用8个子进程锁定内存区域，运行60秒。

### 内存文件描述符测试(memfd)
```bash
      --memfd N             start N workers allocating memory with memfd_create
      --memfd-bytes N       allocate N bytes for each stress iteration
      --memfd-fds N         number of memory fds to open per stressors
      --memfd-ops N         stop after N memfd bogo operations
```
```bash
stress-ng --memfd 4 \
    --memfd-bytes 1G \        # 每次分配1G
    --memfd-ops 1000 \        # 1000次操作后停止
    --memfd-fds 10           # 每个stressor打开10个fd
```


1. **基本内存文件描述符测试**：
   ```bash
   stress-ng --memfd 4 --memfd-bytes 512M --timeout 60
   ```
   启动4个进程，每个使用memfd_create分配512MB内存，运行60秒。

2. **多文件描述符测试**：
   ```bash
   stress-ng --memfd 2 --memfd-bytes 256M --memfd-fds 8 --timeout 60
   ```
   启动2个进程，每个打开8个内存文件描述符，每个分配256MB，运行60秒。

3. **指定操作数量**：
   ```bash
   stress-ng --memfd 2 --memfd-bytes 1G --memfd-ops 5000
   ```
   启动2个进程，每个分配1GB内存，执行5,000次操作后停止。

### 数据段测试(Brk)
```bash
      --brk N               start N workers performing rapid brk calls
      --brk-ops N           stop after N brk bogo operations
      --brk-mlock           attempt to mlock newly mapped brk pages
      --brk-notouch         don't touch (page in) new data segment page
```
```bash
stress-ng --brk 4 \
    --brk-ops 1000 \          # 1000次操作后停止
    --brk-mlock \             # 锁定新映射的页面
    --brk-notouch            # 不触及新数据段页面
```

1. **基本程序堆测试**：
   ```bash
   stress-ng --brk 4 --timeout 60
   ```
   启动4个进程，执行快速的brk系统调用，运行60秒。

2. **指定操作数量**：
   ```bash
   stress-ng --brk 2 --brk-ops 50000
   ```
   启动2个进程，执行50,000次brk操作后停止。

3. **锁定程序堆测试**：
   ```bash
   stress-ng --brk 2 --brk-mlock --timeout 60
   ```
   启动2个进程，执行brk调用并尝试锁定新映射的页面，运行60秒。

### 大堆测试选项
```bash
-B N, --bigheap N           start N workers that grow the heap using calloc()
      --bigheap-ops N       stop after N bogo bigheap operations
      --bigheap-growth N    grow heap by N bytes per iteration
```
- `--bigheap N`  
  启动 `N` 个线程，不断扩展堆内存直到失败  
  ```bash
  stress-ng --bigheap 2  # 启动2个线程扩展堆内存
  ```

- `--bigheap-growth N`  
  指定每次堆内存增长的字节数  
  ```bash
  stress-ng --bigheap 1 --bigheap-growth 1024  # 每次增长1KB
  ```

```bash
stress-ng --bigheap 2 --bigheap-bytes 4G --timeout 5m
```  
启动2个进程，通过`calloc()`持续增长堆内存至4GB，测试内存分配器性能[[9]]。

#### 基本大堆测试
```bash
stress-ng --bigheap 2 --bigheap-growth 10M --timeout 60
```
此命令启动2个进程，每次迭代增加10MB的堆内存，测试系统处理逐渐增长的堆的能力。


### 栈测试(Stack)
```bash
      --stack N             start N workers generating stack overflows
      --stack-ops N         stop after N bogo stack overflows
      --stack-fill          fill stack, touches all new pages
      --stack-mlock         mlock stack, force pages to be unswappable
      --stackmmap N         start N workers exercising a filebacked stack
      --stackmmap-ops N     stop after N bogo stackmmap operations
```
```bash
# 栈溢出测试
stress-ng --stack 4 \
    --stack-ops 1000 \        # 1000次操作后停止
    --stack-fill \            # 填充栈空间
    --stack-mlock            # 锁定栈页面

# 文件支持的栈测试
stress-ng --stackmmap 4 --stackmmap-ops 1000
```

1. **基本堆栈测试**：
   ```bash
   stress-ng --stack 4 --timeout 60
   ```
   启动4个生成堆栈压力的进程，运行60秒。

2. **填充堆栈测试**：
   ```bash
   stress-ng --stack 2 --stack-fill --timeout 60
   ```
   启动2个进程，填充堆栈并触碰所有新页面，运行60秒。

3. **锁定堆栈测试**：
   ```bash
   stress-ng --stack 2 --stack-mlock --timeout 60
   ```
   启动2个进程，锁定堆栈页面使其不可交换，运行60秒。

4. **文件支持的堆栈测试**：
   ```bash
   stress-ng --stackmmap 2 --timeout 60
   ```
   启动2个测试文件支持的堆栈的进程，运行60秒。

### 内存速率测试(memrate)
```bash
      --memrate N           start N workers exercised memory read/writes
      --memrate-ops N       stop after N memrate bogo operations
      --memrate-bytes N     size of memory buffer being exercised
      --memrate-rd-mbs N    read rate from buffer in megabytes per second
      --memrate-wr-mbs N    write rate to buffer in megabytes per second
```
```bash
stress-ng --memrate 4 \
    --memrate-bytes 1G \      # 测试缓冲区大小1G
    --memrate-rd-mbs 100 \    # 读取速率100MB/s
    --memrate-wr-mbs 100 \    # 写入速率100MB/s
    --memrate-ops 1000       # 1000次操作后停止
```

1. **基本内存速率测试**：
   ```bash
   stress-ng --memrate 4 --memrate-bytes 1G --timeout 60
   ```
   启动4个进程，每个测试1GB内存的读写速率，运行60秒。

2. **控制读写速率**：
   ```bash
   stress-ng --memrate 2 --memrate-bytes 1G --memrate-rd-mbs 100 --memrate-wr-mbs 50 --timeout 60
   ```
   启动2个进程，设置100MB/s的读取速率和50MB/s的写入速率，运行60秒。

3. **指定操作数量**：
   ```bash
   stress-ng --memrate 2 --memrate-bytes 512M --memrate-ops 1000
   ```
   启动2个进程，每个测试512MB内存，执行1,000次操作后停止。

### 内存争用测试选项(memthrash)
```bash
      --memthrash N         start N workers thrashing a 16MB memory buffer
      --memthrash-ops N     stop after N memthrash bogo operations
      --memthrash-method M  specify memthrash method M, default is all
```

- `--memthrash N`  
  高频读写16MB内存缓冲区（模拟内存抖动）  
  ```bash
  stress-ng --memthrash 4 --memthrash-ops 1e6  # 执行100万次操作
  ```

#### 内存争用测试
```bash
stress-ng --memthrash 4 --timeout 60
```
此命令测试多个进程争用同一内存区域的情况，对并发性能很有用。

#### 特定方法内存争用测试
```bash
stress-ng --memthrash 2 --memthrash-method random --timeout 60
```
此命令使用随机方法进行内存争用测试。

### 管道测试(Pipe)
```bash
-p N, --pipe N              start N workers exercising pipe I/O
      --pipe-ops N          stop after N pipe I/O bogo operations
      --pipe-data-size N    set pipe size of each pipe write to N bytes
      --pipe-size N         set pipe size to N bytes
-p N, --pipeherd N          start N multi-process workers exercising pipes I/O
      --pipeherd-ops N      stop after N pipeherd I/O bogo operations
      --pipeherd-yield      force processes to yield after each write
```
```bash
# 基础管道测试
stress-ng --pipe 4 \
    --pipe-ops 1000 \         # 1000次操作后停止
    --pipe-data-size 4K \     # 每次写入4K
    --pipe-size 64K          # 设置管道大小为64K

# 多进程管道测试
stress-ng --pipeherd 4 \
    --pipeherd-ops 1000 \     # 1000次操作后停止
    --pipeherd-yield         # 每次写入后强制进程让出CPU
```

```bash
stress-ng --pipeherd 4 --pipeherd-yield --timeout 30s
```  
启动4个进程通过管道进行读写，模拟高并发IPC场景


1. **基本管道I/O测试**：
   ```bash
   stress-ng --pipe 4 --timeout 60
   ```
   启动4个测试管道I/O的进程，运行60秒。

2. **控制管道大小**：
   ```bash
   stress-ng --pipe 2 --pipe-size 64K --timeout 60
   ```
   启动2个进程，设置管道大小为64KB，运行60秒。

3. **控制数据大小**：
   ```bash
   stress-ng --pipe 2 --pipe-data-size 16K --timeout 60
   ```
   启动2个进程，每次写入16KB数据，运行60秒。

4. **多进程管道测试**：
   ```bash
   stress-ng --pipeherd 4 --timeout 60
   ```
   启动4个多进程测试管道I/O的工作进程，运行60秒。

5. **进程让出测试**：
   ```bash
   stress-ng --pipeherd 2 --pipeherd-yield --timeout 60
   ```
   启动2个进程，强制进程在每次写入后让出CPU，运行60秒。

### 多进程管道测试选项
```bash
-p N, --pipeherd N          start N multi-process workers exercising pipes I/O
      --pipeherd-ops N      stop after N pipeherd I/O bogo operations
      --pipeherd-yield      force processes to yield after each write
```
#### 多进程管道测试
```bash
stress-ng --pipeherd 2 --timeout 60
```
此命令测试多进程间的管道I/O性能。

#### 进程让步管道测试
```bash
stress-ng --pipeherd 2 --pipeherd-yield --timeout 60
```
此命令测试进程在每次写入后让出CPU的效果。

### 内存同步测试选项(msync)
```bash
      --msync N             start N workers syncing mmap'd data with msync
      --msync-ops N         stop msync workers after N bogo msyncs
      --msync-bytes N       size of file and memory mapped region to msync
```

- `--msync N`  
  使用 `msync` 同步内存映射文件到磁盘  
  ```bash
  stress-ng --msync 2 --msync-bytes 128M  # 同步128MB映射文件
  ```


#### 内存同步测试
```bash
stress-ng --msync 2 --msync-bytes 64M --timeout 60
```
此命令测试msync系统调用，该调用同步内存映射到磁盘。

### 内存核心测试选项(mincore)
```bash
      --mincore N           start N workers exercising mincore
      --mincore-ops N       stop after N mincore bogo operations
      --mincore-random      randomly select pages rather than linear scan
```
- `--mincore N`  
  测试 `mincore()` 检查内存页驻留状态  
  ```bash
  stress-ng --mincore 1 --mincore-random  # 随机检查内存页
  ```

#### 基本内存核心测试
```bash
stress-ng --mincore 2 --timeout 60
```
此命令测试mincore系统调用，该调用检查页面是否在内存中。

#### 随机内存核心测试
```bash
stress-ng --mincore 2 --mincore-random --timeout 60
```
此命令随机选择页面进行mincore测试，而不是线性扫描。

### 内存建议测试选项(madvise)
```bash
      --madvise N           start N workers exercising madvise on memory
      --madvise-ops N       stop after N bogo madvise operations
```

- `--madvise N`  
  频繁调用 `madvise()` 改变内存管理策略  
  ```bash
  stress-ng --madvise 2 --madvise-ops 5000  # 执行5千次madvise
  ```

```bash
stress-ng --madvise 1 --madvise-hints random --timeout 1m
```  
通过`madvise`系统调用随机设置内存建议策略（如`MADV_RANDOM`）[[9]]。


#### 内存建议测试
```bash
stress-ng --madvise 2 --timeout 60
```
此命令测试madvise系统调用，该调用提供内存使用模式提示。

### 内存耗尽测试选项(oom)
```bash
      --oom-pipe N          start N workers exercising large pipes
      --oom-pipe-ops N      stop after N oom-pipe bogo operations
```

- `--oom-pipe N`  
  创建大管道消耗内存（触发OOM Killer）  
```bash
stress-ng --oom-pipe 2 --oom-pipe-ops 100  # 创建100个大管道
```
  
#### 管道内存耗尽测试
```bash
stress-ng --oom-pipe 2 --timeout 60
```
此命令通过大型管道消耗内存，测试系统在内存不足情况下的行为。

### 用户空间页面错误处理测试选项
```bash
      --userfaultfd N       start N page faulting workers with userspace handling
      --userfaultfd-ops N   stop after N page faults have been handled
```
#### 用户空间页面错误测试
```bash
stress-ng --userfaultfd 2 --timeout 60
```
此命令测试用户空间页面错误处理机制，对高级内存管理很有用。

### 页面故障测试(fault)
```bash
      --fault N             start N workers producing page faults
      --fault-ops N         stop after N page fault bogo operations
```
```bash
# 触发页面故障
stress-ng --fault 2 
```
此命令测试系统处理页面错误的能力。

- **页错误触发**  
  ```bash
  stress-ng --fault 1 --fault-ops 10000
  ```  
  强制触发10,000次页错误，测试页表处理性能。

### NUMA测试选项
```bash
      --numa N              start N workers stressing NUMA interfaces
      --numa-ops N          stop after N NUMA bogo operations
```
- `--numa N`  
  测试NUMA（非统一内存访问）接口压力  
  ```bash
  stress-ng --numa 1 --numa-ops 5000  # 执行5千次NUMA操作
  ```

```bash
stress-ng --numa 2 --numa-membind 0,1 --timeout 2m
```  
在NUMA节点0和1上分配内存，测试跨节点内存访问性能。


#### NUMA接口测试
```bash
stress-ng --numa 2 --timeout 60
```
此命令测试非统一内存访问(NUMA)接口，对多处理器系统很有用。

### 零设备测试选项(zero)
```bash
      --zero N              start N workers reading /dev/zero
      --zero-ops N          stop after N /dev/zero bogo read operations
```

- `--zero N`  
  通过读取 `/dev/zero` 消耗内存  
  ```bash
  stress-ng --zero 4 --zero-ops 1e5  # 读取10万次/dev/zero
  ```


#### 零设备读取测试
```bash
stress-ng --zero 2 --timeout 60
```
此命令测试从/dev/zero读取数据的性能。

### 监控建议
	1. 使用top/htop监控内存使用情况
	2. 通过/proc/meminfo查看详细内存状态
	3. 使用vmstat监控虚拟内存统计
	4. dmesg查看内核日志是否有OOM等错误
	5. 监控工具推荐：`vmstat 1`, `dmesg -w`, `sar -r ALL 1`
