% Конструкция `match`

Простого `if`/`else` часто недостаточно, потому что нужно проверить больше, чем
два возможных варианта. Да и к тому же условия в `else` часто становятся очень
сложными. Как же решить эту проблему?

В Rust есть ключевое слово `match`, позволяющее заменить группы операторов
`if`/`else` чем-то более удобным. Смотрите:

```rust
let x = 5;

match x {
    1 => println!("один"),
    2 => println!("два"),
    3 => println!("три"),
    4 => println!("четыре"),
    5 => println!("пять"),
    _ => println!("что-то ещё"),
}
```

`match` принимает выражение и выбирает одну из ветвей исполнения согласно его
значению. Каждая *ветвь* имеет форму `значение => выражение`. Выражение ветви
вычисляется, когда значение данной ветви совпадает со значением, принятым
оператором `match` (в данном случае, `x`). Эта конструкция называется `match`
(сопоставление), потому что она выполняет сопоставление значения неким
«шаблонам». Глава «[Шаблоны][patterns]» описывает все шаблоны, которые можно
использовать в `match`.

[patterns]: patterns.html

Так в чём же преимущества данной конструкции? Их несколько. Во-первых, ветви
`match` *проверяются на полноту*. Видите последнюю ветвь, со знаком
подчёркивания (`_`)? Если мы удалим её, Rust выдаст ошибку:

```text
error: non-exhaustive patterns: `_` not covered
```

Другими словами, компилятор сообщает нам, что мы забыли сопоставить какие-то
значения. Поскольку `x` — это целое число, оно может принимать разные значения —
например, `6`. Однако, если мы убираем ветвь `_`, ни одна ветвь не совпадёт,
поэтому такой код не скомпилируется. `_` — это «совпадение с любым значением».
Если ни одна другая ветвь не совпала, совпадёт ветвь с `_`. Поскольку в примере
выше есть ветвь с `_`, мы покрываем всё множество значений `x`, и наша программа
скомпилируется.

`match` также является выражением. Это значит, что мы можем использовать его в
правой части оператора `let` или непосредственно как выражение:

```rust
let x = 5;

let numer = match x {
    1 => "one",
    2 => "two",
    3 => "three",
    4 => "four",
    5 => "five",
    _ => "something else",
};
```

Иногда с помощью `match` можно удобно преобразовать значения одного типа в
другой.

# Сопоставление с образцом для перечислений

Другой полезный способ использования `match` — обработка возможных вариантов
перечисления:

```rust
enum Message {
    Quit,
    ChangeColor(i32, i32, i32),
    Move { x: i32, y: i32 },
    Write(String),
}

fn quit() { /* ... */ }
fn change_color(r: i32, g: i32, b: i32) { /* ... */ }
fn move_cursor(x: i32, y: i32) { /* ... */ }

fn process_message(msg: Message) {
    match msg {
        Message::Quit => quit(),
        Message::ChangeColor(r, g, b) => change_color(r, g, b),
        Message::Move { x: x, y: y } => move_cursor(x, y),
        Message::Write(s) => println!("{}", s),
    };
}
```

Как обычно, компилятор Rust проверяет полноту, поэтому в `match` должна быть
ветвь для каждого варианта перечисления. Если какой-то вариант отсутствует,
программа не скомпилируется и вам придётся использовать `_`.

Здесь мы не можем использовать обычный `if` вместо `match`, в отличие от кода,
который мы видели раньше. Но мы могли бы использовать [`if let`][if-let] — его
можно воспринимать как сокращённую форму записи `match`.

[if-let]: if-let.html