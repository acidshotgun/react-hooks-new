# createPortal

- [ ] `createPortal` - позволяет рендерить дочерние элементы в разные части `DOM-дерева`. Т.е. вне иерархии компонентов.

- [x] Например:

  + `Модальные окна`.
  + `Всплывающие уведомления`.
     
<hr>
<br>
<br>

<h2>Пример</h2>

- [ ] Чтобы создать `портал` - нужно вызвать метод `createPortal` и передать два аргумента:

  + `Компонент / элемент`, который должен быть отрендерен.
  + `domNode` - место в `DOM`, где будет отрендерен компонент через портал (например `document.body`).
  + `key` - опционально.
     
```javascript
  <div>
    <SomeComponent />
    {createPortal(children, domNode, key?)}
  </div>
```

<br>
<br>

- [x] ПРИМ. `Portal` - меняет только физическое в узла `DOM`
- [x] Компонент внутри портала - это по прежнему `JSX` - элемент и будет иметь доступ к контексту стейтам и тд.
- [x] Грубо говоря - он внутри реакта но телепортируется в `DOM`/

<hr>
<br>
<br>

<h2>Использование:</h2>

  <h3>Рендер в другой части DOM</h3>

  [БАЗОВЫЙ ПРИМЕР РЕНДЕРА В ДРУГОЙ ЧАСТИ DOM ДЕРЕВА ПРИ ПОМОЩИ ПОРТАЛА](https://codesandbox.io/p/sandbox/react-dev-forked-ctczn5?file=%2Fsrc%2FApp.js)

  + Есть два компонента. Один рендерится внутри `clipping-container`, а второй внутри второго.
  + `<NoPortalExample  />` будет отрендерен внутри род блока.
  + `<PortalExample />` с точки зрения `JSX` будет находится внутри дерева, но рендерится будит внутри `document.body`

  <br>

  - [x] Эти рендеры можно увидеть в консоли. (Портал рендерит в `document.body` новый элемент / или удаляет).

  <br>

  + Сам же портал - будет находится внутри компонента `PortalExample` отрисовываться на основе стейта `showModal`
  + ПС. Находится внутри компонента, но рендерится в другом `DOM` узле. Те в корне `DOM - дерева` (`document.body`).

  ```javascript
    export default function PortalExample() {
      const [showModal, setShowModal] = useState(false);
      return (
        <>
          <button onClick={() => setShowModal(true)}>
            Show modal using a portal
          </button>
          {showModal && createPortal(
            <ModalContent onClose={() => setShowModal(false)} />,
            document.body
          )}
        </>
      );
    }
  ```

<hr>
<br>
<br>

<h2>Альтернативный варик:</h2>

- [ ] Можно сделать отдельный компонент портала, который будет принимать в себя `children` и рендерить нужный компонент через портал.

```javascript
  // Отдельный компонент для портала принимает два аргумента 
  //  1) child - ребенок(то что мы хотим отрендерить отрендерит элементы которые будут переданы внутрь компонента (как props.children)). 
  //  2) Контейнер, куда мы помещаем child, и чаще всего для них создаются отдельные элементы при помощи DOM - дерева.
  // Эти элементы (контейнеры) уже помещаются на страницу в body.
  const Portal = (props) => {
      const node = document.createElement('div');
      document.body.appendChild(node);
  
      return ReactDOM.createPortal(props.children, node)
  }
```

- [ ] Затем в рендерим сам портал, с переданным в него компонентом.
- [x] Плюс можно добавить стейт и кнопку для открытия/закрытия. 

<hr>
<br>
<br>
