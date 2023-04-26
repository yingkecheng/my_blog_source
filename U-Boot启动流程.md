---
title: U-Boot启动流程
date: 2023-04-22 17:36:56
tags: [Linux, 笔记, Temporary Doc]
---

```mermaid
flowchart TB
	entry([entry of save_boot_params_ret])
	step_1(Disable interrupts)
	step_2(Setup vector)
	step_3[[cpu_init_cp15]]
	step_4[[cpu_init_crit]]
	step_5[[_main]]
	exit([exit of save_boot_params_ret])
	entry --> step_1 --> step_2 --> step_3 --> step_4 --> step_5 --> exit
```
| 函数            | 功能                                     |
| --------------- | ---------------------------------------- |
| `cpu_init_cp15` | Setup CP15 registers (cache, MMU, TLBs). |
| `cpu_init_crit` | Setup important registers. |

> Q：如果我想了解 CP15 该去查找什么资料呢
> A：如果你想了解 CP15，可以查找 ARMv7-A 架构参考手册，其中包含了关于 CP15 寄存器的详细说明和操作方法。另外，你也可以查找相关的嵌入式系统开发书籍，比如《嵌入式系统实战开发》，其中也有涉及到 CP15 的介绍。

`cpu_init_crit`调用了`lowlevel_init`。

```mermaid
flowchart TB
	entry([entry of lowlevel_init])
	step_1(Setup a temporary stack)
	step_2[[s_init]]
	exit([exit of lowlevel_init])
	entry --> step_1 --> step_2 --> exit
	
```

```mermaid
flowchart TB
	entry([entry of _main])
	subgraph sub1[ ]
		op1(Set up initial environment)
        op2a[[board_init_f_alloc_reserve]]
        op2b[[board_init_f_init_reserve]]
        op2c[[board_init_f]]
        
        op1 --> op2a --> op2b --> op2c
	end
		
	subgraph sub2[ ]
        op3(Set up intermediate environment)
        op4a[[relocate_code]]
        op4b[[relocate_vectors]]
        
        op3 --> op4a --> op4b
	end
	
	subgraph sub3[ ]
		op5[[c_runtime_cpu_setup]]
		op6(Clear the BSS section)
        op7[[board_init_r]]
		
		op5 --> op6 --> op7
	end

	exit([exit of _main])
	
	entry --> sub1 --> sub2 --> sub3 --> exit
	
	
```

| Function           | Purpose |
| ------------------ | ------- |
| `board_init_f`     |         |
| `relocate_code`    |         |
| `relocate_vectors` |         |
| `board_init_r `| |

```mermaid

  


```

