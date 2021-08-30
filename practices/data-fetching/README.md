## Дисклеймер

1. Комментарии в коде на русском обусловлены тем, что это учебно-показательный материал, и в боевом коде они строго под запретом

2. Перед прочтением настоятельно рекомендуется ознакомиться (если еще не знакомы) с:

- [React хуками](https://ru.reactjs.org/docs/hooks-intro.html)
- [Graphql](https://graphql.org)

## Стек

- Graphql + [Apollo](https://www.apollographql.com)
- React

## Абстрактное пояснение стека

**Graphql** - это язык запросов, использующийся для общения между клиентом и сервером в более удобном формате

**Apollo** - библиотека, упрощающая работу с graphql (в том числе с React)

## Задача

### Пре-реквизиты

У нас есть фрагмент - список тудушек. Тудушки необходимо получать из внешнего источника данных, соответственно:

### Формулировка

1. Сделать запрос на эндпоинт для получения списка задач
2. Отображать Loading пока не придут данные
3. Отрисовать список задач как только они придут

## Пошаговая реализация

### Куда делать запросы?

Url источника данных указывается в `ApolloProvider`. При использовании Nextjs это достаточно сделать один раз в `_app.tsx`, с этим поможет библиотека [next-app-with-apollo](https://github.com/atls/nextjs/tree/master/packages/next-app-with-apollo)

### Как делать запросы

Здесь уже понадобится библиотека `@apollo/client`, упрощающая работу с Apollo в React приложении, она предоставляет привычный для react разработки набор инструментов

Сделаем рабочий прототип (с комментариями):

```javascript
import React from 'react'
import { FC } from 'react'
import { gql } from '@apollo/client'
import { useQuery } from '@apollo/client'
import { List } from '@ui/list' 
import { Text } from '@ui/text'

const TodoList: FC = () => {
  // пишем graphql запрос
  const GET_TASKS = gql`
    query GetTasks {
      tasks {
        title
        done
      }
    }
  `
  const { data, loading, error } = useQuery(GET_TASKS) // исполняем запрос

  return (
    <>
      {/* Если идет загрузка - рисуем Loading... */}
      {loading && <Text>Loading...</Text>}
      {/* Если есть данные - рисуем данные */}
      {data && <List items={tasks} />}
    </>
  )
}
```

### Почему для исполнения запроса используется хук?

Чтобы компонент узнал о том, что пришли данные и произошел ререндер

### Теперь приведем код к боевому виду

Сам процесс запроса данных разделяется на следующие шаги:

1. Создание gql запроса
2. Исполнение запроса
3. Обработка ответа

Все эти этапы тащат за собой определенное количество шаблонного кода, который мы можем скрыть за более понятными человеку абстракциями. Сделаем это поэтапно:

0. Так как работа с данными - это в своем роде инкапсулированный процесс, то создадим директорию data: `project/fragments/todo-list/data`. В ней будем описывать всю логику работы с данными

1. Запрос

Запрос вынесем в отдельный файл: `project/fragments/todo-list/data/tasks.query.ts`

```javascript
import { gql } from '@apollo/client'

const GET_TASKS = gql`
  query GetTasks {
    tasks {
      title
      done
    }
  }
`

export { GET_TASKS }
```

2. Исполнение

Тут ситуация чуть более неочевидная: для исполнения запроса используется хук (useQuery), а хуки, как мы знаем, можно использовать только внутри компонентов

Это обходится с помощью коллбека. Создадим файл `project/fragments/todo-list/data/useData.ts`:

```javascript
import { useQuery } from '@apollo/client'

import { GET_TASKS } from './tasks.query'

const useData = () => {
  const { data, loading, error } = useQuery(GET_TASKS)

  if (error) {
    throw new Error(error.message)
  }

  if (data) {
    return [data.tasks, loading]
  }

  return [[], loading]
}
```

Здесь уже появились новые элементы:

- **Блок с обработкой ошибки** - обработка ошибок это правило хорошего тона при разработке, в случае чего очень поможет при отладке.
- **Два ретёрна?** - первый отработает только в том случае, если данные пришли. Причем, что стоит заметить, мы возвращаем `data.tasks`, вместо data, чтобы в самом компоненте нам не пришлось приводить объект из ответа к нужному виду. Второй сработает если данных нет.
- **Почему во втором ретёрне возвращаем пустой массив?** - для безопасности типов. Мы ожидаем получить массив с задачами, но если задач нет - получим пустой массив. Это необходимо, чтобы исключить вероятность того, что мы будем пытаться вызвать метод `map` у null или undefined.

3. Отрисовка

Теперь наш компонент преобразовался в:

```javascript
import React from 'react'
import { FC } from 'react'
import { List } from '@ui/list' 
import { Text } from '@ui/text'

import { useData } from './data'

const TodoList: FC = () => {
  const [tasks, loading] = useData()

  return (
    <>
      {loading && <Text>Loading...</Text>}
      {data && <List items={tasks} />}
    </>
  )
}
```
