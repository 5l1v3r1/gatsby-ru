---
title: Добавление метаданных страницы
---

Если вы проводили [аудиты с помощью Lighthouse](/docs/audit-with-lighthouse/), то могли заметить невысокий рейтинг в категории "SEO". Давайте разберёмся, как это можно исправить.

Добавление метаданных на страницы (например, заголовок или описание) ― это простой способ помочь поисковым движкам, таким как Google, понять содержимое на странице и определить, когда страница появится в поисковой выдаче.

[React Helmet](https://github.com/nfl/react-helmet) ― React-компонент для управления содержимым [head документа](https://developer.mozilla.org/ru/docs/Web/HTML/Element/head).

[Плагин React Helmet](/packages/gatsby-plugin-react-helmet/) для Gatsby предлагает встроенную поддержку для сгенерированных сервером данных, добавленных с помощью React Helmet. При использовании этого плагина, атрибуты, которые вы добавили в React Helmet, будут включены в статические HTML-страницы, которые собирает Gatsby.

### Использование `React Helmet` и `gatsby-plugin-react-helmet`

1. Установите оба плагина:

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2. Добавьте плагин в массив `plugins` в файле `gatsby-config.js`.

```javascript:title=gatsby-config.js
{
  plugins: [`gatsby-plugin-react-helmet`]
}
```

3. Используйте `React Helmet` на ваших страницах:

```jsx
import React from "react"
import { Helmet } from "react-helmet"

class Application extends React.Component {
  render() {
    return (
      <div className="application">
        {/* highlight-start */}
        <Helmet>
          <meta charSet="utf-8" />
          <title>My Title</title>
          <link rel="canonical" href="http://mysite.com/example" />
        </Helmet>
        {/* highlight-end */}
      </div>
    )
  }
}
```

> 💡 Пример выше взят из [документации React Helmet](https://github.com/nfl/react-helmet#example). Ознакомьтесь с ней, чтобы найти больше примеров!

Возможно, вам также будет интересно узнать про [добавление SEO-компонента](/docs/add-seo-component/).
