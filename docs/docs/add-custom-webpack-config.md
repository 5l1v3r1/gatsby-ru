---
title: "Добавление пользовательского файла конфигурации Webpack"
---

_Перед созданием пользовательского файла конфигурации Webpack проверьте [раздел плагинов](/docs/plugins/):
возможно, ваша проблема была решена в уже существующем Gatsby плагине. Если ничего найти не удалось, а ваш случай распространен,
пожалуйста, добавьте ваш плагин в библиотеку плагинов Gatsby, чтобы другие люди (включая вас самого в будущем 😀) смогли им воспользоваться._

Чтобы добавить пользовательский файл конфигурации Webpack необходимо создать
(если уже не создан) `gatsby-node.js` файл в корневой директории. Внутри этого файла
экспортируйте функцию с именем `onCreateWebpackConfig`.

Когда Gatsby будет генерировать собственный конфиг Webpack, эта функция
будет вызвана, позволяя вам изменить настройку Webpack по умолчанию с помощью
[webpack-merge](https://github.com/survivejs/webpack-merge).

Gatsby генерирует несколько webpack билдов с отличающейся друг от друга
конфигурацией. В Gatsby каждый из таких билдов называют "стадия". Существуют следующие стадии:

1.  develop: при запуске `gatsby develop` команды. Включает настройку для hot reloading и CSS
    инъекций на страницу.
2.  develop-html: то же самое, что и develop, но без react-hmre в конфиге babel для рендеринга
    HTML компонента.
3.  build-javascript: продакшен билд JavaScript и CSS. Создает маршрут для JS бандлов, а так же чанки для JS и CSS.
4.  build-html: продакшен билд статических HTML страниц

Рекомендуем посмотреть исходный код в
[webpack.config.js](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby/src/utils/webpack.config.js).

Примеры использования этого API можно найти в плагинах из
Gatsby репозитории, таких как [Sass](/packages/gatsby-plugin-sass/),
[TypeScript](/packages/gatsby-plugin-typescript/),
[Glamor](/packages/gatsby-plugin-glamor/) и многих других.

## Примеры

Пример добавления дополнительной глобальной переменной с помощью `DefinePlugin` и `less-loader`:

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({
  stage,
  rules,
  loaders,
  plugins,
  actions,
}) => {
  actions.setWebpackConfig({
    module: {
      rules: [
        {
          test: /\.less$/,
          use: [
            // Добавлять ExtractText плагин нет необходимости,
            // потому что gatsby уже содержит его и следит за тем,
            // чтобы он запускался только там, где это нужно
            // (к примеру, не в стадии development)
            loaders.miniCssExtract(),
            loaders.css({ importLoaders: 1 }),
            // postcss loader обладает отличными настройками по умолчанию,
            // включая autoprefixer для указанных нами браузеров
            loaders.postcss(),
            `less-loader`,
          ],
        },
      ],
    },
    plugins: [
      plugins.define({
        __DEVELOPMENT__: stage === `develop` || stage === `develop-html`,
      }),
    ],
  })
}
```

### Абсолютные импорты

Вместо прописывания `import Header from '../../components/header'` снова и снова, вы можете упростить написание до `import Header from 'components/header'`
используя абсолютные импорты:

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({ stage, actions }) => {
  actions.setWebpackConfig({
    resolve: {
      modules: [path.resolve(__dirname, "src"), "node_modules"],
    },
  })
}
```

Более подробная информация о _resolve_ и других опциях в официальной [документации Webpack](https://webpack.js.org/concepts/).

### Модификация babel loader

Это понадобится, к примеру, при транспилинге отдельных частей `node_modules` и прочих похожих манипуляций.

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({ actions, loaders, getConfig }) => {
  const config = getConfig()

  config.module.rules = [
    // Опустим правило test === '\.jsx?$', выставленное по умолчанию
    ...config.module.rules.filter(
      rule => String(rule.test) !== String(/\.jsx?$/)
    ),

    // Создадим его заново с собственным фильтром исключения
    {
      // Вызванный без аргументов, `loaders.js` вернет
      // объект вида:
      // {
      //   options: undefined,
      //   loader: '/path/to/node_modules/gatsby/dist/utils/babel-loader.js',
      // }
      // Если вы не собираетесь менять Babel на другой транспилер, вам,
      // вероятно, понадобится следующая строка для того, чтобы
      // Gatsby применил необходимые Babel пресеты и плагины. Конфигурация из
      // `babel.config.js` так же будет добавлена в вашу.
      ...loaders.js(),

      test: /\.jsx?$/,

      // Исключаем все node_modules из транспиляции, кроме 'swiper' и 'dom7'
      exclude: modulePath =>
        /node_modules/.test(modulePath) &&
        !/node_modules\/(swiper|dom7)/.test(modulePath),
    },
  ]

  // Эта строка полностью заменит конфиг webpack на модифицированный объект
  actions.replaceWebpackConfig(config)
}
```
