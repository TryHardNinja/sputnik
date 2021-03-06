# Stylus

## Синтаксис в Stylus

* Используйте вложенность с `&` ([амперсанд](https://ru.wikipedia.org/wiki/Амперсанд)), так будет видна зависимость и иерархическое дерево классов.

  ```stylus
  // Плохо
  .project { /* ... */ }
  .project__name { /* ... */ }
  .project__description { /* ... */ }

  // Хорошо
  .project
    // ...

    &__name
      // ...

    &__description
      // ...
  ```

* Используйте табы для отступов, не используйте табы и пробелы одновременно - стили не скомпилируются.<br>
  В этом примере `∙∙∙∙` - четыре пробела, а `――――` - один отступ с табуляцией.

  ```stylus
  // Плохо
  .project
  ∙∙∙∙// ...

  ∙∙∙∙&__name
  ∙∙∙∙∙∙∙∙// ...

  // Хорошо
  .project
  ――――// ...

  ――――&__name
  —―――――――// ...
  ```

* Между классами с группой свойств добавляте перенос строки для лучшей читабельности.

  ```stylus
  // Плохо
  .project
    // ...
    &__name
      // ...
      &:before
        // ...

  // Хорошо
  .project
    // ...

    &__name
      // ...

      &:before
        // ...
  ```

* Опускайте использование фигурных скобок `{}`, двоеточий `:` и точек с запятыми `;`, в Stylus можно обходиться без них.

  ```stylus
  // Плохо
  .button {
    position: relative;
    font-size: 14px;
    line-height: 1;
    background-color: #08f;
  }

  // Хорошо
  .button
    position relative
    font-size 14px
    line-height 1
    background-color #08f
  ```

* Не используйте `&-` для описания имен составных блоков. Это затрудняет их поиск.

  ```stylus
  // Плохо
  .project
    // ...

    &-container
      // ...

    &__name
      // ...

      &:before
        // ...

  // Хорошо
  .project
    // ...

    &__name
      // ...

      &:before
        // ...

  .project-container
    // ...
  ```

* Один файл — один блок. Имя файла совпадает с именем блока. Все стили для блока должны быть описаны в этом файле. Блок не должен встречаться в других файлах.

  ```stylus
  // Плохо
  // main.styl
  .project
    // ...

    &__name
      // ...

    &__before
      // ...

  .project-container
    // ...

  .portfolio
    // ...
  ```

// Хорошо
// project.styl
.project
// ...

    &__name
      // ...

    &__before
      // ...

// project-container.styl
.project-container
// ...

// portfolio.styl
.portfolio
// ...

````
- [Rupture](https://github.com/jenius/rupture) - примеси содержат в себе только блок свойств, селекторов внутри быть не должно.

```stylus
// Плохо
.block
  +below(666px)
    &:first-child
      margin-top: 5px

// Хорошо
.block
  &:first-child
    +below(666px)
      margin-top: 5px
````

* Исходя из всех предыдущих получаем следующую иерархию и последовательность

  ```stylus
  // Блок
  .block
    // ...

    // @media-примеси блока
    +below(640px)
      // ...

    // Псевдоэлементы блока
    &::after
      // ...

    // Псевдоклассы блока
    &:first-child
      // ...

      // Псевдоэлементы с псевдоклассом блока
      &::after
        // ...

    // Модификаторы блока
    &_key_val
      // ...

      // Псевдоклассы модификатора блока
      &:first-child
        // ...

      // Псевдоэлементы модификатора блока
      &::after
        // ...

    // Элементы
    &__element
      // ...

      // @media-примеси элемента
      +below(640px)
        // ...

      // Псевдоэлементы элемента
      &::after
        // ...

      // Псевдоклассы элемента
      &:first-child
        // ...

        // Псевдоэлементы с псевдоклассом элемента
        &::after
          // ...

      // Модификаторы элемента
      &_key_val
        // ...

        // Псевдоклассы модификатора элемента
        &:first-child
          // ...

        // Псевдоэлементы модификатора элемента
        &::after
          // ...

    // Псевдоклассы блока, влияющие на элементы
    &:first-child &__element
      // ...

    // Модификаторы блока, влияющие на элементы
    &_key_val &__element
      // ...

    // N элементов ещё...
  ```

* Используйте примеси (mixins) для частоповторяющихся участков кода.
* Используйте циклы для однотипных строк с различием в значениях.

Полезные ссылки:

* [Документация Stylus](http://stylus-lang.com/).
* [CSS2Stylus](http://css2stylus.com/) - онлайн препроцессор CSS в Stylus. Для табов в options выбрать Indentation - tab.

## Переменные

При частой записи одиноковых значений следует использовать переменные:

* Название шрифтов.
* Фирменные цвета.
* Ресурсы в `data-uri`.

## Возможные проблемы и пути их решения

* Когда комментируются свойства, нужно комментировать ещё и селектор, иначе он будет брать свойства вместе со следующим селектором или, если следующего селектора нет, компилятор выдаст ошибку.

  ```stylus
  // Плохо
  .block
      // ...

      &__item
          // color #08f

  // Хорошо
  .block
      // ...

      // &__item
      //    color #08f
  ```
