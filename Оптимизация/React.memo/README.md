# React.memo

- [ ] **`React.memo`** - Это `компонент высшего порядка (HOC)` - позволяет пропустить `повторный рендеринг компонента`, если его пропсы `не изменись`.

[ДИАГРАММА ПО React.memo](https://app.diagrams.net/#G1p2gz0WijFbZoPXcUkoU722JXsSJXRF-2#%7B%22pageId%22%3A%22VT9J4h6Pbm1LJgYql30u%22%7D)

<br>

+ Это отличает его от обычного компонента, который перерисовывается всегда, когда перерисовывается родительский компонент.

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/85917791-0e11-4e14-ad55-30240865c5a8)

<br>

- [x] Суть:

  + Мы имеем родительский компонент, который может часто производить ререндер.
  + Этот компонент имеет дочерние компоненты, которые будут ререндерится вместе с ним.
  + Если вниз передаются пропсы и они меняются то ок. Но что если пропсы не меняются? Зачем тогда вызывать доп рендеры для дочерних компонентов?

  <br>

  + В этом случае мы можем обернуть эти дочерние компоненты в `React.memo`.
  + Эти компоненты будут ререндерится `ТОЛЬКО` когда `меняются передающиеся в них пропсы`.

<hr>
<br>
<br>

<h2>Использование React.memo</h2>

- [ ] `React.memo` - принимает компонент и возвращает модифицированную версию компонента.

- [ ] `React.memo` - можно использовать двумя способоами.

<br>

  + С использованием дефолтного алгоритма сравнения. (работает по умолчанию).
  + Обычно его достаточно и работает он превосходно.
    
  ```typescript
    const UserCard = memo(({ name, email, phone, avatar }) => {
      return (
        <div clasName="user-card">
          <img src={avatar} alt={name} />
          <h2>{name}</h2>
          <p>{email}</p>
          <p>{phone}</p>
        </div>
      );
    });
  ```

  <br>

  + С использованием кастомного (своего) алгоритма сравнения.
     
  ```typescript
    const areEqual = (prevProps, nextProps) => {
      // Сравнение пропсов
    }
    
    const UserCard = memo(({ name, email, phone, avatar }) => {
      return (
        <div clasName="user-card">
          <img src={avatar} alt={name} />
          <h2>{name}</h2>
          <p>{email}</p>
          <p>{phone}</p>
        </div>
      );
    }, areEqual);
  ```

<hr>
<br>
<br>

<h2>Простой пример</h2>

- [ ]  `React` сравнивает каждый реквизит в вашем компоненте с его предыдущим значением с помощью сравнения `Object.is`. Обратите внимание, что `Object.is(3, 3) - true`, а `Object.is({}, {}) - false`.

