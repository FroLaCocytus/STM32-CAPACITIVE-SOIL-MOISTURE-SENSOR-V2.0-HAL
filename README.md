<h1 align="center">STM32 HAL</h1>

<p align="center"><b>Capacitive soil moisture sensor v2.0</b><p>

<p align="center">
  <img src="https://drive.google.com/uc?export=view&id=1g5pYPf5x-LOnBUrd1XObgtMll5xMcTXE" width="400" alt="400">
</p>

---

<p align="center"><b>Порядок применения</b><p>

1) Создайте проект в STM32CubeIDE. Подключите датчик к аналоговому пину (АЦП/ADC) . Сгенерируйте проект.
2) Скопируйте файлы `capacitive_soil_moisture_v2.h` и `capacitive_soil_moisture_v2.c` в папки `Inc` и `Src` соответсвенно.
3) Создайте переменную `CSMV2_sensor` и передайте в неё созданный автоматически средой STM32CubeIDE параметр `hadc`.
4) Вызовите функцию `CSMV2_getData()` для получения информации с датчика.

---

<p align="center"><b>Пример кода</b><p>

```c
#include "capacitive_soil_moisture_v2.h"

int main(void) {
  
  //Создание объекта дачика влажности почвы
  static CSMV2_sensor csmv2_sensor = {&hadc1};
  
  while(1) {

    char message[80]; 
    CSMV2_data csmv2_data = CSMV2_getData(&csmv2_sensor); //Получение данных с датчика
    sprintf(message, "ADC value: %u, soil moisture in percent: %u\n", csmv2_data.adc_value, csmv2_data.moisture_percent);
    
    HAL_UART_Transmit(&huart2, (uint8_t*)message, strlen(message), HAL_MAX_DELAY); //Отправляем через UART
  }
}
```

<p align="center">
  <img src="https://drive.google.com/uc?export=view&id=1HGc_fZsZ6nLXU-a0ZuqaDOJJEi5yrYba">
</p>

---

<p align="center"><b>Калибровка датчика :)</b><p>

В файле `capacitive_soil_moisture_v2.h` есть два параметра:

```c
#define ADC_MAX_MOISTURE 1200		// Значение ADC достигнутое при максимальной влажности почвы
#define ADC_MIN_MOISTURE 2400		// Значение ADC достигнутое при минимальной влажности почвы
```

Для калибровки нужно менять эти параметры проведя несколько тестов со своим растением (значение брать из `csmv2_data.adc_value`):

* Измеряем предельно сухую землю у растения и меняем параметр `ADC_MAX_MOISTURE`.
* Поливаем растение, измеряем влажность почвы и меняем параметр `ADC_MIN_MOISTURE`.

---