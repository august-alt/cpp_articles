[исходник](https://zetcode.com/gui/qt5/widgets/)

# Виджеты Qt

В этой части обучающего курса по Qt5 мы будем говорить о базовых виджетах Qt5. Например, QLabel, QSlider, QComboBox, QSpin, QLineEdit, QMainWindow.

Виджеты явялются базовыми блоками для построения приложений с графическим интерфейсом. В библиотеке Qt5 много различных виджетов.

## Виджет QLabel в Qt5

Виджет QLabel используется для отображения текста и изображения. Взаимодействие с пользователем недоступно для этого виджета. Нижеследующий пример отображает текст.

~~~
#pragma once

#include <QWidget>
#include <QLabel>

class Label : public QWidget {

  public:
    Label(QWidget *parent = nullptr);

  private:
    QLabel *label;
};
~~~

Заголовочный файл для нашего примера.

~~~
#include <QVBoxLayout>
#include <QFont>
#include "label.h"

Label::Label(QWidget *parent)
    : QWidget(parent) {

  QString lyrics = "Who doesn't long for someone to hold\n\
Who knows how to love you without being told\n\
Somebody tell me why I'm on my own\n\
If there's a soulmate for everyone\n\
\n\
Here we are again, circles never end\n\
How do I find the perfect fit\n\
There's enough for everyone\n\
But I'm still waiting in line\n\
\n\
Who doesn't long for someone to hold\n\
Who knows how to love you without being told\n\
Somebody tell me why I'm on my own\n\
If there's a soulmate for everyone";

  label = new QLabel(lyrics, this);
  label->setFont(QFont("Purisa", 10));

  auto *vbox = new QVBoxLayout();
  vbox->addWidget(label);
  setLayout(vbox);
}
~~~

Мы используем QLabel виджет для отображения стихотворения в окне.

~~~
label = new QLabel(lyrics, this);
label->setFont(QFont("Purisa", 10))
~~~

Мы создаем виджет QLabel и устанавливаем шрифт для него.

~~~
#include <QApplication>
#include <QTextStream>
#include "label.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Label window;

  window.setWindowTitle("QLabel");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## QSlider в  Qt5

Виджет QSlider имеет простую ручку. Эту ручку можно передвигать назад и вперед. Передвигая ручку, можно выбрать значение для какой либо задачи.

~~~
#pragma once

#include <QWidget>
#include <QSlider>
#include <QLabel>

class Slider : public QWidget {

  Q_OBJECT

  public:
    Slider(QWidget *parent = nullptr);

  private:
    QSlider *slider;
    QLabel *label;
};
~~~

Заголовочный файл для примера.

~~~
#include <QHBoxLayout>
#include "slider.h"

Slider::Slider(QWidget *parent)
    : QWidget(parent) {

  auto *hbox = new QHBoxLayout(this);

  slider = new QSlider(Qt::Horizontal , this);
  hbox->addWidget(slider);

  label = new QLabel("0", this);
  hbox->addWidget(label);

//   connect(slider, &QSlider::valueChanged, label,
//     static_cast<void (QLabel::*)(int)>(&QLabel::setNum));

  connect(slider, &QSlider::valueChanged, label,
    qOverload<int>(&QLabel::setNum));
}
~~~

Мы отображаем два виджета, QSlader и QLabel. Слайдер определяет значение, которое отображает QLabel.

>slider = new QSlider(Qt::Horizontal , this);

Создали горизонтальный QSlider.

>connect(slider, &QSlider::valueChanged, label,
> qOverload<int>(&QLabel::setNum));

В этом коде мы соединяем сигнал о смене значения со встроенным слотом setNum. Поскольку метод setNum перегружен, мы используем qOverload для правильного метода.

~~~
#include <QApplication>
#include "slider.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Slider window;

  window.setWindowTitle("QSlider");
  window.show();

  return app.exec();
}

~~~

Это файл main.

## QComboBox в  Qt5

QComboBox это виджет, который показывает список для выбора опций пользователем, устроенный таким образом, чтобы занимать наименьшее пространство на экране. Это виджет выбора, который отображает текущий элемент и может показать выпадающий список элементов для выбора. QComboBox может быть редактируемым, позволяя пользователю модифицировать каждый элемент в списке.

