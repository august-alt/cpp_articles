[источник](https://zetcode.com/gui/qt5/firstprograms/ "взято отсюда")

# Первая программа в Qt5 #

В этой части учебника по программированию на Qt5, мы создадим свои первые программы.

Мы отобразим подсказку и разные курсоры мыши. Мы центрируем окно на экране и представим механизм сигналов и слотов.

## Простой пример ##

Мы начнем с очень простого примера.

~~~
#include <QApplication>
#include <QWidget>

int main(int argc, char *argv[]) {

    QApplication app(argc, argv);

    QWidget window;

    window.resize(250, 150);
    window.setWindowTitle("Simple example");
    window.show();

    return app.exec();
}
~~~

Этот пример показывает обычное окно на экране.

~~~
#include <QApplication>
#include <QWidget>
~~~

Включаем необходимые заголовочные файлы.

~~~
QApplication app(argc, argv);
~~~

Это объект приложения. Каждое приложение на Qt5 должно создать такой объект. (Исключая приложения командной строки.)

~~~
QWidget window;
~~~

Наш главный виджет.

~~~
window.resize(250, 150);
window.setWindowTitle("Simple example");
window.show();
~~~

Здесь мы меняем размеры виджета и устанавливаем заголовок главного окна. В нашем случае, QWidget это наше главное окно. В конце, мы показываем виджет на экране.

~~~
return app.exec();
~~~

Метод exec запускает главный цикл приложения.

## Всплывающая подсказка ##

Всплывающая подсказка - это специфическая подсказка для элемента в приложении. Нижеследующий пример продемонстрирует, как мы можем создавать всплывающие подсказки с помощью программной библиотеки в Qt5.

~~~
#include <QApplication>
#include <QWidget>

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  QWidget window;

  window.resize(350, 250);
  window.setWindowTitle("ToolTip");
  window.setToolTip("QWidget");
  window.show();

  return app.exec();
}
~~~

Пример показывает всплывающую подсказку для главного QWidget.

~~~
window.setWindowTitle("ToolTip");
~~~

Устанавливаем всплывающую подсказку для виджета QWidget с помощью метода setToolTip.

## Курсоры в Qt5 ##

Курсор - это маленькая иконка, которая показывает положение указателя мыши. В следующем примере мы покажем разные курсоры, которые мы можем использовать в своих программах.

~~~
#include <QApplication>
#include <QWidget>
#include <QFrame>
#include <QGridLayout>

class Cursors : public QWidget {

 public:
     Cursors(QWidget *parent = nullptr);
};

Cursors::Cursors(QWidget *parent)
    : QWidget(parent) {

  auto *frame1 = new QFrame(this);
  frame1->setFrameStyle(QFrame::Box);
  frame1->setCursor(Qt::SizeAllCursor);

  auto *frame2 = new QFrame(this);
  frame2->setFrameStyle(QFrame::Box);
  frame2->setCursor(Qt::WaitCursor);

  auto *frame3 = new QFrame(this);
  frame3->setFrameStyle(QFrame::Box);
  frame3->setCursor(Qt::PointingHandCursor);

  auto *grid = new QGridLayout(this);
  grid->addWidget(frame1, 0, 0);
  grid->addWidget(frame2, 0, 1);
  grid->addWidget(frame3, 0, 2);

  setLayout(grid);
}

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Cursors window;

  window.resize(350, 150);
  window.setWindowTitle("Cursors");
  window.show();

  return app.exec();
}
~~~

В этом примере, мы используем три фрейма. У каждого фрейма установлены разные курсоры.

~~~
auto *frame1 = new QFrame(this);
~~~

QFrame виджет создан.

~~~
frame1->setFrameStyle(QFrame::Box);
~~~

Курсор для фрейма устанавливается с помощью метода setCursor.

~~~
auto *grid = new QGridLayout(this);
grid->addWidget(frame1, 0, 0);
grid->addWidget(frame2, 0, 1);
grid->addWidget(frame3, 0, 2);
setLayout(grid);
~~~

## QPushButton Qt5 ##

В следующем примере кода, мы отобразим кнопку в окне. По нажатию этой кнопки, мы закрываем приложение.

