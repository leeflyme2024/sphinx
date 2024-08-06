# 函数调用
```bash
// ti-processor-sdk-linux-rt-am62xx-evm-08.06.00.42/board-support/linux-rt-5.10.168+gitAUTOINC+c1a1291911-gc1a1291911/kernel/rcu/tree_stall.h  
show_rcu_gp_kthreads  
  
check_cpu_stall  
    print_cpu_stall  
        print_cpu_stall_info
        rcu_check_gp_kthread_starvation
	        sched_show_task
        rcu_dump_cpu_stacks  
        panic_on_rcu_stall  
    print_other_cpu_stall  
        print_cpu_stall_info  
        rcu_print_task_stall  
        rcu_dump_cpu_stacks
        rcu_check_gp_kthread_starvation
        panic_on_rcu_stall
```

## rcu_state
```bash
/*
 * RCU global state, including node hierarchy.  This hierarchy is
 * represented in "heap" form in a dense array.  The root (first level)
 * of the hierarchy is in ->node[0] (referenced by ->level[0]), the second
 * level in ->node[1] through ->node[m] (->node[1] referenced by ->level[1]),
 * and the third level in ->node[m+1] and following (->node[m+1] referenced
 * by ->level[2]).  The number of levels is determined by the number of
 * CPUs and by CONFIG_RCU_FANOUT.  Small systems will have a "hierarchy"
 * consisting of a single rcu_node.
 */
struct rcu_state {
	struct rcu_node node[NUM_RCU_NODES];	/* Hierarchy. */
	struct rcu_node *level[RCU_NUM_LVLS + 1];
						/* Hierarchy levels (+1 to */
						/*  shut bogus gcc warning) */
	int ncpus;				/* # CPUs seen so far. */

	/* The following fields are guarded by the root rcu_node's lock. */

	u8	boost ____cacheline_internodealigned_in_smp;
						/* Subject to priority boost. */
	unsigned long gp_seq;			/* Grace-period sequence #. */
	unsigned long gp_max;			/* Maximum GP duration in */
						/*  jiffies. */
	struct task_struct *gp_kthread;		/* Task for grace periods. */
	struct swait_queue_head gp_wq;		/* Where GP task waits. */
	short gp_flags;				/* Commands for GP task. */
	short gp_state;				/* GP kthread sleep state. */
	unsigned long gp_wake_time;		/* Last GP kthread wake. */
	unsigned long gp_wake_seq;		/* ->gp_seq at ^^^. */

	/* End of fields guarded by root rcu_node's lock. */

	struct mutex barrier_mutex;		/* Guards barrier fields. */
	atomic_t barrier_cpu_count;		/* # CPUs waiting on. */
	struct completion barrier_completion;	/* Wake at barrier end. */
	unsigned long barrier_sequence;		/* ++ at start and end of */
						/*  rcu_barrier(). */
	/* End of fields guarded by barrier_mutex. */

	struct mutex exp_mutex;			/* Serialize expedited GP. */
	struct mutex exp_wake_mutex;		/* Serialize wakeup. */
	unsigned long expedited_sequence;	/* Take a ticket. */
	atomic_t expedited_need_qs;		/* # CPUs left to check in. */
	struct swait_queue_head expedited_wq;	/* Wait for check-ins. */
	int ncpus_snap;				/* # CPUs seen last time. */
	u8 cbovld;				/* Callback overload now? */
	u8 cbovldnext;				/* ^        ^  next time? */

	unsigned long jiffies_force_qs;		/* Time at which to invoke */
						/*  force_quiescent_state(). */
	unsigned long jiffies_kick_kthreads;	/* Time at which to kick */
						/*  kthreads, if configured. */
	unsigned long n_force_qs;		/* Number of calls to */
						/*  force_quiescent_state(). */
	unsigned long gp_start;			/* Time at which GP started, */
						/*  but in jiffies. */
	unsigned long gp_end;			/* Time last GP ended, again */
						/*  in jiffies. */
	unsigned long gp_activity;		/* Time of last GP kthread */
						/*  activity in jiffies. */
	unsigned long gp_req_activity;		/* Time of last GP request */
						/*  in jiffies. */
	unsigned long jiffies_stall;		/* Time at which to check */
						/*  for CPU stalls. */
	unsigned long jiffies_resched;		/* Time at which to resched */
						/*  a reluctant CPU. */
	unsigned long n_force_qs_gpstart;	/* Snapshot of n_force_qs at */
						/*  GP start. */
	const char *name;			/* Name of structure. */
	char abbr;				/* Abbreviated name. */

	raw_spinlock_t ofl_lock ____cacheline_internodealigned_in_smp;
						/* Synchronize offline with */
						/*  GP pre-initialization. */
};
```

```bash
static struct rcu_state rcu_state = {
	.level = { &rcu_state.node[0] },
	.gp_state = RCU_GP_IDLE,
	.gp_seq = (0UL - 300UL) << RCU_SEQ_CTR_SHIFT,
	.barrier_mutex = __MUTEX_INITIALIZER(rcu_state.barrier_mutex),
	.name = RCU_NAME,
	.abbr = RCU_ABBR,
	.exp_mutex = __MUTEX_INITIALIZER(rcu_state.exp_mutex),
	.exp_wake_mutex = __MUTEX_INITIALIZER(rcu_state.exp_wake_mutex),
	.ofl_lock = __RAW_SPIN_LOCK_UNLOCKED(rcu_state.ofl_lock),
};
```

### rcu_state.name
```bash
/*
 * In order to export the rcu_state name to the tracing tools, it
 * needs to be added in the __tracepoint_string section.
 * This requires defining a separate variable tp_<sname>_varname
 * that points to the string being used, and this will allow
 * the tracing userspace tools to be able to decipher the string
 * address to the matching string.
 */
#ifdef CONFIG_PREEMPT_RCU
#define RCU_ABBR 'p'
#define RCU_NAME_RAW "rcu_preempt"
#else /* #ifdef CONFIG_PREEMPT_RCU */
#define RCU_ABBR 's'
#define RCU_NAME_RAW "rcu_sched"
#endif /* #else #ifdef CONFIG_PREEMPT_RCU */

#ifndef CONFIG_TRACING
#define RCU_NAME RCU_NAME_RAW
#else /* #ifdef CONFIG_TRACING */
static char rcu_name[] = RCU_NAME_RAW;
static const char *tp_rcu_varname __used __tracepoint_string = rcu_name;
#define RCU_NAME rcu_name
#endif /* #else #ifdef CONFIG_TRACING */
```


## sched_show_task
```c
void sched_show_task(struct task_struct *p)
{
        unsigned long free = 0;
        int ppid;

        if (!try_get_task_stack(p))
                return;

        pr_info("task:%-15.15s state:%c", p->comm, task_state_to_char(p));

        if (p->state == TASK_RUNNING)
                pr_cont("  running task    ");
#ifdef CONFIG_DEBUG_STACK_USAGE
        free = stack_not_used(p);
#endif
        ppid = 0;
        rcu_read_lock();
        if (pid_alive(p))
                ppid = task_pid_nr(rcu_dereference(p->real_parent));
        rcu_read_unlock();
        pr_cont(" stack:%5lu pid:%5d ppid:%6d flags:0x%08lx\n",
                free, task_pid_nr(p), ppid,
                (unsigned long)task_thread_info(p)->flags);

        print_worker_info(KERN_INFO, p);
        show_stack(p, NULL, KERN_INFO);
        put_task_stack(p);
}
EXPORT_SYMBOL_GPL(sched_show_task);

```

# 参考文档

