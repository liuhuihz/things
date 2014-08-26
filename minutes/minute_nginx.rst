Welcome to Nginx's documentation!
=================================

Contents:

.. toctree::
   :maxdepth: 2


内存池 （ ngx_pool_t ）
----------------------------
| src/core/ngx_palloc.h
| src/core/ngx_palloc.c

.. code-block:: c

  typedef void (*ngx_pool_cleanup_pt)(void *data);

  typedef struct ngx_pool_cleanup_s  ngx_pool_cleanup_t;

  struct ngx_pool_cleanup_s {
      ngx_pool_cleanup_pt   handler;
      void                 *data;
      ngx_pool_cleanup_t   *next;
  };


  typedef struct ngx_pool_large_s  ngx_pool_large_t;

  struct ngx_pool_large_s {
      ngx_pool_large_t     *next;    /* 大内存块链表中的下个大内存块 */
      void                 *alloc;   /* 大内存地址 */
  };


  typedef struct {
      u_char               *last;    /* 可分配内存的起始地址 */
      u_char               *end;     /* 内存块的结束地址 */
      ngx_pool_t           *next;    /* 内存池链表中下个内存池 */
      ngx_uint_t            failed;  /* 失败的分配次数 */
  } ngx_pool_data_t;


  struct ngx_pool_s {
      ngx_pool_data_t       d;       /* 内存块元数据 */
      size_t                max;     /* 最大可分配内存大小 */
      ngx_pool_t           *current; /* 内存池链表中当前使用的内存池 */
      ngx_chain_t          *chain;   /* 内存池链表 */
      ngx_pool_large_t     *large;   /* 大内存块链表 */
      ngx_pool_cleanup_t   *cleanup; /* 清理函数链表 */
      ngx_log_t            *log;     /* 日志 */
  };


采用类似 slab 实现方式

| ngx_alloc
| ngx_calloc
| src/os/unix/ngx_alloc.h
| src/os/unix/ngx_alloc.c
| 平台相关内存分配函数的封装。

| ngx_create_pool

| ngx_palloc
| 从内存池中分配内存（需对齐）

| ngx_pnalloc
| 从内存池中分配内存（不需对齐）

| ngx_pcalloc
| 从内存池中分配内存（需对齐），并置 0 。

| ngx_palloc_block

| ngx_palloc_large
| ngx_pmemalign

| ngx_pfree
| 如果是分配的大内存块，则进行释放。



数组 （ ngx_array_t ）
----------------------------
| src/core/ngx_array.h
| src/core/ngx_array.c

| ngx_array_create
| 在内存池中分配固定大小和元素数目的数组。

| ngx_array_push
| ngx_array_push_n
| 从数组对象中取出一个元素，如数组满了，则在内存池中扩展（内存池中剩余空间足够）
或重新分配更大（两倍于当前的元素数目）的数组。


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

