[источник](https://zetcode.com/gui/qt5/containers/ "взято отсуюда")

## Контейнеры в Qt5 ##

В этой части учебника по Qt5, мы поговорим о контейнерах в Qt5. Мы рассмотрим следующие контейнеры: QVector, QList, QStringList, QSet, and QMap.

Контейнеры - это классы общего назначения, которые хранят элементы определенного типа в памяти. В C++ есть Стандартная библиотека шаблонов (Standart Template Library - STL), которая имеет свои собственные контейнеры. В Qt мы можем использовать контейнеры Qt или контейнеры STL.

Существует два вида контейнеров: последовательные и ассоциативные. Последовательные контейнеры хранят элементы один за другим, ассоциативные контейнеры хранят пары ключ-значение. QList, QVector, QLinkedList являются последовательными контейнерами, а QMap и QHash - примеры ассоциативных контейнеров.

Поскольку мы создаем программы для командной строки, то нам не нужен модуль GUI. Мы можем добавить QT -= gui в файл проекта.

## Qt5 QVector ##

QVector это шаблон класса, который реализует динамический массив. Он хранит элементы в смежных ячейках памяти и обеспечвает быстрый доступ по индексу. Для больших векторов, операции вставки медленные и для этого рекомендуется использование контейнера QList.

~~~
myvector.cpp

#include <QVector>
#include <QTextStream>

int main(void) {

    QTextStream out(stdout);

    QVector<int> vals = {1, 2, 3, 4, 5};

    out << "The size of the vector is: " << vals.size() << endl;

    out << "The first item is: " << vals.first() << endl;
    out << "The last item is: " << vals.last() << endl;

    vals.append(6);
    vals.prepend(0);

    out << "Elements: ";

    for (int val : vals) {

        out << val << " ";
    }

    out << endl;

    return 0;
}

~~~

Этот пример работает с вектором целых чисел (int).

~~~

QVector<int> vals = {1, 2, 3, 4, 5};

~~~

Создание вектора, содержащего int.

~~~

out << "The size of the vector is: " << vals.size() << endl;

~~~

Метод size возвращает размер вектора - число элементов в нем.

~~~

out << "The first item is: " << vals.first() << endl;

~~~

Получение первого элемента с помощью метода first.

~~~

out << "The last item is: " << vals.last() << endl;

~~~

Получение последнего элемента с помощью метода last.

~~~

vals.append(6);

~~~

Метод append вставляет элемент в конец вектора.

~~~

vals.prepend(0);

~~~

Метод prepend вставляет элемент в начало вектора.

~~~

for (int val : vals) {

    out << val << " ";
}

~~~

Перебираем все элементы вектора с помощью цикла и печатаем их значения.

~~~

$ ./myvector
The size of the vector is: 5
The first item is: 1
The last item is: 5
Elements: 0 1 2 3 4 5 6

~~~

## Qlist в Qt5 ##

QList это контейнер, который создает список элементов. Он похож на QVector. Он содержит список элементов и предоставляет доступ по индексу такой же быстрый, как вставка и удаление. Это один из самых частоиспользуемых контейнеров в Qt.

~~~
mylist.cpp

#include <QTextStream>
#include <QList>
#include <algorithm>

int main(void) {

    QTextStream out(stdout);

    QList<QString> authors = {"Balzac", "Tolstoy",
        "Gulbranssen", "London"};

    for (int i=0; i < authors.size(); ++i) {

        out << authors.at(i) << endl;
    }

    authors << "Galsworthy" << "Sienkiewicz";

    out << "***********************" << endl;

    std::sort(authors.begin(), authors.end());

    out << "Sorted:" << endl;
    for (QString author : authors) {

        out << author << endl;
    }

    return 0;
}

~~~

Этот пример демонстрирует контейнер QList.

~~~

QList<QString> authors = {"Balzac", "Tolstoy",
    "Gulbranssen", "London"};

~~~

QList контейнер создан. Он содержит имена писателей.

~~~

for (int i=0; i < authors.size(); ++i) {

    out << authors.at(i) << endl;
}

~~~

В цикле мы перебираем содержимое контейнера и печатаем его элементы. Метод at возвращает элемент с указанным индексом.

~~~

authors << "Galsworthy" << "Sienkiewicz";

~~~

Оператор << используется для вставки занчений в список.

~~~

std::sort(authors.begin(), authors.end());

~~~

std::sort метод сортирует список в возрастающем порядке.

~~~

out << "Sorted:" << endl;
for (QString author : authors) {

    out << author << endl;
}

~~~

Теперь мы распечатываем отсортированный список.

~~~

$ ./mylist
Balzac
Tolstoy
Gulbranssen
London
***********************
Sorted:
Balzac
Galsworthy
Gulbranssen
London
Sienkiewicz
Tolstoy

~~~

## QStringList ###

QStringList это удобный контейнер, который состоит из списка строк. Он имеет быстрый доступ по индексу, такой же быстрый как вставка и удаление.

~~~
mystringlist.cpp

#include <QTextStream>
#include <QList>

int main(void) {

    QTextStream out(stdout);

    QString string = "coin, book, cup, pencil, clock, bookmark";
    QStringList items = string.split(",");
    QStringListIterator it(items);

    while (it.hasNext()) {

        out << it.next().trimmed() << endl;
    }

    return 0;
}

~~~

В этом примеремы создаем списко строк из строки и печатаем его элементы в консоль.

~~~

QString string = "coin, book, cup, pencil, clock, bookmark";
QStringList items = string.split(",");

~~~

Метод split класса QString разделяет строку на подстроки, по указанному разделителю. Подстроки возвращаются как список.

~~~

QStringListIterator it(items);

~~~

QStringListIterator представляет собой итератор для QStringList в Java-стиле.

~~~

while (it.hasNext()) {

    out << it.next().trimmed() << endl;
}

~~~

Используя созданный итератор, мы печатаем элементы списка в терминал. Метод trimmed обрезает пробелы в строках.

~~~

$ ./mystringlist
coin
book
cup
pencil
clock
bookmark

~~~

## Qt5 Qset ##

QSet представляет собой набор уникальных значений с быстрым поиском. Значения хранятся в неупорядоченными.

~~~
myset.cpp

#include <QSet>
#include <QList>
#include <QTextStream>
#include <algorithm>

int main(void) {

    QTextStream out(stdout);

    QSet<QString> cols1 = {"yellow", "red", "blue"};
    QSet<QString> cols2 = {"blue", "pink", "orange"};

    out << "There are " << cols1.size() << " values in the set" << endl;

    cols1.insert("brown");

    out << "There are " << cols1.size() << " values in the set" << endl;

    cols1.unite(cols2);

    out << "There are " << cols1.size() << " values in the set" << endl;

    for (QString val : cols1) {
        out << val << endl;
    }

    QList<QString> lcols = cols1.values();
    std::sort(lcols.begin(), lcols.end());

    out << "*********************" << endl;
    out << "Sorted:" << endl;

    for (QString val : lcols) {
        out << val << endl;
    }

   return 0;
}

~~~

В данном примере QSet используется для хранения цветов. Нет смысла указывать одно значение несколько раз. 

~~~

QSet<QString> cols1 = {"yellow", "red", "blue"};
QSet<QString> cols2 = {"blue", "pink", "orange"};

~~~

У нас два набора цветов. Голубой цвет есть в обоих наборах.

~~~

out << "There are " << cols1.size() << " values in the set" << endl;

~~~

Метод size возвращает размер набора.

~~~

cols1.insert("brown");

~~~

Мы добавляем новое значение в набор(множество) с помощью метода insert.

~~~

cols1.unite(cols2);

~~~

Метод unite объединяет два множества. В набор cols1 будут вставлены все элементы множества col2, которого там еще нет, в нашем случае, исключая голубой цвет.

~~~

for (QString val : cols1) {

    out << val << endl;
}

~~~

С помощью цикла, мы печатаем все элементы множества col1.

~~~

QList<QString> lcols = cols1.values();
std::sort(lcols.begin(), lcols.end());

~~~

Сортировка множеств не поддерживается. Мы можем создать список из множества и отсортировать его. Метод values возвращает новый QList, содержащий элементы множества. Порядок элементов в Qlist не определен.

~~~

$ ./myset
There are 3 values in the set
There are 4 values in the set
There are 6 values in the set
pink
orange
brown
blue
yellow
red
*********************
Sorted:
blue
brown
orange
pink
red
yellow

~~~

## Qt5 Map ##

QMap это ассоциативный массив (словарь), который хранит пары ключ-значение. Он обладает быстрым поиском значения по ключу.

~~~
myqmap.cpp

#include <QTextStream>
#include <QMap>

int main(void) {

    QTextStream out(stdout);

    QMap<QString, int> items = { {"coins", 5}, {"books", 3} };

    items.insert("bottles", 7);

    QList<int> values = items.values();

    out << "Values:" << endl;

    for (int val : values) {
        out << val << endl;
    }

    QList<QString> keys = items.keys();

    out << "Keys:" << endl;
    for (QString key : keys) {
        out << key << endl;
    }

    QMapIterator<QString, int> it(items);

    out << "Pairs:" << endl;

    while (it.hasNext()) {
        it.next();
        out << it.key() << ": " << it.value() << endl;
    }

    return 0;
}

~~~

В этом примере, мы создали словарь со строковыми ключами и целочисленными значениями.

~~~

QMap<QString, int> items = { {"coins", 5}, {"books", 3} };

~~~

Создали словарь с двумя парами ключ-значение.

~~~

items.insert("bottles", 7);

~~~

Вставили новую пару с помощью метода insert.

~~~

QList<int> values = items.values();

out << "Values:" << endl;

for (int val : values) {
    out << val << endl;
}

~~~

Мы получаем все значения и печатаем из в консоль. Метод values возвращает список значений.

~~~

QList<QString> keys = items.keys();

out << "Keys:" << endl;
for (QString key : keys) {
    out << key << endl;
}

~~~

Также, мы печатаем все ключи из словаря. Метод keys возвращает список, содержащий все ключи из словаря.

~~~

QMapIterator<QString, int> it(items);

~~~

QMapIterator это JAVA-подобный итератор для QMap. Он может ыбть использован для перебора всех элементов словаря.

~~~

while (it.hasNext()) {

    it.next();
    out << it.key() << ": " << it.value() << endl;
}

~~~

С помощью итератора, мы прозодим по всем элеменатм словаря. Метод key возвращает текущий ключ, а метод value возвращает текущее значение.

~~~

$ ./myqmap
Values:
3
7
5
Keys:
books
bottles
coins
Pairs:
books: 3
bottles: 7
coins: 5

~~~

## Сортировка пользовательских объектов ##

В следующем примере, мы будем сортировать пользовательские объекты в QList.

~~~
book.h

class Book {

    public:
        Book(QString, QString);
        QString getAuthor() const;
        QString getTitle() const;

    private:
        QString author;
        QString title;
};

~~~

Это заголовочный файл для пользовательского класса Book.

~~~
book.cpp

#include <QString>
#include "book.h"

Book::Book(QString auth, QString tit) {

    author = auth;
    title = tit;
}

QString Book::getAuthor() const {

    return author;
}

QString Book::getTitle() const {

    return title;
}

~~~

Это реализация класса Book, в которой есть два метода для доступа к полям класса.

~~~
sortcustomclass.cpp

#include <QTextStream>
#include <QList>
#include <algorithm>
#include "book.h"

bool compareByTitle(const Book &b1, const Book &b2) {

  return b1.getTitle() < b2.getTitle();
}

int main(void) {

    QTextStream out(stdout);

    QList<Book> books = {

        Book("Jack London", "The Call of the Wild"),
        Book("Honoré de Balzac", "Father Goriot"),
        Book("Leo Tolstoy", "War and Peace"),
        Book("Gustave Flaubert", "Sentimental education"),
        Book("Guy de Maupassant", "Une vie"),
        Book("William Shakespeare", "Hamlet")
    };

    std::sort(books.begin(), books.end(), compareByTitle);

    for (Book book : books) {
        out << book.getAuthor() << ": " << book.getTitle() << endl;
    }

    return 0;
}

~~~

В этом примере мы создаем несколько  объектов Book и сортируем их с помощью std::sort.

~~~

bool compareByTitle(const Book &b1, const Book &b2) {

  return b1.getTitle() < b2.getTitle();
}

~~~

Функция compareByTitle это функция сравнения, которую использует алгоритм сортировки.

~~~

std::sort(books.begin(), books.end(), compareByTitle);

~~~

std::sort сортирует книги по названию.

