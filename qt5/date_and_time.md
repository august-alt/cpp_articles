## Дата и время в Qt5

В этой части учебника по программированию на Qt5 C++ мы поговорим о времени и дате.

В Qt5 есть классы QDate, QTime и QDateTime для работы с датой и временем. QDate - это класс для работы с календарной датой в григорианском календаре. В нем есть методы для определения даты, сравнения или манипулирования датами. Класс QTime работает с часовым временем. Он предоставляет методы для сравнения времени, определения времени и различные другие методы манипулирования временем. QDateTime - это класс, который объединяет объекты QDate и QTime в один объект.

В этой главе мы создавали программы командной строки; поэтому нам не нужен модуль Qt GUI. Мы можем добавить объявление QT -= gui в файл проекта.


## Инициализация объектов даты и времени в Qt5

Объекты даты и времени могут быть инициализированы двумя основными способами. Мы инициализируем их в конструкторе объекта или можем создать пустые объекты и заполнить их данными позже. 

~~~
#include <QTextStream>
#include <QDate>
#include <QTime>

int main(void) {

   QTextStream out(stdout);

   QDate dt1 { 2020, 4, 12 };
   out << "The date is " << dt1.toString() << endl;

   QDate dt2;
   dt2.setDate(2020, 3, 3);
   out << "The date is " << dt2.toString() << endl;

   QTime tm1 { 17, 30, 12, 55 };
   out << "The time is " << tm1.toString("hh:mm:ss.zzz") << endl;

   QTime tm2;
   tm2.setHMS(13, 52, 45, 155);
   out << "The time is " << tm2.toString("hh:mm:ss.zzz") << endl;
}
~~~
Мы инициализируем объект даты и времени обоими способами. 
~~~
QDate dt1 { 2020, 4, 12 };
~~~
Конструктор объекта QDate принимает три параметра: год, месяц и день. 
~~~
out << "The date is " << dt1.toString() << endl;
~~~
Дата выводится на консоль. Мы используем метод toString для преобразования объекта даты в строку. 
~~~
QTime tm2;
tm2.setHMS(13, 52, 45, 155);
~~~

Создается пустой объект QTime. Мы заполняем объект данными с помощью метода setHMS. Параметрами являются часы, минуты, секунды и миллисекунды.
~~~
out << "The time is " << tm2.toString("hh:mm:ss.zzz") << endl;
~~~
Мы выводим объект QTime на консоль. Мы используем специальный формат, который включает также миллисекунды, которые по умолчанию опущены.
~~~
$ ./init
The date is Sun Apr 12 2020
The date is Tue Mar 3 2020
The time is 17:30:12.055
The time is 13:52:45.155
~~~
Qt5 текущая дата и время
В следующем примере мы выводим текущее местное время и дату на консоль. 
~~~
#include <QTextStream>
#include <QTime>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QDate cd = QDate::currentDate();
   QTime ct = QTime::currentTime();

   out << "Current date is: " << cd.toString() << endl;
   out << "Current time is: " << ct.toString() << endl;
}
~~~
Обратите внимание, что файл не должен называться time.cpp. 
~~~
QDate cd = QDate::currentDate();
~~~
Статическая функция QDate::currentDate возвращает текущую дату.
~~~
QTime ct = QTime::currentTime();
~~~
Статическая функция QTime::currentTime возвращает текущее время.
~~~
out << "Current date is: " << cd.toString() << endl;
out << "Current time is: " << ct.toString() << endl;
~~~
Мы используем метод toString для преобразования объектов даты и времени в строки. 
~~~
$ ./current 
Current date is: Thu Dec 3 2020
Current time is: 10:21:48
~~~

## Qt5 сравнение дат

