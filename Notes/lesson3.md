# Асинхронные запросы. Основы асинхронного JavaScript. AJAX, JSON, Promises

## Основные виды запросов

* `GET` - запрос на предоставление ресурса. Может только извлекать данные;
* `POST` - запрос для отправки данных на сервер. Часто приводит к изменению со`стояния` сервера;
* `PUT` - для полного обновления некоей сущности;
* `PATCH` - для частичного обновления некоей сущности;
* `DELETE`  - для удаления сущности;
* `HEAD` - запрашивает ресурс так же как GET, но без тела ответа;
* `CONNECT` - устанавливает "туннель" к серверу.
* `OPTIONS` - для определения параметров соединения с удаленным ресурсом. Он всегда уходит перед выполнением другого типа запроса, чтобы реализовать защитный механизм CORS. Эта политика не позволяет выполнять кросс-доменные запросы, и это решается либо отправкой бекэндом специальных политик, разрешающих кросс-доменные запросы, либо через проксирование запросов.
* `TRACE` - выполняет вызов возвращаемого текстового сообщения с ресурса.

## AJAX - асинхронный JavaScript

AJAX - набор инструментов для работы  сервером без перезагрузки страницы. Расшифровывается как Asynchronous JavaScript and XML. При использовании AJAX, запрос к серверу не блокирует страницу и не перезагружает ее.

При AJAX запросе общение с сервером может идти в одном из следующих форматов:

* JSON - самый популярный формат. Основан на записи обычного объекта JavaScript
* XML - язык разметки, похожий на HTML
* HTML/text - обычная HTML разметка или текст
* Binary - набор байтов. Обычно используется для передачи данных

## Формат JSON

Клиент с сервером общаются в тексте в формате JSON.
Чтобы привести объект к JSON, мы исползуем `JSON.stringify(object)`;
Чтобы распарсить объект, исползуем `JSON.parse(string)`, который вернет нам JS объект.

## Объект xhr

Для отправки запросов в браузер встроен объект `XMLHttpRequest`. В IE он называется иначе - `ActiveXObject("Microsoft.XMLHTTP")`, поэтому для инициализации используют такую конструкцию:

```javascript
var xhr;

if (window.XMLHttpRequest) {
    // Chrome, Moziila, Opera, Safari
    xhr = new XMLHttpRequest();
} else if (window.ActiveXObject) {
    // Internet Explorer
    xhr = new ActiveXObject("Microsoft.XMLHTTP");
}
```

Чтобы поймать момент, когда ответ сервера получен, можно воспользоваться свойством `onreadystatechange`:

```javascript
xhr.onreadystatechange = function() {
    if (xhr.readystate === 4) {
        //Этот код выполнится после выполнения запроса
        if (xhr.state === 200) {
            //Этот код выполнится, если запрос выполнен успешно
        }
    }
}
```

Возможные статусы:

* 0 - запрос не инициирован;
* 1 - загрузка;
* 2 - запрос принят
* 3 - обмен данными
* 4 - запрос выполнен

Чтобы инциализировать отправку запроса, используется метод `open()`:

```javascript
xhr.open('GET', 'http://example.com', true)
/*
Первый параметр - тип запроса
Второй параметр - адрес ресурса
Третий параметр - указатель асинхронности (true - значение по умолчанию)
*/
```

Если указатель асинхронности стоит true - выполнение запроса не будет блокировать выполнение других скриптов на странице

У каждого запроса есть определенный timeout - время, в течение которого мы ждем ответа:

```javascript
xhr.timeout = 15000;
```

Если запрос не выполнится за определенное время, срабатывает функция, переданная в поле **ontimeout**:

```javascript
xhr.ontimeount = function() {
    //Этот код выполнится, если превышено время ожидания запроса
}
```

Отправить запрос можно методом **send()**:

```javascript
xhr.send();
```

Также при отправке запроса можно выставить заголовки. Заголовок содержит служебную информацию, чтобы серверу было проще обработать запрос. Делается это с помощью метода **setRequestHeader**. Например, при отпраке POST-запроса нужно выставить тип данных:

```javascript
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
```

Ответ от сервера можно получить в виде строки текста или XML:

```javascript
xhr.responseText;
xhr.responseXML;
```

## Асинхронные функции

Один вариант решить проблему асинхронных функций - через функции обратного вызова

```javascript
const asyncFunc = (a, callback) => {
setTimeout( () => {
    const b = a + 1;
    callback(b);
}, 200)
};

asyncFunc(5, (val) => console.log(val))
```

Проблема функций обратного вызова в том, что при необходимости сделать некую последовательность действий может получиться очень много вложенных
коллбеков.

## Promise

Чтобы избежать проблем с вложенностью коллбеков в ES6 сделали объект "Promise"

У Promise 3 состояния:

* Pending
* Resolved
* Rejected

Синтаксис:

```javascript
const promise = new Promise( (resolve, reject) => {
    if (true) {
        resolve(1);
    } else {
        reject(2);
    }
})

promise.then( (a) => {
    a // 1
}).catch( (err) => {
    err // 2
})
```

`resolve` - callback, когда успешно
`reject` - callback, если ошибка

При этом можно объединять промисы в цепочки.

Асинхронные запросы async/await.

для этого перед асинхронными методами мы добавляем async:

```javascript
async methodName() { //method body };
```