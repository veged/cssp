## Предварительные требования:

* Node.js: http://nodejs.org
* npm: https://github.com/isaacs/npm

##Установка, удаление и обновление:

Установка: `npm install cssp`

Обновление: `npm update cssp`

Удаление: `npm uninstall cssp`

## Описание

По умолчанию CSSP разбирает входной CSS-текст в дерево (parser -- P), затем отправляет дерево на трансформацию (transformer -- TF), после чего транслирует в CSS-текст (translator -- TL).

Таким образом полный цикл выглядит как CSS -> P -> TF -> TL -> CSS, и без указания ключей CSSP отдаст тот же текст, что был на входе.

Дополнительный элемент CMP — сжатие CSS: убирает из дерева лишние пробельные символы, комментарии.

## Использование

Использование command line интерфейса:

    cssp
        показывает этот текст
    cssp <имя_файла>
        считывает CSS из <имя_файла> и записывает результат полного цикла (тот же CSS) в stdout
    cssp <имя_файла> -dp
    cssp <имя_файла> --parser
        считывает CSS из <имя_файла> и записывает результат CSS -> P -> stdout
    cssp <имя_файла> -df
    cssp <имя_файла> --transformer
        считывает CSS из <имя_файла> и записывает результат CSS -> P -> TF -> stdout
    cssp <имя_файла> -dc
    cssp <имя_файла> --compressor
        считывает CSS из <имя_файла> и записывает результат CSS -> P -> CMP -> stdout
    cssp <имя_файла> -dl
    cssp <имя_файла> --translator
        считывает CSS из <имя_файла> и записывает результат CSS -> P -> TF -> TL -> stdout
    cssp <имя_файла> -c <имя_файла>
    cssp <имя_файла> --compress <имя_файла>
        считывает CSS из <имя_файла> и записывает результат CSS -> P -> CMP -> TL -> stdout
    cssp <имя_файла> -r <имя_правила>
    cssp <имя_файла> --rule <имя_правила>
        считывает CSS из <имя_файла> и передаёт в цикл (P TF TL) <имя_правила>, которое надо обработать
    cssp <имя_файла> -t
    cssp <имя_файла> --trim
        считывает CSS из <имя_файла> и удаляет начальные и концевые пробельные символы

Примеры:

    1) test.css = 'color: red'
    > cssp test.css -r declaration -dp
    > ['declaration',
        ['property',
          ['ident', 'color']],
        ['value',
          ['s', ' '],
          ['ident', 'red']]]
    2) test.css = '10px'
    > cssp test.css -r dimension -dp -dl
    > ['dimension',
        ['number', '10'], 'px']
      10px

Пример программного использования (Node.js):

    var cssp = require('cssp'),
        src = 'a { color: red }',
        tree, trans,
        dst;

    tree = cssp.parse(src);
    trans = cssp.transform(tree);
    dst = cssp.translate(trans);

    console.log('Source CSS:');
    console.log(src);
    console.log('Parser out:');
    console.log(tree);
    console.log('Transformer out:');
    console.log(trans);
    console.log('Translator out:');
    console.log(dst);