Реляционные операторы можно использовать для сравнения дат. Мы можем сравнить их положение в календаре. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QDate dt1 { 2020, 4, 5 };
   QDate dt2 { 2019, 4, 5 };

   if (dt1 < dt2) {

       out << dt1.toString() << " comes before "
            << dt2.toString() << endl;
   } else {

       out << dt1.toString() << " comes after "
            << dt2.toString() << endl;
   }
}
~~~
Пример сравнения двух дат.
~~~
QDate dt1 { 2020, 4, 5 };
QDate dt2 { 2019, 4, 5 };
~~~
Мы имеем две разные даты.
~~~
if (dt1 < dt2) {

    out << dt1.toString() << " comes before "
            << dt2.toString() << endl;
} else {

    out << dt1.toString() << " comes after "
            << dt2.toString() << endl;
}
~~~
Мы сравниваем даты с помощью оператора сравнения lower than (<) и определяем, какая из них расположена раньше в календаре.
~~~
$ ./compare 
Sun Apr 5 2020 comes after Fri Apr 5 2019
~~~
Операторы сравнения можно легко использовать и для объектов QTime и QDateTime. 


## Qt5 определение високосного года


 Високосный год - это год, содержащий дополнительный день. Причина появления дополнительного дня в календаре заключается в разнице между астрономическим и календарным годом. В календарном году ровно 365 дней, а в астрономическом - 365,25 дня, т.е. время, за которое Земля совершает один оборот вокруг Солнца.

Разница составляет 6 часов, что означает, что за четыре года мы теряем один день. Поскольку мы хотим, чтобы наш календарь был синхронизирован с временами года, мы добавляем один день к февралю каждые четыре года. (Есть исключения.) В григорианском календаре в феврале високосного года 29 дней вместо обычных 28. А год длится 366 дней вместо обычных 365.

Статический метод QDate::isLeapYear определяет, является ли год високосным. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QList<int> years({2010, 2011, 2012, 2013, 2014, 2015, 2016, 2020, 2024});

   for (int year: years) {

       if (QDate::isLeapYear(year)) {

           out << year << " is a leap year" << endl;
       } else {

           out << year << " is not a leap year" << endl;
       }
   }
}
~~~

В примере у нас есть список годов. Мы проверяем каждый год, является ли он високосным.
~~~
QList<int> years({2010, 2011, 2012, 2013, 2014, 2015, 2016, 2020, 2024});
~~~
Инициализируем список годов.
~~~
for (int year: years) {

    if (QDate::isLeapYear(year)) {

        out << year << " is a leap year" << endl;
    } else {

        out << year << " is not a leap year" << endl;
    }
}
~~~
Мы проходим по списку и определяем, является ли данный год високосным. Функция QDate::isLeapYear возвращает булево значение true или false. 
~~~
$ ./leapyear 
2010 is not a leap year
2011 is not a leap year
2012 is a leap year
2013 is not a leap year
2014 is not a leap year
2015 is not a leap year
2016 is a leap year
2020 is a leap year
2024 is a leap year
~~~


## Предопределенные форматы даты в Qt5

В Qt5 есть несколько встроенных форматов даты. Метод toString объекта QDate принимает формат даты в качестве параметра. По умолчанию в Qt5 используется формат даты Qt::TextDate. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QDate cd = QDate::currentDate();

   out << "Today is " << cd.toString(Qt::TextDate) << endl;
   out << "Today is " << cd.toString(Qt::ISODate) << endl;
   out << "Today is " << cd.toString(Qt::SystemLocaleShortDate) << endl;
   out << "Today is " << cd.toString(Qt::SystemLocaleLongDate) << endl;
   out << "Today is " << cd.toString(Qt::DefaultLocaleShortDate) << endl;
   out << "Today is " << cd.toString(Qt::DefaultLocaleLongDate) << endl;
   out << "Today is " << cd.toString(Qt::SystemLocaleDate) << endl;
   out << "Today is " << cd.toString(Qt::LocaleDate) << endl;
}
~~~
 В примере мы показываем восемь различных форматов даты для текущей даты.
~~~
out << "Today is " << cd.toString(Qt::ISODate) << endl;
~~~
Здесь мы выводим текущую дату в формате Qt::ISODate, который является международным стандартом для отображения дат.
~~~
$ ./predefineddateformats 
Today is Thu Dec 3 2020
Today is 2020-12-03
Today is 12/3/20
Today is Thursday, December 3, 2020
Today is 12/3/20
Today is Thursday, December 3, 2020
Today is 12/3/20
Today is 12/3/20
~~~


## Пользовательские форматы даты в Qt5

