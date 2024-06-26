# useContext

- [ ] `useContext()` - это хук, который поволяет подписываться и читать контекст из любого компонента в приложении.
- [x] Тесно связан с хуком `useState()`. 

<br>

- [x] Позволяет передавать состояния через деревом компонентов без явной передачи пропсов.
- [x] По сути контекст - это сущность, которая позволяет передавать состояния внутри всего приложения.
- [x] Позволяет избежать так называемого `property drill`.

<br>

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/1f00d5d6-7f90-4ae7-8d8f-0c2e519a9f19)

<hr>
<br>
<br>

<h2>Использование</h2>

[ПРОСТОЙ ПРИМЕР](https://codesandbox.io/p/devbox/usecontext-forked-swjgmd?file=%2Fsrc%2FApp.tsx%3A13%2C1)

- [ ] Основные параметры для создания контекста:

  + Создание контекста при помощи `createContext()`.
  + Создаем состояние при помощи хука `useState()`.
  + Оборачиваем в `Provider` компоненты, которые должны будут следить за контекстом.
  + Внутри компонента получаем состояние из контекста (+ сеттер для состояния если есть) при помощи хука `useContext()`
     
<br>
<br>

<h3>+ Создание контекста createContext</h3>

- [ ] Для создания контекста используется `createContext(value)`

  + `value` - это значение, которое будет передаваться в случае, если при вызове хука `useContext()` значение внутрь не было передано.
  + Это своего рода резервное значение, выводимое, если наименование контекста при вызове не указано и компонент не сломается.
     
```typescript
  const ThemeContext = createContext(null);
```
     
<br>

- [x] ПРИМ. Контекст создается там же, где будет находится `Provider`

<br>
<br>

<h3>+ Создание состояния для контекста / Provider</h3>

- [ ] `Состояние`, которое будет передаваться при помощи `контекста` - создается при помощи `useState()`.

  + Важно понимать, что `контекст - это не состояние`.
  + `Контекст - передает состояние, избегая потребности в пропсах`.
     
<br>

- [ ] `Provider` - служит для передачи контекста компонентам. Оборачивает компоненты, которым должен быть доступен контекст.

  + Принимает `value` - это параметр (состояние), которое будет передаваться. + Сеттер, который меняет состояние.
  + Обычно для удобства создание `контекста` и `провайдера` объеденяют в отдельный компонент.
  + Этот компонент и будет внутри себя рендерить компоненты как `children`.
     
  ```typescript
    import { createContext, useState } from "react";

    // Создаем контекст
    export const AuthContext = createContext(null);
    
    export const AuthProvider = ({ children }) => {
      // Создаем state + setState, которые будут передаваться в контексте
      const [isLogged, setIsLogged] = useState(false);

  /*
    Оборачиваем {children} в провайдер, передавая value (это состояние и смена состояния)
    Компоненты, обёрнутые в провайдер имеют доступ к состоянию в контексте.
  */
      return (
        <AuthContext.Provider
          value={{
            isLogged,
            setIsLogged,
          }}
        >
          {children}
        </AuthContext.Provider>
      );
    };
  ```

<br>
<br>

<h3>+ Подписка и чтение состояния</h3>

- [ ] Чтобы подписаться на состояние в контексте и иметь к нему доступ/изменять - мы должны вызвать хук `useContext()` в компоненте, с параметрами, которые хотим читать.

- [x] Пример кнопки, которая меняет состояние + изменяется сама в зависимости от стейта.

```typescript
import { useContext } from "react";
import { AuthContext } from "./contexts/AuthContext";

export const Button = () => {
  // Достаем state и setState из нужного контекста (AuthContext)
  const { isLogged, setIsLogged } = useContext(AuthContext);

  // Рендеры (кнопка меняет как обычный стейт)
  return (
    <button onClick={() => setIsLogged((isLogged: boolean) => !isLogged)}>
      {!isLogged ? "Войти в систему" : "Выйти из системы"}
    </button>
  );
};
```

<hr>
<br>
<br>

<h2>Прошлый пример но более клёвый с кастомным хуком</h2>

[УБЕР ПРИМЕР](https://codesandbox.io/p/devbox/usecontext-gwnmfc?file=%2Fsrc%2FApp.tsx%3A12%2C2)

- [ ] Чтобы не производить постоянные импорты `контекста` и хука `useContext()`, можно создать кастомный хук.

<br>

- [x] Как и что:

  + Так же создаем `AuthProvider` в контексте `AuthContext`, который будет принимать в себя компоненты как `children`, тем самым передавай состояние по контексту.
  + Чтобы в каждом компоненте не импортировать каждый раз контекст + хук мы можем завернуть это все в кастомный хук.
  + Таким образом компоненты будут подписываться на стейт при помощи хука.
     
  ```typescript
  import { useContext } from "react";
  import { AuthContext } from "../contexts/AuthContext";

  // Кастомный хук
  // Получает контекст при помощи хука useContext()
  export const useAuth = () => {
    const { isLogged, setIsLogged } = useContext(AuthContext);

  // Хук возвращает состояние и сеттер для его изменения.
    return { isLogged, setIsLogged };
  };
  ```

  - [x] Итог:
     
  + Вызвав кастомный хук - мы получаем состояние и сеттер для слежки и изменения.
  + Избегая лишних импортов и код.
     
  ```typescript
  import { useAuth } from "./hooks/useAuth";

  export const Button = () => {
    // Получааем стейт и стейт-сеттер
    const { isLogged, setIsLogged } = useAuth();
  
    return (
      <button onClick={() => setIsLogged((isLogged: boolean) => !isLogged)}>
        {!isLogged ? "Войти в систему" : "Выйти из системы"}
      </button>
    );
  };
  ```

<hr>
<br>
<br>

<h2>ЕЩЕ РАЗ БЕСТ ПРАКТИС</h2>

- [ ] Это поможет избержать засорения и компонентов.
- [x] Для провайдеров создается отдельный файл - компонент 

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/090f2811-94ed-4945-97f8-82ca7d32030c)

<br>

- [x] Внутри компонента объявляется и контекст и состояние и провайдер.
- [x] Не забыть `value` - что передает провайдер и `стейт` и `смена стейта`.

```typescript
import { createContext, useReducer } from "react";

export const TasksContext = createContext(null);

export const TasksProvider = ({ children }) => {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  return (
    <TasksContext.Provider value={{ tasks, dispatch }}>
      {children}
    </TasksContext.Provider>
  );
};
```

<br>

- [x] В корне импортируем компоненты провайдеров и оборачиваем ими приложение, чтобы оно читало контекст.

```typescript
export default function TaskApp() {
return (
  <TasksProvider>
    <h1>Day off in Kyoto</h1>
    <AddTask />
    <TaskList />
  </TasksProvider>
);
}
``` 


<hr>
<br>
<br>

<h2>Оптимизация повторного рендеринга при передаче объектов и функций</h2>

- [ ] В случае, если какой-то компонент возвращает дочерние компоненты, обернутые в контекст, в котором переданы объект или ф-я в кач-ве `value` - обновление род. компонента вызовер ререндер для всех компонентов, которые следят за контекстом.

  + это происходит потому, что при ререндере ф-я создается новая.
  + То же самое и с объектом.
  + `ПОМНИМ ПРО ЭТО`.
     
- [x] Нпример:

+ Тут мы имеем ф-ю `login`, которая будет пересоздаваться.
+ И объект со стейтом и ф-ей в кач-ве `value`

```javascript
  function MyApp() {
  // Стейт и стейт-сеттер
    const [currentUser, setCurrentUser] = useState(null);

  // Ф-я изменения стейта + еще какие то данные
  // Эта ф-я будет пересоздана и вызовет ререндер.
    function login(response) {
      storeCredentials(response.credentials);
      setCurrentUser(response.user);
    }

  // Передаем как value контекста объект с состоянием и ф-ей
    return (
      <AuthContext.Provider value={{ currentUser, login }}>
        <Page />
      </AuthContext.Provider>
    );
  }
```

<br>

- [ ] Решение.

- [ ] В кач-ве решения нам нужно мемоизировать создание ф-ии при помощи `useCallback()`
- [ ] Плюс обернуть объект, который передается в `value` в `useMemo()` - чтобы объект стал мемоизированным результатом вычислений.
- [ ] В `value` будет передан объект со стейтом и ф-й. Объект будет мемоизирован
- [x] Это уберет пересоздавания и ререндеры. 

```javascript
  import { useCallback, useMemo } from 'react';

  function MyApp() {
  // Стейт
    const [currentUser, setCurrentUser] = useState(null);

  // Обернули ф-ю в useCallback
    const login = useCallback((response) => {
      storeCredentials(response.credentials);
      setCurrentUser(response.user);
    }, []);

  // Объект теперь - это результат вычислений обернутый в useMemo()
    const contextValue = useMemo(() => ({
      currentUser,
      login
    }), [currentUser, login]);

  // value теперь мемоизированный объект.
    return (
      <AuthContext.Provider value={contextValue}>
        <Page />
      </AuthContext.Provider>
    );
  }
```

<hr>
<br>
<br>

<h2>useContext() + useReducer()</h2>

- [ ] `useReducer()` - позволяет более гибко управлять состоянием, путем применения `actions`.

<br>

- [x] `useReducer()` - это точно такой же хук для работы с состоянием, как и `useState()`, поэтому его так же можно комбинировать с `useContext()`.

<br>
<br>

<h3>+ Проблема</h3>

[ПРИМЕР useReducer без контекста](https://codesandbox.io/p/devbox/usereducer-default-forked-wrm58j?file=%2Fsrc%2FApp.tsx)

- [ ] Проблема заключается в `propsDrilling`:

  + Состояние так же создается в главном компоненте и передается ниже дочерним.
  + Много компонентов === проблема `propsDrilling`

<br>
<br>

<h3>+ Решение combine useReducer + useContext</h3>

- [ ] Чтобы решить проблему нужно:

  + Создать контекст (Как отделный компонент с состоянием и провайдером, чтобы не засорять компоненты).
  + Поместить `состояние` и `dispatch` в контекст.
  + Использовать контекст везде, где нужно его содержимое.
     
<br>
<br>
<br>

[ПРИМЕР useReducer + useContext](https://codesandbox.io/p/devbox/usereducer-usecontext-tkds7w?file=%2Fsrc%2FApp.tsx%3A6%2C11)

- [x] Создание контекста:

  + Мы создали отдельный компонент контекста (`TaskContext`) (что правильно и удобно).
  + Инициализировали `контекст`.
  + Объявили состояние при помощи хука `useReducer()`
  + Объявили ф-ю `reduser` для создания `actions` для смены состояния.
  + Объявили начальное состояние.

  <br>

  + Контекст провайдер имеет зн-е `value`, в котором лежит объект с `состоянием` и `dispatch`, чтобы преедавать это по компонентам.
  + Компонент обязательно возвращает `children` который обёрнут в `контекст-провайдер` со значением контекста, чтобы внутри рендерились компоненты, следящие за контекстом.
     
<br>
<br>

- [x] Помещение `состояния` и `dispatch` в контекст:

  + Это было сделано в прошлом пункте для удобства.
  + `Контекст.Provider` имеет `value` - туда и помещается `состояние` и `dispatch` для передачи.

<br>
<br>

- [x] Использование контекста везде где надо:

  + Компоненты, которые должны читать состояние импортируют хук `useContext()` для чтения контекста.
  + Достаем `state` и `dispatch` при помощи `useContext()`.
  + Первый отрисовывает данные, а `dispatch` меняет стейт при помощи `action.type` и параметров, которые передаются внутри `action`.
     
  ```typescript
    dispatch({
      type: "added",
      id: nextId++,
      text: text,
    });
  ```

<br>
<br>

- [x] Итог:

  + Избегаем пропс дриллинг.
  + Любой компонент имеет доступ к контексту.
  + Любой компонент может прочитать и изменить стейт.
