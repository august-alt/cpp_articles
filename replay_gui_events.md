# Глава 4: Воспроизведение событий графического интерфейса

Как воспроизводить события графического интерфейса

В этой главе мы покажем, как симулировать события графического интерфейса и как сохранять серию событий графического интерфейса так же как и воспроизводить их на виджете.

Подход к сохранению серии событий и их воспроизведение немного немного похож на подход, описанный в главе 2. Все что вам нужно, это добавить функцию данных в ваш тестовый класс:

~~~
class TestGui: public QObject
{
    Q_OBJECT

private slots:
    void testGui_data();
    void testGui();
};
~~~

## Написание функции данных
***

Как и раньше, тестовая функция ассоциируется с функцией данных тем же именемем, с добавлением _data к имени функции.

~~~
void TestGui::testGui_data()
{
    QTest::addColumn<QTestEventList>("events");
    QTest::addColumn<QString>("expected");

    QTestEventList list1;
    list1.addKeyClick('a');
    QTest::newRow("char") << list1 << "a";

    QTestEventList list2;
    list2.addKeyClick('a');
    list2.addKeyClick(Qt::Key_Backspace);
    QTest::newRow("there and back again") << list2 << "";
}
~~~

Сначала мы объявляем елементы таблицы, используя функцию QTest::addColunm(): список событий графического интерфейса, и ожидаемых результатов применения списка событий к QWidget. Обратите внимание, что тип первого элемента QTestEventList/

QTestEnevtList может быть заполнен событиями графического интерфейса, которые можно сохранить для последующего использования к любому QWidget.

В нашей текущей функции данных, мы создаем два элемента QTestEventList. Первый список состоит из одинарного нажатия клавиши 'a'. Мы добавляем событие в список использую функцию QTestEventList::addKeyClick(). Затем мы используем фукнцию QTest::newRow(), чтобы дать имя набору данных, и перенаправляем поток событий и ожидаемых результатов.

Второй список состоит из двух нажатий: клавиши 'a' и последущим нажатием 'backspace'. Снова мы использем функцию QTestEventList::addKeyClick() для доабвления событий в список, и функцию QTest::newRow() для добавления списка событий и ожиадемого результата в талицу с ассоциированным именем.

Переделка тестовой функции
***

Теперь наша тестовая функция может быть переписана:

~~~
void TestGui::testGui()
{
    QFETCH(QTestEventList, events);
    QFETCH(QString, expected);

    QLineEdit lineEdit;

    events.simulate(&lineEdit);

    QCOMPARE(lineEdit.text(), expected);
}
~~~

Функция TestGui::testGui() будет исполняться дважды, по одному разу с каждым набором данных, которые мы создали и ассоциировали в функции TEstGui::testGui_data().

Сначала мы получаем два набора данных с помощью макроса QFETCH(). Этот макрос принимает два аргумента: тип данных элемента и и его имя. Затем мы создали объект типа QLineEdit и применили список событий на этот виджет, используя функцию QTestEventList::simulate(). 

В конце, мы используем макрос QCOMPARE() для проверки совпадения текста QLineEdit с ожидаемым результатом.

Как и ранее, для сборки тестов в отдельный исполняемый файл, нужны две следующие строки

~~~
QTEST_MAIN(TestGui)
#include "testgui.moc"
~~~

Макрос QTEST_MAIN() разворачивается в простую функцию main(), которая запускает все тестовые функции, а поскольку, объявление и реализация нашего тестового класса в файле .cpp, на также нужно включить сгенерированный moc файл для интроспекции Qt.