~~~
#pragma once

#include <QWidget>
#include <QComboBox>
#include <QLabel>

class ComboBoxEx : public QWidget {

  Q_OBJECT

  public:
    ComboBoxEx(QWidget *parent = nullptr);

  private:
    QComboBox *combo;
    QLabel *label;
};
~~~

Мы работаем с двумя элементами: QComboBox и QLabel.

~~~
#include <QHBoxLayout>
#include "combobox.h"

ComboBoxEx::ComboBoxEx(QWidget *parent)
    : QWidget(parent) {

  QStringList distros = {"Arch", "Xubuntu", "Redhat", "Debian",
      "Mandriva"};

  auto *hbox = new QHBoxLayout(this);

  combo = new QComboBox();
  combo->addItems(distros);

  hbox->addWidget(combo);
  hbox->addSpacing(15);

  label = new QLabel("Arch", this);
  hbox->addWidget(label);

  connect(combo, qOverload<const QString &>(&QComboBox::activated),
      label, &QLabel::setText);
}
~~~

В этом примере, выбранный элемент QComboBox отображается в QLabel.

>QStringList distros = {"Arch", "Xubuntu", "Redhat", "Debian",
>    "Mandriva"};

QstringList хранит данные QComboBox. У нас это список дистрибутивов Linux.

>combo = new QComboBox();
>combo->addItems(distros);

QComboBox создан и элементы добавлены в него с помощью addItems метода.

>connect(combo, qOverload<const QString &>(&QComboBox::activated),
>    label, &QLabel::setText);

Сигнал активации QComboBox соединен со слотом setText Qlabel.

~~~
#include <QApplication>
#include "combobox.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  ComboBoxEx window;

  window.resize(300, 150);
  window.setWindowTitle("QComboBox");
  window.show();

  return app.exec();
}
~~~

Файл main для приложения.

## QSpinBox Qt5

QSpinBox это виджет, который используется для установки целочисленных или других наборов дискретных значений. В нашем пример кода, мы используем один виджет QSpinBox. Мы можем выбрать значения от 0 до 99. Текущее выбранное значение мы отображаем в QLabel виджете.

~~~
#pragma once

#include <QWidget>
#include <QSpinBox>

class SpinBox : public QWidget {

  Q_OBJECT

  public:
    SpinBox(QWidget *parent = nullptr);

  private:
    QSpinBox *spinbox;
};
~~~

Заголовочный файл для примера с QSpinBox.

~~~
#include <QHBoxLayout>
#include <QLabel>
#include "spinbox.h"

SpinBox::SpinBox(QWidget *parent)
    : QWidget(parent) {

  auto *hbox = new QHBoxLayout(this);
  hbox->setSpacing(15);

  spinbox = new QSpinBox(this);
  auto *lbl = new QLabel("0", this);

  hbox->addWidget(spinbox);
  hbox->addWidget(lbl);

  connect(spinbox, qOverload<int>(&QSpinBox::valueChanged),
    lbl, qOverload<int>(&QLabel::setNum));
}
~~~

Мы помещаем spinbox в окно и соединяем его сигнал valueChanged со слотом setNum QLabel.

>connect(spinbox, qOverload<int>(&QSpinBox::valueChanged),
>  lbl, qOverload<int>(&QLabel::setNum));

Нам нужно использовать qOverload дважды, потому что оба, сигнал и слот перегружены.

~~~
#include <QApplication>
#include "spinbox.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  SpinBox window;

  window.resize(250, 150);
  window.setWindowTitle("QSpinBox");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## QLineEdit в Qt5

QLineEdit это виджет, который позволяет вводить и редактировать одну строку текста. Функции отметить/повторить, вырезать/вставить и перетащить и вставить доступны для QLineEdit виджета.

В нашем примере мы покажем три QLabel и три QLineEdit.

~~~
#pragma once

#include <QWidget>

class Ledit : public QWidget {

  public:
    Ledit(QWidget *parent = nullptr);
};
~~~

Это заголовочный файл.

~~~
#include <QGridLayout>
#include <QLabel>
#include <QLineEdit>
#include "ledit.h"

