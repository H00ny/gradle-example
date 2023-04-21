# gradle-example

Пример сборки Gradle Project

## Содержание:

1. [Установка Java/Gradle](#установка-java--gradle)
2. [Создаем Gradle Project](#создаем-gradle-project)
3. [Lyfecycle Gradle](#lifecycle-gradle)
4. [А как же превратиться в Spring Boot?](#а-как-же-превратиться-в-spring-boot)

## [Установка Java / Gradle](#содержание)

В данном примере используется Java 11 - amazon version, и Gradle:latest

Ссылки для быстрой загрузки используемых ресурсов

| Софт    |                                     Ссылка                                     | Версия |
| ------- | :----------------------------------------------------------------------------: | :----: |
| Java 11 | https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html | amazon |
| Gradle  |                          https://gradle.org/releases/                          | latest |

Далее добавляем установленные ссылки на распакованные архивы в System или User PATH переменную

Для проверки работоспособности, используем команды:

```ps
java --version
gradle --version
```

> В случае успеха, увидите сообщения об установленных версиях

## [Создаем Gradle Project](#содержание)

Для того чтобы создать проект автоматически, можем воспользоваться командой из Gradle-Документации:

```ps
gradle init
```

Выбираем тип приложения:

```
Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2
```

Выбираем язык программирования

```
Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Scala
  6: Swift
Enter selection (default: Java) [1..6] 3
```

Выбираем язык для конфигурации

```
Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1
```

Выбираем фрейморк для тестирования

```
Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit 4) [1..4] 4
```

Выбираем артефакты приложения

```
Project name (default: gradle-example): gradle-example
Source package (default: gradle.example): ru.cmx.gradle.example
```

Далее, для удобства запускаем терминал и перемещаемся в директорию проекта. Пример:

```bat
cd app
```

И если все прошло успешно при генерации, то у вас в проекте появится следующая структура:

```
- pom.xml
- src:
  - main.java.ru.cmx.mave.example:
    - App.java
  - test.java.ru.cmx.mave.example:
    - AppTest.java
```

Проверяется командой:

```cmd
tree /f
```

## [Lifecycle Gradle](#содержание)

Для очистки всего свяазанного с старым билдом используем clean:

```ps
gradle clean
```

Для компилирования кода используем compileJava:

```ps
gradle compileJava
```

Для компилирования кода тестов используем compileTest:

```ps
gradle compileTest
```

Для запуска тестов используем test:

```ps
mvn test
```

> Результаты тестов можно посмотреть в build/reports

Для сборки в jar приложение со всеми предыдущими фазами используем build:

```ps
gradle build
```

Для того, чтобы использовать ваш проект, в другом вашем локальном проекте (например библиотеку) используем publishToMavenLocal, но перед этим необходимо добавить плагин в build.gradle:

```groovy
plugins {
    id 'maven-publish'
}
```

И добавить настройки для ключа артефакта:

```groovy
publishing {
    publications {
        maven(MavenPublication) {
            groupId = 'ru.cmx.gradle.example'
            artifactId = 'gradle-example'
            version = '0.1.0-SNAPSHOT'

            from components.java
        }
    }
}
```

После, использовать команду для публикации в локальный репозиторий .m2:

```ps
gradle publishToMavenLocal
```

После этого, в вашей локальной папке для зависимостей (.m2) появится ваш проект, в собранном виде, который можно использовать в других проектах

> добавить в зависимости и готово!

```groovy
dependencies {
    implementation 'ru.cmx.gradle.example:maven-example:0.1.0-SNAPSHOT'
}
```

Так же фазы можно запускать последовательно одной командой:

```ps
gradle clean compileJava compileTest build run
```

Ну и главное - а как запустить проект?

```ps
gradle run
```

Также можно посмотреть дерево зависимостей вашего проета, командой:

```ps
gradle dependencies
```

## [А как же превратиться в Spring Boot?](#содержание)

Для примера будем использовать версию Spring Boot 2.7.1, но работает аналогично на большинстве других

Добавляем плагин с парентом для Spring Boot в build.gradle:

```groovy
plugins {
    id 'application'
    id 'org.springframework.boot' version '2.7.1'
	id 'io.spring.dependency-management' version '1.1.0'
}
```

и spring-boot-starter-web в dependencies:

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

Добавляем аннотацию `@SpringBootApplication` в наш раннер класс, и запускаем цикл спринг-приложения `SpringApplication.run()`:

```java
package ru.cmx.maven.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class App
{
    public static void main( String[] args )
    {
        SpringApplication.run(App.class, args);
    }
}
```

Проверить можно, перейдя по адресу сервера http://localhost:8080
