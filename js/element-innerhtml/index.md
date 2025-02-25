---
title: "`.innerHTML`"
description: "Читаем и изменяем содержание HTML-элемента."
authors:
  - Windrushfarer
related:
  - js/web-security
  - js/dom
  - js/element
tags:
  - doka
---

## Кратко

Свойство `innerHTML` позволяет считать содержимое элемента в виде HTML-строки или установить новый HTML.

Новое значение HTML необходимо передавать в виде строки и оно заменит текущее содержимое элемента. При передаче невалидной HTML-строки будет выброшена [ошибка](/js/errors/). HTML-строкой является строка, которая содержит валидную HTML-разметку, в `innerHTML` нельзя передать DOM-элемент.

## Пример
```html
<form>
  <label>Логин</label>
  <input type="text" id="login" />
  <div class="error">Введите логин</div>
</form>
```

```js
const form = document.querySelector('form')

console.log(form.innerHTML)
// '<label>Логин</label><input type="text" id="login" /><div class="error">Введите логин</div>'

// Меняем содержимое новым html
form.innerHTML = '<div class="success">Вход выполнен</div>'
```

HTML после изменения:
```html
<form>
  <div class="success">Вход выполнен</div>
</form>
```

<iframe title="Element.innerHTML — Element.innerHTML — Дока" src="demos/index/" height="650"></iframe>

## Как понять

Браузер предоставляет разработчику возможность управлять содержимым на странице и менять его как угодно. `innerHTML` – самый простой способ считать или изменить HTML-содержимое элемента. Это свойство использует строки, что даёт возможность легко менять и очищать содержимое элементов.

Когда в `innerHTML` присваивается новое значение, все предыдущее содержимое удаляется и создаётся новое, что приводит к [перерисовке страницы](/js/how-the-browser-creates-pages/).

## Как пишется

Обращение к свойству `innerHTML` вернёт содержимое элемента в виде HTML-строки. Просмотреть или изменить содержимое можно у всех элементов, включая [`<html>`](/html/html/) и [`<body>`](/html/body/). Присвоение нового значения к свойству очистит всё текущее содержимое и заменит его новым HTML.

```js
document.body.innerHTML = '<h1>Hello Inner HTML!<h1>'
```

В результате в документ будет вставлен HTML:

```html
<h1>Hello Inner HTML!<h1>
```

<aside>

☝️ Стоит помнить, что строка с HTML-разметкой это не DOM-элемент. `innerHTML` работает только со строками, самостоятельно разбирает её содержимое и создаёт элементы.

</aside>

```js
const divEl = document.createElement('div') // <-- Здесь создается полноценный DOM-элемент

document.body.innerHTML = divEl
```

Так как в `divEl` находится объект DOM-элемента, то при присвоении в `innerHTML` он приведётся к строке, в результате в элемент вставится строка `"[object HTMLDivElement]"`.

```html
<body>[object HTMLDivElement]<body>
```

Если передать в `innerHTML` строку с невалидным HTML, то будет выброшена ошибка. Однако большинство современных браузеров помогают разработчику, умеют самостоятельно дополнять разметку (например если забыли закрыть тег) и даже дают возможность для кастомных тегов. Потому встретить ошибку при передаче в `innerHTML` невалидного HTML очень сложно.

<aside>

☝️ Несмотря на то, что с помощью `innerHTML` вставить любой HTML, есть некоторые ограничения, связанные с [безопасностью веб-приложений](/js/web-security/).

Если передать в `innerHTML` строку с тегом [`<script>`](/html/script/), то элемент успешно вставится в страницу, но скрипт, который содержится внутри не исполнится. Но исполнить вредоносный код возможно и без тега `<script>`. Потому **не рекомендуется** вставлять с помощью `innerHTML` любой HTML из ненадёжных источников. Например, если вы получаете разметку с неизвестного сервера.

Так же не рекомендуется использовать `innerHTML`, если нужно просто изменить текст в элементе. Для этой задачи есть свойство [`innerText`](/js/element-innertext/) или [`textContent`](/js/element-textcontent/).

</aside>

```js
// Скрипт станет частью body, но не выполнится
document.body.innerHTML = '<script>alert("Мы не смогли вас взломать :(")</script>'

// После вставки в html картинка не загрузится и тогда сработает код из onerror
document.body.innerHTML = '<img src="broken-link" onerror="alert("Теперь вы точно взломаны!")">'
```
