# Smart-ball
Проект датчика качества воздуха с дисплеем.

Устройство построено на базе ESP32-s3 supermini, но с таким же успехом можно использовать платы C3, S2 Tiny, S3 Zero и прочие в аналогичном форм-факторе.
Устройство может использоваться совместно как с Home assistant, так и автономно. 

Функционал:
1. Измерение показаний c помощью датчика углекислого газа SCD40 (SCD41): содержание CO2, температуры, влажности. Графическая шкала значения СО2 в диапазоне от 0 до 2000 ppm, графическая шкала значения температуры в диапазоне от 0 до 35 °C, цифровое отображение значений СО2 (ppm), температуры (°C) и влажности (%). Цветовая градация значений СО2 по условным границам:
      -   "Идеально" до 400 ppm;
      -   "Хорошо" от 400 до 600 ppm;
      -   "Допустимо" от 600 до 800 ppm;
      -   "Превышение" от 800 до 1000 ppm;
      -   "Не рекомендуется" от 1000 до 1200 ppm;
      -   "Опасно" свыше 1200 ppm.
   
2. Измерение уровня шума с помощью микрофонного модуля INMP441. Графическая шкала уровня шума c отображением среднего и пикового значения. Цветовая и графическая информация о погоровых значениях шума с условными границами:
      -  "Тихо" до 30 дБ;
      -  "Норма" до 30 дБ;
      -  "Шумно" до 90 дБ;
      -  "Очень шумно" до 130 дБ;
      -  "Опасно" свыше 130 дБ.

```
# Калибровка пикового значения
    peak:
      name: "Peak Loudness (dBFS)"
      id: LAeq_1min
      internal: true
      filters:
        - calibrate_linear:
            - -73.0 -> 38.0
            - -52.0 -> 64.0
            - -57.0 -> 57.6
            - -43.5 -> 85.0
            - -30.0 -> 91.4
            - -16.4 -> 100.7
# Калибровка среднего значения
    rms:
      name: "Average Loudness (dBFS)"
      id: rms_level
      internal: True
      filters:
        - calibrate_linear:
            - -73.0 -> 38.0
            - -52.0 -> 64.0
            - -57.0 -> 57.6
            - -43.5 -> 85.0
            - -30.0 -> 91.4
            - -16.4 -> 100.7
```

3. Измерение уровня освещенности с помощью модуля GY-2561. Позволяет производить замер уровня освещенности в помещении (в комнате, на рабочем месте и пр.) Автоматически регулирует подсветку дисплея для приглушения яркости в вечернее и ночное время и увеличения днем. Используемый в проекте датчик света был откалиброван с помощью поверенного прибора MOBIPLUS WT81B. Значения калибровки из конфигарации можно принять как усредненное, но стоит отметить, что датчики даже из одной поставки/партии дают различные показания.
```
        - platform: tsl2561
          name: "TSL2561 Ambient Light"
          id: level_light
          address: 0x39
          update_interval: 1s
          filters:
            - calibrate_linear: # Calibrated using the MOBIPLUS WT81B
                - 0.0 -> 0.0
                - 4.0 -> 11.8
                - 7.3 -> 34.5
                - 10.3 -> 81.2
                - 129.3 -> 190.5
```
4. Переключение страниц дисплея с помощью сенсорной кнопки:
   - главная страница;
   - страница шумометра;
   - страница с информацией устройства (имя wi-fi сети, IP-адрес, уровень освещенности в lux и пр., логотип);
   - стринички с забавными смайлами (чтоб заполнить оставшуюся память ESP)

Внимание!!!! В связи с колебанием цен, отсутствием товара или недобросовестностью продавцов, все ссылки на товары приведены исключительно для идентификации внешнего вида и ознакомлением с основными характеристикими!!!

Компоненты устройства:
1. ESP32-s3 supermini (https://aliexpress.ru/item/1005007524137218.html?spm=a2g2w.orderlist.0.0.64834aa6qNJTFR&sku_id=12000049786394478)
2. Круглый дисплей 1,5" ST77916 разрешением 360*360 (https://aliexpress.ru/item/1005009627590639.html?spm=a2g2w.orderlist.0.0.64834aa6qNJTFR&sku_id=12000049677701832)
3. Датчик CO2 SCD40 (https://aliexpress.ru/popular/scd40-co2-sensor?g=y&page=1&searchInfo=aB4g4uQLZm2THUw4LKVvmBarryb4X7RpDda4mUz9NNSFAvrdPlowPlnEJqm0UZU2CdoZfPXr35CyJY11DPImKHGQPcGu01RANW5DLc3KIoQu0kZFSy5L5Ij1GJBng-8ryqANQdi4dj3n)
4. Датчик уровня освещенности GY-2561 (на TSL2561) (https://aliexpress.ru/item/1005010472955275.html?shpMethod=CAINIAO_ECONOMY&sku_id=12000052524620301&spm=a2g2w.productlist.search_results.2.130d2064q6tgL2)
5. Микрофонный модуль INMP441 (https://aliexpress.ru/item/1005010226611558.html?shpMethod=CAINIAO_SUPER_ECONOMY&ysclid=mlzod67b3144706673&sku_id=12000051594745071)
6. Металлический контакт в качестве тач-кнопки для переключения режимов дисплея.
7. Корпус устройства, выполненный по технологии 3D-печати (case/*all*.obj)

Устройство корпуса.

<img width="735" height="533" alt="image" src="https://github.com/user-attachments/assets/bc2e50ea-2e91-43cf-b19b-c8250c1b445a" />
<img width="735" height="533" alt="image" src="https://github.com/user-attachments/assets/852a6a56-7035-4a29-a06f-13326e6df65c" />
<img width="735" height="533" alt="image" src="https://github.com/user-attachments/assets/9f0ac4e3-2908-4370-9819-ff8b223b1cde" />

<img width="735" height="533" alt="image" src="https://github.com/user-attachments/assets/1a6de048-b265-4460-9bc9-7cd1ec1852ec" />
<img width="735" height="533" alt="image" src="https://github.com/user-attachments/assets/d4128b36-be05-436c-bb02-661f21efc638" />
<img width="735" height="533" alt="image" src="https://github.com/user-attachments/assets/d4e742a7-1f00-4ee6-b3eb-c07ddf6bac14" />

Сборка

<img width="735" height="533" alt="image" src="https://github.com/user-attachments/assets/742f60de-affb-4397-864d-2aa2145d9b02" />




   <img width="841" height="1094" alt="image" src="https://github.com/user-attachments/assets/e11b30bc-e739-4809-a9e0-4c093b5e0f4b" />