[kernel.org/doc/Documentation/RCU/stallwarn.txt](https://www.kernel.org/doc/Documentation/RCU/stallwarn.txt)
[ltp/testcases/kernel/device-drivers/rcu/rcu\_torture.sh at master · linux-test-project/ltp · GitHub](https://github.com/linux-test-project/ltp/blob/master/testcases/kernel/device-drivers/rcu/rcu_torture.sh)
[RCU concepts — The Linux Kernel documentation](https://docs.kernel.org/RCU/index.html)
[Site Unreachable](https://blog.csdn.net/weixin_45030965/article/details/133278953)
[Linux内核 失速（STALL） 警告说明文档翻译\_rcu stall-CSDN博客](https://blog.csdn.net/lxmega/article/details/122968282)
[Site Unreachable](https://blog.csdn.net/juS3Ve/article/details/80248793)


# 调试方法

## 测试脚本

### 测试脚本1

```bash
# /etc/systemd/system/reboot-automate.service  
[Unit]  
Description=Reboot after boot up  
  
[Service]  
ExecStart=/sbin/reboot  
  
[Install]  
WantedBy=multi-user.target
```

```bash
systemctl enable reboot-automate.service  
systemctl start reboot-automate.service
```


### 测试脚本2
```bash
[Unit]
Description=autorun
After=basic.service
[Service]
ExecStart=/etc/autorun.sh
[Install]
WantedBy=multi-user.target
```

```bash
#!/bin/sh

if [ -e boottimes ];then  
boottimes=`cat boottimes`  
fi

if [ -z "$boottimes" ];then  
boottimes=0  
fi

let boottimes=boottimes+1  
echo boot $boottimes times > /dev/ttyS2  
echo $boottimes > boottimes

sleep 30s  
reboot
```

### 测试脚本3
```bash
for i in {1..23}; do memtester 50m | grep FAIL >> /tmp/memtest.log & done && \
   sleep 1 && 
   echo $(expr $(free | awk '/^Mem:/{print $4}') / 1024)MB Free
```

## config
```bash
#
# RCU Subsystem
#
CONFIG_TREE_RCU=y
CONFIG_PREEMPT_RCU=y
# CONFIG_RCU_EXPERT is not set
CONFIG_SRCU=y
CONFIG_TREE_SRCU=y
CONFIG_TASKS_RCU_GENERIC=y
CONFIG_TASKS_RCU=y
CONFIG_TASKS_TRACE_RCU=y
CONFIG_RCU_STALL_COMMON=y
CONFIG_RCU_NEED_SEGCBLIST=y
CONFIG_RCU_BOOST=y
CONFIG_RCU_BOOST_DELAY=500
# end of RCU Subsystem
```

```bash
#
# RCU Debugging
#
# CONFIG_RCU_SCALE_TEST is not set
# CONFIG_RCU_TORTURE_TEST is not set
# CONFIG_RCU_REF_SCALE_TEST is not set
CONFIG_RCU_CPU_STALL_TIMEOUT=21
CONFIG_RCU_TRACE=y
# CONFIG_RCU_EQS_DEBUG is not set
# end of RCU Debugging
```

![[Pasted image 20240805155932.png]]

## 启动 log
```bash
[    0.000000] rcu: Preemptible hierarchical RCU implementation.
[    0.000000] rcu:     RCU event tracing is enabled.
[    0.000000] rcu:     RCU restricting CPUs from NR_CPUS=256 to nr_cpu_ids=2.
[    0.000000] rcu:     RCU priority boosting: priority 1 delay 500 ms.
[    0.000000] rcu:     RCU_SOFTIRQ processing moved to rcuc kthreads.
[    0.000000]  No expedited grace period (rcu_normal_after_boot).
[    0.000000]  Trampoline variant of Tasks RCU enabled.
[    0.000000]  Tracing variant of Tasks RCU enabled.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 100 jiffies.
[    0.000000] rcu: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=2

...

[    0.002656] rcu: Hierarchical SRCU implementation.
```

## 线程
### ps
```bash
root@am62xx:/# ps
  PID USER       VSZ STAT COMMAND
    1 root      9780 S    {systemd} /sbin/init
    2 root         0 SW   [kthreadd]
    3 root         0 IW<  [rcu_gp]
    4 root         0 IW<  [rcu_par_gp]
    6 root         0 IW<  [kworker/0:0H-kb]
    8 root         0 IW<  [mm_percpu_wq]
    9 root         0 SW   [rcu_tasks_kthre]
   10 root         0 SW   [rcu_tasks_trace]
   11 root         0 SW   [ksoftirqd/0]
   12 root         0 IW   [rcu_preempt]
   13 root         0 SW   [rcub/0]
   14 root         0 SW   [rcuc/0]
   15 root         0 SW   [migration/0]
   16 root         0 SW   [irq_work/0]
   17 root         0 SW   [cpuhp/0]
   18 root         0 SW   [cpuhp/1]
   19 root         0 SW   [irq_work/1]
   20 root         0 SW   [migration/1]
   21 root         0 SW   [rcuc/1]
   22 root         0 SW   [ksoftirqd/1]
   25 root         0 SW   [kdevtmpfs]
   26 root         0 IW<  [netns]
   28 root         0 SW   [oom_reaper]
   29 root         0 IW<  [writeback]
   30 root         0 SW   [kcompactd0]
   31 root         0 SWN  [ksmd]
   40 root         0 IW<  [cryptd]
   62 root         0 IW<  [kintegrityd]
   63 root         0 IW<  [kblockd]
   64 root         0 IW<  [blkcg_punt_bio]
   67 root         0 IW<  [tpm_dev_wq]
   68 root         0 IW<  [edac-poller]
   69 root         0 IW<  [devfreq_wq]
   70 root         0 SW   [watchdogd]
   73 root         0 IW<  [kworker/0:1H-kb]
   74 root         0 IW<  [rpciod]
   75 root         0 IW<  [kworker/u5:0]
   76 root         0 IW<  [xprtiod]
  104 root         0 SW   [kswapd0]
  105 root         0 IW<  [nfsiod]
  107 root         0 IW<  [kpcitest]
  108 root         0 IW<  [kpcintb]
  109 root         0 IW<  [vfio-irqfd-clea]
  110 root         0 IW<  [goodix_wq]
  112 root         0 SW   [irq/21-4d000000]
  129 root         0 IW   [kworker/0:16-ev]
  130 root         0 IW   [kworker/0:17-ev]
  132 root         0 SW   [irq/54-gpmc]
  133 root         0 SW   [irq/16-4900000.]
  134 root         0 SW   [irq/19-2b200000]
  135 root         0 SW   [irq/37-20000000]
  136 root         0 SW   [irq/38-20010000]
  137 root         0 SW   [irq/39-20020000]
  138 root         0 SW   [pr/ttyS2]
  139 root         0 SW   [irq/55-25010000]
  140 root         0 SW   [irq/44-fc40000.]
  141 root         0 SW   [irq/56-485c0100]
  142 root         0 SW   [irq/17-4b00000.]
  143 root         0 SW   [spi0]
  144 root         0 SW   [irq/40-20100000]
  145 root         0 SW   [spi2]
  148 root         0 SW   [ptp0]
  149 root         0 SW   [irq/267-8000000]
  150 root         0 SW   [irq/144-8000000]
  151 root         0 SW   [irq/234-8000000]
  152 root         0 SW   [irq/270-xhci-hc]
  153 root         0 SW   [irq/271-xhci-hc]
  154 root         0 IW<  [sdhci]
  155 root         0 SW   [irq/53-2b10000.]
  156 root         0 SW   [irq/52-2b10000.]
  160 root         0 SW   [irq/86-485c0100]
  161 root         0 SW   [irq/41-mmc0]
  162 root         0 IW<  [sdhci]
  163 root         0 SW   [irq/74-485c0100]
  164 root         0 SW   [irq/42-mmc1]
  165 root         0 SW   [irq/42-s-mmc1]
  166 root         0 SW   [irq/41-s-mmc0]
  167 root         0 SW   [irq/122-485c010]
  168 root         0 SW   [irq/104-485c010]
  169 root         0 IW<  [mmc_complete]
  171 root         0 IW<  [mmc_complete]
  172 root         0 SW   [scsi_eh_0]
  173 root         0 IW<  [scsi_tmf_0]
  174 root         0 SW   [usb-storage]
  175 root         0 SW   [scsi_eh_1]
  176 root         0 IW<  [scsi_tmf_1]
  177 root         0 SW   [usb-storage]
  178 root         0 SW   [irq/368-gt9xx]
  179 root         0 SW   [card0-crtc0]
  180 root         0 SW   [irq/45-tidss]
  181 root         0 IW<  [sdhci]
  182 root         0 SW   [irq/43-mmc2]
  183 root         0 SW   [irq/43-s-mmc2]
  184 root         0 SW   [irq/350-powerdo]
  185 root         0 SW   [irq/30-2800000.]
  223 root         0 IW<  [kworker/1:3H-kb]
  327 root         0 SW   [jbd2/mmcblk0p2-]
  328 root         0 IW<  [ext4-rsv-conver]
  331 root         0 SW<  [loop0]
  378 root         0 SW   [jbd2/mmcblk0p3-]
  379 root         0 IW<  [ext4-rsv-conver]
  397 root         0 IW<  [ipv6_addrconf]
  420 rpc       3680 S    /usr/sbin/rpcbind -w -f
  421 root     17440 S    /lib/systemd/systemd-journald
  429 root         0 IW<  [cryptodev_queue]
  440 root     13600 S    /lib/systemd/systemd-udevd
  453 systemd- 81068 S    /lib/systemd/systemd-timesyncd
  457 messageb  4508 S    /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
  461 root     78356 S    /usr/sbin/irqbalance --foreground
  482 root      2532 S    /usr/sbin/tee-supplicant
  487 root         0 IW<  [optee_bus_scan]
  490 systemd-  7688 S    /lib/systemd/systemd-networkd
  494 root         0 SW   [irq/51-23110000]
  506 root      7116 S    /lib/systemd/systemd-logind
  507 root         0 SW   [hwrng]
  512 systemd-  7124 S    /lib/systemd/systemd-resolved
  513 root         0 SW   [irq/123-485c010]
  515 root         0 SW   [irq/105-485c010]
  517 root         0 SW   [irq/124-485c010]
  520 root         0 SW   [irq/106-485c010]
  521 root         0 SW   [irq/125-485c010]
  522 root      2784 S    /usr/sbin/telnetd
  523 root         0 SW   [irq/107-485c010]
  524 root         0 SW   [irq/126-485c010]
  526 root         0 SW   [irq/108-485c010]
  533 root         0 SW   [irq/47-mbox-m4-]
  543 avahi     4828 S    avahi-daemon: running [am62xx.local]
  545 avahi     4700 S    avahi-daemon: chroot helper
  550 rpcuser   3132 S    /usr/sbin/rpc.statd -F
  560 root     11924 S    /usr/sbin/snmpd -Ls0-6d -a -f
  590 root      2036 S    /sbin/agetty -o -p -- \u --noclear tty1 linux
  594 root      4532 S    /bin/login -f
  611 root         0 SW   [irq/47-mbox-r5-]
  613 root         0 IW   [kworker/1:6-pm]
  624 root         0 IW<  [cfg80211]
 1065 root         0 SW   [jbd2/mmcblk1p2-]
 1067 root         0 IW<  [ext4-rsv-conver]
 1127 root      8736 S    /lib/systemd/systemd --user
 1135 root     10984 S    (sd-pam)
 1231 root      4444 S    -sh
 1279 root      2728 S    /usr/libexec/ipsec/starter --daemon charon --nofork
 1282 root      3108 S    /usr/sbin/netserver
 1287 root      460m S    /usr/libexec/ipsec/charon
 1365 root         0 IW   [kworker/1:0-cgr]
 1385 root         0 IW<  [kworker/1:1H-kb]
 1661 root         0 IW   [kworker/u4:3-go]
 1662 root         0 IW   [kworker/u4:0-ev]
 1664 root         0 IW   [kworker/u4:4-ev]
 1665 root         0 IW   [kworker/u4:1-ev]
```

- `PID`: 进程 ID。
- `USER`: 拥有该进程的用户。
- `VSZ`: 进程使用的虚拟内存大小。
- `STAT`: 进程状态标识。常见标识包括：
  - `R`: 运行状态。
  - `S`: 休眠状态。
  - `I`: 空闲状态。
  - `D`: 不可中断状态。
  - `T`: 停止或追踪状态。
  - `Z`: 僵尸状态。
  - `W`：正在被交换（过时且很少见的状态，表示进程没有足够的内存并且被交换到磁盘中，通常表示的是进程等待某种资源）。

### rcu
```bash
root@am62xx:/# ps |grep rcu
    3 root         0 IW<  [rcu_gp]
    4 root         0 IW<  [rcu_par_gp]
    9 root         0 SW   [rcu_tasks_kthre]
   10 root         0 SW   [rcu_tasks_trace]
   12 root         0 IW   [rcu_preempt]
   13 root         0 SW   [rcub/0]
   14 root         0 SW   [rcuc/0]
   21 root         0 SW   [rcuc/1]
```

#### rcu_tasks_trace
```bash
root@am62xx:/# ps |grep rcu_tasks_trace
   10 root         0 SW   [rcu_tasks_trace]
```
##### rcu_tasks
```c
/**
 * Definition for a Tasks-RCU-like mechanism.
 * @cbs_head: Head of callback list.
 * @cbs_tail: Tail pointer for callback list.
 * @cbs_wq: Wait queue allowning new callback to get kthread's attention.
 * @cbs_lock: Lock protecting callback list.
 * @kthread_ptr: This flavor's grace-period/callback-invocation kthread.
 * @gp_func: This flavor's grace-period-wait function.
 * @gp_state: Grace period's most recent state transition (debugging).
 * @gp_sleep: Per-grace-period sleep to prevent CPU-bound looping.
 * @init_fract: Initial backoff sleep interval.
 * @gp_jiffies: Time of last @gp_state transition.
 * @gp_start: Most recent grace-period start in jiffies.
 * @n_gps: Number of grace periods completed since boot.
 * @n_ipis: Number of IPIs sent to encourage grace periods to end.
 * @n_ipis_fails: Number of IPI-send failures.
 * @pregp_func: This flavor's pre-grace-period function (optional).
 * @pertask_func: This flavor's per-task scan function (optional).
 * @postscan_func: This flavor's post-task scan function (optional).
 * @holdout_func: This flavor's holdout-list scan function (optional).
 * @postgp_func: This flavor's post-grace-period function (optional).
 * @call_func: This flavor's call_rcu()-equivalent function.
 * @name: This flavor's textual name.
 * @kname: This flavor's kthread name.
 */
struct rcu_tasks {
	struct rcu_head *cbs_head;
	struct rcu_head **cbs_tail;
	struct wait_queue_head cbs_wq;
	raw_spinlock_t cbs_lock;
	int gp_state;
	int gp_sleep;
	int init_fract;
	unsigned long gp_jiffies;
	unsigned long gp_start;
	unsigned long n_gps;
	unsigned long n_ipis;
	unsigned long n_ipis_fails;
	struct task_struct *kthread_ptr;
	rcu_tasks_gp_func_t gp_func;
	pregp_func_t pregp_func;
	pertask_func_t pertask_func;
	postscan_func_t postscan_func;
	holdouts_func_t holdouts_func;
	postgp_func_t postgp_func;
	call_rcu_func_t call_func;
	char *name;
	char *kname; // 内核线程的名字
};
```

```c
#define DEFINE_RCU_TASKS(rt_name, gp, call, n)				\
static struct rcu_tasks rt_name =					\
{									\
	.cbs_tail = &rt_name.cbs_head,					\
	.cbs_wq = __WAIT_QUEUE_HEAD_INITIALIZER(rt_name.cbs_wq),	\
	.cbs_lock = __RAW_SPIN_LOCK_UNLOCKED(rt_name.cbs_lock),		\
	.gp_func = gp,							\
	.call_func = call,						\
	.name = n,							\
	.kname = #rt_name,						\
}
```

```c
void call_rcu_tasks_trace(struct rcu_head *rhp, rcu_callback_t func);
DEFINE_RCU_TASKS(rcu_tasks_trace, rcu_tasks_wait_gp, call_rcu_tasks_trace,
		 "RCU Tasks Trace");
```

##### rcu_spawn_tasks_kthread_generic
```c
/* Spawn RCU-tasks grace-period kthread. */
static void __init rcu_spawn_tasks_kthread_generic(struct rcu_tasks *rtp)
{
	struct task_struct *t;

	t = kthread_run(rcu_tasks_kthread, rtp, "%s_kthread", rtp->kname);
	if (WARN_ONCE(IS_ERR(t), "%s: Could not start %s grace-period kthread, OOM is now expected behavior\n", __func__, rtp->name))
		return;
	smp_mb(); /* Ensure others see full kthread. */
}
```

##### kthread_run
```c
/**
 * kthread_run - create and wake a thread.
 * @threadfn: the function to run until signal_pending(current).
 * @data: data ptr for @threadfn.
 * @namefmt: printf-style name for the thread.
 *
 * Description: Convenient wrapper for kthread_create() followed by
 * wake_up_process().  Returns the kthread or ERR_PTR(-ENOMEM).
 */
#define kthread_run(threadfn, data, namefmt, ...)			   \
({									   \
	struct task_struct *__k						   \
		= kthread_create(threadfn, data, namefmt, ## __VA_ARGS__); \
	if (!IS_ERR(__k))						   \
		wake_up_process(__k);					   \
	__k;								   \
})
```
`kthread_run` 是 Linux 内核编程中使用的一个宏，用于创建并启动一个内核线程（kernel thread）。这个宏是内核线程编程API的一部分，允许开发者创建一个线程，该线程可以执行指定的函数，并且可以指定线程的优先级。

具体来说，`kthread_run` 的使用通常如下所示：
```c
kthread_run(thread_function, data, "thread_name");
```
其中：
- `thread_function` 是线程要执行的函数。
- `data` 是传递给线程函数的参数。
- `"thread_name"` 是线程的名称，用于调试目的。

当 `kthread_run` 被调用时，它会创建一个新的内核线程，并将指定的函数和数据作为参数传递给这个线程。然后，这个线程会被唤醒并开始执行。这个宏的返回值通常是一个指向新线程的指针，如果创建失败，则返回错误代码。

### irq
```bash

root@am62xx:/# ps |grep irq
   11 root         0 SW   [ksoftirqd/0]
   16 root         0 SW   [irq_work/0]
   19 root         0 SW   [irq_work/1]
   22 root         0 SW   [ksoftirqd/1]
  109 root         0 IW<  [vfio-irqfd-clea]
  112 root         0 SW   [irq/21-4d000000]
  132 root         0 SW   [irq/54-gpmc]
  133 root         0 SW   [irq/16-4900000.]
  134 root         0 SW   [irq/19-2b200000]
  135 root         0 SW   [irq/37-20000000]
  136 root         0 SW   [irq/38-20010000]
  137 root         0 SW   [irq/39-20020000]
  139 root         0 SW   [irq/55-25010000]
  140 root         0 SW   [irq/44-fc40000.]
  141 root         0 SW   [irq/56-485c0100]
  142 root         0 SW   [irq/17-4b00000.]
  144 root         0 SW   [irq/40-20100000]
  149 root         0 SW   [irq/267-8000000]
  150 root         0 SW   [irq/144-8000000]
  151 root         0 SW   [irq/234-8000000]
  152 root         0 SW   [irq/270-xhci-hc]
  153 root         0 SW   [irq/271-xhci-hc]
  155 root         0 SW   [irq/53-2b10000.]
  156 root         0 SW   [irq/52-2b10000.]
  160 root         0 SW   [irq/86-485c0100]
  161 root         0 SW   [irq/41-mmc0]
  163 root         0 SW   [irq/74-485c0100]
  164 root         0 SW   [irq/42-mmc1]
  165 root         0 SW   [irq/42-s-mmc1]
  166 root         0 SW   [irq/41-s-mmc0]
  167 root         0 SW   [irq/122-485c010]
  168 root         0 SW   [irq/104-485c010]
  178 root         0 SW   [irq/368-gt9xx]
  180 root         0 SW   [irq/45-tidss]
  182 root         0 SW   [irq/43-mmc2]
  183 root         0 SW   [irq/43-s-mmc2]
  184 root         0 SW   [irq/350-powerdo]
  185 root         0 SW   [irq/30-2800000.]
  461 root     78356 S    /usr/sbin/irqbalance --foreground
  494 root         0 SW   [irq/51-23110000]
  513 root         0 SW   [irq/123-485c010]
  515 root         0 SW   [irq/105-485c010]
  517 root         0 SW   [irq/124-485c010]
  520 root         0 SW   [irq/106-485c010]
  521 root         0 SW   [irq/125-485c010]
  523 root         0 SW   [irq/107-485c010]
  524 root         0 SW   [irq/126-485c010]
  526 root         0 SW   [irq/108-485c010]
  533 root         0 SW   [irq/47-mbox-m4-]
  611 root         0 SW   [irq/47-mbox-r5-]
```

#### 分类
下面是根据模块名称进行分类的中断线程列表：

##### `Mailbox`
- setup_irq_thread:1363 `irq/21-4d000000.mailbox thr_012`
```bash
[    0.599491] lwy setup_irq_thread:1363 irq/21-4d000000.mailbox thr_012
[    0.599512] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.599520] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.599529] Workqueue: events deferred_probe_work_func
[    0.599553] Call trace:
[    0.599555]  dump_backtrace+0x0/0x1b0
[    0.599570]  show_stack+0x18/0x38
[    0.599577]  dump_stack+0xe8/0x124
[    0.599589]  setup_irq_thread+0x58/0x17c
[    0.599597]  __setup_irq+0x6e0/0x728
[    0.599606]  request_threaded_irq+0xe8/0x1a8
[    0.599612]  ti_msgmgr_queue_startup+0x138/0x210
[    0.599624]  mbox_request_channel+0x1c4/0x220
[    0.599632]  mbox_request_channel_byname+0xb8/0x148
[    0.599639]  ti_sci_probe+0x2e4/0xb88
[    0.599647]  platform_drv_probe+0x54/0xa8
[    0.599655]  really_probe+0xec/0x400
[    0.599661]  driver_probe_device+0x58/0xb8
[    0.599667]  __device_attach_driver+0xb8/0xe0
[    0.599673]  bus_for_each_drv+0x78/0xc8
[    0.599679]  __device_attach+0xf8/0x188
[    0.599684]  device_initial_probe+0x14/0x20
[    0.599690]  bus_probe_device+0x9c/0xa8
[    0.599695]  deferred_probe_work_func+0x88/0xc0
[    0.599701]  process_one_work+0x1a0/0x348
[    0.599712]  worker_thread+0x1f8/0x440
[    0.599718]  kthread+0x174/0x198
[    0.599727]  ret_from_fork+0x10/0x30
```

##### `GPMC`
- setup_irq_thread:1363 `irq/54-gpmc`
```bash
[    0.679977] lwy setup_irq_thread:1363 irq/54-gpmc
[    0.679986] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.679993] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.680001] Workqueue: events deferred_probe_work_func
[    0.680021] Call trace:
[    0.680022]  dump_backtrace+0x0/0x1b0
[    0.680035]  show_stack+0x18/0x38
[    0.680042]  dump_stack+0xe8/0x124
[    0.680052]  setup_irq_thread+0x58/0x17c
[    0.680059]  __setup_irq+0x6e0/0x728
[    0.680067]  request_threaded_irq+0xe8/0x1a8
[    0.680073]  gpmc_probe+0x578/0x610
[    0.680082]  platform_drv_probe+0x54/0xa8
[    0.680089]  really_probe+0xec/0x400
[    0.680095]  driver_probe_device+0x58/0xb8
[    0.680101]  __device_attach_driver+0xb8/0xe0
[    0.680107]  bus_for_each_drv+0x78/0xc8
[    0.680112]  __device_attach+0xf8/0x188
[    0.680118]  device_initial_probe+0x14/0x20
[    0.680123]  bus_probe_device+0x9c/0xa8 ##
[    0.680128]  deferred_probe_work_func+0x88/0xc0
[    0.680134]  process_one_work+0x1a0/0x348
[    0.680143]  worker_thread+0x1f8/0x440
[    0.680150]  kthread+0x174/0x198
[    0.680157]  ret_from_fork+0x10/0x30
```

##### `I2C`
- setup_irq_thread:1363 `irq/16-4900000.i2c`
- setup_irq_thread:1363 `irq/19-2b200000.i2c`
- setup_irq_thread:1363 `irq/37-20000000.i2c`
- setup_irq_thread:1363 `irq/38-20010000.i2c`
- setup_irq_thread:1363 `irq/39-20020000.i2c`

```bash
[    0.681395] lwy setup_irq_thread:1363 irq/16-4900000.i2c
[    0.681409] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.681415] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.681422] Workqueue: events deferred_probe_work_func
[    0.681440] Call trace:
[    0.681442]  dump_backtrace+0x0/0x1b0
[    0.681454]  show_stack+0x18/0x38
[    0.681461]  dump_stack+0xe8/0x124
[    0.681471]  setup_irq_thread+0x58/0x17c
[    0.681479]  __setup_irq+0x6e0/0x728
[    0.681487]  request_threaded_irq+0xe8/0x1a8
[    0.681493]  devm_request_threaded_irq+0x78/0xf8
[    0.681502]  omap_i2c_probe+0x53c/0x740
[    0.681512]  platform_drv_probe+0x54/0xa8
[    0.681520]  really_probe+0xec/0x400
[    0.681525]  driver_probe_device+0x58/0xb8
[    0.681531]  __device_attach_driver+0xb8/0xe0
[    0.681537]  bus_for_each_drv+0x78/0xc8
[    0.681542]  __device_attach+0xf8/0x188
[    0.681548]  device_initial_probe+0x14/0x20
[    0.681553]  bus_probe_device+0x9c/0xa8 ##
[    0.681559]  deferred_probe_work_func+0x88/0xc0
[    0.681565]  process_one_work+0x1a0/0x348
[    0.681573]  worker_thread+0x1f8/0x440
[    0.681580]  kthread+0x174/0x198
[    0.681588]  ret_from_fork+0x10/0x30
```


1. `[    0.681409] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5`
   - 出问题的CPU是1号核。
   - 进程ID（PID）是74，进程名是`kworker/1:6`。
   - 系统版本是`5.10.168-rt83-gc1a1291911`，未修改（Not tainted）。

2. `[    0.681415] Hardware name: ZHIYUAN Electronics AM6232 (DT)`
   - 硬件名称是“ZHIYUAN Electronics AM6232”，使用设备树（DT）。

3. `[    0.681422] Workqueue: events deferred_probe_work_func`
   - 事件是通过`deferred_probe_work_func`工作队列处理的。


##### `SPI`
- setup_irq_thread:1363 `irq/44-fc40000.spi`
- setup_irq_thread:1363 `irq/17-4b00000.spi`
- setup_irq_thread:1363 `irq/40-20100000.spi`

```bash
[    0.784771] lwy setup_irq_thread:1363 irq/44-fc40000.spi
[    0.784785] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.784791] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.784799] Workqueue: events deferred_probe_work_func
[    0.784819] Call trace:
[    0.784820]  dump_backtrace+0x0/0x1b0
[    0.784832]  show_stack+0x18/0x38
[    0.784839]  dump_stack+0xe8/0x124
[    0.784849]  setup_irq_thread+0x58/0x17c
[    0.784857]  __setup_irq+0x6e0/0x728
[    0.784866]  request_threaded_irq+0xe8/0x1a8
[    0.784873]  devm_request_threaded_irq+0x78/0xf8
[    0.784881]  cqspi_probe+0x32c/0x9e8
[    0.784890]  platform_drv_probe+0x54/0xa8
[    0.784897]  really_probe+0xec/0x400
[    0.784902]  driver_probe_device+0x58/0xb8
[    0.784908]  __device_attach_driver+0xb8/0xe0
[    0.784914]  bus_for_each_drv+0x78/0xc8
[    0.784919]  __device_attach+0xf8/0x188
[    0.784925]  device_initial_probe+0x14/0x20
[    0.784931]  bus_probe_device+0x9c/0xa8 ## 
[    0.784936]  deferred_probe_work_func+0x88/0xc0
[    0.784942]  process_one_work+0x1a0/0x348
[    0.784951]  worker_thread+0x1f8/0x440
[    0.784958]  kthread+0x174/0x198
[    0.784965]  ret_from_fork+0x10/0x30
```

```bash
[    0.797827] lwy setup_irq_thread:1363 irq/17-4b00000.spi
[    0.797843] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.797850] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.797859] Workqueue: events deferred_probe_work_func
[    0.797882] Call trace:
[    0.797884]  dump_backtrace+0x0/0x1b0
[    0.797898]  show_stack+0x18/0x38
[    0.797906]  dump_stack+0xe8/0x124
[    0.797917]  setup_irq_thread+0x58/0x17c
[    0.797925]  __setup_irq+0x6e0/0x728
[    0.797935]  request_threaded_irq+0xe8/0x1a8
[    0.797941]  devm_request_threaded_irq+0x78/0xf8
[    0.797951]  omap2_mcspi_probe+0x3a0/0x548
[    0.797960]  platform_drv_probe+0x54/0xa8
[    0.797968]  really_probe+0xec/0x400
[    0.797974]  driver_probe_device+0x58/0xb8
[    0.797981]  __device_attach_driver+0xb8/0xe0
[    0.797987]  bus_for_each_drv+0x78/0xc8
[    0.797992]  __device_attach+0xf8/0x188
[    0.797997]  device_initial_probe+0x14/0x20
[    0.798003]  bus_probe_device+0x9c/0xa8 ## 
[    0.798009]  deferred_probe_work_func+0x88/0xc0
[    0.798014]  process_one_work+0x1a0/0x348
[    0.798025]  worker_thread+0x1f8/0x440
[    0.798032]  kthread+0x174/0x198
[    0.798040]  ret_from_fork+0x10/0x30
```

##### `ECC`
- setup_irq_thread:1363 `irq/55-25010000.ecc`

```bash
[    0.777534] lwy setup_irq_thread:1363 irq/55-25010000.ecc
[    0.777553] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.777561] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.777570] Workqueue: events deferred_probe_work_func
[    0.777594] Call trace:
[    0.777595]  dump_backtrace+0x0/0x1b0
[    0.777610]  show_stack+0x18/0x38
[    0.777617]  dump_stack+0xe8/0x124
[    0.777629]  setup_irq_thread+0x58/0x17c
[    0.777637]  __setup_irq+0x6e0/0x728
[    0.777646]  request_threaded_irq+0xe8/0x1a8
[    0.777653]  devm_request_threaded_irq+0x78/0xf8
[    0.777663]  elm_probe+0xa0/0x1a0
[    0.777669]  platform_drv_probe+0x54/0xa8
[    0.777677]  really_probe+0xec/0x400
[    0.777683]  driver_probe_device+0x58/0xb8
[    0.777690]  __device_attach_driver+0xb8/0xe0
[    0.777695]  bus_for_each_drv+0x78/0xc8
[    0.777701]  __device_attach+0xf8/0x188
[    0.777706]  device_initial_probe+0x14/0x20
[    0.777712]  bus_probe_device+0x9c/0xa8 ##
[    0.777717]  deferred_probe_work_func+0x88/0xc0
[    0.777723]  process_one_work+0x1a0/0x348
[    0.777734]  worker_thread+0x1f8/0x440
[    0.777741]  kthread+0x174/0x198
[    0.777749]  ret_from_fork+0x10/0x30
```

##### `DMA Controller`
- setup_irq_thread:1363 `irq/56-485c0100.dma-controller chan0`
- setup_irq_thread:1363 `irq/74-485c0100.dma-controller chan1`
- setup_irq_thread:1363 `irq/86-485c0100.dma-controller chan1`
- setup_irq_thread:1363 `irq/122-485c0100.dma-controller chan2`
- setup_irq_thread:1363 `irq/104-485c0100.dma-controller chan2`

###### chan0
```bash
[    0.789219] lwy setup_irq_thread:1363 irq/56-485c0100.dma-controller chan0
[    0.789229] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.789236] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.789241] Workqueue: events deferred_probe_work_func
[    0.789255] Call trace:
[    0.789257]  dump_backtrace+0x0/0x1b0
[    0.789265]  show_stack+0x18/0x38
[    0.789273]  dump_stack+0xe8/0x124
[    0.789281]  setup_irq_thread+0x58/0x17c
[    0.789288]  __setup_irq+0x6e0/0x728
[    0.789296]  request_threaded_irq+0xe8/0x1a8
[    0.789302]  bcdma_alloc_chan_resources+0x2c4/0x7a0
[    0.789313]  dma_chan_get+0xa8/0x1f8
[    0.789322]  find_candidate+0xfc/0x1a8
[    0.789328]  __dma_request_channel+0x84/0xd8
[    0.789335]  dma_request_chan_by_mask+0x28/0x90
[    0.789342]  cqspi_probe+0x710/0x9e8
[    0.789350]  platform_drv_probe+0x54/0xa8
[    0.789357]  really_probe+0xec/0x400
[    0.789363]  driver_probe_device+0x58/0xb8
[    0.789368]  __device_attach_driver+0xb8/0xe0
[    0.789374]  bus_for_each_drv+0x78/0xc8
[    0.789379]  __device_attach+0xf8/0x188
[    0.789384]  device_initial_probe+0x14/0x20
[    0.789391]  bus_probe_device+0x9c/0xa8 ##
[    0.789396]  deferred_probe_work_func+0x88/0xc0
[    0.789402]  process_one_work+0x1a0/0x348
[    0.789410]  worker_thread+0x1f8/0x440
[    0.789416]  kthread+0x174/0x198
[    0.789423]  ret_from_fork+0x10/0x30
```

###### chan1
```bash
[    0.989229] lwy setup_irq_thread:1363 irq/74-485c0100.dma-controller chan1
[    0.989238] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.989245] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.989250] Workqueue: events deferred_probe_work_func
[    0.989261] Call trace:
[    0.989263]  dump_backtrace+0x0/0x1b0
[    0.989271]  show_stack+0x18/0x38
[    0.989278]  dump_stack+0xe8/0x124
[    0.989286]  setup_irq_thread+0x58/0x17c
[    0.989294]  __setup_irq+0x6e0/0x728
[    0.989301]  request_threaded_irq+0xe8/0x1a8
[    0.989307]  bcdma_alloc_chan_resources+0x56c/0x7a0
[    0.989317]  dma_chan_get+0xa8/0x1f8
[    0.989325]  find_candidate+0xfc/0x1a8
[    0.989331]  __dma_request_channel+0x84/0xd8
[    0.989338]  udma_of_xlate+0x84/0x110
[    0.989345]  of_dma_request_slave_channel+0x16c/0x2a0
[    0.989352]  dma_request_chan+0x3c/0x2f8
[    0.989359]  davinci_mcasp_probe+0x574/0xc90
[    0.989368]  platform_drv_probe+0x54/0xa8
[    0.989375]  really_probe+0xec/0x400
[    0.989381]  driver_probe_device+0x58/0xb8
[    0.989387]  __device_attach_driver+0xb8/0xe0
[    0.989393]  bus_for_each_drv+0x78/0xc8
[    0.989398]  __device_attach+0xf8/0x188
[    0.989405]  device_initial_probe+0x14/0x20
[    0.989411]  bus_probe_device+0x9c/0xa8 ##
[    0.989416]  deferred_probe_work_func+0x88/0xc0
[    0.989422]  process_one_work+0x1a0/0x348
[    0.989429]  worker_thread+0x1f8/0x440
[    0.989436]  kthread+0x174/0x198
[    0.989443]  ret_from_fork+0x10/0x30
```

###### chan1
```bash
[    0.997237] lwy setup_irq_thread:1363 irq/86-485c0100.dma-controller chan1
[    0.997255] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.997263] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.997271] Workqueue: events deferred_probe_work_func
[    0.997292] Call trace:
[    0.997294]  dump_backtrace+0x0/0x1b0
[    0.997305]  show_stack+0x18/0x38
[    0.997313]  dump_stack+0xe8/0x124
[    0.997323]  setup_irq_thread+0x58/0x17c
[    0.997330]  __setup_irq+0x6e0/0x728
[    0.997339]  request_threaded_irq+0xe8/0x1a8
[    0.997345]  bcdma_alloc_chan_resources+0x2c4/0x7a0
[    0.997355]  dma_chan_get+0xa8/0x1f8
[    0.997364]  find_candidate+0xfc/0x1a8
[    0.997371]  __dma_request_channel+0x84/0xd8
[    0.997377]  udma_of_xlate+0x84/0x110
[    0.997384]  of_dma_request_slave_channel+0x16c/0x2a0
[    0.997391]  dma_request_chan+0x3c/0x2f8
[    0.997398]  snd_dmaengine_pcm_register+0xa8/0x210
[    0.997406]  devm_snd_dmaengine_pcm_register+0x50/0xa8
[    0.997412]  udma_pcm_platform_register+0x1c/0x28
[    0.997418]  davinci_mcasp_probe+0xa1c/0xc90
[    0.997425]  platform_drv_probe+0x54/0xa8
[    0.997432]  really_probe+0xec/0x400
[    0.997438]  driver_probe_device+0x58/0xb8
[    0.997444]  __device_attach_driver+0xb8/0xe0
[    0.997450]  bus_for_each_drv+0x78/0xc8
[    0.997456]  __device_attach+0xf8/0x188
[    0.997461]  device_initial_probe+0x14/0x20
[    0.997467]  bus_probe_device+0x9c/0xa8 ##
[    0.997473]  deferred_probe_work_func+0x88/0xc0
[    0.997479]  process_one_work+0x1a0/0x348
[    0.997488]  worker_thread+0x1f8/0x440
[    0.997496]  kthread+0x174/0x198
[    0.997503]  ret_from_fork+0x10/0x30
```

###### chan2
```bash
[    1.004835] lwy setup_irq_thread:1363 irq/122-485c0100.dma-controller chan2
[    1.004849] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    1.004856] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    1.004862] Workqueue: events deferred_probe_work_func
[    1.004878] Call trace:
[    1.004879]  dump_backtrace+0x0/0x1b0
[    1.004890]  show_stack+0x18/0x38
[    1.004897]  dump_stack+0xe8/0x124
[    1.004907]  setup_irq_thread+0x58/0x17c
[    1.004914]  __setup_irq+0x6e0/0x728
[    1.004922]  request_threaded_irq+0xe8/0x1a8
[    1.004929]  bcdma_alloc_chan_resources+0x2c4/0x7a0
[    1.004937]  dma_chan_get+0xa8/0x1f8
[    1.004946]  find_candidate+0xfc/0x1a8
[    1.004952]  __dma_request_channel+0x84/0xd8
[    1.004959]  udma_of_xlate+0x84/0x110
[    1.004965]  of_dma_request_slave_channel+0x16c/0x2a0
[    1.004972]  dma_request_chan+0x3c/0x2f8
[    1.004979]  snd_dmaengine_pcm_register+0xa8/0x210
[    1.004986]  devm_snd_dmaengine_pcm_register+0x50/0xa8
[    1.004992]  udma_pcm_platform_register+0x1c/0x28
[    1.004998]  davinci_mcasp_probe+0xa1c/0xc90
[    1.005005]  platform_drv_probe+0x54/0xa8
[    1.005013]  really_probe+0xec/0x400
[    1.005018]  driver_probe_device+0x58/0xb8
[    1.005025]  __device_attach_driver+0xb8/0xe0
[    1.005030]  bus_for_each_drv+0x78/0xc8
[    1.005036]  __device_attach+0xf8/0x188
[    1.005041]  device_initial_probe+0x14/0x20
[    1.005047]  bus_probe_device+0x9c/0xa8 ##
[    1.005053]  deferred_probe_work_func+0x88/0xc0
[    1.005059]  process_one_work+0x1a0/0x348
[    1.005068]  worker_thread+0x1f8/0x440
[    1.005075]  kthread+0x174/0x198
[    1.005082]  ret_from_fork+0x10/0x30
```


###### chan2

```bash
[    1.005506] lwy setup_irq_thread:1363 irq/104-485c0100.dma-controller chan2
[    1.005512] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    1.005519] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    1.005523] Workqueue: events deferred_probe_work_func
[    1.005533] Call trace:
[    1.005535]  dump_backtrace+0x0/0x1b0
[    1.005542]  show_stack+0x18/0x38
[    1.005549]  dump_stack+0xe8/0x124
[    1.005557]  setup_irq_thread+0x58/0x17c
[    1.005563]  __setup_irq+0x6e0/0x728
[    1.005570]  request_threaded_irq+0xe8/0x1a8
[    1.005577]  bcdma_alloc_chan_resources+0x56c/0x7a0
[    1.005584]  dma_chan_get+0xa8/0x1f8
[    1.005591]  find_candidate+0xfc/0x1a8
[    1.005597]  __dma_request_channel+0x84/0xd8
[    1.005603]  udma_of_xlate+0x84/0x110
[    1.005609]  of_dma_request_slave_channel+0x16c/0x2a0
[    1.005616]  dma_request_chan+0x3c/0x2f8
[    1.005623]  snd_dmaengine_pcm_register+0xa8/0x210
[    1.005630]  devm_snd_dmaengine_pcm_register+0x50/0xa8
[    1.005636]  udma_pcm_platform_register+0x1c/0x28
[    1.005642]  davinci_mcasp_probe+0xa1c/0xc90
[    1.005648]  platform_drv_probe+0x54/0xa8
[    1.005655]  really_probe+0xec/0x400
[    1.005660]  driver_probe_device+0x58/0xb8
[    1.005666]  __device_attach_driver+0xb8/0xe0
[    1.005673]  bus_for_each_drv+0x78/0xc8
[    1.005678]  __device_attach+0xf8/0x188
[    1.005683]  device_initial_probe+0x14/0x20
[    1.005689]  bus_probe_device+0x9c/0xa8 ##
[    1.005695]  deferred_probe_work_func+0x88/0xc0
[    1.005701]  process_one_work+0x1a0/0x348
[    1.005708]  worker_thread+0x1f8/0x440
[    1.005715]  kthread+0x174/0x198
[    1.005722]  ret_from_fork+0x10/0x30
```

##### `Ethernet`
- setup_irq_thread:1363 `irq/144-8000000.ethernet-tx0`
- setup_irq_thread:1363 `irq/234-8000000.ethernet`
- setup_irq_thread:1363 `irq/267-8000000.ethernet`

###### tx
```bash
[    0.860464] lwy setup_irq_thread:1363 irq/144-8000000.ethernet-tx0
[    0.860474] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.860481] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.860487] Workqueue: events deferred_probe_work_func
[    0.860503] Call trace:
[    0.860505]  dump_backtrace+0x0/0x1b0
[    0.860515]  show_stack+0x18/0x38
[    0.860522]  dump_stack+0xe8/0x124
[    0.860531]  setup_irq_thread+0x58/0x17c
[    0.860539]  __setup_irq+0x6e0/0x728
[    0.860547]  request_threaded_irq+0xe8/0x1a8
[    0.860553]  devm_request_threaded_irq+0x78/0xf8
[    0.860562]  am65_cpsw_nuss_init_tx_chns+0x2b0/0x3c8
[    0.860568]  am65_cpsw_nuss_register_ndevs+0x30/0x338
[    0.860573]  am65_cpsw_nuss_probe+0x9a0/0xd60
[    0.860579]  platform_drv_probe+0x54/0xa8
[    0.860586]  really_probe+0xec/0x400
[    0.860591]  driver_probe_device+0x58/0xb8
[    0.860597]  __device_attach_driver+0xb8/0xe0
[    0.860603]  bus_for_each_drv+0x78/0xc8
[    0.860609]  __device_attach+0xf8/0x188
[    0.860614]  device_initial_probe+0x14/0x20
[    0.860620]  bus_probe_device+0x9c/0xa8 ## 
[    0.860625]  deferred_probe_work_func+0x88/0xc0
[    0.860631]  process_one_work+0x1a0/0x348
[    0.860640]  worker_thread+0x1f8/0x440
[    0.860647]  kthread+0x174/0x198
[    0.860654]  ret_from_fork+0x10/0x30
```

###### rx
```bash
[    0.861821] lwy setup_irq_thread:1363 irq/234-8000000.ethernet
[    0.861829] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.861837] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.861843] Workqueue: events deferred_probe_work_func
[    0.861856] Call trace:
[    0.861857]  dump_backtrace+0x0/0x1b0
[    0.861867]  show_stack+0x18/0x38
[    0.861874]  dump_stack+0xe8/0x124
[    0.861883]  setup_irq_thread+0x58/0x17c
[    0.861891]  __setup_irq+0x6e0/0x728
[    0.861898]  request_threaded_irq+0xe8/0x1a8
[    0.861905]  devm_request_threaded_irq+0x78/0xf8
[    0.861912]  am65_cpsw_nuss_init_rx_chns+0x20c/0x398
[    0.861919]  am65_cpsw_nuss_register_ndevs+0x6c/0x338
[    0.861925]  am65_cpsw_nuss_probe+0x9a0/0xd60
[    0.861930]  platform_drv_probe+0x54/0xa8
[    0.861937]  really_probe+0xec/0x400
[    0.861943]  driver_probe_device+0x58/0xb8
[    0.861949]  __device_attach_driver+0xb8/0xe0
[    0.861955]  bus_for_each_drv+0x78/0xc8
[    0.861960]  __device_attach+0xf8/0x188
[    0.861966]  device_initial_probe+0x14/0x20
[    0.861972]  bus_probe_device+0x9c/0xa8 ##
[    0.861977]  deferred_probe_work_func+0x88/0xc0
[    0.861983]  process_one_work+0x1a0/0x348
[    0.861992]  worker_thread+0x1f8/0x440
[    0.861999]  kthread+0x174/0x198
[    0.862006]  ret_from_fork+0x10/0x30
```

###### cpts
```bash
[    0.859336] lwy setup_irq_thread:1363 irq/267-8000000.ethernet
[    0.859345] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.859353] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.859361] Workqueue: events deferred_probe_work_func
[    0.859384] Call trace:
[    0.859386]  dump_backtrace+0x0/0x1b0
[    0.859400]  show_stack+0x18/0x38
[    0.859407]  dump_stack+0xe8/0x124
[    0.859419]  setup_irq_thread+0x58/0x17c
[    0.859427]  __setup_irq+0x6e0/0x728
[    0.859436]  request_threaded_irq+0xe8/0x1a8
[    0.859443]  devm_request_threaded_irq+0x78/0xf8
[    0.859452]  am65_cpts_create+0x45c/0x668
[    0.859462]  am65_cpsw_nuss_probe+0x5b4/0xd60
[    0.859468]  platform_drv_probe+0x54/0xa8
[    0.859476]  really_probe+0xec/0x400
[    0.859481]  driver_probe_device+0x58/0xb8
[    0.859487]  __device_attach_driver+0xb8/0xe0
[    0.859493]  bus_for_each_drv+0x78/0xc8
[    0.859498]  __device_attach+0xf8/0x188
[    0.859504]  device_initial_probe+0x14/0x20
[    0.859510]  bus_probe_device+0x9c/0xa8 ##
[    0.859515]  deferred_probe_work_func+0x88/0xc0
[    0.859521]  process_one_work+0x1a0/0x348
[    0.859531]  worker_thread+0x1f8/0x440
[    0.859539]  kthread+0x174/0x198
[    0.859547]  ret_from_fork+0x10/0x30
```


##### `USB`
- setup_irq_thread:1363 `irq/270-xhci-hcd:usb1`
- setup_irq_thread:1363 `irq/271-xhci-hcd:usb3`

###### usb1
```bash
[    0.867021] lwy setup_irq_thread:1363 irq/270-xhci-hcd:usb1
[    0.867029] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.867036] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.867045] Workqueue: events deferred_probe_work_func
[    0.867067] Call trace:
[    0.867069]  dump_backtrace+0x0/0x1b0
[    0.867082]  show_stack+0x18/0x38
[    0.867090]  dump_stack+0xe8/0x124
[    0.867100]  setup_irq_thread+0x58/0x17c
[    0.867108]  __setup_irq+0x6e0/0x728
[    0.867117]  request_threaded_irq+0xe8/0x1a8
[    0.867124]  usb_add_hcd+0x478/0x670
[    0.867135]  xhci_plat_probe+0x5e4/0x720
[    0.867145]  platform_drv_probe+0x54/0xa8
[    0.867153]  really_probe+0xec/0x400
[    0.867158]  driver_probe_device+0x58/0xb8
[    0.867164]  __device_attach_driver+0xb8/0xe0
[    0.867170]  bus_for_each_drv+0x78/0xc8
[    0.867175]  __device_attach+0xf8/0x188
[    0.867181]  device_initial_probe+0x14/0x20
[    0.867187]  bus_probe_device+0x9c/0xa8
[    0.867193]  device_add+0x350/0x728
[    0.867203]  platform_device_add+0x100/0x238
[    0.867210]  dwc3_host_init+0x20c/0x318
[    0.867218]  dwc3_probe+0xd30/0xea0
[    0.867223]  platform_drv_probe+0x54/0xa8
[    0.867230]  really_probe+0xec/0x400
[    0.867236]  driver_probe_device+0x58/0xb8
[    0.867242]  __device_attach_driver+0xb8/0xe0
[    0.867248]  bus_for_each_drv+0x78/0xc8
[    0.867253]  __device_attach+0xf8/0x188
[    0.867259]  device_initial_probe+0x14/0x20
[    0.867265]  bus_probe_device+0x9c/0xa8
[    0.867277]  of_device_add+0x54/0x68
[    0.867287]  of_platform_device_create_pdata+0x98/0x128
[    0.867294]  of_platform_bus_create+0x180/0x3d8
[    0.867302]  of_platform_populate+0x58/0xf8
[    0.867309]  dwc3_ti_probe+0x290/0x400
[    0.867316]  platform_drv_probe+0x54/0xa8
[    0.867323]  really_probe+0xec/0x400
[    0.867328]  driver_probe_device+0x58/0xb8
[    0.867334]  __device_attach_driver+0xb8/0xe0
[    0.867340]  bus_for_each_drv+0x78/0xc8
[    0.867346]  __device_attach+0xf8/0x188
[    0.867352]  device_initial_probe+0x14/0x20
[    0.867358]  bus_probe_device+0x9c/0xa8 ##
[    0.867363]  deferred_probe_work_func+0x88/0xc0
[    0.867369]  process_one_work+0x1a0/0x348
[    0.867379]  worker_thread+0x1f8/0x440
[    0.867387]  kthread+0x174/0x198
[    0.867394]  ret_from_fork+0x10/0x30
```

###### usb3
```bash
[    0.872230] lwy setup_irq_thread:1363 irq/271-xhci-hcd:usb3
[    0.872238] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.872245] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.872253] Workqueue: events deferred_probe_work_func
[    0.872276] Call trace:
[    0.872277]  dump_backtrace+0x0/0x1b0
[    0.872290]  show_stack+0x18/0x38
[    0.872297]  dump_stack+0xe8/0x124
[    0.872309]  setup_irq_thread+0x58/0x17c
[    0.872317]  __setup_irq+0x6e0/0x728
[    0.872326]  request_threaded_irq+0xe8/0x1a8
[    0.872333]  usb_add_hcd+0x478/0x670
[    0.872344]  xhci_plat_probe+0x5e4/0x720
[    0.872353]  platform_drv_probe+0x54/0xa8
[    0.872361]  really_probe+0xec/0x400
[    0.872367]  driver_probe_device+0x58/0xb8
[    0.872373]  __device_attach_driver+0xb8/0xe0
[    0.872379]  bus_for_each_drv+0x78/0xc8
[    0.872384]  __device_attach+0xf8/0x188
[    0.872390]  device_initial_probe+0x14/0x20
[    0.872395]  bus_probe_device+0x9c/0xa8
[    0.872401]  device_add+0x350/0x728
[    0.872411]  platform_device_add+0x100/0x238
[    0.872417]  dwc3_host_init+0x20c/0x318
[    0.872427]  dwc3_probe+0xd30/0xea0
[    0.872432]  platform_drv_probe+0x54/0xa8
[    0.872439]  really_probe+0xec/0x400
[    0.872445]  driver_probe_device+0x58/0xb8
[    0.872451]  __device_attach_driver+0xb8/0xe0
[    0.872456]  bus_for_each_drv+0x78/0xc8
[    0.872462]  __device_attach+0xf8/0x188
[    0.872467]  device_initial_probe+0x14/0x20
[    0.872473]  bus_probe_device+0x9c/0xa8
[    0.872478]  device_add+0x350/0x728
[    0.872486]  of_device_add+0x54/0x68
[    0.872496]  of_platform_device_create_pdata+0x98/0x128
[    0.872503]  of_platform_bus_create+0x180/0x3d8
[    0.872510]  of_platform_populate+0x58/0xf8
[    0.872518]  dwc3_ti_probe+0x290/0x400
[    0.872524]  platform_drv_probe+0x54/0xa8
[    0.872532]  really_probe+0xec/0x400
[    0.872537]  driver_probe_device+0x58/0xb8
[    0.872543]  __device_attach_driver+0xb8/0xe0
[    0.872549]  bus_for_each_drv+0x78/0xc8
[    0.872554]  __device_attach+0xf8/0x188
[    0.872560]  device_initial_probe+0x14/0x20
[    0.872566]  bus_probe_device+0x9c/0xa8 ##
[    0.872571]  deferred_probe_work_func+0x88/0xc0
[    0.872578]  process_one_work+0x1a0/0x348
[    0.872588]  worker_thread+0x1f8/0x440
[    0.872595]  kthread+0x174/0x198
[    0.872603]  ret_from_fork+0x10/0x30
```


##### `McASP`
- setup_irq_thread:1363 `irq/52-2b10000.mcasp_tx`
- setup_irq_thread:1363 `irq/53-2b10000.mcasp_rx`

```bash
[    0.677175] lwy setup_irq_thread:1363 irq/52-2b10000.mcasp_tx
[    0.677181] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    0.677188] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    0.677194] Workqueue: events deferred_probe_work_func
[    0.677206] Call trace:
[    0.677208]  dump_backtrace+0x0/0x1b0
[    0.677216]  show_stack+0x18/0x38
[    0.677223]  dump_stack+0xe8/0x124
[    0.677231]  setup_irq_thread+0x58/0x17c
[    0.677238]  __setup_irq+0x6e0/0x728
[    0.677246]  request_threaded_irq+0xe8/0x1a8
[    0.677252]  devm_request_threaded_irq+0x78/0xf8
[    0.677260]  davinci_mcasp_probe+0x268/0xc90
[    0.677268]  platform_drv_probe+0x54/0xa8
[    0.677275]  really_probe+0xec/0x400
[    0.677281]  driver_probe_device+0x58/0xb8
[    0.677286]  __device_attach_driver+0xb8/0xe0
[    0.677292]  bus_for_each_drv+0x78/0xc8
[    0.677298]  __device_attach+0xf8/0x188
[    0.677303]  device_initial_probe+0x14/0x20
[    0.677309]  bus_probe_device+0x9c/0xa8 ##
[    0.677315]  deferred_probe_work_func+0x88/0xc0
[    0.677320]  process_one_work+0x1a0/0x348
[    0.677328]  worker_thread+0x1f8/0x440
[    0.677335]  kthread+0x174/0x198
[    0.677342]  ret_from_fork+0x10/0x30
```


##### `MMC/SD`
- setup_irq_thread:1363 `irq/41-mmc0`
- setup_irq_thread:1363 `irq/42-mmc1`
- setup_irq_thread:1363 `irq/43-mmc2`

```bash
[    2.749298] lwy setup_irq_thread:1363 irq/43-mmc2
[    2.749316] CPU: 1 PID: 76 Comm: kworker/u4:1 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    2.749323] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    2.749332] Workqueue: events_unbound async_run_entry_fn
[    2.749361] Call trace:
[    2.749363]  dump_backtrace+0x0/0x1b0
[    2.749377]  show_stack+0x18/0x38
[    2.749385]  dump_stack+0xe8/0x124
[    2.749397]  setup_irq_thread+0x58/0x17c
[    2.749405]  __setup_irq+0x6e0/0x728
[    2.749413]  request_threaded_irq+0xe8/0x1a8
[    2.749420]  __sdhci_add_host+0xf4/0x330
[    2.749428]  sdhci_am654_probe+0x698/0x770
[    2.749433]  platform_drv_probe+0x54/0xa8
[    2.749445]  really_probe+0xec/0x400
[    2.749450]  driver_probe_device+0x58/0xb8
[    2.749456]  __device_attach_driver+0xb8/0xe0
[    2.749462]  bus_for_each_drv+0x78/0xc8 ## --
[    2.749468]  __device_attach_async_helper+0xbc/0xe0
[    2.749477]  async_run_entry_fn+0x44/0x120
[    2.749483]  process_one_work+0x1a0/0x348
[    2.749493]  worker_thread+0x4c/0x440
[    2.749501]  kthread+0x174/0x198
[    2.749508]  ret_from_fork+0x10/0x30
```


##### `Touchscreen (GT9xx)`
- setup_irq_thread:1363 `irq/341-gt9xx`
- setup_irq_thread:1363 `irq/368-gt9xx`

```bash
[    1.146016] lwy setup_irq_thread:1363 irq/341-gt9xx
[    1.146026] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    1.146034] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    1.146043] Workqueue: events deferred_probe_work_func
[    1.146066] Call trace:
[    1.146068]  dump_backtrace+0x0/0x1b0
[    1.146082]  show_stack+0x18/0x38
[    1.146090]  dump_stack+0xe8/0x124
[    1.146101]  setup_irq_thread+0x58/0x17c
[    1.146109]  __setup_irq+0x6e0/0x728
[    1.146119]  request_threaded_irq+0xe8/0x1a8
[    1.146125]  goodix_ts_probe+0x618/0x6f4
[    1.146134]  i2c_device_probe+0xa8/0x2b0
[    1.146143]  really_probe+0xec/0x400
[    1.146149]  driver_probe_device+0x58/0xb8
[    1.146155]  __device_attach_driver+0xb8/0xe0
[    1.146161]  bus_for_each_drv+0x78/0xc8
[    1.146167]  __device_attach+0xf8/0x188
[    1.146173]  device_initial_probe+0x14/0x20
[    1.146179]  bus_probe_device+0x9c/0xa8 ##
[    1.146185]  deferred_probe_work_func+0x88/0xc0
[    1.146190]  process_one_work+0x1a0/0x348
[    1.146201]  worker_thread+0x1f8/0x440
[    1.146208]  kthread+0x174/0x198
[    1.146217]  ret_from_fork+0x10/0x30
```


##### `TIDSS`
- setup_irq_thread:1363 `irq/45-tidss`

```bash
[    2.676666] lwy setup_irq_thread:1363 irq/45-tidss
[    2.676700] CPU: 1 PID: 74 Comm: kworker/1:6 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    2.676708] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    2.676716] Workqueue: events deferred_probe_work_func
[    2.676737] Call trace:
[    2.676740]  dump_backtrace+0x0/0x1b0
[    2.676753]  show_stack+0x18/0x38
[    2.676761]  dump_stack+0xe8/0x124
[    2.676772]  setup_irq_thread+0x58/0x17c
[    2.676780]  __setup_irq+0x6e0/0x728
[    2.676789]  request_threaded_irq+0xe8/0x1a8
[    2.676795]  drm_irq_install+0x84/0x118
[    2.676806]  tidss_probe+0xa4/0x180
[    2.676814]  platform_drv_probe+0x54/0xa8
[    2.676822]  really_probe+0xec/0x400
[    2.676828]  driver_probe_device+0x58/0xb8
[    2.676834]  __device_attach_driver+0xb8/0xe0
[    2.676840]  bus_for_each_drv+0x78/0xc8
[    2.676845]  __device_attach+0xf8/0x188
[    2.676851]  device_initial_probe+0x14/0x20
[    2.676856]  bus_probe_device+0x9c/0xa8 ##
[    2.676862]  deferred_probe_work_func+0x88/0xc0
[    2.676868]  process_one_work+0x1a0/0x348
[    2.676878]  worker_thread+0x1f8/0x440
[    2.676886]  kthread+0x174/0x198
[    2.676894]  ret_from_fork+0x10/0x30
```


##### `Powerdown`
- setup_irq_thread:1363 `irq/350-powerdown_irq`

```bash
[    2.764541] lwy setup_irq_thread:1363 irq/350-powerdown_irq
[    2.764553] CPU: 1 PID: 1 Comm: swapper/0 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    2.764560] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    2.764566] Call trace:
[    2.764568]  dump_backtrace+0x0/0x1b0
[    2.764592]  show_stack+0x18/0x38
[    2.764600]  dump_stack+0xe8/0x124
[    2.764612]  setup_irq_thread+0x58/0x17c
[    2.764620]  __setup_irq+0x6e0/0x728
[    2.764629]  request_threaded_irq+0xe8/0x1a8
[    2.764636]  devm_request_threaded_irq+0x78/0xf8
[    2.764646]  powerdown_probe+0x134/0x248
[    2.764657]  platform_drv_probe+0x54/0xa8
[    2.764667]  really_probe+0xec/0x400
[    2.764673]  driver_probe_device+0x58/0xb8
[    2.764678]  device_driver_attach+0x74/0x80
[    2.764684]  __driver_attach+0x68/0xf8
[    2.764690]  bus_for_each_dev+0x70/0xc0 ##--
[    2.764696]  driver_attach+0x24/0x30
[    2.764702]  bus_add_driver+0x150/0x210
[    2.764709]  driver_register+0x64/0x120
[    2.764715]  __platform_driver_register+0x48/0x58
[    2.764722]  powerdown_init+0x1c/0x28
[    2.764733]  do_one_initcall+0x54/0x1b8
[    2.764739]  kernel_init_freeable+0x230/0x298
[    2.764749]  kernel_init+0x14/0x114
[    2.764755]  ret_from_fork+0x10/0x30
```

##### `Serial`
- setup_irq_thread:1363 `irq/30-2800000.serial`

```bash
[    2.785859] lwy setup_irq_thread:1363 irq/30-2800000.serial
[    2.785873] CPU: 1 PID: 1 Comm: swapper/0 Not tainted 5.10.168-rt83-gc1a1291911 #5
[    2.785879] Hardware name: ZHIYUAN Electronics AM6232 (DT)
[    2.785886] Call trace:
[    2.785888]  dump_backtrace+0x0/0x1b0
[    2.785907]  show_stack+0x18/0x38
[    2.785915]  dump_stack+0xe8/0x124
[    2.785933]  setup_irq_thread+0x58/0x17c
[    2.785942]  __setup_irq+0x6e0/0x728
[    2.785951]  request_threaded_irq+0xe8/0x1a8
[    2.785958]  omap_8250_startup+0x18c/0x218
[    2.785966]  serial8250_startup+0x18/0x38
[    2.785975]  uart_startup.part.0+0x13c/0x348
[    2.785981]  uart_port_activate+0x60/0xa8
[    2.785986]  tty_port_open+0x90/0x130
[    2.785995]  uart_open+0x1c/0x30
[    2.786004]  tty_open+0x110/0x580
[    2.786013]  chrdev_open+0xa4/0x1a0
[    2.786020]  do_dentry_open+0x12c/0x3e0
[    2.786029]  vfs_open+0x2c/0x38
[    2.786035]  path_openat+0x818/0xcd0
[    2.786051]  do_filp_open+0x7c/0x100
[    2.786058]  file_open_name+0xd0/0x190
[    2.786064]  filp_open+0x38/0x78
[    2.786070]  console_on_rootfs+0x24/0x6c
[    2.786080]  kernel_init_freeable+0x258/0x298
[    2.786087]  kernel_init+0x14/0x114
[    2.786094]  ret_from_fork+0x10/0x30
```

#### 中断号
[【原创】Linux中断子系统（一）-中断控制器及驱动分析 - LoyenWang - 博客园](https://www.cnblogs.com/LoyenWang/p/12996812.html)
[【原创】Linux中断子系统（二）-通用框架处理 - LoyenWang - 博客园](https://www.cnblogs.com/LoyenWang/p/13052677.html)


#### 函数调用
```c
request_threaded_irq/setup_percpu_irq/__request_percpu_irq/request_percpu_nmi/request_nmi/
	__setup_irq
		setup_irq_thread
```

#### struct irqaction
```c
/**
 * struct irqaction - per interrupt action descriptor
 * @handler:	interrupt handler function
 * @name:	name of the device
 * @dev_id:	cookie to identify the device
 * @percpu_dev_id:	cookie to identify the device
 * @next:	pointer to the next irqaction for shared interrupts
 * @irq:	interrupt number
 * @flags:	flags (see IRQF_* above)
 * @thread_fn:	interrupt handler function for threaded interrupts
 * @thread:	thread pointer for threaded interrupts
 * @secondary:	pointer to secondary irqaction (force threading)
 * @thread_flags:	flags related to @thread
 * @thread_mask:	bitmask for keeping track of @thread activity
 * @dir:	pointer to the proc/irq/NN/name entry
 */
struct irqaction {
	irq_handler_t		handler;
	void			*dev_id;
	void __percpu		*percpu_dev_id;
	struct irqaction	*next;
	irq_handler_t		thread_fn;
	struct task_struct	*thread;
	struct irqaction	*secondary; //
	unsigned int		irq;
	unsigned int		flags;
	unsigned long		thread_flags;
	unsigned long		thread_mask;
	const char		*name; // 名字
	struct proc_dir_entry	*dir;
} ____cacheline_internodealigned_in_smp;
```

#### setup_irq_thread
```c
// kernel/irq/manage.c
static int setup_irq_thread(struct irqaction *new, unsigned int irq, bool secondary)
{
	struct task_struct *t;

	if (!secondary) {
		printk("\n\nlwy %s:%d irq/%d-%s\n", __func__, __LINE__, irq, new->name);
		dump_stack();
		printk("\n\n");
		t = kthread_create(irq_thread, new, "irq/%d-%s", irq,
				   new->name);
	} else {
		printk("\n\nlwy %s:%d irq/%d-s-%s\n", __func__, __LINE__, irq, new->name);
		dump_stack();
		printk("\n\n");
		t = kthread_create(irq_thread, new, "irq/%d-s-%s", irq,
				   new->name);
	}

	if (IS_ERR(t))
		return PTR_ERR(t);

	/*
	 * We keep the reference to the task struct even if
	 * the thread dies to avoid that the interrupt code
	 * references an already freed task_struct.
	 */
	new->thread = get_task_struct(t);
	/*
	 * Tell the thread to set its affinity. This is
	 * important for shared interrupt handlers as we do
	 * not invoke setup_affinity() for the secondary
	 * handlers as everything is already set up. Even for
	 * interrupts marked with IRQF_NO_BALANCE this is
	 * correct as we want the thread to move to the cpu(s)
	 * on which the requesting code placed the interrupt.
	 */
	set_bit(IRQTF_AFFINITY, &new->thread_flags);
	return 0;
}
```

#### kthread_create
```c
/**
 * kthread_create - create a kthread on the current node
 * @threadfn: the function to run in the thread
 * @data: data pointer for @threadfn()
 * @namefmt: printf-style format string for the thread name
 * @arg...: arguments for @namefmt.
 *
 * This macro will create a kthread on the current node, leaving it in
 * the stopped state.  This is just a helper for kthread_create_on_node();
 * see the documentation there for more details.
 */
#define kthread_create(threadfn, data, namefmt, arg...) \
	kthread_create_on_node(threadfn, data, NUMA_NO_NODE, namefmt, ##arg)  
```

#### kthread_create_on_node 
```c
/**
 * kthread_create_on_node - create a kthread.
 * @threadfn: the function to run until signal_pending(current).
 * @data: data ptr for @threadfn.
 * @node: task and thread structures for the thread are allocated on this node
 * @namefmt: printf-style name for the thread.
 *
 * Description: This helper function creates and names a kernel
 * thread.  The thread will be stopped: use wake_up_process() to start
 * it.  See also kthread_run().  The new thread has SCHED_NORMAL policy and
 * is affine to all CPUs.
 *
 * If thread is going to be bound on a particular cpu, give its node
 * in @node, to get NUMA affinity for kthread stack, or else give NUMA_NO_NODE.
 * When woken, the thread will run @threadfn() with @data as its
 * argument. @threadfn() can either call do_exit() directly if it is a
 * standalone thread for which no one will call kthread_stop(), or
 * return when 'kthread_should_stop()' is true (which means
 * kthread_stop() has been called).  The return value should be zero
 * or a negative error number; it will be passed to kthread_stop().
 *
 * Returns a task_struct or ERR_PTR(-ENOMEM) or ERR_PTR(-EINTR).
 */
struct task_struct *kthread_create_on_node(int (*threadfn)(void *data),
					   void *data, int node,
					   const char namefmt[],
					   ...)
{
	struct task_struct *task;
	va_list args;

	va_start(args, namefmt);
	task = __kthread_create_on_node(threadfn, data, node, namefmt, args);
	va_end(args);

	return task;
}
EXPORT_SYMBOL(kthread_create_on_node);
```

#### __kthread_create_on_node 

```c
static __printf(4, 0)
struct task_struct *__kthread_create_on_node(int (*threadfn)(void *data),
						    void *data, int node,
						    const char namefmt[],
						    va_list args)
{
	DECLARE_COMPLETION_ONSTACK(done);
	struct task_struct *task;
	struct kthread_create_info *create = kmalloc(sizeof(*create),
						     GFP_KERNEL);

	if (!create)
		return ERR_PTR(-ENOMEM);
	create->threadfn = threadfn;
	create->data = data;
	create->node = node;
	create->done = &done;

	spin_lock(&kthread_create_lock);
	list_add_tail(&create->list, &kthread_create_list);
	spin_unlock(&kthread_create_lock);

	wake_up_process(kthreadd_task);
	/*
	 * Wait for completion in killable state, for I might be chosen by
	 * the OOM killer while kthreadd is trying to allocate memory for
	 * new kernel thread.
	 */
	if (unlikely(wait_for_completion_killable(&done))) {
		/*
		 * If I was SIGKILLed before kthreadd (or new kernel thread)
		 * calls complete(), leave the cleanup of this structure to
		 * that thread.
		 */
		if (xchg(&create->done, NULL))
			return ERR_PTR(-EINTR);
		/*
		 * kthreadd (or new kernel thread) will call complete()
		 * shortly.
		 */
		wait_for_completion(&done);
	}
	task = create->result;
	if (!IS_ERR(task)) {
		static const struct sched_param param = { .sched_priority = 0 };

		// include/linux/sched.h
		// #define TASK_COMM_LEN 16
		char name[TASK_COMM_LEN]; // TASK_COMM_LEN 限制irq的显示名称的长度

		/*
		 * task is already visible to other tasks, so updating
		 * COMM must be protected.
		 */
		vsnprintf(name, sizeof(name), namefmt, args);
		set_task_comm(task, name);
		/*
		 * root may have changed our (kthreadd's) priority or CPU mask.
		 * The kernel thread should not inherit these properties.
		 */
		sched_setscheduler_nocheck(task, SCHED_NORMAL, &param);
		set_cpus_allowed_ptr(task,
				     housekeeping_cpumask(HK_FLAG_KTHREAD));
	}
	kfree(create);
	return task;
}
```

### kworker
```bash
root@am62xx:/# ps |grep kworker
    6 root         0 IW<  [kworker/0:0H-kb]
   73 root         0 IW<  [kworker/0:1H-kb]
   75 root         0 IW<  [kworker/u5:0]
  129 root         0 IW   [kworker/0:16-ev]
  130 root         0 IW   [kworker/0:17-ev]
  223 root         0 IW<  [kworker/1:3H-kb]
  613 root         0 IW   [kworker/1:6-pm]
 1365 root         0 IW   [kworker/1:0-cgr]
 1385 root         0 IW<  [kworker/1:1H-kb]
 1662 root         0 IW   [kworker/u4:0-ev]
 1691 root         0 IW   [kworker/u4:3-ev]
 1692 root         0 IW   [kworker/u4:1-go]
 1697 root         0 IW   [kworker/u4:2-ev]
```

## sysfs
### srcutree
```bash
root@am62xx:/sys/module/srcutree/parameters# cat counter_wrap_check
4611686018427387903

root@am62xx:/sys/module/srcutree/parameters# cat exp_holdoff
25000
```

这些参数用于调节 SRCU（Sleepable RCU）的行为：
1. **counter_wrap_check (4611686018427387903)**: SRCU 计数器包装检查值。
2. **exp_holdoff (25000)**: SRCU 延迟时间（纳秒）。

### rcutree
```bash
root@am62xx:/sys/module/rcutree/parameters# ll
-r--r--r--    1 root     root          4096 Oct 25 14:01 blimit
-r--r--r--    1 root     root          4096 Oct 25 14:01 dump_tree
-r--r--r--    1 root     root          4096 Oct 25 14:01 gp_cleanup_delay
-r--r--r--    1 root     root          4096 Oct 25 14:01 gp_init_delay
-r--r--r--    1 root     root          4096 Oct 25 14:01 gp_preinit_delay
-rw-r--r--    1 root     root          4096 Oct 25 14:01 jiffies_till_first_fqs
-rw-r--r--    1 root     root          4096 Oct 25 14:01 jiffies_till_next_fqs
-r--r--r--    1 root     root          4096 Oct 25 14:01 jiffies_till_sched_qs
-r--r--r--    1 root     root          4096 Oct 25 14:01 jiffies_to_sched_qs
-r--r--r--    1 root     root          4096 Oct 25 14:01 kthread_prio
-r--r--r--    1 root     root          4096 Oct 25 14:01 qhimark
-r--r--r--    1 root     root          4096 Oct 25 14:01 qlowmark
-r--r--r--    1 root     root          4096 Oct 25 14:01 qovld
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_divisor
-r--r--r--    1 root     root          4096 Oct 25 14:01 rcu_fanout_exact
-r--r--r--    1 root     root          4096 Oct 25 14:01 rcu_fanout_leaf
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_kick_kthreads
-r--r--r--    1 root     root          4096 Oct 25 14:01 rcu_min_cached_objs
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_resched_ns
-r--r--r--    1 root     root          4096 Oct 25 14:01 sysrq_rcu

root@am62xx:/sys/module/rcutree/parameters# cat blimit
10

root@am62xx:/sys/module/rcutree/parameters# cat dump_tree
N

root@am62xx:/sys/module/rcutree/parameters# cat gp_cleanup_delay
0

root@am62xx:/sys/module/rcutree/parameters# cat gp_init_delay
0

root@am62xx:/sys/module/rcutree/parameters# cat gp_preinit_delay
0

root@am62xx:/sys/module/rcutree/parameters# cat jiffies_till_first_fqs
3

root@am62xx:/sys/module/rcutree/parameters# cat jiffies_till_next_fqs
3
root@am62xx:/sys/module/rcutree/parameters# cat jiffies_till_sched_qs
18446744073709551615

root@am62xx:/sys/module/rcutree/parameters# cat jiffies_to_sched_qs
100

root@am62xx:/sys/module/rcutree/parameters# cat kthread_prio
1

root@am62xx:/sys/module/rcutree/parameters# cat qhimark
10000

root@am62xx:/sys/module/rcutree/parameters# cat qlowmark
100

root@am62xx:/sys/module/rcutree/parameters# cat qovld
20000

root@am62xx:/sys/module/rcutree/parameters# cat rcu_divisor
7

root@am62xx:/sys/module/rcutree/parameters# cat rcu_fanout_exact
N

root@am62xx:/sys/module/rcutree/parameters# cat rcu_fanout_leaf
16

root@am62xx:/sys/module/rcutree/parameters# cat rcu_kick_kthreads
N

root@am62xx:/sys/module/rcutree/parameters# cat rcu_min_cached_objs
5

root@am62xx:/sys/module/rcutree/parameters# cat rcu_resched_ns
3000000

root@am62xx:/sys/module/rcutree/parameters# cat sysrq_rcu
N

```

这些参数用于调节 RCU 机制在 Linux 内核中的行为：

1. **blimit (10)**: 每个批次要处理的最大回调数量。调整这个值可以控制批处理的规模。
2. **dump_tree (N)**: 是否在 RCU 超时时转储 RCU 树。默认为 'N'（否）。
3. **gp_cleanup_delay (0), gp_init_delay (0), gp_preinit_delay (0)**: 这些参数控制 RCU 的各种延迟时间。默认值为 0，表示没有额外的延迟。
4. **jiffies_till_first_fqs (3), jiffies_till_next_fqs (3)**: RCU 调度器的首次和后续强制完成时间（以 jiffies 计算）。3 表示 3 个 jiffies。
5. **jiffies_till_sched_qs (18446744073709551615), jiffies_to_sched_qs (100)**: RCU 调度的 jiffies 配置。第一个参数设置为最大值（无穷大），第二个参数设置为 100 jiffies。
6. **kthread_prio (1)**: RCU 内核线程的优先级。默认值为 1。
7. **qhimark (10000), qlowmark (100), qovld (20000)**: RCU 回调队列的高水位线、低水位线和过载线。
8. **rcu_divisor (7)**: RCU 频率除数，用于调节 RCU 周期的频率。
9. **rcu_fanout_exact (N), rcu_fanout_leaf (16)**: 控制 RCU fanout 的具体和叶节点的 fanout 数量。
10. **rcu_kick_kthreads (N)**: 是否在 RCU stall 时踢 RCU 线程。默认为 'N'。
11. **rcu_min_cached_objs (5)**: RCU 最小缓存对象数。
12. **rcu_resched_ns (3000000)**: RCU 调度延迟时间（纳秒）。
13. **sysrq_rcu (N)**: 是否启用 SysRq 键来触发 RCU。

### rcupdate
```bash
root@am62xx:/sys/module/rcupdate/parameters# ll
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_cpu_stall_ftrace_dump
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_cpu_stall_suppress
-r--r--r--    1 root     root          4096 Oct 25 14:01 rcu_cpu_stall_suppress_at_boot
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_cpu_stall_timeout
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_task_ipi_delay
-rw-r--r--    1 root     root          4096 Oct 25 14:01 rcu_task_stall_timeout

root@am62xx:/sys/module/rcupdate/parameters# cat rcu_cpu_stall_ftrace_dump
0
root@am62xx:/sys/module/rcupdate/parameters# cat rcu_cpu_stall_suppress
0
root@am62xx:/sys/module/rcupdate/parameters# cat rcu_cpu_stall_suppress_at_boot
0
root@am62xx:/sys/module/rcupdate/parameters# cat rcu_cpu_stall_timeout
21
root@am62xx:/sys/module/rcupdate/parameters# cat rcu_task_ipi_delay
0
root@am62xx:/sys/module/rcupdate/parameters# cat rcu_task_stall_timeout
600000
```

这些参数用于更新和控制 RCU 的行为：
1. **rcu_cpu_stall_ftrace_dump (0)**: 是否在 RCU CPU stall 时进行 ftrace dump。0 表示禁用。
2. **rcu_cpu_stall_suppress (0)**: 是否抑制 RCU CPU stall 警告。0 表示不抑制。
3. **rcu_cpu_stall_suppress_at_boot (0)**: 是否在系统启动时抑制 RCU CPU stall 警告。0 表示不抑制。
4. **rcu_cpu_stall_timeout (21)**: RCU CPU stall 超时时间（秒）。21 表示 21 秒。
5. **rcu_task_ipi_delay (0)**: RCU 任务 IPI 延迟时间（秒）。0 表示无延迟。
6. **rcu_task_stall_timeout (600000)**: RCU 任务 stall 超时时间（毫秒）。600000 表示 600 秒（10 分钟）。

### rcu_normal
```bash
root@am62xx:/# cat ./sys/kernel/rcu_normal
1
```
### rcu_expedited
```bash
root@am62xx:/# cat ./sys/kernel/rcu_expedited
0
```

**/sys/kernel/rcu_normal (1), /sys/kernel/rcu_expedited (0)**: 控制 RCU 操作模式。
`rcu_normal` 设置为 1，表示使用正常模式。`rcu_expedited` 设置为 0，表示不使用加速模式。

### panic_on_rcu_stall
```bash
root@am62xx:/# cat ./proc/sys/kernel/panic_on_rcu_stall
0
```

**/proc/sys/kernel/panic_on_rcu_stall (0)**: 在 RCU stall 时是否触发 panic。0 表示不触发 panic。

# 常见错误

## INFO: rcu_preempt self-detected stall on CPU

### 错误1：irq/29-2800000
```bash
root@am62xx:/# ps |grep 2800000
  185 root         0 SW   [irq/29-2800000.]
```
```bash
[18970.190386] rcu: INFO: rcu_preempt self-detected stall on CPU
[18970.190414] rcu:     0-....: (21284 ticks this GP) idle=736/1/0x4000000000000002 softirq=0/0 fqs=5113
[18970.190435]  (t=21000 jiffies g=1503153 q=736)
[18970.190444] Task dump for CPU 0:
[18970.190449] task:irq/29-2800000. state:R  running task     stack:    0 pid: 1552 ppid:     2 flags:0x00000028
[18970.190467] Call trace:
[18970.190469]  dump_backtrace+0x0/0x1b0
[18970.190491]  show_stack+0x18/0x38
[18970.190502]  sched_show_task+0x150/0x178
[18970.190513]  dump_cpu_task+0x44/0x54
[18970.190524]  rcu_dump_cpu_stacks+0xb8/0xfc
[18970.190531]  rcu_sched_clock_irq+0x9e0/0xd20
[18970.190542]  update_process_times+0x60/0xa0
[18970.190553]  tick_sched_handle.isra.0+0x34/0x50
[18970.190562]  tick_sched_timer+0x4c/0xa8
[18970.190568]  __hrtimer_run_queues+0x128/0x210
[18970.190576]  hrtimer_interrupt+0xe8/0x248
[18970.190582]  arch_timer_handler_phys+0x34/0x48
[18970.190594]  handle_percpu_devid_irq+0x84/0x148
[18970.190605]  generic_handle_irq+0x30/0x48
[18970.190614]  __handle_domain_irq+0x64/0xc0
[18970.190620]  gic_handle_irq+0x58/0x128
[18970.190628]  el1_irq+0xcc/0x180
[18970.190634]  omap8250_irq+0x200/0x360
[18970.190643]  irq_forced_thread_fn+0x3c/0xd0
[18970.190650]  irq_thread+0x194/0x2b8
[18970.190656]  kthread+0x174/0x198
[18970.190664]  ret_from_fork+0x10/0x30

[19010.561476] mmc0: cqhci: timeout for tag 18
[19010.561507] mmc0: cqhci: ============ CQHCI REGISTER DUMP ===========
[19010.561511] mmc0: cqhci: Caps:      0x000030c8 | Version:  0x00000510
[19010.561516] mmc0: cqhci: Config:    0x00000101 | Control:  0x00000000
[19010.561520] mmc0: cqhci: Int stat:  0x00000002 | Int enab: 0x00000006
[19010.561524] mmc0: cqhci: Int sig:   0x00000006 | Int Coal: 0x00000000
[19010.561528] mmc0: cqhci: TDL base:  0x85a81000 | TDL up32: 0x00000000
[19010.561533] mmc0: cqhci: Doorbell:  0x00000000 | TCN:      0x000c0000
[19010.561537] mmc0: cqhci: Dev queue: 0x00000000 | Dev Pend: 0x00000000
[19010.561543] mmc0: cqhci: Task clr:  0x00000000 | SSC1:     0x00011000
[19010.561548] mmc0: cqhci: SSC2:      0x00000001 | DCMD rsp: 0x00000000
[19010.561552] mmc0: cqhci: RED mask:  0xfdf9a080 | TERRI:    0x00000000
[19010.561556] mmc0: cqhci: Resp idx:  0x0000002f | Resp arg: 0x00000900
[19010.561561] mmc0: sdhci: ============ SDHCI REGISTER DUMP ===========
[19010.561566] mmc0: sdhci: Sys addr:  0x00000030 | Version:  0x00001004
[19010.561571] mmc0: sdhci: Blk size:  0x00000200 | Blk cnt:  0x00000000
[19010.561575] mmc0: sdhci: Argument:  0x00130000 | Trn mode: 0x00000023
[19010.561579] mmc0: sdhci: Present:   0x01ff00f0 | Host ctl: 0x0000003c
[19010.561586] mmc0: sdhci: Power:     0x0000000f | Blk gap:  0x00000080
[19010.561590] mmc0: sdhci: Wake-up:   0x00000000 | Clock:    0x00000007
[19010.561594] mmc0: sdhci: Timeout:   0x0000000e | Int stat: 0x00004000
[19010.561599] mmc0: sdhci: Int enab:  0x02ff4000 | Sig enab: 0x02ff4000
[19010.561603] mmc0: sdhci: ACmd stat: 0x00000000 | Slot int: 0x00000001
[19010.561608] mmc0: sdhci: Caps:      0x7decc801 | Caps_1:   0x18002407
[19010.561612] mmc0: sdhci: Cmd:       0x00002f3a | Max curr: 0x00000000
[19010.561616] mmc0: sdhci: Resp[0]:   0x00000900 | Resp[1]:  0xffffffff
[19010.561623] mmc0: sdhci: Resp[2]:   0x320f5903 | Resp[3]:  0x00000900
[19010.561627] mmc0: sdhci: Host ctl2: 0x0000000b
[19010.561631] mmc0: sdhci: ADMA Err:  0x00000000 | ADMA Ptr: 0x0000000085ac7248
[19010.561635] mmc0: sdhci: ============================================
[19010.561679] mmc0: running CQE recovery
```

以下是对 `rcu:     0-....: (21284 ticks this GP) idle=736/1/0x4000000000000002 softirq=0/0 fqs=5113` 输出每个字段的详细解释：
1. **`0-....`**:
   - 表示 CPU 的编号。`0` 是 CPU 的编号，后面的 `....` 表示更多的 CPU 相关信息（如状态、标志等）。

2. **`(21284 ticks this GP)`**:
   - 这是表示 CPU 在当前 RCU（Read-Copy Update）进程周期（GP，Grace Period）中已经经历的时钟滴答（ticks）数量。
   - `21284 ticks this GP` 表示从 RCU 开始到现在已经过去了 21284 个时钟滴答。

3. **`idle=736/1/0x4000000000000002`**:
   - **`736`**: 表示 CPU 在当前 GP 中累计的空闲时间，以 jiffies 为单位。也就是说，CPU 在这个时间段内处于空闲状态的时间是 736 jiffies。
   - **`1`**: 表示 CPU 在当前 GP 中进入空闲状态的次数。这里是 1，意味着 CPU 进入了空闲状态一次。
   - **`0x4000000000000002`**: 这是一个标志位字段，表示 CPU 的空闲状态标志。这个十六进制值代表 CPU 当前处于的空闲状态或特定的节能模式，这通常取决于 CPU 架构和内核配置。

4. **`softirq=0/0`**:
   - **`softirq=0`**: 表示 CPU 上的软中断状态。在这里，它显示软中断处理计数器为 0，意味着在当前的时间段内没有软中断发生。
   - **`0`**: 这是待处理的软中断数量。表示当前没有待处理的软中断任务，即软中断队列为空。

5. **`fqs=5113`**:
   - 表示在当前时间段内，软中断队列中的数量。`5113` 表示有 5113 个软中断任务在队列中等待处理。这通常指示系统的软中断队列有相当大的积压，可能导致系统处理软中断的延迟。

6. **`(t=21000 jiffies g=1503153 q=736)`**:
   - **`t=21000 jiffies`**: 表示当前时间为 21000 个 jiffies。Jiffies 是系统用来跟踪时间的基本单位。
   - **`g=1503153`**: 表示当前 RCU 的序列号或当前 GP 的序列号是 1503153。这个值用于跟踪 RCU 进程的进度。
   - **`q=736`**: 这是与某个特定的状态相关的计数值。在这个上下文中，它通常与空闲状态或队列的状态有关。

以下是对 `task:irq/29-2800000. state:R  running task     stack:    0 pid: 1552 ppid:     2 flags:0x00000028` 字段的详细解释：

1. **`task:irq/29-2800000`**:
   - **`task:`**: 表示正在描述一个特定的任务或线程。
   - **`irq/29-2800000`**: 这是任务的名称，表示这个任务处理 IRQ（中断请求） 29，且它是一个内核线程或中断处理程序，内核线程通常以 `irq` 开头，表示它专门用于处理硬件中断。`2800000` 通常是任务的标识符或特定的上下文。

2. **`state:R`**:
   - **`state:`**: 表示任务的状态。
   - **`R`**: 表示任务的当前状态为 `Running`（运行中）。这意味着该任务当前正在 CPU 上执行。

3. **`running task`**:
   - 表示任务当前正在运行。这是状态 `R` 的详细描述。

4. **`stack:    0`**:
   - **`stack:`**: 表示任务的堆栈信息。
   - **`0`**: 这里显示的是堆栈的指针或地址。在这个上下文中，`0` 可能表示堆栈信息不可用或堆栈指针为空。

5. **`pid: 1552`**:
   - **`pid:`**: 进程标识符（Process ID）。每个正在运行的进程都有一个唯一的 PID，这里 `1552` 是任务的 PID。

6. **`ppid:     2`**:
   - **`ppid:`**: 父进程标识符（Parent Process ID）。这表示该任务的父进程的 PID。`2` 通常表示 `kthreadd`，即内核线程创建者，是所有内核线程的父进程。

7. **`flags:0x00000028`**:
   - **`flags:`**: 任务的状态标志，表示任务的特殊属性或状态。`0x00000028` 是一个十六进制值，表示一组标志的位掩码。每个位或位组的组合代表特定的标志或属性。



![[企业微信截图_17228374791902.png]]

### 错误2：irq/120-485c010
```bash
root@am62xx:/# ps |grep 485c010
  141 root         0 SW   [irq/56-485c0100]
  160 root         0 SW   [irq/86-485c0100]
  163 root         0 SW   [irq/74-485c0100]
  167 root         0 SW   [irq/122-485c010]
  168 root         0 SW   [irq/104-485c010]
  513 root         0 SW   [irq/123-485c010]
  515 root         0 SW   [irq/105-485c010]
  517 root         0 SW   [irq/124-485c010]
  520 root         0 SW   [irq/106-485c010]
  521 root         0 SW   [irq/125-485c010]
  523 root         0 SW   [irq/107-485c010]
  524 root         0 SW   [irq/120-485c010]
  526 root         0 SW   [irq/108-485c010]
```
```bash
[117483.212482] rcu: INFO: rcu_preempt self-detected stall on CPU  
[117483.212486] rcu:    0-....: (17976855 ticks this GP) idle=b82/1/0x4000000000000002 softirq=0/0 fqs=452  
[117483.212496]         (t=17976855 jiffies g=111251277 q=2136)  
[117483.212501] rcu: rcu_preempt kthread starved for 63003 jiffies! g111251277 f0x0 RCU_GP_WAIT_FQS(5) ->state=0x402 ->cpu=1  
[117483.212508] rcu:    Unless rcu_preempt kthread gets sufficient CPU time, OOM is now expected behavior.  
[117483.212511] rcu: RCU grace-period kthread stack dump:  
[117483.212514] task:rcu_preempt     state:I stack:    0 pid:   12 ppid:     2 flags:0x00000028  
[117483.212521] Call trace:  
[117483.212523]  __switch_to+0xc0/0x118  
[117483.212531]  __schedule+0x298/0x818  
[117483.212537]  schedule+0xb0/0x148  
[117483.212542]  schedule_timeout+0x1b8/0x2b8  
[117483.212548]  rcu_gp_kthread+0x594/0xc00  
[117483.212554]  kthread+0x174/0x198  
[117483.212560]  ret_from_fork+0x10/0x30  

[117483.212569] Task dump for CPU 0:  
[117483.212572] task:irq/120-485c010 state:R  running task     stack:    0 pid:21787 ppid:     2 flags:0x00000028  
[117483.212581] Call trace:  
[117483.212583]  dump_backtrace+0x0/0x1b0  
[117483.212591]  show_stack+0x18/0x38  
[117483.212598]  sched_show_task+0x150/0x178  
[117483.212604]  dump_cpu_task+0x44/0x54  
[117483.212611]  rcu_dump_cpu_stacks+0xb8/0xfc  
[117483.212620]  rcu_sched_clock_irq+0x9e0/0xd20  
[117483.212627]  update_process_times+0x60/0xa0  
[117483.212635]  tick_sched_handle.isra.0+0x34/0x50  
[117483.212642]  tick_sched_timer+0x4c/0xa8  
[117483.212649]  __hrtimer_run_queues+0x128/0x210  
[117483.212654]  hrtimer_interrupt+0xe8/0x248  
[117483.212660]  arch_timer_handler_phys+0x34/0x48  
[117483.212669]  handle_percpu_devid_irq+0x84/0x148  
[117483.212677]  generic_handle_irq+0x30/0x48  
[117483.212683]  __handle_domain_irq+0x64/0xc0  
[117483.212689]  gic_handle_irq+0x58/0x128  
[117483.212695]  el1_irq+0xcc/0x180  
[117483.212700]  _raw_spin_unlock_irq+0x18/0x68  
[117483.212707]  irq_forced_thread_fn+0x70/0xd0  
[117483.212714]  irq_thread+0x194/0x2b8  
[117483.212720]  kthread+0x174/0x198  
[117483.212726]  ret_from_fork+0x10/0x30
```

### 错误3： irq/104-485c010
![[企业微信截图_17222147705706.png]]

### 错误4：irq/43-tidss
![[企业微信截图_17223240264127.png]]

### 错误5：irq/41-tidss
```bash
root@am62xx:/# ps |grep tidss
  180 root         0 SW   [irq/41-tidss]
```
```bash
[   16.644388] am65-cpsw-nuss 8000000.ethernet eth0: Link is Up - 1Gbps/Full - flow control rx/tx
[   22.148522] xhci-hcd xhci-hcd.0.auto: Timeout while waiting for configure endpoint command
[   22.148593] usb 1-1.2: Not enough bandwidth for altsetting 0
[   24.196494] ------------[ cut here ]------------
[   24.196512] NETDEV WATCHDOG: eth1 (am65-cpsw-nuss): transmit queue 0 timed out
[   24.196595] WARNING: CPU: 1 PID: 22 at net/sched/sch_generic.c:467 dev_watchdog+0x374/0x380
[   24.196622] Modules linked in: option(+) usb_wwan usbserial qmi_wwan(+) cdc_wdm usbnet wlcore_sdio wl18xx wlcore mac80211 cfg80211 rfkill ti_k3_r5_remoteproc phy_can_transceiver virtio_rpmsg_bus ti_k3_m4_remoteproc rti_wdt sa2ul sha512_generic j721e_csi2rx authenc mcrc cdns_dphy videobuf2_dma_contig pruss snd_soc_tlv320aic3x optee_rng rng_core sch_fq_codel cryptodev(O) ipv6
[   24.196715] CPU: 1 PID: 22 Comm: ksoftirqd/1 Tainted: G           O      5.10.168-rt83-gc1a1291911 #1
[   24.196724] Hardware name: ZHIYUAN Electronics AM6234 (DT)
[   24.196728] pstate: 40000005 (nZcv daif -PAN -UAO -TCO BTYPE=--)
[   24.196739] pc : dev_watchdog+0x374/0x380
[   24.196744] lr : dev_watchdog+0x374/0x380
[   24.196750] sp : ffff8000114ebc20
[   24.196752] x29: ffff8000114ebc20 x28: ffff00000ee69b00
[   24.196759] x27: 0000000000000004 x26: 0000000000000180
[   24.196765] x25: 00000000ffffffff x24: 0000000000000001
[   24.196772] x23: ffff00000ee683e0 x22: ffff00000ee68000
[   24.196779] x21: ffff00000ee684b0 x20: ffff800011229000
[   24.196786] x19: 0000000000000000 x18: 0000000000000000
[   24.196792] x17: 0000000000000000 x16: 0000000000000000
[   24.196799] x15: ffff000000160560 x14: ffffffffffffffff
[   24.196806] x13: ffff80001136d92e x12: ffff80001136d921
[   24.196814] x11: 0000000000000000 x10: ffff80001124dfc8
[   24.196820] x9 : 00000000fffffffe x8 : 6575657571207469
[   24.196827] x7 : 6d736e617274203a x6 : ffff8000114eba70
[   24.196833] x5 : ffff00003fd9cb38 x4 : 0000000000000000
[   24.196840] x3 : 0000000000000027 x2 : 0000000100000000
[   24.196846] x1 : 9d69c9227b3d1400 x0 : 0000000000000000
[   24.196855] Call trace:
[   24.196858]  dev_watchdog+0x374/0x380
[   24.196864]  call_timer_fn.isra.0+0x24/0x80
[   24.196878]  run_timer_softirq+0x4d4/0x548
[   24.196886]  efi_header_end+0x110/0x214
[   24.196896]  run_ksoftirqd+0x50/0xb8
[   24.196904]  smpboot_thread_fn+0x2e4/0x330
[   24.196913]  kthread+0x174/0x198
[   24.196921]  ret_from_fork+0x10/0x30
[   24.196931] ---[ end trace 0000000000000002 ]---
[   25.197239] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:10579 dql_avail:-98 free_desc:511
[   31.364503] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:16747 dql_avail:-98 free_desc:511
[   32.387499] xhci-hcd xhci-hcd.0.auto: xHCI host not responding to stop endpoint command.
[   32.387521] xhci-hcd xhci-hcd.0.auto: USBSTS: 0x00000008 EINT
[   32.403664] xhci-hcd xhci-hcd.0.auto: xHCI host controller not responding, assume dead
[   32.403723] xhci-hcd xhci-hcd.0.auto: HC died; cleaning up
[   32.403862] usb 1-1: USB disconnect, device number 2
[   32.403875] usb 1-1.2: USB disconnect, device number 3
[   32.403944] qmi_wwan: probe of 1-1.2:1.4 failed with error -62
[   32.404063] usbcore: registered new interface driver qmi_wwan
[   32.404112] option 1-1.2:1.0: GSM modem (1-port) converter detected
[   32.404608] usb 1-1.2: GSM modem (1-port) converter now attached to ttyUSB0
[   32.405598] usb-conn-gpio connector: supply vbus not found, using dummy regulator
[   32.413903] option1 ttyUSB0: GSM modem (1-port) converter now disconnected from ttyUSB0
[   32.414002] option 1-1.2:1.0: device disconnected
[   33.411510] ti-sci 44043000.system-controller: Mbox timedout in resp(caller: ti_sci_cmd_get_device+0x18/0x28)
[   33.411555] ti-sci 44043000.system-controller: Mbox send fail -110

[   35.487479] rcu: INFO: rcu_preempt self-detected stall on CPU
[   35.487501] rcu:     0-....: (1 GPs behind) idle=df6/1/0x4000000000000002 softirq=0/0 fqs=4943
[   35.487515]  (t=21000 jiffies g=6993 q=4415)
[   35.487523] Task dump for CPU 0:
[   35.487527] task:irq/41-tidss    state:R  running task     stack:    0 pid:  155 ppid:     2 flags:0x0000002a
[   35.487543] Call trace:
[   35.487546]  dump_backtrace+0x0/0x1b0
[   35.487565]  show_stack+0x18/0x38
[   35.487576]  sched_show_task+0x150/0x178
[   35.487586]  dump_cpu_task+0x44/0x54
[   35.487598]  rcu_dump_cpu_stacks+0xb8/0xfc
[   35.487606]  rcu_sched_clock_irq+0x9dc/0xd20
[   35.487615]  update_process_times+0x60/0xa0
[   35.487627]  tick_sched_handle.isra.0+0x34/0x50
[   35.487636]  tick_sched_timer+0x4c/0xa8
[   35.487642]  __hrtimer_run_queues+0x128/0x210
[   35.487650]  hrtimer_interrupt+0xe8/0x248
[   35.487655]  arch_timer_handler_phys+0x34/0x48
[   35.487667]  handle_percpu_devid_irq+0x84/0x148
[   35.487677]  generic_handle_irq+0x30/0x48
[   35.487685]  __handle_domain_irq+0x64/0xc0
[   35.487692]  gic_handle_irq+0x58/0x128
[   35.487699]  el1_irq+0xcc/0x180
[   35.487705]  rt_spin_lock+0x3c/0x88
[   35.487714]  __local_bh_disable_ip+0x148/0x208
[   35.487722]  irq_forced_thread_fn+0x2c/0xd0
[   35.487730]  irq_thread+0x194/0x2b8
[   35.487737]  kthread+0x174/0x198
[   35.487745]  ret_from_fork+0x10/0x30
[   36.995503] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:22378 dql_avail:-98 free_desc:511
[   42.115501] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:27498 dql_avail:-98 free_desc:511
[   48.259504] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:33642 dql_avail:-98 free_desc:511
[   53.892508] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:39275 dql_avail:-98 free_desc:511
[   59.011503] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:44394 dql_avail:-98 free_desc:511
[   64.131502] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:49514 dql_avail:-98 free_desc:511
[   70.276502] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:55659 dql_avail:-98 free_desc:511
[   74.885507] mmc0: cqhci: timeout for tag 24
[   74.885538] mmc0: cqhci: ============ CQHCI REGISTER DUMP ===========
[   74.885542] mmc0: cqhci: Caps:      0x000030c8 | Version:  0x00000510
[   74.885548] mmc0: cqhci: Config:    0x00000101 | Control:  0x00000000
[   74.885553] mmc0: cqhci: Int stat:  0x00000002 | Int enab: 0x00000006
[   74.885557] mmc0: cqhci: Int sig:   0x00000006 | Int Coal: 0x00000000
[   74.885561] mmc0: cqhci: TDL base:  0x8efb2000 | TDL up32: 0x00000000
[   74.885566] mmc0: cqhci: Doorbell:  0x00000000 | TCN:      0xff008f01
[   74.885571] mmc0: cqhci: Dev queue: 0x00000000 | Dev Pend: 0x00000000
[   74.885575] mmc0: cqhci: Task clr:  0x00000000 | SSC1:     0x00011000
[   74.885579] mmc0: cqhci: SSC2:      0x00000001 | DCMD rsp: 0x00000000
[   74.885584] mmc0: cqhci: RED mask:  0xfdf9a080 | TERRI:    0x00000000
[   74.885589] mmc0: cqhci: Resp idx:  0x0000002f | Resp arg: 0x00000900
[   74.885594] mmc0: sdhci: ============ SDHCI REGISTER DUMP ===========
[   74.885598] mmc0: sdhci: Sys addr:  0x00000008 | Version:  0x00001004
[   74.885603] mmc0: sdhci: Blk size:  0x00000200 | Blk cnt:  0x00000000
[   74.885607] mmc0: sdhci: Argument:  0x001d0000 | Trn mode: 0x00000023
[   74.885612] mmc0: sdhci: Present:   0x01ff00f0 | Host ctl: 0x0000003c
[   74.885616] mmc0: sdhci: Power:     0x0000000f | Blk gap:  0x00000080
[   74.885621] mmc0: sdhci: Wake-up:   0x00000000 | Clock:    0x00000007
[   74.885626] mmc0: sdhci: Timeout:   0x0000000e | Int stat: 0x00004000
[   74.885631] mmc0: sdhci: Int enab:  0x02ff4000 | Sig enab: 0x02ff4000
[   74.885635] mmc0: sdhci: ACmd stat: 0x00000000 | Slot int: 0x00000001
[   74.885641] mmc0: sdhci: Caps:      0x7decc801 | Caps_1:   0x18002407
[   74.885646] mmc0: sdhci: Cmd:       0x00002f3a | Max curr: 0x00000000
[   74.885650] mmc0: sdhci: Resp[0]:   0x00000900 | Resp[1]:  0xffffffff
[   74.885654] mmc0: sdhci: Resp[2]:   0x320f5903 | Resp[3]:  0x00000900
[   74.885658] mmc0: sdhci: Host ctl2: 0x0000000b
[   74.885665] mmc0: sdhci: ADMA Err:  0x00000000 | ADMA Ptr: 0x000000008e80ae0c
[   74.885670] mmc0: sdhci: ============================================
[   74.885723] mmc0: running CQE recovery
[   75.908508] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:61291 dql_avail:-98 free_desc:511
[   81.028506] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:66411 dql_avail:-98 free_desc:511
[   85.123487] mmc0: Timeout waiting for hardware interrupt.
[   85.123504] mmc0: sdhci: ============ SDHCI REGISTER DUMP ===========
[   85.123508] mmc0: sdhci: Sys addr:  0x00000008 | Version:  0x00001004
[   85.123514] mmc0: sdhci: Blk size:  0x00000200 | Blk cnt:  0x00000000
[   85.123520] mmc0: sdhci: Argument:  0x00000000 | Trn mode: 0x00000023
[   85.123524] mmc0: sdhci: Present:   0x01ff00f0 | Host ctl: 0x0000003d
[   85.123529] mmc0: sdhci: Power:     0x0000000f | Blk gap:  0x00000080
[   85.123534] mmc0: sdhci: Wake-up:   0x00000000 | Clock:    0x00000007
[   85.123538] mmc0: sdhci: Timeout:   0x00000000 | Int stat: 0x00018002
[   85.123543] mmc0: sdhci: Int enab:  0x00ff0003 | Sig enab: 0x00ff0003
[   85.123548] mmc0: sdhci: ACmd stat: 0x00000000 | Slot int: 0x00000001
[   85.123552] mmc0: sdhci: Caps:      0x7decc801 | Caps_1:   0x18002407
[   85.123559] mmc0: sdhci: Cmd:       0x00000c13 | Max curr: 0x00000000
[   85.123563] mmc0: sdhci: Resp[0]:   0x00000900 | Resp[1]:  0xffffffff
[   85.123569] mmc0: sdhci: Resp[2]:   0x320f5903 | Resp[3]:  0x00000900
[   85.123573] mmc0: sdhci: Host ctl2: 0x0000000b
[   85.123578] mmc0: sdhci: ADMA Err:  0x00000000 | ADMA Ptr: 0x000000008e80ae0c
[   85.123582] mmc0: sdhci: ============================================
[   86.147505] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:71530 dql_avail:-98 free_desc:511
[   92.291504] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:77674 dql_avail:-98 free_desc:511
[   95.363491] mmc0: Timeout waiting for hardware interrupt.
[   95.363508] mmc0: sdhci: ============ SDHCI REGISTER DUMP ===========
[   95.363514] mmc0: sdhci: Sys addr:  0x00000008 | Version:  0x00001004
[   95.363519] mmc0: sdhci: Blk size:  0x00000200 | Blk cnt:  0x00000000
[   95.363524] mmc0: sdhci: Argument:  0x00000001 | Trn mode: 0x00000023
[   95.363530] mmc0: sdhci: Present:   0x01ff00f0 | Host ctl: 0x0000003d
[   95.363534] mmc0: sdhci: Power:     0x0000000f | Blk gap:  0x00000080
[   95.363539] mmc0: sdhci: Wake-up:   0x00000000 | Clock:    0x00000007
[   95.363543] mmc0: sdhci: Timeout:   0x00000000 | Int stat: 0x00018003
[   95.363547] mmc0: sdhci: Int enab:  0x00ff0003 | Sig enab: 0x00ff0003
[   95.363552] mmc0: sdhci: ACmd stat: 0x00000000 | Slot int: 0x00000001
[   95.363557] mmc0: sdhci: Caps:      0x7decc801 | Caps_1:   0x18002407
[   95.363562] mmc0: sdhci: Cmd:       0x00003013 | Max curr: 0x00000000
[   95.363568] mmc0: sdhci: Resp[0]:   0x00400900 | Resp[1]:  0xffffffff
[   95.363573] mmc0: sdhci: Resp[2]:   0x320f5903 | Resp[3]:  0x00000900
[   95.363577] mmc0: sdhci: Host ctl2: 0x0000000b
[   95.363581] mmc0: sdhci: ADMA Err:  0x00000000 | ADMA Ptr: 0x000000008e80ae0c
[   95.363586] mmc0: sdhci: ============================================
[   97.923506] am65-cpsw-nuss 8000000.ethernet eth1: txq:0 DRV_XOFF:0 tmo:83306 dql_avail:-98 free_desc:511

```

## rcu: INFO: rcu_preempt detected stalls on CPUs/tasks
### 错误1：readlink
```bash
[2024-03-30 03:32:41]  [   57.546399] rcu: INFO: rcu_preempt detected stalls on CPUs/tasks:
[2024-03-30 03:33:04]  [   57.546419] rcu: Tasks blocked on level-0 rcu_node (CPUs 0-1): P2453/1:b..l P2452/1:b..l
[2024-03-30 03:33:04]  [   57.546445] (detected by 0, t=21002 jiffies, g=22877, q=2545)
[2024-03-30 03:33:04]  [   57.546453] task:readlink        state:D stack:    0 pid: 2452 ppid:  2443 flags:0x00000028
[2024-03-30 03:33:04]  [   57.546465] Call trace:
[2024-03-30 03:33:04]  [   57.546467]  __switch_to+0xc0/0x118
[2024-03-30 03:33:04]  [   57.546495]  __schedule+0x298/0x818
[2024-03-30 03:33:04]  [   57.546505]  preempt_schedule_lock+0x2c/0x58
[2024-03-30 03:33:04]  [   57.546512]  rt_spin_lock_slowlock_locked+0x118/0x2a8
[2024-03-30 03:33:04]  [   57.546521]  rt_spin_lock_slowlock+0x5c/0x90
[2024-03-30 03:33:04]  [   57.546527]  rt_spin_lock+0x74/0x88
[2024-03-30 03:33:04]  [   57.546535]  free_unref_page_list+0x150/0x598
[2024-03-30 03:33:04]  [   57.546545]  release_pages+0x2c4/0x358
[2024-03-30 03:33:04]  [   57.546554]  pagevec_lru_move_fn+0xcc/0x108
[2024-03-30 03:33:04]  [   57.546560]  lru_add_drain_cpu+0xbc/0x2e0
[2024-03-30 03:33:04]  [   57.546566]  lru_add_drain+0x80/0x1d0
[2024-03-30 03:33:04]  [   57.546572]  exit_mmap+0x98/0x178
[2024-03-30 03:33:04]  [   57.546581]  mmput+0x84/0x158
[2024-03-30 03:33:04]  [   57.546589]  do_exit+0x2b4/0xa40
[2024-03-30 03:33:04]  [   57.546596]  do_group_exit+0x44/0xa0
[2024-03-30 03:33:04]  [   57.546605]  __wake_up_parent+0x0/0x30
[2024-03-30 03:33:04]  [   57.546611]  el0_svc_common.constprop.0+0x78/0x1c8
[2024-03-30 03:33:04]  [   57.546621]  do_el0_svc+0x24/0xa0
[2024-03-30 03:33:04]  [   57.546628]  el0_svc+0x14/0x20
[2024-03-30 03:33:04]  [   57.546634]  el0_sync_handler+0xb0/0xb8
[2024-03-30 03:33:04]  [   57.546640]  el0_sync+0x180/0x1c0

[2024-03-30 03:33:04]  [   57.546648] task:readlink        state:D stack:    0 pid: 2453 ppid:  2440 flags:0x00000008
[2024-03-30 03:33:04]  [   57.546657] Call trace:
[2024-03-30 03:33:04]  [   57.546659]  __switch_to+0xc0/0x118
[2024-03-30 03:33:04]  [   57.546667]  __schedule+0x298/0x818
[2024-03-30 03:33:04]  [   57.546673]  preempt_schedule_lock+0x2c/0x58
[2024-03-30 03:33:04]  [   57.546679]  rt_spin_lock_slowlock_locked+0x118/0x2a8
[2024-03-30 03:33:04]  [   57.546686]  rt_spin_lock_slowlock+0x5c/0x90
[2024-03-30 03:33:04]  [   57.546692]  rt_spin_lock+0x74/0x88
[2024-03-30 03:33:04]  [   57.546699]  lru_cache_add+0x8c/0x258
[2024-03-30 03:33:04]  [   57.546705]  lru_cache_add_inactive_or_unevictable+0x34/0x120
[2024-03-30 03:33:04]  [   57.546712]  wp_page_copy+0x1bc/0x890
[2024-03-30 03:33:04]  [   57.546720]  do_wp_page+0x98/0x5c8
[2024-03-30 03:33:04]  [   57.546727]  handle_mm_fault+0x774/0xe38
[2024-03-30 03:33:04]  [   57.546733]  do_page_fault+0x130/0x3b0
[2024-03-30 03:33:04]  [   57.546742]  do_mem_abort+0x40/0xa0
[2024-03-30 03:33:04]  [   57.546749]  el0_da+0x28/0x38
[2024-03-30 03:33:04]  [   57.546754]  el0_sync_handler+0x88/0xb8
[2024-03-30 03:33:04]  [   57.546759]  el0_sync+0x180/0x1c0
```

### 错误2：tr
```bash
[29906.171225] rcu: INFO: rcu_preempt detected stalls on CPUs/tasks:  
[29906.171245] rcu:     Tasks blocked on level-0 rcu_node (CPUs 0-1): P3897786/1:b..l  
[29906.171266]  (detected by 0, t=1722137 jiffies, g=39045777, q=440935)  
[29906.171275] task:tr              state:D stack:    0 pid:3897786 ppid:3897780 flags:0x00000000  
[29906.171291] Call trace:  
[29906.171294]  __switch_to+0xc0/0x118  
[29906.171314]  __schedule+0x298/0x818  
[29906.171323]  preempt_schedule_lock+0x2c/0x58  
[29906.171331]  rt_spin_lock_slowlock_locked+0x118/0x2a8  
[29906.171339]  rt_spin_lock_slowlock+0x5c/0x90  
[29906.171346]  rt_spin_lock+0x74/0x88  
[29906.171354]  lru_cache_add+0x8c/0x258  
[29906.171363]  lru_cache_add_inactive_or_unevictable+0x34/0x120  
[29906.171370]  wp_page_copy+0x1bc/0x890  
[29906.171378]  do_wp_page+0x98/0x5c8  
[29906.171384]  handle_mm_fault+0x774/0xe38  
[29906.171390]  do_page_fault+0x130/0x3b0  
[29906.171400]  do_mem_abort+0x40/0xa0  
[29906.171406]  el0_da+0x28/0x38  
[29906.171417]  el0_sync_handler+0x88/0xb8  
[29906.171423]  el0_sync+0x180/0x1c0
```

### 错误3：grep
```bash
[12417.698660] rcu: INFO: rcu_preempt detected stalls on CPUs/tasks:  
[12417.698684] rcu:     Tasks blocked on level-0 rcu_node (CPUs 0-3): P1773646/1:b..l  
[12417.698715]  (detected by 2, t=1470117 jiffies, g=8493769, q=167072)  
[12417.698724] task:grep            state:D stack:    0 pid:1773646 ppid:1773642 flags:0x00000000  
[12417.698735] Call trace:  
[12417.698737]  __switch_to+0xc0/0x118  
[12417.698759]  __schedule+0x298/0x818  
[12417.698769]  preempt_schedule_lock+0x2c/0x58  
[12417.698776]  rt_spin_lock_slowlock_locked+0x118/0x2a8  
[12417.698785]  rt_spin_lock_slowlock+0x5c/0x90  
[12417.698792]  rt_spin_lock+0x74/0x88  
[12417.698799]  lockref_get_not_dead+0x18/0x50  
[12417.698810]  __legitimize_path.isra.0+0x3c/0x90  
[12417.698827]  try_to_unlazy+0x48/0xb8  
[12417.698835]  complete_walk+0x40/0xc8  
[12417.698841]  path_openat+0x4f0/0xcd0  
[12417.698849]  do_filp_open+0x7c/0x100  
[12417.698857]  do_sys_openat2+0x210/0x2c8  
[12417.698864]  do_sys_open+0x58/0xa0  
[12417.698871]  __arm64_sys_openat+0x24/0x30  
[12417.698877]  el0_svc_common.constprop.0+0x78/0x1c8  
[12417.698887]  do_el0_svc+0x24/0xa0  
[12417.698895]  el0_svc+0x14/0x20  
[12417.698900]  el0_sync_handler+0xb0/0xb8  
[12417.698905]  el0_sync+0x180/0x1c0
```

### 错误4：wc
```bash
[2024-04-16 05:15:42]  [33824.463476] rcu: INFO: rcu_preempt detected stalls on CPUs/tasks:  
[2024-04-16 05:15:42]  [33824.463504] rcu: Tasks blocked on level-0 rcu_node (CPUs 0-3): P1013973/1:b..l  
[2024-04-16 05:15:42]  [33824.463530] (detected by 0, t=903072 jiffies, g=25390449, q=567855)  
[2024-04-16 05:15:42]  [33824.463538] task:wc              state:D stack:    0 pid:1013973 ppid:1013967 flags:0x00000000  
[2024-04-16 05:15:42]  [33824.463550] Call trace:  
[2024-04-16 05:15:42]  [33824.463552]  __switch_to+0xc0/0x118  
[2024-04-16 05:15:42]  [33824.463573]  __schedule+0x298/0x818  
[2024-04-16 05:15:42]  [33824.463583]  preempt_schedule_lock+0x2c/0x58  
[2024-04-16 05:15:42]  [33824.463590]  rt_spin_lock_slowlock_locked+0x118/0x2a8  
[2024-04-16 05:15:42]  [33824.463599]  rt_spin_lock_slowlock+0x5c/0x90  
[2024-04-16 05:15:42]  [33824.463609]  rt_spin_lock+0x74/0x88  
[2024-04-16 05:15:42]  [33824.463617]  dput+0x100/0x2f8  
[2024-04-16 05:15:42]  [33824.463627]  terminate_walk+0x60/0x138  
[2024-04-16 05:15:42]  [33824.463636]  path_openat+0x800/0xcd0  
[2024-04-16 05:15:42]  [33824.463644]  do_filp_open+0x7c/0x100  
[2024-04-16 05:15:42]  [33824.463652]  do_sys_openat2+0x210/0x2c8  
[2024-04-16 05:15:42]  [33824.463660]  do_sys_open+0x58/0xa0  
[2024-04-16 05:15:42]  [33824.463667]  __arm64_sys_openat+0x24/0x30  
[2024-04-16 05:15:42]  [33824.463673]  el0_svc_common.constprop.0+0x78/0x1c8  
[2024-04-16 05:15:42]  [33824.463684]  do_el0_svc+0x24/0xa0  
[2024-04-16 05:15:42]  [33824.463691]  el0_svc+0x14/0x20  
[2024-04-16 05:15:42]  [33824.463696]  el0_sync_handler+0xb0/0xb8  
[2024-04-16 05:15:42]  [33824.463701]  el0_sync+0x180/0x1c0
```

```bash
[2024-04-16 05:39:37]  [35273.578480] rcu: INFO: rcu_preempt detected stalls on CPUs/tasks:
[2024-04-16 05:39:51]  [35273.578506] rcu: Tasks blocked on level-0 rcu_node (CPUs 0-3): P1013973/1:b..l
[2024-04-16 05:39:51]  [35273.578533] (detected by 0, t=2352187 jiffies, g=25390449, q=1682733)
[2024-04-16 05:39:51]  [35273.578542] task:wc              state:D stack:    0 pid:1013973 ppid:1013967 flags:0x00000000
[2024-04-16 05:39:51]  [35273.578561] Call trace:
[2024-04-16 05:39:51]  [35273.578563]  __switch_to+0xc0/0x118
[2024-04-16 05:39:51]  [35273.578585]  __schedule+0x298/0x818
[2024-04-16 05:39:51]  [35273.578596]  preempt_schedule_lock+0x2c/0x58
[2024-04-16 05:39:51]  [35273.578605]  rt_spin_lock_slowlock_locked+0x118/0x2a8
[2024-04-16 05:39:51]  [35273.578615]  rt_spin_lock_slowlock+0x5c/0x90
[2024-04-16 05:39:51]  [35273.578623]  rt_spin_lock+0x74/0x88
[2024-04-16 05:39:51]  [35273.578630]  dput+0x100/0x2f8
[2024-04-16 05:39:51]  [35273.578640]  terminate_walk+0x60/0x138
[2024-04-16 05:39:51]  [35273.578650]  path_openat+0x800/0xcd0
[2024-04-16 05:39:51]  [35273.578658]  do_filp_open+0x7c/0x100
[2024-04-16 05:39:51]  [35273.578666]  do_sys_openat2+0x210/0x2c8
[2024-04-16 05:39:51]  [35273.578677]  do_sys_open+0x58/0xa0
[2024-04-16 05:39:51]  [35273.578687]  __arm64_sys_openat+0x24/0x30
[2024-04-16 05:39:51]  [35273.578694]  el0_svc_common.constprop.0+0x78/0x1c8
[2024-04-16 05:39:51]  [35273.578705]  do_el0_svc+0x24/0xa0
[2024-04-16 05:39:51]  [35273.578713]  el0_svc+0x14/0x20
[2024-04-16 05:39:51]  [35273.578718]  el0_sync_handler+0xb0/0xb8
[2024-04-16 05:39:51]  [35273.578724]  el0_sync+0x180/0x1c0
[2024-04-16 05:39:51]  module:rtc                  success:34601                error:0    
[2024-04-16 05:39:59]  Data verification failed
[2024-04-16 05:40:00]  module:uart78               success:34011                error:0    
[2024-04-16 05:40:28]  [35312.235923] systemd-timesyn invoked oom-killer: gfp_mask=0x100cca(GFP_HIGHUSER_MOVABLE), order=0, oom_score_adj=0
[2024-04-16 05:40:30]  [35312.235972] CPU: 0 PID: 655 Comm: systemd-timesyn Tainted: G           O      5.10.168-rt83-gc1a1291911 #3
[2024-04-16 05:40:30]  [35312.235982] Hardware name: ZHIYUAN Electronics AM6234 (DT)
[2024-04-16 05:40:30]  [35312.235989] Call trace:
[2024-04-16 05:40:30]  [35312.235990]  dump_backtrace+0x0/0x1b0
[2024-04-16 05:40:30]  [35312.236014]  show_stack+0x18/0x38
[2024-04-16 05:40:30]  [35312.236022]  dump_stack+0xe8/0x124
[2024-04-16 05:40:30]  [35312.236030]  dump_header+0x48/0x19c
[2024-04-16 05:40:30]  [35312.236041]  oom_kill_process+0x240/0x248
[2024-04-16 05:40:30]  [35312.236055]  out_of_memory+0xe4/0x330
[2024-04-16 05:40:30]  [35312.236062]  __alloc_pages_slowpath.constprop.0+0x76c/0xa60
[2024-04-16 05:40:30]  [35312.236074]  __alloc_pages_nodemask+0x20c/0x260
[2024-04-16 05:40:30]  [35312.236081]  pagecache_get_page+0x138/0x348
[2024-04-16 05:40:30]  [35312.236089]  filemap_fault+0x6c4/0xaa0
[2024-04-16 05:40:30]  [35312.236095]  __do_fault+0x3c/0x258
[2024-04-16 05:40:30]  [35312.236103]  handle_mm_fault+0x840/0xe38
[2024-04-16 05:40:30]  [35312.236108]  do_page_fault+0x130/0x3b0
[2024-04-16 05:40:30]  [35312.236118]  do_translation_fault+0x58/0x68
[2024-04-16 05:40:30]  [35312.236125]  do_mem_abort+0x40/0xa0
[2024-04-16 05:40:30]  [35312.236132]  el0_ia+0x64/0xb8
[2024-04-16 05:40:30]  [35312.236138]  el0_sync_handler+0x98/0xb8
[2024-04-16 05:40:30]  [35312.236144]  el0_sync+0x180/0x1c0
[2024-04-16 05:40:30]  [35312.236151] Mem-Info:
[2024-04-16 05:40:30]  [35312.236156] active_anon:384 inactive_anon:24455 isolated_anon:0
[2024-04-16 05:40:30]  [35312.236156]  active_file:72 inactive_file:216 isolated_file:26
[2024-04-16 05:40:30]  [35312.236156]  unevictable:0 dirty:0 writeback:0
[2024-04-16 05:40:30]  [35312.236156]  slab_reclaimable:45998 slab_unreclaimable:147529
[2024-04-16 05:40:30]  [35312.236156]  mapped:3386 shmem:17353 pagetables:493 bounce:0
[2024-04-16 05:40:30]  [35312.236156]  free:2344 free_pcp:4 free_cma:0
[2024-04-16 05:40:30]  [35312.236172] Node 0 active_anon:1536kB inactive_anon:97820kB active_file:288kB inactive_file:864kB unevictable:0kB isolated(anon):0kB isolated(file):104kB mapped:13544kB dirty:0kB writeback:0kB shmem:69412kB writeback_tmp:0kB kernel_stack:38624kB all_unreclaimable? no
[2024-04-16 05:40:30]  [35312.236196] DMA free:9376kB min:11992kB low:12940kB high:13888kB reserved_highatomic:0KB active_anon:1536kB inactive_anon:97820kB active_file:464kB inactive_file:1044kB unevictable:0kB writepending:0kB present:1048576kB managed:952352kB mlocked:0kB pagetables:1972kB bounce:0kB free_pcp:16kB local_pcp:0kB free_cma:0kB
[2024-04-16 05:40:30]  [35312.236212] lowmem_reserve[]: 0 0 0 0
[2024-04-16 05:40:30]  [35312.236226] DMA: 1964*4kB (UME) 239*8kB (UM) 0*16kB 0*32kB 0*64kB 0*128kB 0*256kB 0*512kB 0*1024kB 0*2048kB 0*4096kB = 9768kB
[2024-04-16 05:40:30]  [35312.236261] Node 0 hugepages_total=0 hugepages_free=0 hugepages_surp=0 hugepages_size=1048576kB
[2024-04-16 05:40:30]  [35312.236267] Node 0 hugepages_total=0 hugepages_free=0 hugepages_surp=0 hugepages_size=32768kB
[2024-04-16 05:40:30]  [35312.236272] Node 0 hugepages_total=0 hugepages_free=0 hugepages_surp=0 hugepages_size=2048kB
[2024-04-16 05:40:30]  [35312.236277] Node 0 hugepages_total=0 hugepages_free=0 hugepages_surp=0 hugepages_size=64kB
[2024-04-16 05:40:30]  [35312.236282] 17667 total pagecache pages
[2024-04-16 05:40:30]  [35312.236286] 0 pages in swap cache
[2024-04-16 05:40:30]  [35312.236288] Swap cache stats: add 0, delete 0, find 0/0
[2024-04-16 05:40:30]  [35312.236292] Free swap  = 0kB
[2024-04-16 05:40:30]  [35312.236294] Total swap = 0kB
[2024-04-16 05:40:30]  [35312.236297] 262144 pages RAM
[2024-04-16 05:40:30]  [35312.236299] 0 pages HighMem/MovableOnly
[2024-04-16 05:40:30]  [35312.236301] 24056 pages reserved
[2024-04-16 05:40:30]  [35312.236303] 0 pages cma reserved
[2024-04-16 05:40:30]  [35312.236305] 0 pages hwpoisoned
[2024-04-16 05:40:30]  [35312.236307] Tasks state (memory values in pages):
[2024-04-16 05:40:30]  [35312.236310] [  pid  ]   uid  tgid total_vm      rss pgtables_bytes swapents oom_score_adj name
[2024-04-16 05:40:30]  [35312.236368] [    623]   999   623      920       57    40960        0             0 rpcbind
[2024-04-16 05:40:30]  [35312.236379] [    624]     0   624    10496     3339    98304        0          -250 systemd-journal
[2024-04-16 05:40:30]  [35312.236389] [    642]     0   642     3476      355    49152        0         -1000 systemd-udevd
[2024-04-16 05:40:30]  [35312.236398] [    655]   992   655    20267      104    57344        0             0 systemd-timesyn
[2024-04-16 05:40:30]  [35312.236413] [    701]   997   701     1116      113    45056        0          -900 dbus-daemon
[2024-04-16 05:40:30]  [35312.236422] [    717]     0   717    19585       69    49152        0             0 irqbalance
[2024-04-16 05:40:30]  [35312.236432] [    734]     0   734      633       24    40960        0             0 tee-supplicant
[2024-04-16 05:40:30]  [35312.236442] [    756]   994   756     3971      122    49152        0             0 systemd-network
[2024-04-16 05:40:31]  [35312.236453] [    794]   993   794     1781      101    49152        0             0 systemd-resolve
[2024-04-16 05:40:31]  [35312.236462] [    895]   998   895      783      144    40960        0             0 rpc.statd
[2024-04-16 05:40:31]  [35312.236471] [    897]     0   897     5508     1310    77824        0             0 snmpd
[2024-04-16 05:40:31]  [35312.236566] [    926]     0   926      509       27    40960        0             0 agetty
[2024-04-16 05:40:31]  [35312.236579] [    927]     0   927     1133       97    45056        0             0 login
[2024-04-16 05:40:31]  [35312.236593] [   1766]     0  1766     2208      257    53248        0             0 systemd
[2024-04-16 05:40:31]  [35312.236603] [   1771]     0  1771     2910      635    57344        0             0 (sd-pam)
[2024-04-16 05:40:31]  [35312.236613] [   1800]     0  1800      915      131    40960        0             0 factory-test.sh
[2024-04-16 05:40:31]  [35312.236623] [   1837]     0  1837      696       21    40960        0             0 telnetd
[2024-04-16 05:40:31]  [35312.236633] [   1947]     0  1947     1115      312    45056        0             0 sh
[2024-04-16 05:40:31]  [35312.236643] [   2051]   997  2051     1115      115    45056        0             0 dbus-daemon
[2024-04-16 05:40:31]  [35312.236654] [   2057]     0  2057      507       21    36864        0             0 brcm_patchram_p
[2024-04-16 05:40:31]  [35312.236665] [   2079]     0  2079     1662       63    45056        0             0 bluetoothd
[2024-04-16 05:40:31]  [35312.236675] [   2122]     0  2122     1722      102    49152        0             0 systemd-logind
[2024-04-16 05:40:31]  [35312.236685] [   2147]     0  2147   305078      250   204800        0             0 function
[2024-04-16 05:40:31]  [35312.236694] [   2150]     0  2150    19555      326    53248        0             0 zywtfw.bin
[2024-04-16 05:40:31]  [35312.236707] [   2438]     0  2438     2807      159    53248        0             0 wpa_supplicant
[2024-04-16 05:40:31]  [35312.236717] [   2840]     0  2840      696       23    40960        0             0 udhcpc
[2024-04-16 05:40:31]  [35312.236727] [   4617]     0  4617      509       27    36864        0             0 agetty
[2024-04-16 05:40:31]  [35312.236737] [  16405]     0 16405      682       62    40960        0             0 starter
[2024-04-16 05:40:31]  [35312.236747] [  16413]     0 16413      777       35    40960        0             0 netserver
[2024-04-16 05:40:31]  [35312.236757] [  16428]     0 16428   298101      294   192512        0             0 charon
[2024-04-16 05:40:31]  [35312.236767] [  17507]     0 17507      880      258    45056        0             0 dropbear
[2024-04-16 05:40:31]  [35312.236777] [  17553]     0 17553     1078      288    45056        0             0 sh
[2024-04-16 05:40:31]  [35312.236787] [  17555]     0 17555      707       72    40960        0             0 dropbear
[2024-04-16 05:40:31]  [35312.236796] [  17582]     0 17582     1245       86    49152        0             0 sftp-server
[2024-04-16 05:40:31]  [35312.236805] [  18796]     0 18796     1198      315    49152        0             0 htop
[2024-04-16 05:40:31]  [35312.236818] [1013951]     0 1013951      882       90    40960        0             0 camera.sh
[2024-04-16 05:40:31]  [35312.236829] [1013964]     0 1013964      882       79    40960        0             0 bluetooth.sh
[2024-04-16 05:40:31]  [35312.236840] [1013967]     0 1013967      849       49    40960        0             0 sh
[2024-04-16 05:40:31]  [35312.236849] [1013973]     0 1013973      259        1    32768        0             0 wc
[2024-04-16 05:40:31]  [35312.236860] [1013983]     0 1013983      882       78    40960        0             0 bluetooth.sh
[2024-04-16 05:40:31]  [35312.236869] [1013984]     0 1013984      501       17    36864        0             0 l2ping
[2024-04-16 05:40:31]  [35312.236879] [1013991]     0 1013991      259        1    32768        0             0 sleep
[2024-04-16 05:40:31]  [35312.236889] [1013994]     0 1013994      259        1    32768        0             0 dmesg
[2024-04-16 05:40:31]  [35312.236898] [1013995]     0 1013995      259        1    32768        0             0 grep
[2024-04-16 05:40:31]  [35312.236918] oom-kill:constraint=CONSTRAINT_NONE,nodemask=(null),cpuset=/,mems_allowed=0,global_oom,task_memcg=/system.slice/snmpd.service,task=snmpd,pid=897,uid=0
[2024-04-16 05:40:31]  [35312.236976] Out of memory: Killed process 897 (snmpd) total-vm:22032kB, anon-rss:5240kB, file-rss:0kB, shmem-rss:0kB, UID:0 pgtables:76kB oom_score_adj:0

```

### 错误5：led
```bash
[157353.867483] rcu: INFO: rcu_preempt detected stalls on CPUs/tasks:  
[157353.867505] rcu:    Tasks blocked on level-0 rcu_node (CPUs 0-1):  
[157353.867514] stop_feed_wdt:244 system will restart within 1.6 seconds!  
[157355.917751]  P1245/1:b..l  
[157355.917761]         (detected by 1, t=4874437 jiffies, g=129370229, q=16861)  
[157355.917770] task:led             state:R  running task     stack:    0 pid: 1245 ppid:     1 flags:0x00000008  
[157355.917784] Call trace:  
[157355.917786]  __switch_to+0xc0/0x118  
[157355.917803]  __schedule+0x298/0x818  
[157355.917812]  preempt_schedule_common+0x2c/0x58  
[157355.917819]  preempt_schedule+0x40/0x50  
[157355.917825]  migrate_enable+0x11c/0x120  
[157355.917837]  rt_spin_unlock+0x18/0xa0  
[157355.917845]  __get_vm_area_node.isra.0+0xf4/0x190  
[157355.917854]  __vmalloc_node_range+0x64/0x2c0  
[157355.917860]  __vmalloc_node+0x5c/0x70  
[157355.917866]  vzalloc+0x34/0x50  
[157355.917871]  n_tty_open+0x1c/0x130  
[157355.917878]  tty_ldisc_open.isra.0+0x40/0xb8  
[157355.917885]  tty_ldisc_setup+0x24/0x70  
[157355.917892]  tty_init_dev+0xd8/0x1e8  
[157355.917901]  tty_open+0x450/0x580  
[157355.917908]  chrdev_open+0xa4/0x1a0  
[157355.917915]  do_dentry_open+0x12c/0x3e0  
[157355.917922]  vfs_open+0x2c/0x38  
[157355.917929]  path_openat+0x818/0xcd0  
[157355.917937]  do_filp_open+0x7c/0x100  
[157355.917945]  do_sys_openat2+0x210/0x2c8  
[157355.917955]  do_sys_open+0x58/0xa0  
[157355.917961]  __arm64_sys_openat+0x24/0x30  
[157355.917967]  el0_svc_common.constprop.0+0x78/0x1c8  
[157355.917977]  do_el0_svc+0x24/0xa0  
[157355.917984]  el0_svc+0x14/0x20  
[157355.917993]  el0_sync_handler+0xb0/0xb8  
[157355.917999]  el0_sync+0x180/0x1c0  

[157355.918007] rcu: rcu_preempt kthread starved for 2051 jiffies! g129370229 f0x0 RCU_GP_WAIT_FQS(5) ->state=0x402 ->cpu=1  
[157355.918016] rcu:    Unless rcu_preempt kthread gets sufficient CPU time, OOM is now expected behavior.  
[157355.918020] rcu: RCU grace-period kthread stack dump:  
[157355.918023] task:rcu_preempt     state:I stack:    0 pid:   12 ppid:     2 flags:0x00000028  
[157355.918032] Call trace:  
[157355.918034]  __switch_to+0xc0/0x118  
[157355.918041]  __schedule+0x298/0x818  
[157355.918047]  schedule+0xb0/0x148  
[157355.918052]  schedule_timeout+0x1b8/0x2b8  
[157355.918060]  rcu_gp_kthread+0x594/0xc00  
[157355.918067]  kthread+0x174/0x198  
[157355.918076]  ret_from_fork+0x10/0x30  
[157356.127655] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4768592 dql_avail:-32 free_desc:470  
[157362.145526] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4774610 dql_avail:-32 free_desc:470  
[157367.778511] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4780243 dql_avail:-32 free_desc:470  
[157372.898521] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4785363 dql_avail:-32 free_desc:470  
[157378.017515] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4790482 dql_avail:-32 free_desc:470  
[157384.161519] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4796626 dql_avail:-32 free_desc:470  
[157389.793510] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4802258 dql_avail:-32 free_desc:470  
[157394.913528] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4807378 dql_avail:-32 free_desc:470  
[157400.033516] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4812498 dql_avail:-32 free_desc:470  
[157406.177533] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4818642 dql_avail:-32 free_desc:470  
[157411.809518] am65-cpsw-nuss 8000000.ethernet eth0: txq:0 DRV_XOFF:0 tmo:4824274 dql_avail:-32 free_desc:470
```