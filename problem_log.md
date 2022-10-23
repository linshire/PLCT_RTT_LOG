# 目前问题

## 关于RT-USING和BSP_USING

linshire在CH32V307的开发中，发现了在驱动中RT-USING和BSP_USING中的混用

下面是在CH32V307的drv_soft_i2c.c中的宏配置，使用了#ifdef BSP_USING_SOFT_I2C

```
#include <board.h>
#include "drv_soft_i2c.h"

#ifdef BSP_USING_SOFT_I2C

//#define DRV_DEBUG
#define LOG_TAG              "drv.i2c"
#include <drv_log.h>

#if !defined(BSP_USING_I2C1) && !defined(BSP_USING_I2C2)
#error "Please define at least one BSP_USING_I2Cx"
/* this driver can be disabled at menuconfig -> RT-Thread Components -> Device Drivers */
#endif

/**************************************************************

//接口
**************************************************************/
#endif /* RT_USING_I2C */

```



而在CH32V307的drv_soft_i2c.c中，则使用了#if defined(RT_USING_SPI) && defined(RT_USING_SPI_BITOPS) && defined(RT_USING_PIN)，

```
#if defined(RT_USING_SPI) && defined(RT_USING_SPI_BITOPS) && defined(RT_USING_PIN)

#define LOG_TAG             "drv.soft_spi"
#include <drv_log.h>


#endif /* defined(RT_USING_SPI) && defined(RT_USING_SPI_BITOPS) && defined(RT_USING_PIN) */

```



至于在其他驱动文件，包括stm32等，BSP_USING与RT_USING混用也比较严重，各式各样，至于Kconfig中则稍好一些

