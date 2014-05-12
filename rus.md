# Breaking Bad (Циклы в библиотеках JavaScript)

Я был достаточно удивлён, когда обнаружил несовместимости в наиболее
популярных библиотеках JavaScript в том, как они обрабатывают циклы each
и forEach.
В этой статье сравниваются:

- цикл forEach, встроенный в JavaScript;
- цикл each в Lo-Dash;
- цикл each в jQuery;
- цикл each в Underscore.js;
- цикл forEach в Underscore.js.

## Цикл forEach, встроенный в JavaScript

Библиотеки JavaScript важны (напр., jQuery, Lo-Dash, Underscore), но в случае
функциональных циклов (`forEach` and `each`) они становятся причиной немалой
путаницы (цикл for можно прервать при помощи `break`).
Рассмотрим пример применения нативного для javaScript метода `forEach`:

    [1,2].forEach(function(v){
      alert(v);
      return false;
    })

Этот код покажет два окошка «alert». Попробуйте сами на [JSFiddle][1].

Это поведение ожидаемо в большинстве случаев, потому как на каждой итерации
мы вызываем функцию заново. В отличие от кода
`for (var i=0; i<arr.length; i++) {}`, где нет никаких функций и итераторов.

Однако, в Lo-Dash и jQuery похожий код разрывает цикл!

## Прерываемый цикл each в Lo-Dash

При использовании Lo-Dash такой код с `each` выведет только **один alert**:

    _.each([1,2],function(v){
      alert(v);
      return false;
    })

Попробуйте сами запустить этот код на [JSFiddle][2].

## Прерываемый цикл each в jQuery

У jQuery похожее поведение, такой код вызовет только первый alert:

    $.each([1,2],function(i, v){
      alert(v);
      return false;
    })

Попробуйте на [JSFiddle][3].

## Неразрывный цикл each в Underscore.js

Чтобы всё усложнить, Underscore.js и Backbone.js остаются верными духу
нативного `forEach` в JavaScript.

В Underscore.js `each` проходит по всем элементам и **не прерывается**:

    _.each([1,2],function(v){
      alert(v);
      return false;
    })

Попробуйте сами на [JSFiddle][4].

## Неразрывный цикл forEach в Underscore.js

Просто ради приличия я проверил также `forEach()` в underscore. Он, ожидаемо,
ведёт себя так же, как нативный `forEach()`: **два alert!**

Код с применением `forEach()` библиотеки Underscore:

    _.forEach([1,2],function(i, v){
      alert(v);
      return false;
    })

Проверьте сами на [JSFiddle][5].

## Разница между Lo-Dash и Underscore, которая может разломать ваш код

Итог этой короткой статьи: **Lo-Dash не то же самое, что Underscore**,
при условии, что не используется особая версия, совместимая с Underscore.
Спасибо Джону-Дэвиду Далтону (@jdalton), что обратил на это моё внимание:

> [@azat_co][6] Всё верно, Lo-Dash не эквивалентная замена, если не
> использовать билд Lo-Dash.underscore.js (на него есть ссылка на главной).
> То же и в cdnjs. — Джон-Дэвид Далтон (@jdalton) [22 ноября 2013][7]

P.S.: `forEach` в Underscore.js более совместим с браузерами, чем нативный
`forEach`, потому что последний был относительно недавно добавлен в API
JavaScript и [не поддерживается более старыми браузерами][8].

 [1]: http://jsfiddle.net/MMbrR/
 [2]: http://jsfiddle.net/x65jp/2/
 [3]: http://jsfiddle.net/x65jp/3/
 [4]: http://jsfiddle.net/x65jp/1/
 [5]: http://jsfiddle.net/x65jp/4/
 [6]: https://twitter.com/azat_co
 [7]: https://twitter.com/jdalton/statuses/403993905575641088

 [8]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#Browser_compatibility