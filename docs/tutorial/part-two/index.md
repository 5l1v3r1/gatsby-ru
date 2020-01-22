---
title: Вступление в стилизацию с Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

<!-- Idea: Create a glossary to refer to. A lot of these terms get jumbled -->

<!--
  - Global styles
  - Component css
  - CSS-in-JS
  - CSS Modules

-->

Добро пожаловать во вторую часть руководства Gatsby!

## Что будет в этом руководстве?

В этой части мы познакомимся с вариантами оформления веб-сайтов Gatsby и углубимся в использование компонентов React для создания сайтов.

## Использование глобальных стилей

Каждый сайт имеет глобальные стили в том или ином виде. Они включают в себя такие вещи, как типография и цвета сайта. Эти стили определяют общее ощущение сайта ― так же, как цвет и текстура стены задают общее ощущение комнаты.

### Создание глобальных стилей с помощью стандартных файлов CSS

Одним из наиболее простых способов добавления глобальных стилей на сайт является использование глобальной таблицы стилей `.css`.

#### ✋ Создадим новый сайт Gatsby

Начнем с создания нового Gatsby сайта. Возможно, будет лучше закрыть окна терминала (особенно для новичков в командной строке), которые мы использовали в [первой части](/tutorial/part-one/) и открыть новый терминал для второй части.

Откройте окно терминала, создайте новый сайт "hello world" и запустите сервер разработки:

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

Теперь у нас есть новый сайт Gatsby (основанный на Gatsby-стартере "hello world") со следующей структурой:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
```

#### ✋ Добавим стили в файл CSS

1. Создайте файл `.css` в нашем новом проекте:

```shell
cd src
mkdir styles
cd styles
touch global.css
```

> Примечание: При желании вы можете создавать эти каталоги и файлы самостоятельно или с помощью редактора.

Теперь у вас должна быть такая структура:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
```

2. Определим какие-то стили в файле `global.css`:

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}
```

> Примечание: Расположение файла css в папке `/src/styles/` является произвольным.

#### ✋ Включим таблицу стилей в `gatsby-browser.js`

1. Создайте `gatsby-browser.js`

```shell
cd ../..
touch gatsby-browser.js
```

Теперь файловая структура вашего проекта должна выглядеть так:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
├── gatsby-browser.js
```

> 💡 Что такое `gatsby-browser.js`? Пока не беспокойтесь об этом. Достаточно знать, что `gatsby-browser.js` ― это один из нескольких специальных файлов, которые Gatsby ищет и использует (если они существуют). В этом случае название файла **важно**. Если вы хотите узнать больше сейчас, ознакомьтесь с [документами](/docs/browser-apis/).

2. Импортируйте недавно созданную таблицу стилей в файл `gatsby-browser.js`:

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"

// or:
// require('./src/styles/global.css')
```

> Примечание: Здесь поддерживается синтаксис из CommonJS (`require`) и ES Module (`import`). Если вы не уверены, какой из них выбрать ― мы используем "import" большую часть времени.

3. Запустите сервер разработки:

```shell
gatsby develop
```

Теперь, если вы откроете проект в браузере, то обнаружите лавандовый фон, примененный к стартеру "hello world":

![Лавандовый Hello World!](global-css.png)

> Примечание: Эта часть руководства посвящена наиболее быстрому и простому способу начать стилизацию сайта Gatsby ― прямому импорту стандартных CSS файлов с использованием `gatsby-browser.js`. В большинстве случаев лучший способ добавить глобальные стили ― использовать общий шаблонный компонент. Узнайте больше в [документации](/docs/creating-global-styles/#how-to-add-global-styles-in-gatsby-with-standard-css-files).

## Использование компонентно-ориентированного CSS

До сих пор мы говорили о более традиционном подходе использования таблиц стилей CSS. Теперь мы рассмотрим модульные CSS подходы для компонентно-ориентированных подходов.

### CSS Модули

Давайте рассмотрим **CSS-модули**. Цитата с
[домашней страницы CSS модулей](https://github.com/css-modules/css-modules):

> **CSS модули** это CSS файлы в которых все названия классов и анимаций
> по умолчанию ограничены локальной областью видимости.

CSS модули очень популярны, потому что они позволяют писать обычный CSS, но с гораздо большей безопасностью. Инструмент автоматически генерирует уникальные имена для классов и анимаций, поэтому вам не нужно беспокоиться о конфликтах глобальных имен.

Gatsby из коробки поддерживает CSS модули. Этот подход настоятельно рекомендуется новичкам в работе с Gatsby (и React в целом).

#### ✋ Создадим новую страницу с помощью CSS-модулей

В этом разделе мы создадим новую страницу и стили для него с помощью CSS модуля.

Сначала создадим новый компонент `Container`.

1. Создайте новый каталог в `src/components`, а затем внутри создайте файл с именем `container.js` и вставьте следующее:

```javascript:title=src/components/container.js
import React from "react"
import containerStyles from "./container.module.css"

export default ({ children }) => (
  <div className={containerStyles.container}>{children}</div>
)
```

Как вы могли заметить, мы импортировали файл CSS модуля с именем `container.module.css`. Давайте создадим этот файл.

2. В том же каталоге (`src/components`) создайте файл `container.module.css` и добавьте следующее:

```css:title=src/components/container.module.css
.container {
  margin: 3rem auto;
  max-width: 600px;
}
```

Обратите внимание на окончание имени файла `.module.css` вместо обычного `.css`. Таким образом вы сообщаете Gatsby, что этот файл CSS должен обрабатываться как модуль.

3. Сделаем новую страницу, создав файл в
`src/pages/about-css-modules.js`:

```javascript:title=src/pages/about-css-modules.js
import React from "react"

