[исходник](https://doc.qt.io/qt-6/qttestlib-tutorial5-example.html)

# Глава 5: Пишем тест производительности

Как написать тест производительности

Это заключительная глава, в которой мы продемонстрируем как написать тест производительности, используя Qt Test.

Пишем тест производительности 
***

Для создания теста производительности мы расширим тестовую функцию макросом QBENCHMARK. Функция тестирования производительности обычно состоит из установочного кода и макроса QBENCHMARK, который содержит код подлежащий измерению. Это тестовая функция QString::localeAwareCompare().

~~~
void TestBenchmark::simple()
{
    QString str1 = QLatin1String("This is a test string");
    QString str2 = QLatin1String("This is a test string");

    QCOMPARE(str1.localeAwareCompare(str2), 0);

    QBENCHMARK {
        str1.localeAwareCompare(str2);
    }
}
~~~

Установка может быть выполнена в начале функции, когда время не измеряется. Время выполнения кода внутри макроса QBENCHMARK будет измерено, возможно, несколько раз, для получения более точных значений.

Некоторые бекэнды досупны и могут быть использованы из командной строки.

Функция данных
***

Функция данных удобная для создания тестов производительности, которые тестируют разные входящие данные, например, данные с использованием локали и стандартные.

~~~
void TestBenchmark::multiple_data()
{
    QTest::addColumn<bool>("useLocaleCompare");
    QTest::newRow("locale aware compare") << true;
    QTest::newRow("standard compare") << false;
}
~~~

Тетосвая функция, которая использует данные для определения производительности.

~~~
void TestBenchmark::multiple()
{
    QFETCH(bool, useLocaleCompare);
    QString str1 = QLatin1String("This is a test string");
    QString str2 = QLatin1String("This is a test string");

    int result;
    if (useLocaleCompare) {
        QBENCHMARK {
            result = str1.localeAwareCompare(str2);
        }
    } else {
        QBENCHMARK {
            result = (str1 == str2);
        }
    }
    Q_UNUSED(result);
}
~~~

Условие "if (useLocaleCompare)" вынесено за пределы макроса QBENCHMARK для того, чтобы не сравнение не попадало в измерение. Каждая тестовая функция производительности моежт иметь только один активный макрос QBENCHMARK/


