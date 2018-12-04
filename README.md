# Тестовое задание javaRush

### 1. Задачи

#### Требуемые технологии:
* Maven 3 (v.3.3.9) (для сборки проекта);
* Tomcat8 (для тестирования своего приложения);
* Spring (версия не ниже 4.3.0.RELEASE);
* Hibernate (версия не ниже 5.2.1.Final);
* MySQL (база данных). 
    >Для упрощения тестирования называйте все свою базу `test`, с логином и паролем `root`(нам не нужно будет для тестирования создавать кучу лишних и ненужных баз);
* Frontend: Spring MVC, или Angular, или Vaadinor, или ZK framework;
* Результат выложить на GitHub или Bitbucket.

>Версии можно смело брать самые последние. Конфликтов быть не должно.

#### CRUD(create, read, update, delete).
Необходимо реализовать стандартное CRUD приложение, которое отображаем список всех книг в базе (с пейджингом по 10 книг на странице). С возможностью их удаления, редактирования, добавления новых, и поиска по уже существующим.

У вас есть всего 1 таблица book. В ней хранится список книг(например, на книжной полке).Книги на полку можно добавлять (create), брать посмотреть(read), заменять на новый выпуск (update), убирать (delete).

**Данные, которые должны быть в таблице:**
* `id` –идентификатор книги в БД;
* `title` –название книги. Можно использовать тип VARCHAR(100);
* `description` –краткое описание о чем книга. Можно использовать тип VARCHAR(255);
* `author` –фамилия и имя автора. Можно использовать тип VARCHAR(100);
* `isbn` –ISBNкниги. Можно использовать тип VARCHAR(20);
* `printYear` –в каком году напечатана книга (INT);
* `readAlready` –читал ли кто-то эту книгу. Это булевополе.

**Бизнес-требование:** 
при редактировании может быть 2 варианта поведения:
* Книгу прочитали, и тогда изменяется только поле `readAlready`, и только, если оно было `false`.Значения поля должно стать `true`.
* Книгу заменили на новое издание. В этом случае должна быть возможность изменить `title`, `description`, `isbn`, `printYear`. А поле `readAlready` нужновыставить в `false`.Поле `author` должно быть неизменяемым с момента создания книги. 
* По какому полю искать – каждый решает для себя сам. Можно ограничиться полем title, но согласитесь, удобно просмотреть книги, которые еще не читал, или, которые вышли после 2016 года.


#### NOTES
Реализовать простенькое приложение **Notes-list**, для отображения списка заметок.

Нужно показывать список уже созданных заметок(с пейджингомпо 10 заметокна странице). Каждуюиз них можно редактировать, добавлять новые, отмечать как «Выполнено», удалять. Список можно фильтровать как «Все заметки», «Только невыполненные», «Выполненные». 

Заметки хранить в базе. Схему таблички для хранения нужно придумать самому (можно ограничиться одной таблицей).

**Бизнес-требование:** кроме фильтрации должна быть возможность сортировки заметокпо дате создания (например, поле `createdDateв` БД). Тип поля – `DATE` или `DATETIME`, или `TIMESTAMP`.

***

### 2. Что нам понадобиться

