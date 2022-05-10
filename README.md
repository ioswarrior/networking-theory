# networking-theory

Работая с внешними данными будто это обновление данных о погоде, стриминг музыки или просто серфинг в браузере ваше приложение использует сетевые запросы HTTP. Именно они отвечают за всё что связано с внешним миром. Для работы с сетевыми запросами Apple предлагает современный и просто в использовании API - URLSession, который представляет из себя объект координирующий группу связанных сетевых задач для передачи данных. URLSession является ключевым компонентом всего стека отвечающим за отправку и прием HTTP запросов. Технически URLSession это и класс и набор классов, которые содержат в себе все методы загрузки и выгрузки контента по протоколу HTTP. 

<img width="1142" alt="image" src="https://user-images.githubusercontent.com/55939234/167616856-e5db1717-d5b4-418a-a672-9c99fd64f182.png">


С помощью API URLSession приложение может создавать множество различных сессий каждая из которых координирует определенные задачи для передачи данных, каждая сессия содержит экземпляр класса URLSessionConfiguration, который позволяет сконфигурировать параметры сессии. 

<img width="1157" alt="image" src="https://user-images.githubusercontent.com/55939234/167617238-e9e6b896-986e-43b0-985f-82a33e9dde7a.png">

Для быстрого создания простого запроса без использования данных используйтесь Singleton - (URLSession.shared.configuration)

Для создания специфических параметров конфигурации доступно 3 метода: 
1. Метод Default - который создает объект дефолтных настроек использующих сохраняемый на диске глобальный кэш учетной записи и хранилище куки. Используется для создания простого запроса с использованием данных. (URLSession.Configuration.default)
2. Метод Ephemeral (Эфэмерал) - который похож на настройки по умолчанию за исключение того, что все данные связанные с сеансом сохраняются в оперативной памяти, это можно представить как частную сессию, используется если вам необходимо создавать сессию без сохранения данных на диске.
3. Метод Background
