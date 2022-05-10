# networking-theory

Работая с внешними данными будто это обновление данных о погоде, стриминг музыки или просто серфинг в браузере ваше приложение использует сетевые запросы HTTP. Именно они отвечают за всё что связано с внешним миром. Для работы с сетевыми запросами Apple предлагает современный и просто в использовании API - URLSession, который представляет из себя объект координирующий группу связанных сетевых задач для передачи данных. URLSession является ключевым компонентом всего стека отвечающим за отправку и прием HTTP запросов. Технически URLSession это и класс и набор классов, которые содержат в себе все методы загрузки и выгрузки контента по протоколу HTTP. 

<img width="1142" alt="image" src="https://user-images.githubusercontent.com/55939234/167616856-e5db1717-d5b4-418a-a672-9c99fd64f182.png">


С помощью API URLSession приложение может создавать множество различных сессий каждая из которых координирует определенные задачи для передачи данных, каждая сессия содержит экземпляр класса URLSessionConfiguration, который позволяет сконфигурировать параметры сессии. 

<img width="1157" alt="image" src="https://user-images.githubusercontent.com/55939234/167617238-e9e6b896-986e-43b0-985f-82a33e9dde7a.png">

Для быстрого создания простого запроса без использования данных используйтесь Singleton - (URLSession.shared.configuration)

Для создания специфических параметров конфигурации доступно 3 метода: 
1. Метод Default - который создает объект дефолтных настроек использующих сохраняемый на диске глобальный кэш учетной записи и хранилище куки. Используется для создания простого запроса с использованием данных. (URLSession.Configuration.default)
2. Метод Ephemeral (Эфэмерал) - который похож на настройки по умолчанию за исключение того, что все данные связанные с сеансом сохраняются в оперативной памяти, это можно представить как частную сессию, используется если вам необходимо создавать сессию без сохранения данных на диске.
3. Метод Background - который позволяет сессии выполнить задание по загрузке и выгрузке данных в фоновом режиме. В этом случае передача данных продолжается даже тогда, когда приложение приостановлено или его работа прекращена. Используется если вам надо сохранить все данные на диске в фоновом режиме. Каждый запрос в себе содержит перечень сетевых задач, которые представлены классом URLSession Task. URLSessionTask это один из базовых классов стека URLSession, который имеет несколько сабклассов: URLSessionDataTask, URLSessionUploadTask и URLSessionDownloadTask.

<img width="1635" alt="image" src="https://user-images.githubusercontent.com/55939234/167622288-f10de5c3-bd96-4edd-95e4-e4f6bf5ff0e4.png">

Экземпляр каждого из перечисленного класса определяет тип задачи, которая всегда является частью сессии и создается путем вызова одного из методов экземпляра класса URLSession. 

Метод dataTask(with:) - позволяет создать экземпляр класса URLSessionDataTask для получения данных.
Метод uploadTask(with:from:) - позволяет создать экземпляр класса URLSessionUploadTask для выгрузки данных (файлов) с устройства на удаленный сервер работает по принципу методов HTTP и может делать выгрузку в фоновом режиме.
Метод dowloadTask(with:) - позволяет создать экземпляр класса URLSessionDownloadTask для загрузки данных (файлов) с удаленного сервера которые сохраняются как временные файлы в соответствующей директории. 

URLSessionTask также имеет такие методы, как:
- cancel
- resume
- suspend
Которые позволяют приостанавливать, возобновлять и отменять задания. При этом класс URLSessionDownloadTask имеет дополнительную возможность при остановке с последующим возобновлением работы.