Дата может быть представлена в различных других форматах. В Qt5 мы можем создавать и свои собственные форматы дат. Другая версия метода toString принимает строку формата, в которой мы можем использовать различные спецификаторы формата. Например, спецификатор d обозначает день как число без ведущего нуля. Спецификатор dd обозначает день как число с ведущим нулем. В следующей таблице перечислены доступные выражения формата даты: 

| Выражение  | Вывод                                                             |
| ---------- | ----------------------------------------------------------------- |
| d          | День как число без ведущего нуля (от 1 до 31)                     |
| dd         | День в виде числа с ведущим нулем (от 01 до 31).                  |
| ddd        | Сокращенное локализованное имя дня (например, от 'Mon' до 'Sun'). Используется QDate::shortDayName. |
| dddd       | Длинное локализованное имя дня (например, от 'Monday' до 'Sunday'). Используется QDate::longDayName. |
| M          | Месяц в виде числа без ведущего нуля (от 1 до 12).                |
| MM         | Месяц в виде числа с ведущим нулем (от 01 до 12)                  |
| MMM        | Сокращенное локализованное название месяца (например, от 'Jan' до 'Dec'). Используется QDate::shortMonthName. |
| MMMM       | Длинное локализованное название месяца (например, от "январь" до "декабрь"). Используется QDate::longMonthName. |
| yy         | Год в виде двузначного числа (от 00 до 99).                       |
| yyyy       | Год как четырехзначное число. Если год отрицательный, дополнительно добавляется знак минус. |

~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QDate cd = QDate::currentDate();

   out << "Today is " << cd.toString("yyyy-MM-dd") << endl;
   out << "Today is " << cd.toString("yy/M/dd") << endl;
   out << "Today is " << cd.toString("d. M. yyyy") << endl;
   out << "Today is " << cd.toString("d-MMMM-yyyy") << endl;
}
~~~

У нас есть четыре пользовательских формата даты. 
~~~
out << "Today is " << cd.toString("yyyy-MM-dd") << endl;
~~~

 Это международный формат даты. Части даты разделяются символом тире. ГГГГ - это год, состоящий из четырех цифр. ММ - это месяц как число с ведущим нулем (от 01 до 12). И dd - день как число с ведущим нулем (от 01 до 31).
~~~
out << "Today is " << cd.toString("yy/M/dd") << endl;
~~~
Это еще один распространенный формат даты. Части разделяются символом косой черты (/). Спецификатор M обозначает месяц как число без ведущего нуля (от 1 до 12).
~~~
out << "Today is " << cd.toString("d. M. yyyy") << endl;
~~~
Этот формат даты используется в Словакии. Части разделяются символом точки. День и месяц не содержат ведущих нулей. Сначала идет день, затем месяц, а последним - год.
~~~
$ ./customdateformats 
Today is 2020-12-03
Today is 20/12/03
Today is 3. 12. 2020
Today is 3-December-2020
~~~

## Qt5 предопределенные форматы времени


Время имеет несколько предопределенных форматов. Стандартные спецификаторы формата идентичны тем, которые используются в форматах даты. По умолчанию в Qt5 используется формат времени Qt::TextDate.

~~~
#include <QTextStream>
#include <QTime>

int main(void) {

   QTextStream out(stdout);

   QTime ct = QTime::currentTime();

   out << "The time is " << ct.toString(Qt::TextDate) << endl;
   out << "The time is " << ct.toString(Qt::ISODate) << endl;
   out << "The time is " << ct.toString(Qt::SystemLocaleShortDate) << endl;
   out << "The time is " << ct.toString(Qt::SystemLocaleLongDate) << endl;
   out << "The time is " << ct.toString(Qt::DefaultLocaleShortDate) << endl;
   out << "The time is " << ct.toString(Qt::DefaultLocaleLongDate) << endl;
   out << "The time is " << ct.toString(Qt::SystemLocaleDate) << endl;
   out << "The time is " << ct.toString(Qt::LocaleDate) << endl;
}
~~~
 В примере мы показываем восемь различных форматов текущего времени.
~~~
out << "The time is " << ct.toString(Qt::ISODate) << endl;
~~~
Здесь мы выводим текущее время в формате Qt::ISODate, который является международным стандартом для отображения времени. 
~~~
$ ./predefinedtimeformats 
The time is 10:59:05
The time is 10:59:05
The time is 10:59 AM
The time is 10:59:05 AM CET
The time is 10:59 AM
The time is 10:59:05 AM CET
The time is 10:59 AM
The time is 10:59 AM
~~~

