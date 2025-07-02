# Требования к окружению

Привет!

Чтобы приступить к началу работы, нужно подготовить рабочее окружение. Рабочее окружение это операционная система, среда разработки и другие рабочие утилиты. Все необходимые для работы инструменты указаны ниже.

Как только выполнишь все пункты и твоё окружение будет готово к работе, сообщи своему ментору.

# 1. Ubuntu@LTS

## Как лучше поставить?

- Второй, либо основной осью
- Следовать только [официальным гайдам по настройке и установке](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview)

## Почему Ubuntu@LTS

- Линукс в целом необходим для полной совместимости с нашим рабочим окружением. Например, [tools](https://github.com/atls/tools) являющиеся частью нашей инфраструктуры довольно нестабильно работает под виндой
- LTS версия предпочтительнее из-за стабильности
- Все члены команды так или иначе работали/работают на Ubuntu. Это значит, что при столкновении с проблемой тебе смогут оперативно оказать помощь

# 2. IDE

Для работы с кодом обязательно потребуется [IDE](https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%B0%D1%8F_%D1%81%D1%80%D0%B5%D0%B4%D0%B0_%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B8) **WebStorm**.

## Почему WebStorm?

По той же причине, почему Ubuntu - все члены команды используют IDE от JetBrains, поэтому тебе помогут в ней освоиться и настроить под себя.

## Необходимые плагины

- [`GraphQL`](https://plugins.jetbrains.com/plugin/8097-graphql)
- [`Inspection lens`](https://plugins.jetbrains.com/plugin/19678-inspection-lens)
- [`env file support`](https://plugins.jetbrains.com/plugin/9525--env-files-support)
- [`ignore`](https://plugins.jetbrains.com/plugin/7495--ignore)
- [`Protocol buffers`](https://plugins.jetbrains.com/plugin/14004-protocol-buffers)
- [`Toml`](https://plugins.jetbrains.com/plugin/8195-toml)

## Nice to have плагины

- [`Git branch cleaner`](https://plugins.jetbrains.com/plugin/10059-git-branch-cleaner)

# 3. NodeJS@LTS

[Что такое NodeJS](https://habr.com/ru/post/420123/) — там же есть ссылка на очень крутое видео про EventLoop, настоятельно рекомендуется ознакомиться.

**Что за `@LTS+`?** — это версия ноды.

## Как ставить?

**[Через nvm](https://github.com/nvm-sh/nvm)**

**Через aptitude:**

`apt install nodejs`

# 4. yarn

## Что такое yarn?

Yarn — это альтернативный npm-клиент для работы в качестве пакетного менеджера JavaScript, совместно созданный Facebook, Google, Exponent и Tilde. Этот менеджер пакетов ускоряет сборку пакетов и делает её более безопасной.

Основная причина использования yarn - система плагинов. В наших проектах мы используем [свой собственный yarn бандл](https://github.com/atls/raijin), который содержит множество полезных утилит. Лишь некоторые из них: 
- webpack с конфигурацией
- eslint с правилами
- Typescript
- создание образов Docker с помощью Buildpacks
- Prettier с собственным плагином сортировки импортов
- husky
- интеграция с GitHub Actions

Последовательная установка пакетов через npm заметно замедляет работу. Yarn же поддерживает параллельную установку, что обычно в несколько раз быстрее. Так же, yarn поддерживает работу через глобальный и локальный кэш, значительно ускоряя установку зависимостей.

По сути, Yarn — отличная замена npm. В своей работе мы используем именно его.

## Как ставить

Yarn ставится в каждый проект, а не глобально. Поэтому для каждого проекта нужно устанавливать его отдельно.

### Первая установка

`yarn set version https://raw.githubusercontent.com/atls/raijin/master/yarn/cli/dist/yarn.mjs`

### Дальнейшее обновление

`yarn set version atls`

> [!NOTE]
> Делаем это регулярно для наших проектов, чтобы иметь последнюю актуальную версию.

# 5. Email

Почтовый клиент и привязанная к нему почта, которая привязана к гитхабу для получения всех уведомлений

# 6. Настройка Github

- [Настроить 2FA](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa) для своего Github аккаунта
- Настроить [проверку подписи коммитов (GPG)](https://docs.github.com/en/authentication/managing-commit-signature-verification)
- Авто подпись коммитов (настраивается в git) `git config --global commit.gpgsign true`
- [Настроить подключение к Github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) через [SSH](https://ru.wikipedia.org/wiki/SSH)
- Настроить [шаблоны ответов](https://docs.github.com/en/get-started/writing-on-github/working-with-saved-replies/creating-a-saved-reply) для наших повседневных задач:

  <details>
  <summary>Видео как добавлять и пользоваться</summary>

  https://github.com/user-attachments/assets/0cacace4-fd4c-43bd-94ea-2ef05878ce9c

  </details>


  <details>
  <summary>Дейлик (DSM)</summary>

  ```md
  #### Какие задачи выполнял вчера? Укажи #issues и сообщи в каком они состоянии
  - 

  #### Какие задачи будешь делать сегодня? Укажи #issues
  - 

  #### Что тебя блокирует? (Этот пункт используется когда тебя что-то блокирует)
  - 

  #### Есть ли личные дела из-за которых нужно отсутствовать на рабочем месте в течение рабочего дня? (Этот пункт используется когда дела есть)
  - 

  ```

  </details>

  <details>
  <summary>Статус на конец дня</summary>

  ```md
  #### Что сделано:
  - 

  #### Что дальше:
  - 

  #### Актуальная ветка:
  - 

  ```

  </details>

  <details>
  <summary>Тело Pull Request (опционально)</summary>

  ```md
  Closes ${задача}

  <details>
  <summary>Пруф</summary>



  </details>
  ```

  </details>

