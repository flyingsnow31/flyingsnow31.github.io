---
title: "AFL++ Custom Mutators"
date: 2023-11-14T16:00:04+08:00
tags: ["AFL++", "Custom Mutators"]
categories: 模糊测试
---

# AFL++ Custom Mutators

用于定制化自己的变异模块，目前支持 C/C++ 和 Python，实验性支持 Rust

## 环境变量

```shell
# 开启Custom Mutators
AFL_CUSTOM_MUTATOR_LIBRARY=1
# 使用到的SO库
AFL_CUSTOM_MUTATOR_LIBRARY=“full/path/to/mutator_first.so;full/path/to/mutator_second.so”
# 开启后只使用Custom Mutators
AFL_CUSTOM_MUTATOR_ONLY=1
```



## API

```c
// 初始化AFL,提供seed
void *afl_custom_init(afl_state_t *afl, unsigned int seed);
// 设置使用该输入进行模糊尝试的次数
unsigned int afl_custom_fuzz_count(void *data, const unsigned char *buf, size_t buf_size);
// 当代码中出现时，便表示激活此方法，用于指示AFL不需要对数据进行合并
void afl_custom_splice_optout(void *data);
// 具体的变异函数，对给定的input即buf进行变异，返回写入out_buf中，返回值为长度，还可以支持额外拼接数据
size_t afl_custom_fuzz(void *data, unsigned char *buf, size_t buf_size, unsigned char **out_buf, unsigned char *add_buf, size_t add_buf_size, size_t max_size);
// 用于返回由最后一次变异后的当前测试用例
const char *afl_custom_describe(void *data, size_t max_description_len);
// 在变异后测试前进行一些正确性相关的检验
size_t afl_custom_post_process(void *data, unsigned char *buf, size_t buf_size, unsigned char **out_buf);
// 对返回的数据进行修建，多用于python模块
int afl_custom_init_trim(void *data, unsigned char *buf, size_t buf_size);
size_t afl_custom_trim(void *data, unsigned char **out_buf);
int afl_custom_post_trim(void *data, unsigned char success);
//
size_t afl_custom_havoc_mutation(void *data, unsigned char *buf, size_t buf_size, unsigned char **out_buf, size_t max_size);
unsigned char afl_custom_havoc_mutation_probability(void *data);
// 决定是否要对所给的filename中的测试用例进行测试，返回1/0
unsigned char afl_custom_queue_get(void *data, const unsigned char *filename);
// 可以自己发送数据到目标，而不是通过afl
void (*afl_custom_fuzz_send)(void *data, const u8 *buf, size_t buf_size);
// 检测测试用例的文件是否发生改变
u8 afl_custom_queue_new_entry(void *data, const unsigned char *filename_new_queue, const unsigned int *filename_orig_queue);
// 
const char* afl_custom_introspection(my_mutator_t *data);
// deinit
void afl_custom_deinit(void *data);
```



SQL fuzz的流程

需要什么接口

可以额外提供一些定制的插桩信息



新数据库只需要连接，变异，生成







