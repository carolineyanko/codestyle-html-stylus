# Кодстайл HTML


### Синтаксис

* Відступи є тільки табами.
* Вкладенні елементи мають відступ 1 таб.
* Після закриваючого тегу не повинно бути пробілів або табів.
* Назви елементів в нижньому реєстрі.
* В атрибутах використовуємо ``""``, а не ``''``.
* Не додаємо слеш в кінці одиночих елементів ([спека](http://dev.w3.org/html5/spec-author-view/syntax.html#syntax-start-tag)).


### Doctype

`<!DOCTYPE html>` і більше нічого не треба знати.

### Кодування символів

`<meta charset="utf-8">`

### Підключення CSS і JavaScript

При підключенні CSS та JavaScript не треба вказувати ``type``.
Згідно зі спекою HTML5 вони не обов’язкові.

### Порядок атрибутів

* `class`
* `id`
* `src`, `href`
* `name`, `for`, `type`
* `title`, `alt`
* `ng-*`, `data-*`

### Кастомні атрибути (Angular, etc)

Переносимо кастомні атрибути (якщо їх більше одного) на наступний рядок. З одним табом відносно елементу.

Без переносу:
```
<ul ng-if="$scope.show">
  <li ng-repeat="item in $scope.children" ng-click="$scope.hello(item.name)" ng-class="{'active': item.active}">
    {item.name}
  </li>
</ul>
```

З переносом:
```
<ul ng-if="$scope.show">
  <li
    ng-repeat="item in $scope.children"
    ng-click="$scope.hello(item.name)"
    ng-class="{'active': item.active}">
    {item.name}
  </li>
</ul>
```

Questions?


# Кодстайл Stylus
>http://learnboost.github.io/stylus/


### Синтаксис

* Використовуємо тільки табами.
* Відступи (коли мало тексту, не робимо.):
  * Між групами стилів та мутаторами, псевдо-елементами і @media — *1 рядок*.
  * Елементами в блоці — *2 рядка*.
  * Блоками — *4 рядка*.
* Не використовуємо `{}`, `:`, `;`.
* Для дробних чисел не додаємо нуль (наприклад, `.5` замість `0.5`).
* Опускаємо одиниці вичіслювання при нульовому значенні (наприклад`, margin 0` замість `margin 0px`).

### Порядок оголошення

Групуються в наступному порядку:

  1. Позиціювання
  2. Блочна модель
  3. Відображення(типографіка, кольора)
  4. Інше(animation, opacity)

Приклад:

```
.block

  // Позиціювання
  position absolute
  top 0
  right 0
  z-index 10

  // Блочна модель
  content " "
  display block
  float left
  vertical-align top
  box-sizing border-box
  overflow hidden
  width 100px
  height 100px
  margin 10px
  padding 10px 20px
  border 10px solid #333

  // Відображення
  background red
  color white
  font bold 15px/1.2 "Helvetica"
  text-align left
  line-height 1.2
  white-space nowrap

  // Інше
  cursor pointer
  opacity .2
  transition/animation

  &:before, &:after

  // Media Query
  @media

  // Modificators, state & children
  &_modificator

  &.state


  &__children

```

### Місце media query

Розміщуємо media queries в правилі, для яких ми їх використовуєш. Не створюуй окремого файлу.

Ось типова структура:
```
.block
  color #000
  @media (max-width: 1000px)
    color #fff
```

Конвертує в:
```
.block {
  color: #000;
}
@media(max-width: 100px) {
  .block {
    color: #fff;
  }
}
```

### Властивості з префіксами

Для всього є [Autprefixer](https://github.com/postcss/autoprefixer) на міддлварі.

### Скорочений запис

Якщо використовується більше 2-х параметрів (три відступи, наприклад), пишемо [скорочено](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties): `margin 10px 10px 0 5px`.
Якщо меньше, то:
```
margin-top 10px
margin-right 2px
```



# Використовуємо легкий BEM
>https://en.bem.info/

```
.block // Блок, презентує найвищий рівень абстракції компонентів

  &_modificator // Модифікацію(колір, шрифт, etc).
  &.state // Стейт(активний, etc).
  &-football // Складовий клас.


  &__element // Презентує «дитину» блоку. В сокупності всі діти і є блок.

    &_modificator
    &.state

```

### Базове

* Використовуємо BEM для іменування класів.
* Записуємо імена класів строковими літерами.
* Не приписуємо елемент класу, наприклад `div.block`.
* Клас самостійного компоненту `.block`.
* Клас дочірнього елементу `.block__element`, записується за допомогою `__`.
* Клас вкладених елементів, які є достатньо важкі, щоб бути окремим блоком, але мають пряме відношення до батька(footer-menu в footer, наприклад)  — `.block-elemblock__element`. Розділяємо через `-`.


### Мутатори

##### Модифікатори
Модифікатори відповідають за візуальну мутацію блоку або елементу (кольори, типографіка, etc). Пишуться через `_`.

```
<button class="btn btn_red"></button>
```
```
.btn
  &_red
    color red
```

##### Складові класи
Складові класи пишуться через `-`. Використовуються тільки для блоків, які фундаментально змінюються через одну властивість(в іконках background-image, наприклад).

```
<i class="icon icon-ball"></i>
```
```
.icons
  width 20px
  height 20px

  &-ball
    background-image url("icon-ball.png")
  &-basketball
    background-image url("icon-bacgkround.png")
```

##### Стейти
Стейти пишуться через `.` (активний стан елемента меню, наприклад). Зазвичай динамічні.

```
<li class="menu__item active"></li>
```
```
.menu
  &__item
    color white

    &.active
      color pink
      font-weight bold
```

### Домішки та вкладенності
Використовуємо для мутації компоненту, в залежності від контексту, в якому він лежить ([React.js](https://facebook.github.io/react/), привіт!).
Для позіцюювання блоку можна використовувати як домішки, так і вкладенність.

##### Домішки
Домішки зручні для позіціювання(абсолютного, наприклад) чогось, що має фіксовану ширину.

```
<button type="button" class="btn btn_red header__profile"></buttton>
```
```
.header
  &__profile
    position absolute
    right 10px
    top 10px
```

##### Вкладенності
Вкладенності зручні для модифікації розмірів одного або декількох блоків, що лежать поруч.
Ще зручно використовувати для обгортки над блоком, що має 100% ширину(інпути, наприклад).

```
<div class="form-group">
  <div class="input">
    <input type="text">
  </div>
  <div class="input">
    <input type="text">
  </div>
</div>
```

```
.form-group
  .input
    width 50%
```


# Інше

### Архітектура Stylus-проекту

```
styles/
├── base/               # Базові стилі(імпортимо в вказаному порядку)
│   ├── typography.styl # Шрифти
│   ├── normalize.styl  # Скидування стилів браузера
│   ├── colors.styl     # Кольори проекту
│   ├── forms.styl      # Форми, інпути, селекти, etc
│   ├── buttons.styl    # Кнопки
│   ├── icons.styl      # Іконки
│   ├── main.styl       # Стилі для body, html, *
├── blocks/             # Блоки(імпортимо просто: @import "blocks/*")
└── common.styl         # Імпортимо стилі
```

### Налаштування редактору

* Інсталніть [Editorconfig](editorconfig.org).
* Таби.
* Стрімимо пробіли в кінці рядків.
* Залишаємо пустий рядок в кінці файлу.