Ledit::Ledit(QWidget *parent)
    : QWidget(parent) {

  auto *name = new QLabel("Name:", this);
  name->setAlignment(Qt::AlignRight | Qt::AlignVCenter);

  auto *age = new QLabel("Age:", this);
  age->setAlignment(Qt::AlignRight | Qt::AlignVCenter);

  auto *occupation = new QLabel("Occupation:", this);
  occupation->setAlignment(Qt::AlignRight | Qt::AlignVCenter);

  auto *le1 = new QLineEdit(this);
  auto *le2 = new QLineEdit(this);
  auto *le3 = new QLineEdit(this);

  auto *grid = new QGridLayout();

  grid->addWidget(name, 0, 0);
  grid->addWidget(le1, 0, 1);
  grid->addWidget(age, 1, 0);
  grid->addWidget(le2, 1, 1);
  grid->addWidget(occupation, 2, 0);
  grid->addWidget(le3, 2, 1);

  setLayout(grid);
}
~~~

Мы отображаем три QLabеl и три QLineEdit. Эти виджеты упорядочены с помощью QGridLayout менеджера.

~~~
#include "ledit.h"
#include <QApplication>

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Ledit window;

  window.setWindowTitle("QLineEdit");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## StatusBar

StatusBar - это панель, которая отображает текущую информацию о приложении.

В насшем примере, у нас есть две кнопки и панель текущей информации StatusBar. Каждая кнопка показывает сообщение, если мы на нее нажмем. StatusBar виджет, это часть QMAinWindow виджета.

~~~
#pragma once

#include <QMainWindow>
#include <QPushButton>

class Statusbar : public QMainWindow {

  Q_OBJECT

  public:
    Statusbar(QWidget *parent = nullptr);

  private slots:
    void OnOkPressed();
    void OnApplyPressed();

  private:
    QPushButton *okBtn;
    QPushButton *aplBtn;
};
~~~

Загловочный файл для примера.

~~~
#include <QLabel>
#include <QFrame>
#include <QStatusBar>
#include <QHBoxLayout>
#include "statusbar.h"

Statusbar::Statusbar(QWidget *parent)
    : QMainWindow(parent) {

  auto *frame = new QFrame(this);
  setCentralWidget(frame);

  auto *hbox = new QHBoxLayout(frame);

  okBtn = new QPushButton("OK", frame);
  hbox->addWidget(okBtn, 0, Qt::AlignLeft | Qt::AlignTop);

  aplBtn = new QPushButton("Apply", frame);
  hbox->addWidget(aplBtn, 1, Qt::AlignLeft | Qt::AlignTop);

  statusBar();

  connect(okBtn, &QPushButton::clicked, this, &Statusbar::OnOkPressed);
  connect(aplBtn, &QPushButton::clicked, this, &Statusbar::OnApplyPressed);
}

void Statusbar::OnOkPressed() {

  statusBar()->showMessage("OK button pressed", 2000);
}

void Statusbar::OnApplyPressed() {

 statusBar()->showMessage("Apply button pressed", 2000);
}
~~~

Это файл statusbar.cpp.

>auto *frame = new QFrame(this);
>setCentralWidget(frame);

QFrame виджет помещен в центральную часть QMainWindow виджета. В центральной части может быть только один виджет.

>okBtn = new QPushButton("OK", frame);
>hbox->addWidget(okBtn, 0, Qt::AlignLeft | Qt::AlignTop);

>aplBtn = new QPushButton("Apply", frame);
>hbox->addWidget(aplBtn, 1, Qt::AlignLeft | Qt::AlignTop);

Мы создаем два QPushButton виджета и распоалагаем их в горизональном порядке. Родитель кнопок - frame виджет.

>statusBar();

Для отображения виджета statusbar, мы вызываем метод statusBar виджета QMainWidget

>void Statusbar::OnOkPressed() {
>
>  statusBar()->showMessage("OK button pressed", 2000);
>}

Метод showMessage показывает сообщение в statusBar. Последний параметр указывает количество миллисекунд, в течение которых сообщение будет отображаться в statusbar.

~~~
#include <QApplication>
#include "statusbar.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Statusbar window;

  window.resize(300, 200);
  window.setWindowTitle("QStatusBar");
  window.show();

  return app.exec();
}
~~~

Это файл main.

В этой части обучающего курса, мы представили некоторые виджеты Qt5.
