# Винятки (Exceptions)
Винятки стаються, коли у програмі виникають _виняткові_ ситуації. Наприклад, що робити програмі, коли ви намагаєтеся прочитати файл, якого не існує? Або якщо ви випадково видалили цей файл під час виконання програми? Такі ситуації вирішуються за допомогою **винятків** _(exceptions)_.

А що, якщо ви написали неправильні інструкіцї вашій програмі? За це відповідає Python, який **піднімає** _(raises)_ руки вгору і повідомляє вам, що тут є **помилка** _(error)_.


## Помилки (Errors)

Уявімо, що ми хочемо викликати функцію `print`. Що, якщо ми випадково напишемо не `print`, а `Print`? Зверніть увагу на велику літеру. У цьому випадку Python видасть синтаксичну помилку.

```python
>>> Print("Hello World")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'Print' is not defined
>>> print("Hello World")
Hello World
```

Зверніть увагу, що падає `NameError`, а також Python підказує місце, у якому ця помилка з'явилась. Це робить **обробник помилок** _(error handler)_.

## Винятки (Exceptions)

**Спробуємо** _(try)_ прочитати введені користувачем дані.  Введіть перший рядок нижче і натисніть клавішу `Enter`.  Коли ваш комп'ютер попросить вас ввести щось, натисніть `[ctrl-d]` на Mac або `[ctrl-z]` у Windows і подивіться, що станеться.  (Якщо ви використовуєте Windows і жоден з варіантів не працює, ви можете спробувати `[ctrl-c]` у командному рядку, щоб згенерувати помилку переривання клавіатури).

```python
>>> s = input('Enter something --> ')
Enter something --> Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
EOFError
```

Python видає помилку `EOFError`, яка означає, що він знайшов символ *кінець файлу* (EOF - end of file, відповідає натиснутій `ctrl-d`), хоча зовсім не очікував його побачити.

## Обробка винятків

Ми можемо ловити та обробляти винятки за допомогою інструкції `try..except`.  По суті, ми розміщуємо код для виконання у блоці `try`, а всі обробники помилок - у блоці `except`.

Приклад (збережіть у файлі `exceptions_handle.py`):

<pre><code class="lang-python">{% include "./programs/exceptions_handle.py" %}</code></pre>

Output:

<pre><code>{% include "./programs/exceptions_handle.txt" %}</code></pre>

**Як це працює**

Ми вставляємо весь код, який може видати виключення/помилки, всередину блоку `try`, а потім створюємо обробники відповідних помилок/винятків у блоці `except`. Оператор `except` може обробляти одну вказану помилку або виняток, або список помилок/винятків у круглих дужках. Якщо не вказано жодної назви помилки або винятку, буде оброблено _усі_ помилки та винятки.

Зверніть увагу, що для кожного блоку `try` має бути оператор `except`. Інакше, який сенс у блоці try?

Якщо будь-яка помилка або виняток не обробляється в коді вами, то викликається стандартний обробник Python, який просто зупиняє виконання програми в тому ж місці і виводить повідомлення про помилку. Ми вже бачили це на прикладі `NameError` вище.

Ви також можете використовувати оператор `else` після блоку `try..except`. Якщо виняток не виникне, то виконається код всередині блоку `else`.

У наступному прикладі ми побачимо, як отримати об'єкт винятку, щоб мати додаткову інформацію.

## Виклик винятків (Raising Exceptions)
Ви можете викликати виняток за допомогою оператору `raise`, вказавши ім'я помилки/винятку та об'єкт винятку, який потрібно викинути.

Помилка або виключення, яке ви можете викликати, має бути класом, який наслідує клас `Exception`.

Приклад (збережіть у файлі `exceptions_raise.py`):

<pre><code class="lang-python">{% include "./programs/exceptions_raise.py" %}</code></pre>

Output:

<pre><code>{% include "./programs/exceptions_raise.txt" %}</code></pre>

**How It Works**

Here, we are creating our own exception type. This new exception type is called `ShortInputException`. It has two fields - `length` which is the length of the given input, and `atleast` which is the minimum length that the program was expecting.

In the `except` clause, we mention the class of error which will be stored `as` the variable name to hold the corresponding error/exception object. This is analogous to parameters and arguments in a function call. Within this particular `except` clause, we use the `length` and `atleast` fields of the exception object to print an appropriate message to the user.

## Try ... Finally {#try-finally}

Suppose you are reading a file in your program. How do you ensure that the file object is closed properly whether or not an exception was raised? This can be done using the `finally` block.

Save this program as `exceptions_finally.py`:

<pre><code class="lang-python">{% include "./programs/exceptions_finally.py" %}</code></pre>

Output:

<pre><code>{% include "./programs/exceptions_finally.txt" %}</code></pre>

**How It Works**

We do the usual file-reading stuff, but we have arbitrarily introduced sleeping for 2 seconds after printing each line using the `time.sleep` function so that the program runs slowly (Python is very fast by nature). When the program is still running, press `ctrl + c` to interrupt/cancel the program.

Observe that the `KeyboardInterrupt` exception is thrown and the program quits. However, before the program exits, the finally clause is executed and the file object is always closed.

Notice that a variable assigned a value of 0 or `None` or a variable which is an empty sequence or collection is considered `False` by Python.  This is why we can use `if f:` in the code above.

Also note that we use `sys.stdout.flush()` after `print` so that it prints to the screen immediately.

## The with statement {#with}

Acquiring a resource in the `try` block and subsequently releasing the resource in the `finally` block is a common pattern. Hence, there is also a `with` statement that enables this to be done in a clean manner:

Save as `exceptions_using_with.py`:

<pre><code class="lang-python">{% include "./programs/exceptions_using_with.py" %}</code></pre>

**How It Works**

The output should be same as the previous example. The difference here is that we are using the `open` function with the `with` statement - we leave the closing of the file to be done automatically by `with open`.

What happens behind the scenes is that there is a protocol used by the `with` statement. It fetches the object returned by the `open` statement, let's call it "thefile" in this case.

It _always_ calls the `thefile.__enter__` function before starting the block of code under it and _always_ calls `thefile.__exit__` after finishing the block of code.

So the code that we would have written in a `finally` block should be taken care of automatically by the `__exit__` method. This is what helps us to avoid having to use explicit `try..finally` statements repeatedly.

More discussion on this topic is beyond scope of this book, so please refer [PEP 343](http://www.python.org/dev/peps/pep-0343/) for a comprehensive explanation.

## Summary

We have discussed the usage of the `try..except` and `try..finally` statements. We have seen how to create our own exception types and how to raise exceptions as well.

Next, we will explore the Python Standard Library.
