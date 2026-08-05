
```java
Извлечение значения из ответа и сохранение в сессию`
      `2 http("request")`
      `3     .get("/api/balance")`
      `4     .check(jsonPath("$.accountId").saveAs("accountId"))`
      `5     .check(status().is(200));`
      `6` 
      `7 // 2. Использование сохранённого значения в следующем запросе`
      `8 http("next-request")`
      `9     .get("/api/accounts/${accountId}")  // интерполяция из сессии`
     `10     .check(status().is(200));`
     `11` 
     `12 // 3. Явная запись в сессию через exec`
     `13 .exec(session -> session.set("customVar", "value"))`
     `14` 
     `15 // 4. Чтение из сессии`
        .exec(session -> {
          String value = session.getString("customVar");`
          System.out.println("Value: " + value);`
          return session;  // обязательно вернуть сессию`
    }
    Simulation → Scenario → Chain → Action
                   ↓
	             Session (передаётся через всё)
 });
 
 1. Simulation (Simulation)

    Корневой объект нагрузочного теста. Определяет:
     - Какие сценарии выполняются
     - Какую нагрузку генерировать (инъекция пользователей)
     - Протоколы (HTTP, JDBC и т.д.)
     - Отчётность

     1 public class BalanceTest extends Simulation {
     2     // Здесь объявляешь сценарии и настройку нагрузки
     3 }

    ---

    2. Scenario (ScenarioBuilder)

    Последовательность действий, которую выполняет один виртуальный пользователь.

     1 ScenarioBuilder scenario = scenario("BalanceTest")
     2     .exec(http("get-balance").get("/api/balance"))
     3     .pause(2)
     4     .exec(http("get-accounts").get("/api/accounts"));

    Один VU = одна копия сценария = одна сессия.

    ---

    3. Chain (цепочка действий)

    Последовательность exec(), pause(), feed() и других шагов внутри сценария. Выполняется линейно.

    ---

    4. Session

    Контекст выполнения сценария. Неизменяемый контейнер с данными, который:
     - Создаётся при старте VU
     - Передаётся через каждый шаг цепочки
     - Уничтожается при завершении

     1 .exec(session -> {
     2     String token = session.getString("oauthToken");  // чтение
     3     return session.set("newVar", "value");           // запись (возврат новой сессии)
     4 })

    ---

    5. Action (Exec, Pause, Loop, During, Exit, ExitBlock)

    Отдельный шаг в цепочке:
     - exec() — выполнить запрос или функцию
     - pause() — задержка
     - loop() / during() — циклы
     - exit() — завершение сценария

    ---

    6. Feeder

    Механизм загрузки данных в сессию (CSV, JSON, JDBC, генераторы).

     1 Feeder<String> feeder = FeederBuilderSeparatedCsv.fromFilename("users.csv").circular();
     2 
     3 scenario("Test")
     4     .feed(feeder)  // добавляет ${username}, ${password} в сессию

    Стратегии:
     - circular() — зациклить данные
     - random() — случайные строки
     - queue() — последовательно, пока не кончатся
     - shuffle() — перемешать

    ---

    7. Check (чекер)

    Валидация ответа + извлечение данных в сессию.

     1 http("request")
     2     .get("/api/user")
     3     .check(status().is(200))              // валидация
     4     .check(jsonPath("$.id").saveAs("userId"));  // извлечение в сессию

    ---

    8. Injection Profile (профиль нагрузки)

    Определяет, как и когда запускаются виртуальные пользователи:

     1 setUp(scenario.injectOpen(
     2     constantUsersPerSec(10).during(60),   // 10 RPS, 60 сек
     3     rampUsersPerSec(5).to(20).during(120) // разгон с 5 до 20 RPS за 2 мин
     4 ));

    Типы:
     - constantUsersPerSec(n).during(t) — постоянная нагрузка
     - rampUsersPerSec(a).to(b).during(t) — плавный разгон
     - atOnceUsers(n) — все сразу
     - nothingFor(t) — пауза

    ---

    9. Protocol

    Настройки соединения (HTTP, HTTPS, JDBC).

     1 HttpProtocolBuilder httpProtocol = http
     2     .baseUrl("https://api.example.com")
     3     .acceptHeader("application/json")
     4     .userAgentHeader("Gatling");

    ---

    10. Virtual User (VU)

    Виртуальный пользователь — изолированный поток выполнения сценария со своей сессией.

    ---
```

## Уровни памяти

![[Память Gatling]]

```java
Паттерны:

      1 // Запись
      2 .exec(session -> session.set("myVar", "value"))
      3 
      4 // Чтение
      5 .exec(session -> {
      6     String val = session.getString("myVar");
      7     System.out.println("Value: " + val);
      8     return session;  // Обязательно вернуть!
      9 })
     10 
     11 // Удаление
     12 .exec(session -> session.remove("myVar"))
     13 
     14 // Массовая запись (как в propagateToSession)
     15 .exec(session -> session.setAll(globalMap))

```

OAUTH_CLIENT_ID: LotosLoadTestsClient
OAUTH_TOKEN_URL: [https://twork-integration-test.dev-tcsgroup.io/auth/connect/token](https://twork-integration-test.dev-tcsgroup.io/auth/connect/token)
OAUTH_CLIENT_SECRET: R5cR8RU_9hsgAbdVtWY9urFIM6bjFesX9jZDtgObDgg
OAUTH_SCOPE: cashback-management:read+cashback-management:write
   
JDBC_URL: jdbc:oracle:thin:@//db-loyco-tst.tcsbank.ru:1522/LOYCO_TST
JDBC_USER: I_A_ROMANOV
JDBC_PASSWORD: zuqjut-nermu0-genqIw

