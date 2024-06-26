# useCallback

- [ ] `useCallback` - это хук React, который позволяет кэшировать определение `функции` `между повторными рендерами`. Кэширует ф-ии, передающиеся в дочерние компоненты.

[ДИАГРАММА ПО useCallback](https://app.diagrams.net/#G1ri5uBpU7P8L-n_jeDHoyr02Q99Mzml2G#%7B%22pageId%22%3A%22VT9J4h6Pbm1LJgYql30u%22%7D)

<br>

![image](https://github.com/acidshotgun/react-hooks-new/assets/117285472/e8a8228d-7c10-48ae-b62f-a63e71fd2dbe)

<hr>
<br>
<br>

<h2>Пример / использование</h2>

- [ ] Ререндер компонента - вызывает пересоздание всего тела компонента + дочерние.
- [ ] Мы можем иметь ф-ю, которая передается ниже дочерним компонентам, которая будет пересоздаваться каждый ререндер.

- [x] Чтобы ф-я не пересоздавалась каждый ререндер ее можно обернуть в хук `useCallback()`

  ```typescript
    // vlad10 task
    const updateTrainChar = useCallback(
      // Наша ф-я
      (charIndex: number, charName: any, value: number) => {
        dispatch(
          updateCharacteristics({
            charIndex: charIndex,
            charName: charName,
            value: +value,
          })
        );
      },
      // Пустая зависимость - реагирует только на первый рендер компонента
      // ТЕ один раз срабатывает (как у useEffect())
      []
    );
  ``` 

<hr>
<br>
<br>

<h2>react.memo</h2>

- [ ] Если компонент обёрнутый в `memo` принимает ф-ю как пропс то нужно иметь ввиду:

  + ф-я пересоздается каждый ререндер родителя === каждый раз новая ф-я === `memo` не работает.
  + Чтобы `memo` работал - эта ф-я должна быть обёрнута в `useCallback` === ф-я одна и та же === `memo` работает.
