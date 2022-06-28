# MIGRATION NEWS-API TO TS TASK WALKTHROUGH

Примерное описание процесса выполнения таски (**следовать на свой страх и риск**). Будет полезно тем, у кого возникают вопросы, что именно нужно делать в таске. Остальным будет мало полезно.

## ПОДГОТОВКА
+ Открываем в **VSCode** папку с приватным репозиторием **RSSchool**
+ Создаем новую ветку с именем таски (**migration-newip-to-ts**)
+ Переходим в созданную ветку
+ Создаем новую папку с именем таски
+ Копируем в нее файлы таски (загружаем **zip-файл** с гитхаба и распаковываем)
+ *(Делаем init коммит)*

## НАСТРОЙКА СБОРКИ И ДОБАВЛЕНИЕ API KEY
+ Выполняем `npm i` для установки исходных пакетов проекта
+ Добавляем **apiKey** (получить можно на сайте **https://newsapi.org**)
	+ В файл **src/components/controller/appLoader.js**
+ *(Делаем коммит)*
+ Добавляем пакет **TS**
	+ `npm i -D typescript`
+ Добавляем пакеты для работы **ESlint** и **Webpack** c **.ts файлами**
	+ `npm i -D @typescript-eslint/parser` (парсер **TS** для **ESlint** - чтобы **ESlint** умел правильно парсить **.ts** файлы)
	+ `npm i -D @typescript-eslint/eslint-plugin` (плагин **TS** для **ESlint** - правила, по которым **ESlint** будет проверять **.ts** файлы)
	+ `npm i -D ts-loader` (лоадер **TS** для **webpack-а** - чтобы **webpack** мог работать с **.ts** файлами)
	+ `npm i -D eslint-webpack-plugin` (плагин **ESlint** - для работы **ESlint** вместе с **Webpack**)
+ Настраиваем конфиги
	+ **TS**
		+ Создаем файл **tsconfig.json** (команда `npx tsc --init`)
		+ Внутри файла **tsconfig.json**
			+ Выключить `"module": "commonjs"` (т.к. сборкой будет заниматься **Webpack**)
			+ Включить `"strict": true` (требование таски)
			+ Включить `"noImplicitAny": true` (требование таски)
	+ **ESlint**
		+ В корень добавляем
			+ `"parser": "@typescript-eslint/parser"` (чтобы **ESlint** понимал какой парсер использовать)
		+ В массив **"plugins"** добавляем
			+ `"@typescript-eslint"` (чтобы **ESlint** видел плагин **TS**)
		+ В массив **"extends"** добавляем
			+ `"eslint:recommended"` (родные правила **ESlint** - на всякий случай)
			+ `"plugin:@typescript-eslint/recommended"` (правила **Typescript**)
		+ В массив **"rules"** добавляем
			+ `"@typescript-eslint/no-explicit-any": 2` (правило, запрещающее использование типа **any**)
	+ **Webpack**
		+ Меняем **"entry"** удаляя из него расширение файла **.js** (чтобы при замене **.js** на **.ts** он его тоже подхватил)
		+ Добавляем в массив **"module.rules"** правило для **.ts-файлов** (чтобы **Wepback** обрабатывал **.ts-файлы**)
			+ `{ test: /\.ts$/i, use: 'ts-loader' },`
		+ Импортируем плагин **ESlint** (чтобы он работал во время работы **Webpack**)
			+ `const EslingPlugin = require('eslint-webpack-plugin');`
		+ Добавляем экземпляр плагина **ESlint** в массив **"plugins"** (в его настройках указываем тип файлов **.ts**)
			+ `new EslingPlugin({ extensions: 'ts' })`
		+ Добавляем в массив **"resolve.extensions"** расширение **.ts-файлов** (это определяет порядок поиска файлов в импортах)
			+ `extensions: ['.ts', '.js']`
+ *(Делаем коммит)*
+ Запускаем дев-сервер командой `npm start`

## ОПИСЫВАЕМ ИНТЕРФЕЙСЫ
+ Пишем интерфейсы для **News API:**
	+ Создаем файл **src/types/index.ts**
	+ Внутри файла объявляем и импортируем интерфейсы **News API** (`export interface <имя интерфейса> { ... }`)
		+ Какие именно поля должны иметь интерфейсы и их типы можно посмотреть в документации **News API** (**https://newsapi.org/docs/endpoints/sources**)
+ *(Делаем коммит)*

## МИГРАЦИЯ ПРОЕКТА
+ Меняем расширения файлов с **.js** на **.ts** в следующем порядке и добиваемся отсутствия ошибок в консоли (для каждого готового **.ts** файла делаем коммит):
	+ src/components/view/news/news.js
	+ src/components/view/sources/sources.js	
	+ src/components/view/appView.js
	+ src/components/controller/loader.js
	+ src/components/controller/appLoader.js
	+ src/components/controller/controller.js
	+ src/components/app/app.js
	+ src/index.js
	
## ДОПОЛНИТЕЛЬНЫЕ ЗАДАЧИ
+ Добавляем в футер лого со ссылкой на курс и ссылку на гитхаб автора
+ *(Делаем коммит)*
+ Добавляем **"фичу"** (например, фильтрацию по алфавиту или еще что-то)
+ *(Делаем коммит)*
+ Убираем из **html темплейта** явное подключение скрипта (если до этого не сделали)
+ *(Делаем коммит)*
+ Реализуем адаптив и наводим красоту (правим **HTML** и **CSS**)
+ *(Делаем коммит)*
+ Фиксим отсутствующий **img/news_placeholder.jpg** (файл **src/components/view/news/news.ts**)
+ *(Делаем коммит)*
+ Меняем **URL** в **fetch** на тот, который заработает в **gh-pages** (**https://nodenews.herokuapp.com/**)
+ *(Последний\* коммит)*
+ Билдим проект командой `npm run build`
+ Копируем билд в ветку **gh-pages**
+ *(Делаем коммит в gh-pages и пушим в удаленный репозиторий)*
- Оформляем **пул-реквест**

**Mission Complete!**
