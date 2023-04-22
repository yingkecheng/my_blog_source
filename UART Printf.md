---
title: UART Printf
date: 2023-04-21 19:53:10
tags: [Temporary Doc]
---

ST 官方的 UART_Printf 例程。

```c
#ifdef __GNUC__
/* With GCC, small printf (option LD Linker->Libraries->Small printf
   set to 'Yes') calls __io_putchar() */
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
#else
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#endif /* __GNUC__ */

UART_HandleTypeDef UartHandle;

PUTCHAR_PROTOTYPE
{
  /* Place your implementation of fputc here */
  /* e.g. write a character to the USART1 and Loop until the end of transmission */
  HAL_UART_Transmit(&UartHandle, (uint8_t *)&ch, 1, 0xFFFF);

  return ch;
}
```

使用 LL 库，可以将它修改为

```c
PUTCHAR_PROTOTYPE
{
  /* Place your implementation of fputc here */
  /* e.g. write a character to the USART1 and Loop until the end of transmission */
  while (!LL_USART_IsActiveFlag_TXE(USART2));
  LL_USART_TransmitData8(USART2, (uint8_t)ch);
  
  return ch;
}
```

