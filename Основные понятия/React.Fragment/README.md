# Fragment (<>...</>)

- [ ] `<Fragment>`, часто используется как `<>...</>` - компонент, позволяющий групировать элементы без `wrapper node`.

  + По-умолчанию все элементы должны быть обёрнуты в компонент или ноду.
     
  ```javascript
  <>
    <OneChild />
    <AnotherChild />
  </>
  ```

  <hr>
  <br>
  <br>

  <h2>Пример</h2>

- [x] `Fragment`
   
  + Оборачивать элементы можно в ситациях, когда нужен один элемент, без кучи обёрток.
  + Групировка элементов во `Fragment` не влияет на результат в `DOM` => `Fragment` будет проигнорирован в итоговом `DOM`.

<br>

- [x] Props

  + Опциональный `key` - При использовании полного `<Fragment></Fragment>` можно опционально указывать `key`.
  + В синтаксисе `<>...</>` - `key` не работает. Только в полном.
     
  ```javascript
    <Fragment key={yourKey}>...</Fragment>.
  ```

    <hr>
  <br>
  <br>

  <h2>Использование</h2>

  <h3>+ Возврат нескольких элементов. </h3>

  + С помощью `Fragment` можно объеденить и поместить несколько элементов, там где элемент ожидается один.
     
  ```javascript
  function Post() {
    return (
      <>
        <PostTitle />
        <PostBody />
      </>
    );
  }
  ```

  + Польза - `Fragment` не влияет на стили.
     
  <br>
  <br>

  <h3>+ Присваивание переменной нескольких элементов</h3>

  + Можно обернуть компоненты во `Fragment` и присвоить перемменной.
  + Затем отрисовать или передать как `props`.

  ```javascript
  function CloseDialog() {
    const buttons = (
      <>
        <OKButton />
        <CancelButton />
      </>
    );
    return (
      <AlertDialog buttons={buttons}>
        Are you sure you want to leave this page?
      </AlertDialog>
    );
  }
  ```

    <br>
  <br>

  <h3>+ Рендер списка Fragments</h3>

  + Пример использования `Fragment` с `key`.
  + Дочерние `Fragment` будут без обёртки в `DOM`.

  ```javascript
  function Blog() {
    return posts.map(post =>
      <Fragment key={post.id}>
        <PostTitle title={post.title} />
        <PostBody body={post.body} />
      </Fragment>
    );
  }
  ```
