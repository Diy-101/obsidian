Builder for Java/Kotlin projects

## Basic Model with Gradle
![[Screenshot 2026-06-25 at 16.34.22.png]]

The settings file (`settings.gradle(.kts)`) is the **entry point** of every Gradle project.
The primary purpose of the _settings file_ is to define the project structure, usually adding subprojects to your build. Therefore in:

- **Single-project** builds, the settings file is **optional**.
- **Multi-project** builds, the settings file is **mandatory** and declares all subprojects.

## Объект project
Это как если бы у тебя было:
Неявно доступно (не нужно импортировать)
val project: Project = gradle.getProject(":load-tests-lotos-utilizations")

---

Что внутри project?
Объект Project (тип: org.gradle.api.Project) содержит всю информацию о проекте:

| Свойство             | Что возвращает        |
| -------------------- | --------------------- |
| project.name         | Имя проекта           |
| project.group        | Группа артефакта      |
| project.version      | Версия                |
| project.properties   | Все свойства (Map)    |
| project.rootDir      | Корневая папка        |
| project.buildDir     | Папка сборки          |
| project.subprojects  | Дочерние модули       |
| project.dependencies | Менеджер зависимостей |
| project.tasks        | Задачи сборки         |

## `build.gradle.kts` VS `settings.gradle.kts`
 
 Аналогия:

![[Screenshot 2026-06-25 at 19.39.39.png]]

## Отличие `project` and `settings` objects

![[Screenshot 2026-06-25 at 19.45.10.png]]

## Versions control `gradle/libs.versions.toml`

The version catalog typically contains four sections:

1. `[versions]` to declare the version numbers that plugins and libraries will reference.
2. `[libraries]` to define the libraries used in the build files.
3. `[bundles]` to define a set of dependencies.
4. `[plugins]` to define plugins.

```toml
gradle/libs.versions.toml

[versions]
guava = "32.1.2-jre"
juneau = "8.2.0"

[libraries]
guava = { group = "com.google.guava", name = "guava", version.ref = "guava" }
juneau-marshall = { group = "org.apache.juneau", name = "juneau-marshall", version.ref = "juneau" }
```

Place this file in the `gradle/` directory of your project as `libs.versions.toml`.