## Пользовательские форматы времени в Qt5


Мы можем создавать дополнительные форматы времени. Мы создаем пользовательский формат времени, в котором мы используем спецификаторы формата времени. В следующей таблице приведен список доступных выражений формата.

| Выражение  | Вывод                                                             |
| ---------- | ----------------------------------------------------------------- |
| h          | час без ведущего нуля (от 0 до 23 или от 1 до 12, если отображается AM/PM) |
| hh         | час с ведущим нулем (от 00 до 23 или от 01 до 12, если отображается AM/PM) |
| H          | час без ведущего нуля (от 0 до 23, даже при отображении AM/PM) |
| HH         | час с ведущим нулем (от 00 до 23, даже при отображении AM/PM) |
| m          | минута без ведущего нуля (от 0 до 59) |
| mm         | минуты с ведущим нулем (от 00 до 59) |
| s          | секунда без ведущего нуля (от 0 до 59) |
| ss         | секунда с ведущим нулем (от 00 до 59) |
| z          | миллисекунды без ведущих нулей (от 0 до 999) |
| zzz        | миллисекунды с ведущими нулями (от 000 до 999) |
|AP or A     | используйте индикацию AM/PM. AP будет заменен на "AM" или "PM".  |
|ap or a     | использовать индикацию am/pm. ap будет заменен либо на "am", либо на "pm". |
|t           | часовой пояс (например, "CEST")  |


~~~
#include <QTextStream>
#include <QTime>

int main(void) {

   QTextStream out(stdout);

   QTime ct = QTime::currentTime();

   out << "The time is " << ct.toString("hh:mm:ss.zzz") << endl;
   out << "The time is " << ct.toString("h:m:s a") << endl;
   out << "The time is " << ct.toString("H:m:s A") << endl;
   out << "The time is " << ct.toString("h:m AP") << endl;
}
~~~
У нас есть четыре пользовательских формата времени.
~~~
out << "The time is " << ct.toString("hh:mm:ss.zzz") << endl;
~~~
В этом формате у нас есть час, минута и секунда с ведущим нулем. Мы также добавляем миллисекунды с ведущими нулями.
~~~
out << "The time is " << ct.toString("h:m:s a") << endl;
~~~
Этот спецификатор формата времени использует час, минуту и секунду без ведущего нуля и добавляет идентификаторы периода am/pm. 
~~~
$ ./customtimeformats 
The time is 11:03:16.007
The time is 11:3:16 am
The time is 11:3:16 AM
The time is 11:3 AM
~~~


## Qt5 получение дня недели

Метод dayOfWeek возвращает число, представляющее день недели, где 1 - понедельник, а 7 - воскресенье. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QDate cd = QDate::currentDate();
   int wd = cd.dayOfWeek();

   QLocale locale(QLocale("en_US"));

   out << "Today is " << locale.dayName(wd) << endl;
   out << "Today is " << locale.dayName(wd, QLocale::ShortFormat) << endl;
}
~~~
 В примере мы выводим короткое и длинное названия текущего дня недели.
~~~
QDate cd = QDate::currentDate();
~~~
Получаем текущую дату.
~~~
int wd = cd.dayOfWeek();
~~~
Из текущей даты получаем день недели.
~~~
out << "Сегодня " << locale.dayName(wd) << endl;
~~~
Получаем длинное имя дня недели.
~~~
out << "Сегодня " << locale.dayName(wd, QLocale::ShortFormat) << endl;
~~~
Мы получаем длинное имя дня недели. 
~~~
$ ./weekday 
Today is Thursday
Today is Thu
~~~


## Номер дня

