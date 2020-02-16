## Домашнее задание к занятию «3.2. SQL»

### Задача №1 - Скоро deadline

Что нужно сделать:

* изучить схему БД (см. файл schema.sql)
* создать Docker Container на базе MySQL 8.0.18 
(прописать создание БД, пользователя, пароля)
* запустить SUT (app-deadline.jar): 

для указания параметров подключения к БД можно 
использовать:

  * либо переменные окружения DB_URL, DB_USER, DB_PASS
  * либо указать их через флаги командной 
  строки при запуске: -P:jdbc.url=..., -P:jdbc.user=...,
  -P:jdbc.password=... 
  (внимание: при запуске флаги не нужно указывать 
  через запятую!)
  * либо можете схитрить и попробовать подобрать значения, 
  зашитые в саму SUT
  
  **Проблемы:**
  
  1. SUT не стартует.
  SUT не создаёт самостоятельно таблицы в БД.  
  Нужно на сайте-описании Docker Image MySQL посмотреть,
   как при инициализации скармливать схему.
  
  1. SUT валится при повторном перезапуске.
       SUT вставляет в БД демо-данные, а поскольку там есть 
       ограничение уникальности, это приводит к ошибкам.  
       Нужно настроить вычистку данных за SUT.
       
   1. В базе данных пароль пользователя хранится в 
   зашифрованном виде.      
    Записать его туда в открытом виде не получится.
    Решение: с помощью DBeaver можно увидеть, что при создании
    нового пользователя всегда генерируется один и тот же пароль. 
    В зашифрованном виде он принадлежит пользователю с логином vasya. 
    Копируем зашифрованный пароль из DBeaver и вставляем в код тестов.
    
    
   **Задачи:** 
   * Нужно посмотреть, как и куда сохраняются коды 
   генерации в СУБД и написать тест, который взяв информацию из БД 
   о сгенерированном коде позволит протестировать "Вход в систему" 
   через веб-интерфейс.
   
   * Проверить, что при трёхкратном неверном вводе пароля система блокируется.
   
   Система: Windows 10 Домашняя версия
   Docker Toolbox
   
   **Запуск:**
   
   1. Запускаем Docker Toolbox
   1. В терминале IDEA набираем docker-compose up
   1. Во втором терминале набираем 
   ````
   java -jar app-deadline.jar -P:jdbc.url=jdbc:mysql://192.168.99.100:3306/app
   ````
   1. Запускаем автотесты


 