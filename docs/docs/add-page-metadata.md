---
title: Добавление метаданных страницы
---

Если Вы запускали [аудиты с Lighthouse](/docs/audit-with-lighthouse/), вВы могли заметить невысокий рейтинг в категории "SEO". Давайте разберёмся, как Вы можете это исправить.

Добавление метаданных страниц (таких как заголовок или описание) - это простой спобоб помочь поисковым движкам, таким как Google, понять, о чём контент на странице, и понять, когда показать Вашу страницу в поисковой выдаче.

[React Helmet](https://github.com/nfl/react-helmet), который предоставляет React интерфейс компонента для управления [head вашего документа](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head).

Gatsby [react helmet плагин](/packages/gatsby-plugin-react-helmet/) предоставляет встроенную поддержку для генерируемых сервером данных, добавленных с React Helmet. При использовании этого плагина, атрибуты, которые Вы добавили в React Helmet, будут добавлены в статические  HTML страницы, которые собирает Gatsby.

### Использование `React Helmet` и `gatsby-plugin-react-helmet`

1. Установите оба плагина:

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2. Добавьте массив `plugins` в Ваш файл `gatsby-config.js`.

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

> 💡 Пример выше приведёт из [документации React Helmet](https://github.com/nfl/react-helmet#example). Почитайте её для изучения больших возможностей!

Вам так же может понравится документация про [добавление SEO компонента](/docs/add-seo-component/).