Мы можем вычислить количество дней в конкретном месяце с помощью метода daysInMonth и количество дней в году с помощью метода daysInYear. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);
   QList<QString> months;

   months.append("January");
   months.append("February");
   months.append("March");
   months.append("April");
   months.append("May");
   months.append("June");
   months.append("July");
   months.append("August");
   months.append("September");
   months.append("October");
   months.append("November");
   months.append("December");

   QDate dt1 { 2020, 9, 18 };
   QDate dt2 { 2020, 2, 11 };
   QDate dt3 { 2020, 5, 1 };
   QDate dt4 { 2020, 12, 11 };
   QDate dt5 { 2020, 2, 29 };

   out << "There are " << dt1.daysInMonth() << " days in "
       << months.at(dt1.month()-1) << endl;
   out << "There are " << dt2.daysInMonth() << " days in "
       << months.at(dt2.month()-1) << endl;
   out << "There are " << dt3.daysInMonth() << " days in "
       << months.at(dt3.month()-1) << endl;
   out << "There are " << dt4.daysInMonth() << " days in "
       << months.at(dt4.month()-1) << endl;
   out << "There are " << dt5.daysInMonth() << " days in "
       << months.at(dt5.month()-1) << endl;

   out << "There are " << dt1.daysInYear() << " days in year "
       << QString::number(dt1.year()) << endl;
}
~~~
Создаются пять объектов даты. Мы вычисляем количество дней в этих месяцах и в конкретном году. 
~~~
QDate dt1 { 2020, 9, 18 };
QDate dt2 { 2020, 2, 11 };
QDate dt3 { 2020, 5, 1 };
QDate dt4 { 2020, 12, 11 };
QDate dt5 { 2020, 2, 29 };
~~~
Создается пять объектов QDate. Каждый из них представляет отдельную дату. 
~~~
out << "There are " << dt1.daysInMonth() << " days in "
    << months.at(dt1.month()-1) << endl;

~~~
Мы используем метод daysInMonth, чтобы получить количество дней в объекте даты.
~~~
out << "There are " << dt1.daysInYear() << " days in year "
    << QString::number(dt1.year()) << endl;
~~~
А здесь мы получаем количество дней в году, используя метод daysInYear для объекта date. 
~~~
$ ./nofdays 
There are 30 days in September
There are 29 days in February
There are 31 days in May
There are 31 days in December
There are 29 days in February
There are 366 days in year 2020
~~~

## Qt5 проверка достоверности даты


Существует метод isValid, который проверяет, является ли дата действительной. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

    QTextStream out(stdout);

    QList<QDate> dates { 
        QDate(2020, 5, 11), QDate(2020, 8, 1),
        QDate(2020, 2, 30)
    };

    for (int i=0; i < dates.size(); i++) {

       if (dates.at(i).isValid()) {

           out << "Date " << i+1 << " is a valid date" << endl;
       } else {
           
           out << "Date " << i+1 << " is not a valid date" << endl;
       }
    }
}
~~~

 В примере мы проверяем достоверность трех дней.
~~~
QList<QDate> dates { 
    QDate(2020, 5, 11), QDate(2020, 8, 1),
    QDate(2020, 2, 30)
};
~~~
Первые два дня действительны. Третий день недействителен. В феврале 28 или 29 дней.
~~~
if (dates.at(i).isValid()) {

    out << "Дата " << i+1 << " является действительной датой" << endl;
} else {
    
    out << "Дата " << i+1 << " не является действительной датой" << endl;
}
~~~
В зависимости от результата метода isValid, мы выводим сообщение о валидности даты в консоль. 
~~~
$ ./validity
Date 1 is a valid date
Date 2 is a valid date
Date 3 is not a valid date
~~~


## Qt5 addDays, daysTo

Мы можем легко вычислить дату через n дней от определенной даты. Для этого мы используем метод addDays. Метод daysTo возвращает количество дней до выбранной даты. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

    QTextStream out(stdout);

    QDate dt { 2020, 5, 11 };
    QDate nd = dt.addDays(55);

    QDate cd = QDate::currentDate();
    int year = cd.year();
    QDate xmas { year, 12, 24 };

    out << "55 days from " << dt.toString() << " is "
        << nd.toString() << endl;
    out << "There are " << QDate::currentDate().daysTo(xmas)
        << " days till Christmas" << endl;
}
~~~
Мы получаем дату на 55 дней позже - 11 мая 2020 года. Мы также получаем количество дней до Рождества.
~~~
QDate dt { 2020, 5, 11 };
QDate nd = dt.addDays(55);
~~~
Метод addDays возвращает QDate, которая находится через 55 дней после заданной даты.
~~~
QDate xmas { год, 12, 24 };
...
out << "До Рождества осталось " << QDate::currentDate().daysTo(xmas)
    << " дней до Рождества" << endl;
