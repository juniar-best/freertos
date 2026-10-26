# 电控组第二次培训作业

**汪君奥**
***20263001769***

STM32F103CBT6 + FreeRTOS，PA0 / PA1 / PA2 轮流点亮，每颗 500ms，一轮 1.5s，然后通过数组简化程序，

## 核心思路

**开启freeRTOS，采用 `vTaskDelay()`**

```
i=0 点亮 LED0 → 等 500ms → 熄灭 → i=1
i=1 点亮 LED1 → 等 500ms → 熄灭 → i=2
i=2 点亮 LED2 → 等 500ms → 熄灭 → i=0（取余回绕）
```

```
static void vLedTask(void *pv)
{
  uint8_t i = 0U;

  (void)pv;

  for (;;)
  {
    HAL_GPIO_WritePin(GPIOA, led_pin[i], GPIO_PIN_SET);    /* 点亮 */
    vTaskDelay(pdMS_TO_TICKS(500));                        /* 非阻塞等待 500ms */
    HAL_GPIO_WritePin(GPIOA, led_pin[i], GPIO_PIN_RESET);  /* 熄灭 */

    i = (uint8_t)((i + 1U) % 3U);
  }
}

```
