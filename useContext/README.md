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

<h3>+ Создание контекста</h3>

- [ ] Для создания контекста используется `createContext(value)`

  + `value` - это значение, которое будет передаваться в случае, если при вызове хука `useContext()` значение внутрь не было передано.
     
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