~~~
Мы используем метод daysTo для вычисления количества дней до Рождества. 
~~~
$ ./daystofrom 
55 days from Mon May 11 2020 is Sun Jul 5 2020
There are 21 days till Christmas
~~~

Класс Qt5 QDateTime

Объект QDateTime содержит календарную дату и часовое время. Он представляет собой комбинацию классов QDate и QTime. У него много похожих методов, а использование идентично этим двум классам. 
~~~
#include <QTextStream>
#include <QDateTime>

int main(void) {

   QTextStream out(stdout);
   QDateTime cdt = QDateTime::currentDateTime();

   out << "The current datetime is " << cdt.toString() << endl;
   out << "The current date is " << cdt.date().toString() << endl;
   out << "The current time is " << cdt.time().toString() << endl;
}
~~~
В примере извлекается текущее время даты.
~~~
out << "The current datetime is " << cdt.toString() << endl;
~~~
Эта строка кода выводит текущее время на терминал.
~~~
out << "Текущая дата - " << cdt.date().toString() << endl;
~~~
Эта строка извлекает часть даты из объекта datetime с помощью метода date. 
~~~
$ ./datetime 
The current datetime is Thu Dec 3 12:29:42 2020
The current date is Thu Dec 3 2020
The current time is 12:29:42
~~~

## Юлианский день

Юлианские сутки - это непрерывный счет дней с начала юлианского периода. Он используется в основном астрономами. Его не следует путать с юлианским календарем. Юлианский период начался в 4713 году до нашей эры. Номер юлианского дня 0 присвоен дню, начинающемуся в полдень 1 января 4713 года до нашей эры. Номер юлианского дня (JDN) - это количество дней, прошедших с начала этого периода. Юлианская дата (JD) любого момента - это номер юлианского дня для предшествующего полудня плюс доля дня, прошедшего с этого момента. (Qt5 не вычисляет эту дробь.) Помимо астрономии, юлианские даты часто используются в военных программах и программах для мэйнфреймов. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QDate cd = QDate::currentDate();

   out << "Gregorian date for today: " << cd.toString(Qt::ISODate) << endl;
   out << "Julian day for today: " << cd.toJulianDay() << endl;
}
~~~
 В примере мы вычисляем григорианскую дату и юлианский день на сегодня.
~~~
out << << "Julian day for today: " << cd.toJulianDay() << endl;
~~~
Юлианский день возвращается с помощью метода toJulianDay.
~~~
$ ./juliandate 
Gregorian date for today: : 2020-12-03
Julian day for today: 2459187
~~~
С помощью юлианской даты легко производить вычисления. 
~~~
#include <QTextStream>
#include <QDate>

int main(void) {

   QTextStream out(stdout);

   QDate bordate(1812, 9, 7);
   QDate slavdate(1805, 12, 2);

   QDate cd = QDate::currentDate();

   int j_today = cd.toJulianDay();
   int j_borodino = bordate.toJulianDay();
   int j_slavkov = slavdate.toJulianDay();

   out << "Days since Slavkov battle: " << j_today - j_slavkov << endl;
   out << "Days since Borodino battle: " << j_today - j_borodino << endl;
}
~~~
В примере подсчитывается количество дней, прошедших с момента двух исторических событий.
~~~
QDate bordate(1812, 9, 7);
QDate slavdate(1805, 12, 2);
~~~
У нас есть две даты сражений наполеоновской эпохи.
~~~
int j_today = cd.toJulianDay();
int j_borodino = bordate.toJulianDay();
int j_slavkov = slavdate.toJulianDay();
~~~
Вычисляем юлианские дни для сегодняшнего дня и для битв при Славкове и Бородино.
~~~
out << << "Days since Slavkov battle: " << j_today - j_slavkov << endl;
out << "Days since Borodino battle: " << j_today - j_borodino << endl;
~~~
Вычисляем количество дней, прошедших после двух сражений.
~~~
$ дата
Thu 03 Dec 2020 12:33:56 PM CET
$ ./battles 
Days since Slavkov battle: 78529
Days since Borodino battle: 76058
~~~
3 декабря 2020 года прошло 78529 дней со дня Славковского сражения и 76058 дней со дня Бородинского сражения. 


