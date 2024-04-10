# useReducer

- [ ] `useReducer()` - это хук, который добавляет `reducer` компонент и позволяет работать с состоянием как `useState`, но более гибко.

  + `Reducer` - это ф-я, которая изменяет состояние (как setState). Принимает аргументом `state` и `action`. Возвращает новое состояние, учитывая `иммутабельность`

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/269afcf8-5c41-488b-bebe-d5ccb61a0a4e)


<h2>Пример</h2>

- [ ] `useReducer` вызывается на верхнем уровне компонента.

```typescript
  import { useReducer } from 'react';

  // reducer принимает state и action
  function reducer(state, action) {
    // ... логика изменения состояния в reducer
  }
  
  function MyComponent() {
    const [state, dispatch] = useReducer(reducer, { age: 42 });
    // ... логика компонента
```

<br>

<h3>+ Параметры</h3>

- [x] `reducer` - это ф-я, которая изменяет состояние (как `setState`). Принимает аргументом `state - это нынешнее состояние, с которым работает reducer` и `action - событие`. Возвращает новое состояние, учитывая `иммутабельность`

- [x] `initialArg` - начальное состояние. Либо само зн-е либо переменная с ним (как в кайф).
- [x] ОПЦИОНАЛЬНО `init:` - Функция инициализатора, которая должна возвращать начальное состояние. Если она не указана, начальное состояние устанавливается в `initialArg`. В противном случае начальное состояние устанавливается в результат вызова `init(initialArg)`.

<br>

<h3>+ Возвращает</h3>

- [x] Текущее состояние - на основе `initialArg` (или `init`)
- [x] `dispatch function` - позволяющая обновить состояние до другого значения и вызвать повторный рендеринг.

<hr>
<br>
<br>

<h2>dispatch function </h2>

- [x] `dispatch` - это ф-я, обновляющая состояние и вызывающая повторный рендер. Параметром принимает только `action`, который вызывает определенный `case` в `reducer` для изменения состояния.
- [x]  По традиции, действие обычно представляет собой объект со свойством `type` (идентифицирующим это действие), и, опционально, другими свойствами с дополнительной информацией (для изменения стейта).

```javascript
  const [state, dispatch] = useReducer(reducer, initialState);

  function handleInputChange(e) {
    // вызов dispatch в каким-то action и параметром, на основе которого будет меняться стейт
    dispatch({
      type: 'changed_name',
      nextName: e.target.value
    }); 
  }
```

<hr>
<br>
<br>

<h2>ПРИМИЧАНИЕ </h2>

- [ ] Если новое значение, которое вы предоставляете, идентично текущему состоянию, что определяется сравнением `Object.is`, React пропустит повторный рендеринг компонента и его дочерних элементов. Это оптимизация.

<hr>
<br>
<br>

<h2>Примеры работы </h2>

[БАЗОВЫЙ ПРИМЕР РАБОТЫ](https://codesandbox.io/p/sandbox/react-dev-forked-s9rrhz?file=%2Fsrc%2FApp.js%3A10%2C9)

- [x] Каво:

  + Создается `reducer`, который обрабатывает каждый `action`.
  + Принимает в себя `state` (ссылает на нынешнее) и сам `action` (`action.type` - это сам экшн)
  + Затем `actions` обрабатываются в кейсах.
     
  ```javascript
    function reducer(state, action) {
      switch (action.type) {
        case "incremented_age": {
          return {
            name: state.name,
            age: state.age + 1,
          };
        }
        case "changed_name": {
          return {
            name: action.nextName,
            age: state.age,
          };
        }
      }
      throw Error("Unknown action: " + action.type);
    }
  ```

  <br>

  + Создается состояние при помощи хука `useReducer` с параметрами (сам редюсер и начальное состояние) (как useState)
  + Возвращается стейт и `dispatch`
  + Смена состояния вызвается путем вызова `dispatch` с объектом, где определеный `type`, и если нужно - передаваемое св-во.

  ```javascript
    export default function Form() {
      const [state, dispatch] = useReducer(reducer, { name: "Taylor", age: 42 });
    
      function handleButtonClick() {
        dispatch({ type: "incremented_age" });
      }
    
      function handleInputChange(e) {
        dispatch({
          type: "changed_name",
          nextName: e.target.value,
        });
      }
    
      return (
        <>
          <input value={state.name} onChange={handleInputChange} />
          <button onClick={handleButtonClick}>Increment age</button>
          <p>
            Hello, {state.name}. You are {state.age}.
          </p>
        </>
      );
    }
  ```