~~~
#include <QApplication>
#include <QWidget>
#include <QPushButton>

class MyButton : public QWidget {

 public:
     MyButton(QWidget *parent = nullptr);
};

MyButton::MyButton(QWidget *parent)
    : QWidget(parent) {

  auto *quitBtn = new QPushButton("Quit", this);
  quitBtn->setGeometry(50, 40, 75, 30);

  connect(quitBtn, &QPushButton::clicked, qApp, &QApplication::quit);
}

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  MyButton window;

  window.resize(250, 150);
  window.setWindowTitle("QPushButton");
  window.show();

  return app.exec();
}
~~~

В этом примере кода, мы используем концепцию сигналов и слотов в первый раз.

~~~
auto *quitBtn = new QPushButton("Quit", this);
quitBtn->setGeometry(50, 40, 75, 30);
~~~

Мы создаем новую кнопку QPushButton. Вручную мы задаем ей размер и располагаем ее в окне с помощью метода setGeometry.

~~~
connect(quitBtn, &QPushButton::clicked, qApp, &QApplication::quit);
~~~

Когда мы нажимаем на кнопку, генерируется сигнал нажатия. Слот - это метод, который реагирует на сигнал. В нашем случае, это слот quit объекта приложения. qApp - это глобальный указатель на объект приложения. Он определен в заголовочном файле QApplication.

## Плюс и минус ##

Мы закончим этот раздел, показав, как виджеты могут взаимодействовать. Этот код разеделен на три файла.

~~~
#pragma once

#include <QWidget>
#include <QApplication>
#include <QPushButton>
#include <QLabel>

class PlusMinus : public QWidget {

  Q_OBJECT

  public:
    PlusMinus(QWidget *parent = nullptr);

  private slots:
    void OnPlus();
    void OnMinus();

  private:
    QLabel *lbl;
};
~~~

Это пример заголовочного файла. В нем мы определили два слота и виджет label.

~~~
class PlusMinus : public QWidget {

  Q_OBJECT
...
~~~

Макрос Q_OBJECT должен быть включен в классы, которые определяют собственные сигналы и слоты.

~~~
#include "plusminus.h"
#include <QGridLayout>

PlusMinus::PlusMinus(QWidget *parent)
    : QWidget(parent) {

  auto *plsBtn = new QPushButton("+", this);
  auto *minBtn = new QPushButton("-", this);
  lbl = new QLabel("0", this);

  auto *grid = new QGridLayout(this);
  grid->addWidget(plsBtn, 0, 0);
  grid->addWidget(minBtn, 0, 1);
  grid->addWidget(lbl, 1, 1);

  setLayout(grid);

  connect(plsBtn, &QPushButton::clicked, this, &PlusMinus::OnPlus);
  connect(minBtn, &QPushButton::clicked, this, &PlusMinus::OnMinus);
}

void PlusMinus::OnPlus() {

  int val = lbl->text().toInt();
  val++;
  lbl->setText(QString::number(val));
}

void PlusMinus::OnMinus() {

  int val = lbl->text().toInt();
  val--;
  lbl->setText(QString::number(val));
}
~~~

Мы создали две кнопки и текстовый виджет. Мы увеличиваем или уменьшаем число в текстовом виджете с помощью кнопок.

~~~
connect(plsBtn, &QPushButton::clicked, this, &PlusMinus::OnPlus);
connect(minBtn, &QPushButton::clicked, this, &PlusMinus::OnMinus);
~~~

Здесь мы соединяем сигналы нажатия со слотами.

~~~
void PlusMinus::OnPlus() {

  int val = lbl->text().toInt();
  val++;
  lbl->setText(QString::number(val));
}
~~~

В методе OnPlus, мы определяем текущее занчение текстового поля. Текстовый виджет отображает строковое значение, поэтому мы должны его преобразовать в числовое. Мы увеличиваем число и устанавливаем новое значение для текстового виджета. Мы преобразуем числов в строковое значение.

~~~
#include "plusminus.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  PlusMinus window;

  window.resize(300, 190);
  window.setWindowTitle("Plus minus");
  window.show();

  return app.exec();
}
~~~

Это пример главного файла.

В этой главе мы создали свою первую программу на Qt5.

