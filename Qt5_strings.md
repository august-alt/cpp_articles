[источник](https://zetcode.com/gui/qt5/strings/ "взято отсюда")

# Строки в Qt5 #

В этой главе мы будем работать со строками. В Qt5 есть класс для работы со строками. Он имеет массу возможностей и большое количество методов.

Класс QString обеспечивает работу со строками в Unicode. Он хранит строки как 16 битные символы типа QChar. Каждый QChar соответствует одному символу в Unicode 4.0. В отличие от строк во многих других языках пограммирования, QString может быть модифицирован.

В примерах этой главы, нам не нужно использовать графический пользовательский интерфейс Qt, мы создадим программы для командной строки. Поскольку графический интерфейс включен по умолчанию, мы может отключить его, добавив объявление QT -= gui в файл проекта.

## Базовый пример с QT5 строками ##

В первом примере, мы поработаем с несколькими методами класса QString.

>#include <QTextStream>
>
>int main(void) {
>
>   QTextStream out(stdout);
>
>   QString a { "love" };
>
>   a.append(" chess");
>   a.prepend("I ");
>
>   out << a << endl;
>   out << "The a string has " << a.count()
>       << " characters" << endl;
>
>   out << a.toUpper() << endl;
>   out << a.toLower() << endl;
>
>   return 0;
>}

В коде примера, мы инициализируем QString. Мы добавляем в конец и в начало дополнительный текст. Мы печатаем на экране длину строки. В конце, мы печатаем модифицированную строку в верхнем и нижнем регистре.

>QString a { "love" };

Инициализация QString.

>a.append(" chess");
>a.prepend("I ");

Мы добавляем в конец и в начало текст к исходной строке. Строка модицифирована в этом месте.

>out << a << endl;

'I love chess' - печать строки.

>out << "The a string has " << a.count()
>    << " characters" << endl;

Метод count() возвращает длину строки в символах. Методы length и  size эквивалентны.

>out << a.toUpper() << endl;
>out << a.toLower() << endl;

Эти два метода возвращают копии строки в верхнем и нижнем регистре. Они не модифицирут строку, они возвращают новую копию строки.

>$ ./basic
>I love chess
>The a string has 12 characters
>I LOVE CHESS
>i love chess

## Инициализация строк в Qt5 ##

Инициализация строки QString может быть выполнена друмя путями.

>#include <QTextStream>

>int main(void) {
>
>   QTextStream out(stdout);
>
>   QString str1 = "The night train";
>   out << str1 << endl;
>
>   QString str2("A yellow rose");
>   out << str2 << endl;
>
>   QString str3 {"An old falcon"};
>   out << str3 << endl;
>
>   std::string s1 = "A blue sky";
>   QString str4 = s1.c_str();
>   out << str4 << endl;
>
>   std::string s2 = "A thick fog";
>   QString str5 = QString::fromLatin1(s2.data(), s2.size());
>   out << str5 << endl;
>
>   char s3[] = "A deep forest";
>   QString str6(s3);
>   out << str6 << endl;
>
>   return 0;
>}

Мы продемонстировали 5 вариантов инициализации Qstring.

> QString str1 = "The night train";

Это традиционный путь инициализации строк в языках программирования.

> QString str2("A yellow rose");

Это объектный путь для инициализации QString.

> QString str3 {"An old falcon"};

Это инициализация с использования фигурных скобок.

>std::string s1 = "A blue sky";
>QString str4 = s1.c_str();

Мы создали объект строки с использованием стандратной библиотеки C++. Мы используем ее метод c_str для создания последовательности символов, заканчивающейся нулем. Этот массив символов классический пример строки, которая может быть использована для создания QString.

>std::string s2 = "A thick fog";
>QString str5 = QString::fromLatin1(s2.data(), s2.size());

В этих строка кода мы преобразуем стандартные строки C++ в строки QString. Мы используем fromLatin1 метод. Он принимает указатель на массив символов. Указатаель возвращает метод data. Второй параметр возвращает длину строки.

>char s3[] = "A deep forest";
>QString str6(s3);

Это строка С; она является массивом символов. Один из конструкторов QString может принимать массив символов в качестве параметра.

>$ ./init 
>The night train
>A yellow rose
>An old falcon
>A blue sky
>A thick fog
>A deep forest

## Доступ к элементам строки ##

QString является последовательностью QChars. Доступ к элементам строки может быть осуществелн с помощь оператора [] или с помощью метода.

>#include <QTextStream>
>
>int main(void) {
>
>   QTextStream out(stdout);
>
>   QString a { "Eagle" };
>
>   out << a[0] << endl;
>   out << a[4] << endl;
>
>   out << a.at(0) << endl;
>
>   if (a.at(5).isNull()) {
>     out << "Outside the range of the string" << endl;
>   }
>
>   return 0;
>}

Мы печатаем отдельные символы из указанной строки QString.

>out << a[0] << endl;
>out << a[4] << endl;

Мы печатаем первый и пятый элемент строки.

out << a.at(0) << endl;

С помощью этого метода м ыполучаем первый символ строки.

>if (a.at(5).isNull()) {
>    out << "Outside the range of the string" << endl;
>}

Метод at возвращает null если мы пытаемся получить доступ к символу за пределами строки.

>$ ./access
>E
>e
>E
>Outside the range of the string

## Длина строки Qt5 ##

Существует три метода для получения длины строки. Эти методы называются size, count и length. Они возвращают количество симвовло в указанной строке.

>#include <QTextStream>
>
>int main(void) {
>
>  QTextStream out(stdout);
>
>  QString s1 = "Eagle";
>  QString s2 = "Eagle\n";
>  QString s3 = "Eagle ";
>  QString s4 = "орел";
>
>  out << s1.length() << endl;
>  out << s2.length() << endl;
>  out << s3.length() << endl;
>  out << s4.length() << endl;
>
>  return 0;
>}

Мы получаем размеры четырех строк.

>QString s2 = "Eagle\n";
>QString s3 = "Eagle ";

Каждая их этих строк включает пробел.

>QString s4 = "орел";

Эта строка содержит русскую букву.

На экране мы можем видеть, что метод lenght также считает и пробелы.

>$ ./length
>5
>6
>6
>4

## Построение строк в Qt5 ##

Динамическое составление строк позволяет заменить специальные символы на необходимые значение. Мы используем метода arg для интерполяции.

>#include <QTextStream>
>
>int main() {
>
>   QTextStream out(stdout);
>
>   QString s1 = "There are %1 white roses";
>   int n = 12;
>
>   out << s1.arg(n) << endl;
>
>   QString s2 = "The tree is %1 m high";
>   double h = 5.65;
>
>   out << s2.arg(h) << endl;
>
>   QString s3 = "We have %1 lemons and %2 oranges";
>   int ln = 12;
>   int on = 4;
>
>   out << s3.arg(ln).arg(on) << endl;
>
>   return 0;
>}

Маркер, который необходимо заменить начинается с символа %. Следующий символ является номером указанного аргумента. В строке может быть несколько аргументов. Метод arg явялется перегруженным, он может принимать целочисленные значения, тип long, char и QString, а также многие другие.

>QString s1 = "There are %1 white roses";
>int n = 12;

Маркер %1 мы заменим на целочисленное значение.

> out << s1.arg(n) << endl;

Метод arg принимает значение типа int. Маркер %1 будет заменен на значение переменной n.

>QString s2 = "The tree is %1 m high";
>double h = 5.65;
>
>out << s2.arg(h) << endl;

Эти три строки кода делают тоже самое, но с типом double. Правильный метод arg вызывается автоматически.

>QString s3 = "We have %1 lemons and %2 oranges";
>int ln = 12;
>int on = 4;
>
>out << s3.arg(ln).arg(on) << endl;

Мы можем использовать множество маркеров, %1 - это первый аргумент, %2 - это второй аргумент. Методы arg вызываются последовательно.

>$ ./building
>There are 12 white roses
>The tree is 5.65 m high
>We have 12 lemons and 4 oranges

## Подстроки Qt5 ##

Работая с текстом, необходимо искать подстроки в обычной строке. Для этого, в нашем распоряжении есть методы left, right, mid.

>#include <QTextStream>
>
>int main(void) {
>
>   QTextStream out(stdout);
>
>   QString str = { "The night train" };
>
>   out << str.right(5) << endl;
>   out << str.left(9) << endl;
>   out << str.mid(4, 5) << endl;
>
>   QString str2("The big apple");
>   QStringRef sub(&str2, 0, 7);
>
>   out << sub.toString() << endl;
>
>   return 0;
>}

Мы используем все три метода для поиска построк в заданной строке.

>out << str.right(5) << endl;

С помощью метода right, мы получаем 5 символов с правого края строки str. Подстрока 'train' выводится на печать.

>out << str.left(9) << endl;

С помощью метода left мы получаем девять символов с левого края строки str. 'The night' выводится на печать.

>out << str.mid(4, 5) << endl;

С помощью метода midб мы получаем 5 символов, начиная с четвертого. 'night' выводится на печать.

>QString str2("The big apple");
>QStringRef sub(&str2, 0, 7);

Класс QStringRef неизменяемая версия QString. Здест мы создали объект QStringRef из части строки str2. Вторым параметром является начало подстроки, а третьим - длина подстроки.

<$ ./substrings
<train
<The night
<night
<The big

## Обход строк в цикле ##

QString состоит из QChars. В цикле мы можем получить доступ к каждому QChar в строке.

>#include <QTextStream>

>int main(void) {
>
>  QTextStream out(stdout);
>
>  QString str { "There are many stars." };
>
>  for (QChar qc: str) {
>    out << qc << " ";
>  }
>
>  out << endl;
>
>  for (QChar *it=str.begin(); it!=str.end(); ++it) {
>    out << *it << " " ;
>  }
>
>  out << endl;
>
>  for (int i = 0; i < str.size(); ++i) {
>    out << str.at(i) << " ";
>  }
>
>  out << endl;
>
>  return 0;
>}

Мы продемонстировали 3 варианта обхода объекта QString. Мы добавили пробел между буквами и напечатали в терминале.

>for (QChar qc: str) {
>  out << qc << " ";
>}

Цикл обхода строки с использованием цикла на основе интервалов (foreach)

>for (QChar *it=str.begin(); it!=str.end(); ++it) {
>  out << *it << " " ;
>}

В это коде мы используем итераторы для обхода строки.

>for (int i = 0; i < str.size(); ++i) {
>  out << str.at(i) << " ";
>}

Мы вычысляем размер строки и используем метод at для доступа к символам(элементам).

>$ ./looping
>T h e r e   a r e   m a n y   s t a r s .
>T h e r e   a r e   m a n y   s t a r s .
>T h e r e   a r e   m a n y   s t a r s .

## Сравнение строк в Qt5 ##

Статический метод QString::compare используется для сравнения двух строк. Это метод возвращает int. Если возвращаемое значение меньше ноля, то первая строка меньше второй. Если возвращает ноль, то строки равны. Если возвращенное значение больше ноля, то первая строка больше второй. Под "меньше" мы имме ввиду, что указанный символ стоит до другого в таблице символов.

Строки сравниваются следующим способом: сначала сравниваются первые символы двух строк, если они равны, то сравниваются следующие два символа строк, до того момента, когда будут найдены отличающиеся символы или конец строки (все символы совпадают).

>#include <QTextStream>
>
>#define STR_EQUAL 0
>
>int main(void) {
>
>   QTextStream out(stdout);
>
>   QString a { "Rain" };
>   QString b { "rain" };
>   QString c { "rain\n" };
>
>   if (QString::compare(a, b) == STR_EQUAL) {
>     out << "a, b are equal" << endl;
>   } else {
>     out << "a, b are not equal" << endl;
>   }
>
>   out << "In case insensitive comparison:" << endl;
>
>   if (QString::compare(a, b, Qt::CaseInsensitive) == STR_EQUAL) {
>     out << "a, b are equal" << endl;
>   } else {
>     out << "a, b are not equal" << endl;
>   }
>
>   if (QString::compare(b, c) == STR_EQUAL) {
>     out << "b, c are equal" << endl;
>   } else {
>     out << "b, c are not equal" << endl;
>   }
>
>   c.chop(1);
>
>   out << "After removing the new line character" << endl;
>
>   if (QString::compare(b, c) == STR_EQUAL) {
>     out << "b, c are equal" << endl;
>   } else {
>     out << "b, c are not equal" << endl;
>   }
>
>   return 0;
>}

Мы будем сравнивать с учетом регистра и без учета регистра символов, с помощью метода compare.

>#define STR_EQUAL 0

Для большей ясновсти кода, определим константу STR_EQUAL.

>QString a { "Rain" };
>QString b { "rain" };
>QString c { "rain\n" };

Будем справнивать эти три строки.

>if (QString::compare(a, b) == STR_EQUAL) {
>    out << "a, b are equal" << endl;
>} else {
>    out << "a, b are not equal" << endl;
>}

Мы сравниваем строки а и b, которые не равны. Они отличаются по первому символу.

>if (QString::compare(a, b, Qt::CaseInsensitive) == STR_EQUAL) {
>    out << "a, b are equal" << endl;
>} else {
>    out << "a, b are not equal" << endl;
>}

Если не учитывать регистр символов, то эти строки равны. Параметр Qt::CaseInsensitive указывает на то, что сранивать строки надо без учета регистра символов.

>c.chop(1);

Метод chop удаляет последний символ в строке c. Теперь строки b и c равны.

>$ ./comparing
>a, b are not equal
>In case insensitive comparison:
>a, b are equal
>b, c are not equal
>After removing the new line character
>b, c are equal

## Преобразование строк ##

Часто возникает необходимость преобразования строк в другие типы данные и наоборот. Методы toInt, toFloat, toLong класса QString преобразуют в строку типы int, float, long. (Там много подобных методов.) Метод setNum преобразует разные типы числовых данных в строку. Это метод перегружен и правильный вызывается автоматически.

>#include <QTextStream>
>
>int main(void) {
>
>  QTextStream out(stdout);
>
>  QString s1 { "12" };
>  QString s2 { "15" };
>  QString s3, s4;
>
>  out << s1.toInt() + s2.toInt() << endl;
>
>  int n1 = 30;
>  int n2 = 40;
>
>  out << s3.setNum(n1) + s4.setNum(n2) << endl;
>
>  return 0;
>}

В этом примере мы преобразуем две строки в int и суммируем их. Затем мы преобразуем два числа в строки и соединяем их.

>out << s1.toInt() + s2.toInt() << endl;

Метод toInt преобразует строку в int. Мы складывает два числа, полученные из строк.

>out << s3.setNum(n1) + s4.setNum(n2) << endl;

В этом случае метод setNum преобразует int d строку. Мы соедниняем две строки.

>$ ./converting
>27
>3040

## Буквы (символы) ##

Символы делятся на несколько категорий: цифры, буквы пробелы и знаки пунктуации. Каждый Qstring состоит из Qhar и содержит методы isDigit, isLetter, isSpace и isPunct для выполнения определнных задач.

>#include <QTextStream>
>
>int main(void) {
>
>  QTextStream out(stdout);
>
>  int digits  = 0;
>  int letters = 0;
>  int spaces  = 0;
>  int puncts  = 0;
>
>  QString str { "7 white, 3 red roses." };
>
>  for (QChar s : str) {
>
>    if (s.isDigit()) {
>      digits++;
>    } else if (s.isLetter()) {
>      letters++;
>    } else if (s.isSpace()) {
>      spaces++;
>    } else if (s.isPunct()) {
>      puncts++;
>    }
>  }
>
>  out << QString("There are %1 characters").arg(str.count()) << endl;
>  out << QString("There are %1 letters").arg(letters) << endl;
>  out << QString("There are %1 digits").arg(digits) << endl;
>  out << QString("There are %1 spaces").arg(spaces) << endl;
>  out << QString("There are %1 punctuation characters").arg(puncts) << endl;
>
>  return 0;
>}

В примере мы определяем простое предложение. Мы подсчитываем количество цифрб буквб пробелов и знаков пунктуации в этом предложении.

>QString str { "7 white, 3 red roses." };

Это предложение мы будем разбирать.

>for (QChar s : str) {
>
>  if (s.isDigit()) {
>    digits++;
>  } else if (s.isLetter()) {
>    letters++;
>  } else if (s.isSpace()) {
>    spaces++;
>  } else if (s.isPunct()) {
>    puncts++;
>  }
>}

Мы используем цикл с диапазонами для подсчета. Каждый элемент является объектом QChar. Мы испольщуем методы QChar класса для определения категории символа.

>out << QString("There are %1 characters").arg(str.count()) << endl;
>out << QString("There are %1 letters").arg(letters) << endl;
>out << QString("There are %1 digits").arg(digits) << endl;
>out << QString("There are %1 spaces").arg(spaces) << endl;
>out << QString("There are %1 punctuation characters").arg(puncts) << endl;

Используя разбивку строк, печатаем полученные числа в терминал.

## Изменение строк Qt5  ##

Некоторые метдоы (например, toLower) возвращает новую модицифированную копию исходной строки. Другие методы меняют саму строку. Мы покажем некоторые их них.

>#include <QTextStream>
>
>int main(void) {
>
>   QTextStream out(stdout);
>
>   QString str { "Lovely" };
>   str.append(" season");
>
>   out << str << endl;
>
>   str.remove(10, 3);
>   out << str << endl;
>
>   str.replace(7, 3, "girl");
>   out << str << endl;
>
>   str.clear();
>
>   if (str.isEmpty()) {
>     out << "The string is empty" << endl;
>   }
>
>   return 0;
>}

Мы опишем четыре метода, которые изменяют исходную строку.

>str.append(" season");

Метод append добавляет новую строку в конец исходной строки.

>str.remove(10, 3);

Метод remove удаляет 3 символа, начиная с десятого.

>str.replace(7, 3, "girl");

Метод replace заменяет 3 символа, начиная с седьмого, указанной строкой.

>str.clear();

Метод clear, очищает строку.

>$ ./modify
>Lovely season
>Lovely sea
>Lovely girl
>The string is empty

## Выравнивание строк в Qt5 ##

Часто возникает необходимость осуществить аккуратный вывод. Мы можем использовать методы leaftJustified и  rightJustified для выравнивания строк.

>#include <QTextStream>
>
>int main(void) {
>
>   QTextStream out(stdout);
>
>   QString field1 { "Name: " }; 
>   QString field2 { "Occupation: " }; 
>   QString field3 { "Residence: " }; 
>   QString field4 { "Marital status: " }; 
>
>   int width = field4.size();
>
>   out << field1.rightJustified(width, ' ') << "Robert\n";
>   out << field2.rightJustified(width, ' ') << "programmer\n";
>   out << field3.rightJustified(width, ' ') << "New York\n";
>   out << field4.rightJustified(width, ' ') << "single\n";
>
>   return 0;
>}

Пример выравнивания строк вправо.

>int width = field4.size();

Сначала мы рассчитываем размер выравнивания по самой длинной строке.

>out << field1.rightJustified(width, ' ') << "Robert\n";

Метод rightJustified возвращает строку указанной ширины. Если исходня строка короче, то недостающие символы будут добавлены. Добавлены будут указанные символы из второго параметра. В нашем случае, это пробел.

>$ ./right_align
>          Name: Robert
>    Occupation: programmer
>     Residence: New York
>Marital status: single

## escape символы ##

В Qt5 есть HtmlEscaped метод, который преобразует текстовую строку в строку HTML с метасимволами <, >, & и ",  заменяя их на сущности HTML.

>$ cat cprog.c
>#include <stdio.h>
>
>int main(void) {
>
>    for (int i=1; i<=10; i++) {
>
>        printf("Bottle %d\n", i);
>    }
>}

Эта С программа с метасимволами HTML.

>#include <QTextStream>
>#include <QFile>
>
>int main(void) {
>
>    QTextStream out(stdout);
>
>    QFile file("cprog.c");
>
>    if (!file.open(QIODevice::ReadOnly)) {
>
>        qWarning("Cannot open file for reading");
>        return 1;
>    }
>
>    QTextStream in(&file);
>
>    QString allText = in.readAll();
>    out << allText.toHtmlEscaped() << endl;
>
>    file.close();
>
>    return 0;
>}

В этом примере программа читает C программу и заменяет в ней метасимволы на их HTML сущности.
