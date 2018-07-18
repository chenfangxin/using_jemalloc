编译jemalloc-5.1.0
./configure --enable-debug --with-jemalloc-prefix=je_

在x86_64平台上，一些常量的定义：
#define LG_SIZEOF_PTR   3 // sizeof(void *) == 2^LG_SIZEOF_PTR
#define LG_TINY_MIN     3 // The range of tine size classes
#define LG_QUANTUM      4
#define LG_PAGE         12 // One page is 2^LG_PAGE bytes
#define LG_HUGEPAGE     21 // One Huge page is 2^LG_HUGEPAGE bytes

#define JEMALLOC_N(n)   je_##n  // 修改一些全局符号的名字

jemalloc-5.1.0流程：

jemalloc_constructor() ==> src/jemalloc.c
jemalloc_init() ==> src/jemalloc.c
    jemalloc_init_hard() ==> src/jemalloc.c

je_malloc(24)       ==> src/jemalloc.c
static_opts_init    ==> src/jemalloc.c
    may_overflow = false
    bump_empty_alloc = false
    assert_nonempty_alloc = false
    null_out_result_on_error = false
    set_errno_no_error = false
    min_alignment = 0
    oom_string = ""
    invalid_alignment_string = ""
    slow = false
dynamic_opts_init   ==> src/jemalloc.c
    result = 0x0
    num_items = 0
    item_size = 0
    alignment = 0
    zero = false
    tcache_ind = 0xfffffffe 
    arena_ind = 0xffffffff
imalloc             ==> src/jemalloc.c

malloc_init         ==> src/jemalloc.c

tsd_fetch           ==> include/jemalloc/internal/tsd.h
tsd_fast            ==> include/jemalloc/internal/tsd.h

imalloc_body        ==> src/jemalloc.c

compute_size_with_overflow ==> src/jemalloc.c

sz_size2index   ==> include/jemalloc/internal/sz.h
sz_sa2u         ==> include/jemalloc/internal/sz.h

check_entry_exit_locking    ==> src/jemalloc.c

imalloc_no_sample       ==> src/jemalloc.c

ipalloct    ==> include/jemalloc/internal/jemalloc_internal_inlines_c.h

iallocztm   ==> include/jemalloc/internal/jemalloc_internal_inlines_c.h

arena_malloc    ==> include/jemalloc/internal/arena_inlines_b.h

tcache_alloc_small  ==> include/jemalloc/internal/tcache_inlines.h
tcache_alloc_large  ==> include/jemalloc/internal/tcache_inlines.h

tcache_small_bin_get    ==> include/jemalloc/internal/jemalloc_internal_inlines_a.h
cache_bin_alloc_easy    ==> include/jemalloc/internal/cache_bin.h
arena_choose            ==> include/jemalloc/internal/jemalloc_internal_inlines_b.h
tcache_alloc_small_hard ==> src/tcache.c

arena_malloc_hard   ==> src/arena.c
large_malloc    ==> src/large.c
large_palloc    ==> src/large.c

----------------------------------------------------------------------------------------------
je_free     ==> src/jemalloc

tsd_fetch_min   ==> include/jemalloc/internal/tsd.h

ifree       ==> src/jemalloc.c


