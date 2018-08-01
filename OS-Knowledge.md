# 1 内存

- 内存基本概念

> 物理内存三级架构

> - 1.内存节点node (数据结构 - pg_data_t)

>> CPU被划分为多个节点(node), 内存则被分簇, 每个CPU对应一个本地物理内存, 即一个CPU-node对应一个内存簇bank，即每个内存簇被认为是一个节点

> - 2.管理区Zone （数据结构 - node_zones）

>> 每个物理内存节点node被划分为多个内存管理区域, 用于表示不同范围的内存, 内核可以使用不同的映射方式映射物理内存

> - 3.页面Page (数据结构 - page)

>> 内存被细分为多个页面帧, 页面是最基本的页面分配的单位

> Linux虚拟内存三级页表

> - 1.PGD: Page Global Directory (页目录)

> - 2.PMD: Page Middle Directory (页目录)

> - 3.PTE:  Page Table Entry  (页表项)

> - PFN : Page Frame Number (页表数目)

> COW (copy-on-write)：写时复制

> TLB (Translation lookaside buffer): 旁路转换缓冲,也叫做快表

- 初始化内存

> bootmem(引导内存分配器)

>> 产生原因

>>> 由于在系统初始化的时候需要执行一些内存管理，内存分配的任务，这个时候buddy系统，slab分配器等并没有被初始化好，此时就引入了一种内存管理器bootmem分配器在系统初始化的时候进行内存管理与分配，当buddy系统和slab分配器初始化好后，在mem_init()中对bootmem分配器进行释放，内存管理与分配由buddy系统，slab分配器等进行接管。

>> bootmem分配器是系统启动初期的内存分配方式。使用一个bitmap来标记物理页是否被占用，分配的时候按照第一适应的原则，从bitmap中进行查找，如果这位为1，表示已经被占用，否则表示未被占用。bootmem分配器每次在bitmap中进行线性搜索，效率非常低，而且在内存的起始端留下许多小的空闲碎片，在需要非常大的内存块的时候，检查位图这一过程就显得代价很高。所以只是在系统启动初期才使用

> memblock(内存分配器)

>> 和bootmem类似，API兼容

- 高端内存 (详见 https://blog.csdn.net/vite_s/article/details/70160373 )

>> 当内核模块代码或线程访问内存时，代码中的内存地址都为逻辑地址，而对应到真正的物理内存地址，需要地址一对一的映射，如逻辑地址0xc0000003对应的物理地址为0x3，0xc0000004对应的物理地址为0x4，… …，逻辑地址与物理地址对应的关系为 物理地址 = 逻辑地址 – 0xC0000000。(注意内核的虚拟地址在高端，但是物理地址在低端)。 这样就无法访问到用户地址空间

>> x86架构中将内核地址空间划分三部分：ZONE_DMA、ZONE_NORMAL和ZONE_HIGHMEM。ZONE_HIGHMEM即为高端内存，这就是内存高端内存概念的由来。

>> - 1.ZONE_DMA        内存开始的16MB

>> - 2.ZONE_NORMAL       16MB~896MB

>> - 3.ZONE_HIGHMEM       896MB ~ 结束

>> 可以看见，内核的高端地址为896 ~ 1G 的 128MB 内存。

>> 高端内存的最基本思想:

>>> 借一段地址空间，建立临时地址映射，用完后释放，达到这段地址空间可以循环使用，访问所有物理内存。


- slab/slub

> slab

- 常见的内存访问错误

> 1.越界访问(out of bounds)

> 2.访问已经释放的内存(use after free)

> 3.重复释放

> 4.内存泄露(memory leak)

> 5.栈溢出(stack overflow)


 