import Container from "../components/container"

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
  </Container>
)
```

Открыв `http//localhost:8000/about-css-modules/`, ваша страница должна выглядеть примерно так:

![Страница со стилями модуля CSS](css-modules-basic.png)

#### ✋ Стилизация компонента с использованием CSS-модулей

В этом разделе мы создадим список людей с именами, аватарами и краткими биографиями. Вы создадите компонент <User /> и стилизируете его с помощью CSS модуля.

1. Создайте CSS файл `src/pages/about-css-modules.module.css`.

2. Со следующим содержимым:

```css:title=src/pages/about-css-modules.module.css
.user {
  display: flex;
  align-items: center;
  margin: 0 auto 12px auto;
}

.user:last-child {
  margin-bottom: 0;
}

.avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
}

.description {
  flex: 1;
  margin-left: 18px;
  padding: 12px;
}

.username {
  margin: 0 0 12px 0;
  padding: 0;
}

.excerpt {
  margin: 0;
}
```

3. Импортируйте новый файл `src/pages/about-css-modules.module.css` на страницу `about-css-modules.js`, которую вы создали ранее, отредактировав первые несколько строк файла следующим образом:

```javascript:title=src/pages/about-css-modules.js
import React from "react"
// highlight-next-line
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

// highlight-next-line
console.log(styles)
```

Код `console.log(styles)` выведет результат импорта, чтобы мы могли увидеть результат обработанного файла `./about-css-modules.module.css`. Если вы откроете консоль разработчика (с помощью инструментов разработчика в Firefox или Chrome) в своем браузере, вы увидите:

![Результат импорта CSS модуля в консоли](css-modules-console.png)

Если вы сравните это с вашим CSS-файлом, вы увидите, что каждый класс теперь является ключом в импортируемом объекте, в виде длинной строки. Например, `avatar` указывает на` src-pages----about-css-modules-module---avatar---2lRF7`. Это имена классов, которые генерируют CSS модули. Они гарантированно будут уникальными на вашем сайте. И поскольку вы должны импортировать их, чтобы использовать классы, никогда не возникает вопроса о том, где используется какой-либо CSS.

4. Создайте компонент `User`.

Создайте новый встроенный компонент `<User />` на странице `about-css-modules.js`.
Измените `about-css-modules.js` следующим образом:

```jsx:title=src/pages/about-css-modules.js
import React from "react"
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

console.log(styles)

// highlight-start
const User = props => (
  <div className={styles.user}>
    <img src={props.avatar} className={styles.avatar} alt="" />
    <div className={styles.description}>
      <h2 className={styles.username}>{props.username}</h2>
      <p className={styles.excerpt}>{props.excerpt}</p>
    </div>
  </div>
)
// highlight-end

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
    {/* highlight-start */}
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
      excerpt="I'm Jane Doe. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
      excerpt="I'm Bob Smith, a vertically aligned type of guy. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    {/* highlight-end */}
  </Container>
)
```

> Примечание: Как правило, если вы используете компонент в нескольких местах на сайте, он должен находиться в отдельном файле модуля в каталоге компонентов. Но, если он используется только в одном файле, то создавайте его встроенным.

Готовая страница должна выглядеть следующим образом:

![Страница списка пользователей с модулями CSS](css-modules-userlist.png)

### CSS-in-JS

CSS-in-JS является компонентно-ориентированным подходом стилизации. В целом, это подход, в котором [CSS встраивается с использованием JavaScript](https://ru.reactjs.org/docs/faq-styling.html#what-is-css-in-js).

#### Использование CSS-in-JS с Gatsby

Существует множество различных библиотек CSS-in-JS, и многие из них уже имеют плагины для Gatsby. Мы не будем рассматривать пример CSS-in-JS в вступительном руководстве, но мы рекомендуем вам [ознакомится](/docs/styling/) с тем, что может предложить экосистема. Существуют мини-руководства для двух библиотек: [Emotion](/docs/emotion/) и [Styled Components](/docs/styled-components/).

#### Что почитать о CSS-in-JS

Если вы хотите узнать больше, то посмотрите [презентацию Кристофера "vjeux" Чидау, от 2014 года](https://speakerdeck.com/vjeux/react-css-in-js), которая начала это движение, а также [более новую статью Марка Далглиша "Унифицированный язык стилей"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660).

### Другие опции CSS

Gatsby поддерживает практически все возможные варианты стилей (если плагин для вашего любимого подхода в стилизации еще не поддерживается ― [пожалуйста, добавьте его!](/contributing/how-to-contrib/))

- [Typography.js](/packages/gatsby-plugin-typography/)
- [Sass](/packages/gatsby-plugin-sass/)
- [JSS](/packages/gatsby-plugin-jss/)
- [Stylus](/packages/gatsby-plugin-stylus/)
- [PostCSS](/packages/gatsby-plugin-postcss/)

и другие!

## Что дальше?

Переходите к [третьей части](/tutorial/part-three/), чтобы узнать о плагинах Gatsby и шаблонных компонентах.