## Время по Гринвичу(UTC)

Наша планета представляет собой сферу. Она вращается вокруг своей оси. Земля вращается в сторону востока. Поэтому Солнце восходит в разное время в разных местах. Земля вращается один раз примерно за 24 часа. Поэтому мир разделен на 24 часовых пояса. В каждом часовом поясе существует свое местное время. Это местное время часто дополнительно изменяется в результате перехода на летнее время.

Существует прагматическая необходимость в едином глобальном времени. Единое глобальное время помогает избежать путаницы в часовых поясах и переходе на летнее время. В качестве основного стандарта времени было выбрано UTC (универсальное координированное время). UTC используется в авиации, при составлении прогнозов погоды, планов полетов, разрешений авиадиспетчеров и карт. В отличие от местного времени, UTC не меняется со сменой времен года. 
~~~
#include <QTextStream>
#include <QDateTime>

int main(void) {

  QTextStream out(stdout);

  QDateTime cdt = QDateTime::currentDateTime();

  out << "Universal datetime: " << cdt.toUTC().toString() << endl;
  out << "Local datetime: " << cdt.toLocalTime().toString() << endl;
}
~~~
В примере мы вычисляем текущее время. Мы выражаем время в формате UTC и локальном времени.
~~~
out << "Universal datetime" << cdt.toUTC().toString() << endl;
~~~
Метод toUTC используется для получения времени UTC.
~~~
out << "Local datetime" << cdt.toLocalTime().toString() << endl;
~~~
Метод toLocalTime используется для получения локального времени.
~~~
$ ./utclocal 
Universal datetime: Thu Dec 3 11:36:19 2020 GMT
Local datetime: Thu Dec 3 12:36:19 2020
~~~
Пример был запущен в Братиславе, где действует центральное европейское время (CET)-UTC + 1 час. 



## Эпоха Unix

Эпоха - это момент времени, выбранный в качестве начала определенной эпохи. Например, в западных христианских странах эпоха времени начинается с 0-го дня, когда родился Иисус. Другой пример - французский республиканский календарь, который использовался в течение двенадцати лет. Эпоха была началом республиканской эры, которая была провозглашена 22 сентября 1792 года, в день провозглашения Первой республики и отмены монархии.

У компьютеров тоже есть свои эпохи. Одна из самых популярных - эпоха Unix. Эпоха Unix - это время 00:00:00 UTC 1 января 1970 года (или 1970-01-01T00:00:00Z ISO 8601). Дата и время на компьютере определяются в соответствии с количеством секунд или тиков часов, прошедших с момента определения эпохи для данного компьютера или платформы.

Время Unix - это количество секунд, прошедших с момента эпохи Unix.
~~~
$ date +%s
1606995554
~~~
Команда Unix date может быть использована для получения времени Unix. В данный конкретный момент с момента эпохи Unix прошло 1606995554 секунды. 
~~~
#include <QTextStream>
#include <QDateTime>
#include <ctime>

int main(void) {

  QTextStream out(stdout);

  time_t t = time(0);
  out << t << endl;

  QDateTime dt;
  dt.setTime_t(t);
  out << dt.toString() << endl;

  QDateTime cd = QDateTime::currentDateTime();
  out << cd.toTime_t() << endl;
}
~~~
В примере мы используем две функции Qt5 для получения времени Unix и преобразования его в человекочитаемую форму.
~~~
#include <ctime>
~~~
Мы включаем стандартный заголовочный файл времени C++.
~~~
time_t t = time(0);
out << t << endl;
~~~
С помощью стандартной функции времени C++ мы получаем время Unix.
~~~
QDateTime dt;
dt.setTime_t(t);
out << dt.toString() << endl;
~~~
Функция setTime_t используется для преобразования времени Unix в DateTime, которое форматируется в человекочитаемую форму.
~~~
QDateTime cd = QDateTime::currentDateTime();
out << cd.toTime_t() << endl;
~~~
Функция Qt5's toTime_t также может быть использована для получения времени Unix.
~~~
$ ./unixepoch 
1606995613
Thu Dec 3 12:40:13 2020
1606995613
~~~
В этой главе мы работали с временем и датой в Qt5. 