*   [IntelliJ IDEA](https://www.jetbrains.com/idea/download/);
*   [JDK 6](http://www.oracle.com/technetwork/java/javase/downloads/index.html) или более поздняя [версия](http://www.oracle.com/technetwork/java/javase/downloads/index.html);
*   [Maven](https://maven.apache.org/download.cgi);
    * Как установить и собрать простой Java-проект с помощью Maven читаем [здесь](https://spring.io/guides/gs/maven/);
*   [MySQL](https://dev.mysql.com/downloads/installer/);
*   [Git](https://git-scm.com/book/ru/v2/%D0%92%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-Git);

### 3. Установка Spring 

Для выполнения задач нам требуется Spring Framework. Мы будем использовать Spring Boot 1.5.8

*   Генерируем Spring с помощью [SPRING INITIALIZR](https://start.spring.io/)
    *   зависимости:
        *   Web - стек пакетов для web разработки с Tomcat и Spring MVC;
        *   JPA - JPA Persistence API, включая Spring-data-jpa, Spring-orm и Hibernate;
        *   JDBC - Базы данных JDBC;
        *   MySQL - MySQL jdbc драйвер.
*   Качаем и распаковываем архив;
*   Импортируем проект в IntelliJ IDEA. Как это сделать читаем [здесь](https://spring.io/guides/gs/intellij-idea/).

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/de7068c267681004c04419305dcc000c858934c9)

### 4. Создание и подключение базы данных
* Создаем пустую базу
    
    `mysql>  CREATE DATABASE IF NOT EXISTS test;`
    
* [Подключаем ее в IntelliJ IDEA](https://www.jetbrains.com/help/idea/working-with-the-database-tool-window.html#create_data_source)
* [Настраиваем подключение в Spring](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-connect-to-production-database)
    
    `application.properties`
    
    ```properties
    spring.jpa.hibernate.ddl-auto=none
    spring.datasource.url=jdbc:mysql://localhost:3306/test?autoReconnect=true&useSSL=false
    spring.datasource.username=root
    spring.datasource.password=root
    ```

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/6e13e0a955338ed46ef796ed3ec1fe0934ace46a)

### 5. Проверяем работоспособность
* Создаем первый контроллер [Controller.HomeAction](https://github.com/vladmeh/javaRushTestTask/blob/a9ec5dfab0c1c38431754aa40ba9b4562e6c35a7/src/main/java/com/vladmeh/javaRushTestTask/Controller/HomeAction.java);
* Первое тестирование
    * [ApplicationTest](https://github.com/vladmeh/javaRushTestTask/blob/07d4d78265f902c29d89e1a5b40f53230ac8cc39/src/test/java/com/vladmeh/javaRushTestTask/ApplicationTest.java) - проверяет что контекст создает наш контроллер, а так же обрабатывает наш входящий HTTP запрос правильно (без затрат на запуск сервера) 
    * [HttpRequestTest](https://github.com/vladmeh/javaRushTestTask/blob/07d4d78265f902c29d89e1a5b40f53230ac8cc39/src/test/java/com/vladmeh/javaRushTestTask/HttpRequestTest.java) - обрабатывает наш входящий HTTP запрос
    
    
>Подробнее о тестировании Spring boot [здесь](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-testing.html) и [здесь](https://docs.spring.io/spring/docs/current/spring-framework-reference/testing.html#integration-testing-support-jdbc)

* Запускаем JavaRushTestTaskApplication в IDEA
    *   в браузере http://localhost:8080 будет выведено "Hello World";
* Запускаем тесты - все тесты должны проити успешно.

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/07d4d78265f902c29d89e1a5b40f53230ac8cc39)

### 6. Создаем таблицу и модель данных
* Сразу же создаем [скрипт sql](https://github.com/vladmeh/javaRushTestTask/blob/2c7eae388eb36dceecb1ea6cdde6cb87db4ce71d/src/main/resources/database.sql) где пишем создание базы, таблицы, полей.
* Создаем модель данных [Entity.Book](https://github.com/vladmeh/javaRushTestTask/blob/fa277c2f0d5697878cb013e5a888d775bde06e92/src/main/java/com/vladmeh/javaRushTestTask/Entity/Book.java)
* Создаем интерфейс репозитария [Repository.BookRepository](https://github.com/vladmeh/javaRushTestTask/blob/fa277c2f0d5697878cb013e5a888d775bde06e92/src/main/java/com/vladmeh/javaRushTestTask/Repository/BookRepository.java) который пока наследуется от CrudRepository
* Сoздаем контроллер [Controller.BookController](https://github.com/vladmeh/javaRushTestTask/blob/fa277c2f0d5697878cb013e5a888d775bde06e92/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) с одним методом `getAllBook()` который будет возвращать полный список наших книг

>Подробнее о создании Spring Data JPA читаем [здесь](https://www.petrikainulainen.net/spring-data-jpa-tutorial)

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/fa277c2f0d5697878cb013e5a888d775bde06e92)

### 7. Реализация сервисного интерфейса
* Обновляем модель данных [database.sql](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/resources/database.sql)
    * в модель данных добаляем сценарий для наполнения данными
* Создаем интерфейс [Service.BookService](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java)
* Создаем класс реализации интерфейса BookService [Service.BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java) 
* Обновляем контроллер [Controller.BookController](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)
    * контроллер теперь реализует свои методы через нашу сервисную службу [BookService](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java)
    * добавляем методы 
        * `findBookById` -  поиск конкретной книги по id
        * `create` - создание новой книги
        * `update` - обновление существующей книги
        * `delete` - удаление существующей книги
* Тестируем
    * Модульное тестирование контроллера с помощью unit теста [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
    * Работоспособность я тестировал с помощью сервиса [Postman Echo](https://www.getpostman.com/), с помощью которого посылаем запрос на наш сервер например добавим запись:
    ```cfml
    POST /books HTTP/1.1
    Host: localhost:8080
    Content-Type: application/json
    Cache-Control: no-cache
    
    {
    "title": "PHP объекты, шаблоны и методики программирования",
    "description": "Создавайте высокопрофессиональный код на PHP, изучив его объектно-орентированные возможности, шаблоны проектирования и важные средства разработки.",
    "autor": "Мет Зандстра",
    "isbn": "978-5-8459-1689-1",
    "printYear": 2013,
    "readAlready": true
    }
    ```
    
    удалим эту запись:
    
    ```cfml
    DELETE /books/14 HTTP/1.1
    Host: localhost:8080
    Cache-Control: no-cache
    ```

На данном этапе по адресу http://localhost:8080/books можем видеть список наших книг в формате json, а по адресу http://localhost:8080/books/1 - данные одной книги. Можем создавать, удавлять и обновлять наши записи.

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/755320b2d6eff2f8023453e9659a238f290d574e)


### 8. Пейджинг и сортировка.
* Модифицируем интерфейс [BookRepository](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Repository/BookRepository.java)
    * изменяем интерфейс так что бы он расширял интерфейс `PagingAndSortingRepository<T, ID extends Serializable>`
* Делаем изменения в интерфейс [BookService](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java)
    * добавляем метод `findAllByPage`, который будет принимать в качестве аргумента экземпляр интерфейса `Pageable`
* Реализуем метод `findAllByPage` в классе [BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java)

#### Разбиение на страницы.
* Модифицируем контроллер [BookController](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)
    * добаляем метод `getPageBooks`, который возращает постраничный список книг, с параметром `page` (номер страницы) с дефолтным значением `1`. Если параметр не указан будет выводиться первая страница.
    >метод `getAllBook` оставляем (изменим только у него `path`, что бы не было конфликтов), он нам может понадобится.
    
Сейчас по запросу в браузере `http://localhost:8080/books?page=2` будет выводиться 2-я страница списка наших книг

#### Сортировка.
* Модифицируем контроллер [BookController](https://github.com/vladmeh/javaRushTestTask/blob/9574047f23b003ab245996c2063547737ab0c078/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)
    * в метод `getPageBooks` добавляем два параметра `String sortBy` и `String order` отвечающих по какому полю будем сортировать и направление сортировки, соответственно. Выставляем дефолтные значения - по умолчанию (без паравметров) список будет сортироваться по `id` и по возрастанию;
    * реализуем сортировку.
    
По запросу в браузере `http://localhost:8080/books?page=2&sortBy=printYear&order=desc` будет выводиться 2-я страница списка наших книг, список отсортирован по году выпуска книги, по убыванию.

#### Тестирование
* Создаем класс [BookBuilder](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/BookBuilder.java)
* Создаем класс [PageBuilder](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/PageBuilder.java)
    >для чего это надо и что это такое читаем [здесь](http://www.natpryce.com/articles/000714.html)
    
В имеющимся тестовом классе `Controller.BookControllerTest` мы, по-сути, тестировали поведение нашего сервиса `BookService`. Поэтому переименуем тестовый класс на [Service.BookServiceTest](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/Service/BookServiceTest.java).

* Переименовываем `Controller.BookControllerTest` -> `Service.BookServiceTest`;
    * пишем модульные тесты для методов `getPageBooks`, `findBookById`, `delete`;
* Создаем новый тестовый класс [Controller.BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java).
    * пишем методы для тестирования ответа сервера на наши запросы, в разных вариациях
    * при тестировании методов `create`, `update`, `delete` используем `Transactional` и `EntityManager` который методом `flush()` сбрасывает все изменения в базе данных.
    
>Подробнее по тестированию Spring MVC читаем [здесь](https://spring.io/guides/tutorials/bookmarks/#_testing_a_rest_service), 
> [здесь](https://www.petrikainulainen.net/programming/spring-framework/unit-testing-of-spring-mvc-controllers-rest-api/) и [здесь](https://www.petrikainulainen.net/programming/spring-framework/unit-testing-of-spring-mvc-controllers-normal-controllers/), а так-же в книге "Spring 4 для профессионалов", которая прилагается в доп.материалах к тестовому заданию.

При тестировании метода `update` контроллера `BookController` обнаружилось что он (метод) работает неправильно. По сути он делает тоже что и метод `create`.

#### Исправляем метод BookController.update() 
* В сервис [Service.BookService](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java) пишем новый метод `Book update()`;
* Реализуем его в классе [Service.BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java);
* Вносим изменения в метод [BookController.update()](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java).
* Пишем модульный тест в [BookServiceTest](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/Service/BookServiceTest.java) для метода `update`

[Итог](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4)

### 9. Поиск
* Расширяем [BookRepository](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Repository/BookRepository.java) 
    * добавляем методы, в кторых с помощью `@Query` директивы делаем запросы к базе данных.
    > подробнее читаем [Spring Data JPA Tutorial: Introduction to Query Methods](https://www.petrikainulainen.net/programming/spring-framework/spring-data-jpa-tutorial-introduction-to-query-methods/)
* Добавляем методы поиска в [BookService](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java)
* Реализуем эти методы в [BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java)
* Добавляем метод `search` в контроллер [BookController](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)

Теперь по запросу в браузере `http://localhost:8080/books/search` с параметрами мы можем производить поиск, сортировать, фильтровать наши данные, например по `http://localhost:8080/books/search?term=java&afterYear=2016&order=desc` будет выведен список книг, которые в заголовке, описаниии или авторе содержат слово "java", напечатанные после 2016 года и отсортированные по убыванию.

* Тестируем
    * добавляем модульный тест в [BookServiceTest](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/test/java/com/vladmeh/javaRushTestTask/Service/BookServiceTest.java)
    * добавляем интеграционные тесты в [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
    
На [данном этапе](https://github.com/vladmeh/javaRushTestTask/tree/ede3e501f82de66a152d0622a139aa6ca1ef6768) мы имеем полноценное (простое) CRUD приложение которое может по нашим запросам возвращать нам данные в формате JSON.

### 10. Создание первого представления

Для реализации визуального представления нашего приложения я использовал [Thymeleaf](http://www.thymeleaf.org/) - шаблонизатор HTML и фреймворк [Bootstrap v.4.0.0](https://getbootstrap.com)

* Добавляем в [pom.xml](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/pom.xml) зависимости Maven для представления
    * ```xml
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.webjars/bootstrap -->
            <dependency>
                <groupId>org.webjars</groupId>
                <artifactId>bootstrap</artifactId>
                <version>4.0.0</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.webjars/jquery -->
            <dependency>
                <groupId>org.webjars</groupId>
                <artifactId>jquery</artifactId>
                <version>3.3.1</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.webjars/webjars-locator -->
            <dependency>
                <groupId>org.webjars</groupId>
                <artifactId>webjars-locator</artifactId>
                <version>0.32-1</version>
            </dependency>
        </dependencies>
      ```
* Пишем основной шаблон [default.html](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/main/resources/templates/default.html) для нашего приложения;
* Пишем шаблон для домашней страницы [index.html](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/main/resources/templates/index.html);
* Изменяем контроллер [Controller.HomeAction](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/main/java/com/vladmeh/javaRushTestTask/Controller/HomeAction.java);
* Тестируем
    * Проверяем визуально
        * На главной странице http://localhost:8080 должен отображаться текст `Hello, World!` в виде заголовка h1;
        * Если добавить параметр name в запросе, например `http://localhost:8080?name=Vlad`, будет отображаться текст `Hello, Vlad!`;
        
    * Запускаем тесты [ApplicationTest](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/test/java/com/vladmeh/javaRushTestTask/ApplicationTest.java) и [HttpRequestTest](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/test/java/com/vladmeh/javaRushTestTask/HttpRequestTest.java).
    
> Для построения шаблонов я также испоьзую [Thymeleaf Layout Dialect](https://ultraq.github.io/thymeleaf-layout-dialect/) который позволяет создавать макеты и шаблоны многократного использования.

Чтобы в дальнейшем иметь возможность получать данные в JSON формате, сохранить наши интеграционные тесты и не иметь конфликтов с url, я переименовал `BookController -> BookRestController`

* Переименовываем контроллер в IJ Idea с помощью `Shift + F6`, что даст нам предложение переименовать упоминание контоллера во всех классах приложения.
    * Автоматически переименуется `BookControllerTest -> BookRestControllerTest`
* В [BookRestController](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookRestController.java) меняем `path = "/books/api"`;
* В [BookRestControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookRestControllerTest.java) меняем все пути `/books/**` на `books/api/**`;
* Запускаем [BookRestControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookRestControllerTest.java) и [BookServiceTest](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/test/java/com/vladmeh/javaRushTestTask/Service/BookServiceTest.java), все тесты должны проходить.

### 11. Предаствление для списка книг

* Создаем новый [BookController](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)
    * Пишем метод `viewBooksList()`    
* Создаем шаблон для списка [book/list.html](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/main/resources/templates/books/list.html)
* Тестируем
    * Визуально по запросу http://localhost:8080/books мы можем видеть список первых десяти книг нашего списка в таблице.
    * По запросу http://localhost:8080/books?page=2 - вторую страницу нашего списка.
    * Пишем интеграционные тесты [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/2e84387722e761060440bcdf7a4b42d3d0792dc3/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
        * Тестируем ответ сервера на наш запрос без параметров
        * Тестируем ответ сервера на наш запрос с параметрами
          
[Итог](https://github.com/vladmeh/javaRushTestTask/tree/2e84387722e761060440bcdf7a4b42d3d0792dc3)

### 12. Дорабатываем представление для списка книг

* Изучаем [Thymeleaf v.2.1](http://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html), т.к. эта версия предложена в Spring boot на данный момент.
* Изучаем [Bootstrap v.4.0.0](https://getbootstrap.com/docs/4.0/getting-started/introduction)
* Вместо таблицы, вывод наших книг оформляем в виде карточек.
* К каждой книге мы добавим обложки (изображения)
    *   В таблицу `book` добавляем новое поле
        ```bash
        mysql> ALTER TABLE book ADD image_str varchar(255) NULL;
        ```
        поле строкового типа, т.к. на данном этапе в базе данных мы будем хранить имена файлов изображений.
    *   В сущность [Entity.Book](https://github.com/vladmeh/javaRushTestTask/blob/6e4e621a213b0596bb10fd35b06acf3fb4fa36ca/src/main/java/com/vladmeh/javaRushTestTask/Entity/Book.java) добавляем приватное поле строкового типа `private String imageStr`, создаем для него геттер и сеттер.
    *   В базу данных для каждой книги добаляем имена файлов изображений, сейчас это делаем вручную. Далее, когда мы будем делать представления для добавления и редактирования книги, то реализуем загрузку файлов изображений через форму. 
* Оформляем и добавляем к шаблону списка [book/list.html](https://github.com/vladmeh/javaRushTestTask/blob/6e4e621a213b0596bb10fd35b06acf3fb4fa36ca/src/main/resources/templates/books/list.html) пагинатор [fragments/pagination.html](https://github.com/vladmeh/javaRushTestTask/blob/6e4e621a213b0596bb10fd35b06acf3fb4fa36ca/src/main/resources/templates/fragments/pagination.html).
* Выделяем прочитанные книги.
 
[Итог](https://github.com/vladmeh/javaRushTestTask/tree/6e4e621a213b0596bb10fd35b06acf3fb4fa36ca)
 
* Устанавливаем и настраиваем фильтры
   * форму поиска
   * фильтр прочитанных и не прочитанных книг
   * фильтр по году издания
 
[Итог](https://github.com/vladmeh/javaRushTestTask/tree/12f9501926b65a0df9473b18c1c7f85c631f09bc)
 
### 13.Представление отображения книги.
 
*   В контроллере [BookController](https://github.com/vladmeh/javaRushTestTask/blob/4e407d0ee6ad0b828e9cda2d3efa1eab22b24bf0/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) пишем метод `viewBook(Long id)`
*   Создаем шаблон представления отображения книги [books/view.html](https://github.com/vladmeh/javaRushTestTask/blob/4e407d0ee6ad0b828e9cda2d3efa1eab22b24bf0/src/main/resources/templates/books/view.html)
*   В представлении списка книг [books/list.html](https://github.com/vladmeh/javaRushTestTask/blob/4e407d0ee6ad0b828e9cda2d3efa1eab22b24bf0/src/main/resources/templates/books/list.html) устанавливаем ссылки на отображение книг `<a th:href="@{/books/{id}(id = ${book.id})}">`
    * теперь по клику на нашу карточку книги в списке, будет открываться книга.
*   В представлении [books/view.html](https://github.com/vladmeh/javaRushTestTask/blob/4e407d0ee6ad0b828e9cda2d3efa1eab22b24bf0/src/main/resources/templates/books/view.html) создаем кнопки и ссылки для действий над нашей книгой
    * прочтение книги
    * новое издание книги
    * удаление
    * редактирование - полное редактирование нам подабиться для того что-бы исправлять ошибки.
    
[Итог](https://github.com/vladmeh/javaRushTestTask/tree/4e407d0ee6ad0b828e9cda2d3efa1eab22b24bf0)
    
### 14.Реализуем действия с книгой
#### Удаление
*   В контроллере [BookController](https://github.com/vladmeh/javaRushTestTask/blob/c529f0d44141f45b5d0b99aa44d2bda612f9c582/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) пишем метод `deleteBook(Long id)`
    *   метод, по переданному параматру id, через сервис `bookService` находит книгу, удаляет ее и делает редирект на страницу со списком книг.
*   Действие на удаление мы реализуем через модальное окно, которое будет требовать подверждение на удаление и через метод `POST` передавать параметр в наш контроллер.
    *   для этого создаем шаблоны модальных окон [fragments/modals.html](https://github.com/vladmeh/javaRushTestTask/blob/c529f0d44141f45b5d0b99aa44d2bda612f9c582/src/main/resources/templates/fragments/modals.html), где пишем "фрагмент" модального окна на удаление книги.
    *   подключаем "фрагмент" к основному шаблону представления отображения книги [books/view.html](https://github.com/vladmeh/javaRushTestTask/blob/c529f0d44141f45b5d0b99aa44d2bda612f9c582/src/main/resources/templates/books/view.html)
    
#### Отметить книгу как прочитанную
*   В контроллере [BookController](https://github.com/vladmeh/javaRushTestTask/blob/c529f0d44141f45b5d0b99aa44d2bda612f9c582/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) пишем метод `isReadyBook(Long id)`
    *   метод, по переданному параматру id, через сервис `bookService` находит книгу, устанавливает поле `read_alreary` в `true`, сохраняет сделанные изменения и делает редирект на страницу книги.
*   Действие в представлении, как и удаление мы реализуем через модальное окно, которое будет требовать подверждение на прочтение и через метод `POST` передавать параметр в наш контроллер.
    *   для этого в шаблоне модальных окон [fragments/modals.html](https://github.com/vladmeh/javaRushTestTask/blob/c529f0d44141f45b5d0b99aa44d2bda612f9c582/src/main/resources/templates/fragments/modals.html), пишем "фрагмент" модального окна на прочтение книги.
    *   подключаем "фрагмент" к основному шаблону представления отображения книги [books/view.html](https://github.com/vladmeh/javaRushTestTask/blob/c529f0d44141f45b5d0b99aa44d2bda612f9c582/src/main/resources/templates/books/view.html)
    
[Итог](https://github.com/vladmeh/javaRushTestTask/tree/c529f0d44141f45b5d0b99aa44d2bda612f9c582)
    
#### Тесты
Совсем забыли про тесты, у нас еще не протестированы методы `viewBook(Long id), deleteBook(Long id), isReadyBook(Long id)` контроллера [BookController](https://github.com/vladmeh/javaRushTestTask/blob/b82e66434594033b1294eca35194b7d5eae7cc1d/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)

*   Пишем интеграционные тесты [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/b82e66434594033b1294eca35194b7d5eae7cc1d/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
    *   Тестируем отображение книги. Тест должен
        * возвращать ответ сервера со статусом `HTTP 200 OK`
        * найти в контенте строку с заголовком книги
    *  Тестируем удаление книги. Тест должен
        * возвращать ответ сервера со статусом `HTTP 302 OK`
        * определить что мы делаем редирект на страницу со списком
    *  Тестируем прочтение книги. Тест должен
        * возвращать ответ сервера со статусом `HTTP 302 OK`
        * определить что мы делаем редирект на страницу книги, которую редактировали

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/b82e66434594033b1294eca35194b7d5eae7cc1d)

#### Замена книги на новое издание
*    В контроллере [BookController](https://github.com/vladmeh/javaRushTestTask/blob/9631982dfc581b2e715be9b25818f64e6b49596a/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) пишем методы
    * `newEditionBook` - метод отвечает за вывод формы редактирования книги
    * `editionSubmit` - метод отвечает за обработку данных полученных из формы рекдактирования и сохранения в базе данных.

*   В методе `public Book update()` сервиса [Service.BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/9631982dfc581b2e715be9b25818f64e6b49596a/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java) дописываем реализацию обновления поля `imageStr` - `if (book.getImageStr() != null) entity.setImageStr(book.getImageStr());`

*   Пишем шаблон представления [books/edition.html](https://github.com/vladmeh/javaRushTestTask/blob/9631982dfc581b2e715be9b25818f64e6b49596a/src/main/resources/templates/books/edition.html), который отображает форму редактирования экземпляра книги.
    * в представлении пишем действие, которое предварительно подгружает новое изображение (обложку) книги, но пока не загружает его на сервер. Загрузку и сохранения изображения мы реализуем позже.
*   Пишем интеграционные тесты [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/9631982dfc581b2e715be9b25818f64e6b49596a/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
    * Тестируем отображение страницы редактирования книги
    * Тестируем ответ сервера на обработку данных полученных с формы и редирект на страниццу отображения книги
    
[Итог](https://github.com/vladmeh/javaRushTestTask/tree/9631982dfc581b2e715be9b25818f64e6b49596a)

#### Создание новой книги
*   В контроллере [BookController](https://github.com/vladmeh/javaRushTestTask/blob/644ac928291075675fe32a93cda368cfc06676d0/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) пишем методы
        * `addBook` - метод отвечает за вывод формы добавления книги
        * `addSubmit` - метод отвечает за обработку данных полученных из формы добавления и сохранения в базе данных.
*   Пишем шаблон представления [books/newBook.html](https://github.com/vladmeh/javaRushTestTask/blob/644ac928291075675fe32a93cda368cfc06676d0/src/main/resources/templates/books/newBook.html), который отображает форму добавления новой книги.
*   Пишем интеграционные тесты [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/644ac928291075675fe32a93cda368cfc06676d0/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
    * Тестируем отображение страницы добавления книги
    * Тестируем ответ сервера на обработку данных полученных с формы и редирект на страниццу списка книг

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/644ac928291075675fe32a93cda368cfc06676d0)

### 15. Загрузка изображений через форму и отображение их в представлении.

Сейчас наши изображения храняться в статической папке приложения `src/main/resource/static`. Загружать в эту папку новые изображения не совсем хорошая идея, т.к. после загрузки изображения и обновления в базе данных записи о загруженном файле, чтобы увидеть изображение в представлении нам придеться каждый раз перезапускать приложение.

Есть решение вынести папку с изображениями в файловую систему (за пределы нашего приложения), которое подробно рассмотрено [здесь](https://spring.io/guides/gs/uploading-files/). Я же воспользуюсь некоторыми идеями из этого руководства и буду сохранять изображения в базе данных.

#### Хранение
*   В таблицу `book` добавляем новое поле
    ```bash
    mysql> ALTER TABLE book ADD image_data mediumblob NULL;
    ```
    
    >* BLOB for 65535 bytes (64 KB) maximum,
    >* MEDIUMBLOB for 16777215 bytes (16 MB),
    >* LONGBLOB for 4294967295 bytes (4 GB).
     
    Изображения явно будут больше чем 64kb, поле типа `MEDIUMBLOB`.  Для ограничения размера загружаемого файла в `application.properties` можно выставить значения для свойств `spring.http.multipart.max-file-size=256KB` и `spring.http.multipart.max-request-size=256KB`

*   В сущность [Entity.Book](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/java/com/vladmeh/javaRushTestTask/Entity/Book.java) добавляем приватное поле типа байтового массива `private byte[] imageData`, добавляем аннотации, создаем для него геттер и сеттер.
  
#### Загрузка
*   В шаблонах [books/edition.html](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/resources/templates/books/edition.html) и [books/newBook.html](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/resources/templates/books/newBook.html) к форме редактирования (добавления) книги добаляем аттрибут `enctype="multipart/form-data"`, поле загрузки файла: `<input type="file" name="file" hidden="hidden" multiple="" accept="image/*" id="imageInput"/>`.

*   В методах `editionSubmit()` и `addSubmit()` контроллера [BookController](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) добавляем параметр `@RequestParam MultipartFile file` и реализуем сохранение полученного файла в базе данных.

```ava_holder_method_tree
@PostMapping(path = "/edition/{id}")
public String editionSubmit(
        @ModelAttribute Book book,
        @PathVariable Long id,
        @RequestParam MultipartFile file,
        RedirectAttributes redirectAttributes
) throws IOException {
    if (!file.isEmpty()){
        String fileName = file.getOriginalFilename();
        book.setImageData(file.getBytes());
        book.setImageStr(fileName);
    }

    bookService.update(book, id);
    redirectAttributes.addAttribute("id", id);
    return "redirect:/books/{id}";
}
```

*   В методе `public Book update()` сервиса [Service.BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java) дописываем реализацию обновления поля `imageData` - `if (book.getImageData() != null) entity.setImageData(book.getImageData());`

#### Отображение
*   В шаблонах [books/edition.html](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/resources/templates/books/edition.html), [books/view.html](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/resources/templates/books/view.html), [books/list.html](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/resources/templates/books/list.html) везде где идет отображение обложки через `${book.getImageStr()}` меняем аттрибут `src` тега `<img>` на `th:src="@{/books/{id}/image(id = ${book.getId()})}"`

```html
<img th:src="@{/books/{id}/image(id = ${book.getId()})}" th:alt="${book.getTitle()}" class="img-thumbnail"/>
```

*   Для того что-бы ссылка `/books/id_книги/image` работала, в контроллер [BookController](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) добавляем метод `getImageData()`

```java_holder_method_tree
@GetMapping(path = "/{id}/image")
@ResponseBody
public ResponseEntity<byte[]> getImageData(@PathVariable Long id){

    byte[] imageData = bookService.findById(id).getImageData();

    return ResponseEntity.ok()
            .header(HttpHeaders.CONTENT_TYPE, MediaType.IMAGE_JPEG_VALUE)
            .header(HttpHeaders.CONTENT_TYPE, MediaType.IMAGE_PNG_VALUE)
            .header(HttpHeaders.CACHE_CONTROL, CacheControl.noCache().getHeaderValue())
            .body(imageData);
}
```

Подробнее об этом и других решенииях читаем [здесь](http://www.baeldung.com/spring-mvc-image-media-data)

#### Тесты
В контроллере мы изменили методы отвечающие за обработку данных переданных с форм.
*   В [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/1cf32425ddda89bc533cf754f3a0189af684643c/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java) изменим тесты `editionSubmit_...` и `addSubmit_`
    * добавим в них тестирование загрузки файла

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/1cf32425ddda89bc533cf754f3a0189af684643c)

### 16. Наводим марафет - Refactoring

*   Оформляем домашнюю страницу - [index.html](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/resources/templates/index.html)
*   Рефакторим основной шаблон - [index.html](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/resources/templates/default.html)
    *   допиливаем стили
    *   организуем footer
    *   исправляем навигационную панель
*   Доводим до ума нашу страницу со списоком [templates/books/newBook.html](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/resources/templates/books/list.html)
*   Пишем еще один фрагмент [templates/fragments/forms.html](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/resources/templates/fragments/forms.html) и выносим туда наши формы со страниц [templates/books/edition.html](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/resources/templates/books/edition.html) и [templates/books/newBook.html](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/resources/templates/books/newBook.html)
*   В методах `addSubmit` и `editionSubmit` контроллера [BookController](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java), код отвечающий за загрузку файлов:
    ```java
    if (!file.isEmpty()){
        String fileName = file.getOriginalFilename();

        book.setImageData(file.getBytes());
        book.setImageStr(fileName);
    }
    ```
    выносим в наш сервис [Service.BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/524ec9c788a84a7f01249226c0319af059ac5779/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java) в метод `public Book uploadFileData`, который после успешной загрузки данных файла возвращает нам экземпляр `Book`.

* Проверяем тесты
    * исправляем текстовый вывод в тестах, которые проверяют строки на выходе.

>Наводить марафет можно бесконечно, поэтому нужно во-время остановиться. На этом ВСЕ, тестовое прижение ГОТОВО!!!!

[ОКОНЧАТЕЛЬНЫЙ ИТОГ](https://github.com/vladmeh/javaRushTestTask/tree/07eef38859fa440e1ae22d9a805102d73412dcb8)

### 16. Сборка и запуск проекта без IDEA
#### Сборка

Если устанавливать Spring boot через [SPRING INITIALIZR](https://start.spring.io/), что мы и делали, то
```bash
$> ./mvnw clean package
```

или обычным способом через Maven
```bash
$> mvn package
```

#### Запуск
```bash
$> java -jar target/javaRushTestTask-0.0.1-SNAPSHOT.jar
```

В браузере набираем `http://localhost:8080`. Восхищаемся шедевром!!!

### В процессе разработки пользовался материалом из источников:

*   Spring 4 для профессионалов (4-е издание)
*   https://docs.spring.io/
*   https://www.jetbrains.com/
*   https://www.petrikainulainen.net/spring-data-jpa-tutorial
*   http://www.natpryce.com/articles/000714.html
*   http://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html
*   https://getbootstrap.com/docs/4.0/getting-started/introduction
*   http://www.baeldung.com/
