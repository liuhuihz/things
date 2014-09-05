.. -*- coding: utf-8 -*-

=================================
Nginx 源代码学习笔记
=================================

:Authors: 刘晖 <liuhui.hz@gmail.com>
:Version: V1.0

.. contents:: 目录

描述
=================================


源代码目录结构
=================================

::

  nginx
  ├── auto
  │   ├── cc
  │   ├── lib
  │   │   ├── geoip
  │   │   ├── google-perftools
  │   │   ├── libatomic
  │   │   ├── libgd
  │   │   ├── libxslt
  │   │   ├── md5
  │   │   ├── openssl
  │   │   ├── pcre
  │   │   ├── perl
  │   │   ├── sha1
  │   │   └── zlib
  │   ├── os
  │   └── types
  ├── conf
  ├── contrib
  │   ├── unicode2nginx
  │   └── vim
  │       ├── ftdetect
  │       ├── indent
  │       └── syntax
  ├── docs
  │   ├── dtd
  │   ├── html
  │   ├── man
  │   ├── text
  │   ├── xml
  │   │   └── nginx
  │   ├── xsls
  │   └── xslt
  ├── misc
  └── src
      ├── core
      │   ├── nginx.c
      │   ├── nginx.h
      │   ├── ngx_array.c
      │   ├── ngx_array.h
      │   ├── ngx_buf.c
      │   ├── ngx_buf.h
      │   ├── ngx_conf_file.c
      │   ├── ngx_conf_file.h
      │   ├── ngx_config.h
      │   ├── ngx_connection.c
      │   ├── ngx_connection.h
      │   ├── ngx_core.h
      │   ├── ngx_cpuinfo.c
      │   ├── ngx_crc.h
      │   ├── ngx_crc32.c
      │   ├── ngx_crc32.h
      │   ├── ngx_crypt.c
      │   ├── ngx_crypt.h
      │   ├── ngx_cycle.c
      │   ├── ngx_cycle.h
      │   ├── ngx_file.c
      │   ├── ngx_file.h
      │   ├── ngx_hash.c
      │   ├── ngx_hash.h
      │   ├── ngx_inet.c
      │   ├── ngx_inet.h
      │   ├── ngx_list.c
      │   ├── ngx_list.h
      │   ├── ngx_log.c
      │   ├── ngx_log.h
      │   ├── ngx_md5.c
      │   ├── ngx_md5.h
      │   ├── ngx_murmurhash.c
      │   ├── ngx_murmurhash.h
      │   ├── ngx_open_file_cache.c
      │   ├── ngx_open_file_cache.h
      │   ├── ngx_output_chain.c
      │   ├── ngx_palloc.c
      │   ├── ngx_palloc.h
      │   ├── ngx_parse.c
      │   ├── ngx_parse.h
      │   ├── ngx_proxy_protocol.c
      │   ├── ngx_proxy_protocol.h
      │   ├── ngx_queue.c
      │   ├── ngx_queue.h
      │   ├── ngx_radix_tree.c
      │   ├── ngx_radix_tree.h
      │   ├── ngx_rbtree.c
      │   ├── ngx_rbtree.h
      │   ├── ngx_regex.c
      │   ├── ngx_regex.h
      │   ├── ngx_resolver.c
      │   ├── ngx_resolver.h
      │   ├── ngx_sha1.h
      │   ├── ngx_shmtx.c
      │   ├── ngx_shmtx.h
      │   ├── ngx_slab.c
      │   ├── ngx_slab.h
      │   ├── ngx_spinlock.c
      │   ├── ngx_string.c
      │   ├── ngx_string.h
      │   ├── ngx_syslog.c
      │   ├── ngx_syslog.h
      │   ├── ngx_times.c
      │   └── ngx_times.h
      ├── event
      │   └── modules
      ├── http
      │   └── modules
      │       └── perl
      ├── mail
      ├── misc
      ├── mysql
      └── os
          ├── unix
          └── win32



数据结构
=================================


字符串 （ ngx_str_t ）
---------------------------------
| src/core/ngx_string.h
| src/core/ngx_string.c

.. code-block:: c

  typedef struct {
      size_t      len;
      u_char     *data;
  } ngx_str_t;



列表 （ ngx_list_t ）
---------------------------------
| src/core/ngx_list.h
| src/core/ngx_list.c

.. code-block:: c

  typedef struct ngx_list_part_s  ngx_list_part_t;

  struct ngx_list_part_s {
      void             *elts;
      ngx_uint_t        nelts;
      ngx_list_part_t  *next;
  };


  typedef struct {
      ngx_list_part_t  *last;
      ngx_list_part_t   part;
      size_t            size;
      ngx_uint_t        nalloc;
      ngx_pool_t       *pool;
  } ngx_list_t;



数组 （ ngx_array_t ）
---------------------------------
| src/core/ngx_array.h
| src/core/ngx_array.c

.. code-block:: c

  typedef struct {
      void        *elts;
      ngx_uint_t   nelts;
      size_t       size;
      ngx_uint_t   nalloc;
      ngx_pool_t  *pool;
  } ngx_array_t;


| ngx_array_create
| 在内存池中分配固定大小和元素数目的数组。

| ngx_array_push
| ngx_array_push_n
| 从数组对象中取出一个元素，如数组满了，则在内存池中扩展（内存池中剩余空间足够）
| 或重新分配更大（两倍于当前的元素数目）的数组。



红黑树 （ ngx_rbtree_t ）
---------------------------------
| src/core/ngx_rbtree.h
| src/core/ngx_rbtree.c



内存池 （ ngx_pool_t ）
---------------------------------
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
