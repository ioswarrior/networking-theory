# networking-theory

Работая с внешними данными будто это обновление данных о погоде, стриминг музыки или просто серфинг в браузере ваше приложение использует сетевые запросы HTTP. Именно они отвечают за всё что связано с внешним миром. Для работы с сетевыми запросами Apple предлагает современный и просто в использовании API - **URLSession**, который представляет из себя объект координирующий группу связанных сетевых задач для передачи данных. **URLSession** является ключевым компонентом всего стека отвечающим за отправку и прием HTTP запросов. Технически **URLSession** это и класс и набор классов, которые содержат в себе все методы загрузки и выгрузки контента по протоколу HTTP. 

<img width="600" alt="image" src="https://user-images.githubusercontent.com/55939234/167616856-e5db1717-d5b4-418a-a672-9c99fd64f182.png">


С помощью API **URLSession** приложение может создавать множество различных сессий каждая из которых координирует определенные задачи для передачи данных, каждая сессия содержит экземпляр класса **URLSessionConfiguration**, который позволяет сконфигурировать параметры сессии. 

<img width="600" alt="image" src="https://user-images.githubusercontent.com/55939234/167617238-e9e6b896-986e-43b0-985f-82a33e9dde7a.png">

Для быстрого создания простого запроса без использования данных используйтесь **Singleton - (URLSession.shared.configuration)**

Для создания специфических параметров конфигурации доступно **3 метода**: 
1. **Метод Default** - который создает объект дефолтных настроек использующих сохраняемый на диске глобальный кэш учетной записи и хранилище куки. Используется для создания простого запроса с использованием данных. **(URLSession.Configuration.default)** <br>
2. **Метод Ephemeral** (Эфэмерал) - который похож на настройки по умолчанию за исключение того, что все данные связанные с сеансом сохраняются в оперативной памяти, это можно представить как частную сессию, используется если вам необходимо создавать сессию без сохранения данных на диске. **(URLSession.Configuration.ephemeral)**<br>
3. **Метод Background** - который позволяет сессии выполнить задание по загрузке и выгрузке данных в фоновом режиме. В этом случае передача данных продолжается даже тогда, когда приложение приостановлено или его работа прекращена. Используется если вам надо сохранить все данные на диске в фоновом режиме. **(URLSession.Configuration.background)**

Каждый запрос в себе содержит перечень сетевых задач, которые представлены классом **URLSessionTask**. **URLSessionTask** это один из базовых классов стека **URLSession**, который имеет несколько сабклассов: **URLSessionDataTask**, **URLSessionUploadTask** и **URLSessionDownloadTask**.

<img width="600" alt="image" src="https://user-images.githubusercontent.com/55939234/167622288-f10de5c3-bd96-4edd-95e4-e4f6bf5ff0e4.png">

Экземпляр каждого из перечисленного класса определяет тип задачи, которая всегда является частью сессии и создается путем вызова одного из методов экземпляра класса **URLSession**. 

Метод **dataTask(with:)** - позволяет создать экземпляр класса **URLSessionDataTask** для получения данных. <br>
Метод **uploadTask(with:from:)** - позволяет создать экземпляр класса **URLSessionUploadTask** для выгрузки данных (файлов) с устройства на удаленный сервер работает по принципу методов HTTP и может делать выгрузку в фоновом режиме. <br>
Метод **dowloadTask(with:)** - позволяет создать экземпляр класса **URLSessionDownloadTask** для загрузки данных (файлов) с удаленного сервера которые сохраняются как временные файлы в соответствующей директории. 

**URLSessionTask** также имеет такие методы, как:
- cancel (отменять)
- resume (возобновлять)
- suspend (приостанавливать)

Которые позволяют отменять, возобновлять и приостанавливать задания. При этом класс **URLSessionDownloadTask** имеет дополнительную возможность при остановке с последующим возобновлением работы.

---------------------------------------------------------------------------------------------------------------------------
Методы протокола передеачи данных HTTP (Hypertext Transfer Protocol):

GET (получить) - для получения данных, именно его использует браузер для загрузки веб страниц, он используется для запроса содержимого расположенного по определенному URL адресу. Содержимое может быть как веб-страницей, так и каким-то медиа файлом. Существует также стандарт W3C (World Wide Web Consortium) - Консорциум всемирной паутины. Это организация которая стандартизирует новые технологии внедряемые в интернет среду. В соответствии с этим стандартом GET запросы осуществляют только чтение данных и не должны быть использованы в операциях изменяющих серверную сторону. Для этих целей используется метод POST (добавить, изменить, удалить). POST посылает данные по URL адресу для дальнейшей их обработки. Он также содержит в себе параметры, которые включены в тело запроса использующего тот же формат что и GET. Например если мы хотим запостить форму содержащей 2 поля (имя и возраст), то в теле запроса нужно прописать что-то похожее на name=Lex&age=37. Когда мы заполняем форму на сайте и нажимаем submit то есть подтверждение, то вероятнее всего этот запрос будет содержать метод POST. Кроме этих двух методов протокол HTTP также содержит в себе еще несколько методов. Метод HEAD - идентичен GET, но работает только с заголовками исключая сами данные. Метод PUT (добавить, заменить) позволяет добавить или заменить данные на сервере. Метод DELETE (удалить) позволяет удалить данные с сервера. Как видите POST объединяет в себе методы: PUT и DELETE, таким образом получение, добавление, изменение и удаление данных могут осуществляться двумя методами GET и POST.

Для отправки запроса на сервер используется URL. URL адрес начинается с протокола, который определяет по какому стандарту делается запрос, в нашем случае это "https", который является безопасно-ориентированным протоколом http. Также могут быть http, ftp, telnet, ssh и прочее. "www.google.com" это имя домена. После имени домена идет слеш (/), который определяет директорию где находится необходимые нам ресурсы, после вопросительного знака идут параметры запроса в нашем случае "q=headsandhands". Они состоят из пар "ключ-значение". При запросе также указывается метод, который говорит как сервер должен обрабатывать этот запрос. Если не указывать метод явно, как на этом примере, то по умолчанию это будет метод GET. Наш URL адрес можно расшифровать так: 
