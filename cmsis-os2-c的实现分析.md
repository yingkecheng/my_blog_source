---
title: CMSIS_RTOS2的FreeRTOS实现分析
date: 2023-04-21 13:34:17
tags: [RTOS, Temporary Doc]
---

# CMSIS_RTOS2 初始化流程

```mermaid
flowchart TB
	osKernelInitialize
	
	subgraph osThreadNew
	xTaskCreateStatic--- |or|xTaskCreate
	end
	
	subgraph osKernelStart
	vTaskStartScheduler
	end
	
	osKernelInitialize --> osThreadNew --> osKernelStart
```