[ПРИМЕР МЕМОИЗАЦИИ КОМПОНЕНТОВ](https://codesandbox.io/p/sandbox/react-memo-pjjgs6?file=%2Fsrc%2FComponent.tsx%3A14%2C2)

  + Компонент `Component` мемоизирован и следит за пропсом `componentCounter`(это в стейте лежит в App).
  + Ререндерится компонент `Component` будет только в том случае, если изменяется состояние `componentCounter`
  + Если изменяется состояние `counter` в App - то только `App`, и `User` произведут ререндер.
  + Если изменяется состояние `componentCounter` в App - то всё, в том числе и `Component` произведет ререндер.

  <br>

  - [ ] Итог:

    + Состояние `counter` меняется - ререндер только `App` и `User`
    + Состояние `componentCounter` меняется - ререндер только всего, в том числе и `Component`, тк `memo` следит за этим состоянием.
       
<hr>
<br>
<br>

<h2>Передача объекта как пропса</h2>

- [ ] Тк каждый ререндер - компонент создается заново - со всем содержимым => объекты и ф-ии будут создаваться по новой. Мы можем это оптимизировать.

<br>

- [ ] Для `object` или `array` в кач-ве пропсов - `React` использоует поверхностное сравнение - [shallowEqual](https://github.com/facebook/react/blob/main/packages/shared/shallowEqual.js).
- [ ] `shallowEqual` - Поверхностно сравнивает объекты пропсов `prevProp` и `nextProp` и в случае если они не равны вызывает ререндер.
- [x] Для простоты и избежания проблем лучше передавать отдельные св-ва объекта (например деструктуризацией), чем объект целиком. 

[ПРИМЕР С ПЕРЕДАЧЕЙ ОБЪЕКТА КАК ПРОПСА](https://codesandbox.io/p/sandbox/react-memo-object-prop-5ldxfp?file=%2Fsrc%2FApp.tsx)

  + В качестве состояния в `App` имеем объект `user` у которого мы изменяем св-во `age`.
  + Внутри `Component` принимаем эти пропсы и оборачиваем в `memo`, который следит за пропсами. (тут это объект)
  + Новый объект в пропс будет сравниваться со старым по принципу `shallowEqual` - `поверхностное сравнение`.
  + Разница есть - ререндер. Разницы нет - нет ререндера.

  <br>

  + ДОП => можно создавать новый объект, обернув его в `useMemo` - тогда создание объекта будет как выражение и оно будет мемоизировано.
  ```typescript
    const newObj = useMemo(() => ({ ...user }), [user]);
  ```

  <br>
  <br>

  <h3>+ Лучшая практика с объектами</h3>

  - [ ] Если в кач-ве пропса `memo` компонент принимает объект он сравнивает их поверхностно.
  - [x] Если мы имеем глубокий объект, то лучше всего в кач-ве пропсов передавать отдельные св-ва этого объекта. (деструктуризированные).
  - [x] Таким образом сравнение будет не как `shallowEqual` а по принципу `prevProp` и `nextProp`.

  <br>

  + Вот пример передачи не всего объекта а отдельных св-в.
  + В таком случае сравнение будет как `prevProp` и `nextProp`

  ```typescript
    const TestComponent = () => {
      const [user, setUser] = useState({
        name: "Debil",
        surname: "Asshole",
        age: 18,
      });

      // Например так
      // или можно просто передать св-ва без деструктуризации
      const { name, surname, age } = user;
    
      return (
        <>
          <MemoComponent prop1={name} prop2={surname} prop3={age} />
        </>
      );
    };
  ```

<hr>
<br>
<br>

<h2>Передача функции как пропса</h2>

- [ ] Ф-ии в JavaScript - это тоже объекты. С ререндером компонента - они всегда пересоздаются так же как и объекты. Соотв.
- [ ] поэтому `memo` - будет всегда принимать одну и ту же ф-ю как новую и поэтому срабатывать не будет. Это можно оптимизировать.

<br>

- [ ] Чтобы ф-я каждый раз не создавалась по новой - ее следует обернуть в `useCallback()`.
- [ ] Обернув ф-ю в `useCallback()` - `memo` будет видеть - пересоздалась ли ф-я или нет. И в зависимости от этого перерисовывать компонент.

[ПРИМЕР С ПЕРЕДАЧЕЙ ФУНКЦИИ КАК ПРОПСА](https://codesandbox.io/p/sandbox/react-memo-function-prop-yw2cjs?file=%2Fsrc%2FApp.tsx)

  + Ф-я `handleClick`, обёрнутая в `useCallback()` - не будет персоздаваться при каждом ререндере `App`
  + `MyComponent`, обернутый в `memo` и следящий за ф-й, как за пропсом не будет перерисован, тк ф-я не будет пересоздана (изменятья).

  <br>

  + ДОП => если `handleClick` не обернут в `useCallback()` - при рендере ф-я пересоздается каждый раз.

<hr>
<br>
<br>

<h2>Минимизация изменений пропсов </h2>

- [x] Когда вы используете `memo`, ваш компонент перерисовывается всякий раз, когда какой-либо реквизит неглубоко равен тому, что было раньше.
- [ ] Это означает, что `React` сравнивает каждый реквизит в вашем компоненте с его предыдущим значением с помощью сравнения `Object.is`. Обратите внимание, что `Object.is(3, 3) - true`, а `Object.is({}, {}) - false`.
- [ ] Аналогично происходит для функций, которые пересоздаются каждый раз заново.
- [ ] ЕЩЕ. Происходит поверхностное сравнение объектов, которые не равны друг-другу ==> если не равны будет ререндер.

<br>

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/63d1fccc-baea-4382-b11d-43b27b2ed601)

<br>

<h3>Решение с useMemo</h3>

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/522a7cf2-6731-467b-8630-59fc35ade7ce)

- [ ] Если дочерний компонент, обернутый в `memo`, будет следить за объектом / массивом, которые выступают в качестве пропса => `memo` так же не будет работать.
- [ ]  Чтобы извлечь максимальную пользу из `memo`, минимизируйте время изменения реквизита. Например, если реквизит является объектом, запретите родительскому компоненту каждый раз заново создавать этот объект с помощью `useMemo`:

  [РЕШЕНИЕ С useMemo](https://codesandbox.io/p/sandbox/react-memo-troubles-nq65wl?file=%2Fsrc%2FApp.tsx)

  + В примере вместо объекта в дочерний компонент мы передаем `мемоизированное вычисление` в которое поместили объект в `useMemo`.
  + Либо дочерник компонентам отдаем пропсы по отдельности (не как объект).
     
<br>

<h3>Решение с useCallback</h3>

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/8f284141-30e3-4f9e-a183-bd69d75e4089)


- [ ] Если дочерний компонент, обернутый в `memo`, будет следить за ф-ей, которая выступает в качестве пропса => `memo` так же не будет работать.
- [ ] Причина => функция пересоздается, по принципу - новая ссылка не равна предыдущей.

  [РЕШЕНИЕ С useCallback](https://codesandbox.io/p/sandbox/react-memo-trouble-callback-yw2cjs?file=%2Fsrc%2FApp.tsx%3A15%2C18)

  + В примере в качестве пропса передается ф-я в дочерний компонент `MyComponent`, что приводит к ее пересозданию при изменении стейта `toggle`.
  + Пересоздание приводит к изменению пропса => дочерний компонент ререндерится
  + Оборачивая в `useCallback` ф-ю, она не будет пересоздана, если зависимости не изменяются `(тут это toggle)` 
