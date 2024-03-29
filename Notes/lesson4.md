# Регулярные выражения

## Что такое регулярные выражения

Иногда надо проверить, соответствует ли строка какому-то формату. Чаще всего используется при валидации полей формы. Иногда нужно заменить символы в тексте

Создание РВ:

```javascript
const option1 = new RegExp('pattern');
const option1 = /pattern/;
```

Это 2 равнозначных варианта.

Для создания и проверки выражения используются сайты типа regex101.com
Основные флаги: g - global, m - multiline, i - case insensitive

Запись с флагами:

```javascript
const option1 = new RegExp('pattern', 'gmi');
const option1 = /pattern/gmi;
```

## Как узнать, есть ли совпадения

Чтобы узнать, есть ли совпадение, используем метод `test`:

```javascript
const rx = /abc/gmi;
const str = '123 abc 456';
rx.test(str); //true
```

## Как получить совпадения

Чтобы найти сами совпадения, используем match:

```javascript
str.match(rx); // ['abc']
```

## Как заменить часть строки на другую

Чтобы заменить, используем метод replace:

```javascript
str.replace(rx, 'def'); // '123 def 456'
```

## Что такое "жадный" поиск

Жадный поиск обозначает, что будет искаться наибольшая группа. Чтобы сделать поиск не жадным, а "ленивым", мы ставим "?" перед "+" или "*" в выражении - в таком случае будет искаться минимальное удовлетворяющее условию выражение.

## Полезные регулярные выражения
