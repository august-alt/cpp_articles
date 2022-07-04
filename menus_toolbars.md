[исходник](https://zetcode.com/gui/qt5/menusandtoolbars/ "взято отсюда")

# Меню и панель инструментов в Qt5 #

В этой части обучающего курса по программированию на C++ в Qt5, мы поговорим о меню и панелях инструментов в приложениях Qt5. 

Панель меню - это обязательная часть приложения с графическим интерфейсом. Группа команд, расположенных в разных местах, называется меню. Меню группируют команды, которые мы можем использовать в приложении. Панели инструментов позволяют получать быстрый доступ к наиболее часто используемым командам.

## Простое меню в Qt5 ##

Первый пример показывает нам простое меню.

~~~
#pragma once

#include <QMainWindow>
#include <QApplication>

class SimpleMenu : public QMainWindow {

  public:
    SimpleMenu(QWidget *parent = nullptr);
};
~~~

Это заголовочный файл к нашему примеру кода.

~~~
#include <QMenu>
#include <QMenuBar>
#include "simple_menu.h"

SimpleMenu::SimpleMenu(QWidget *parent)
    : QMainWindow(parent) {

  auto *quit = new QAction("&Quit", this);

  QMenu *file = menuBar()->addMenu("&File");
  file->addAction(quit);

  connect(quit, &QAction::triggered, qApp, QApplication::quit);
}
~~~

У нас есть панель меню, меню и действия. Для работы с меню мы должны наследоваться от QMainWindow виджета.

~~~
auto *quit = new QAction("&Quit", this);
~~~

В этом коде мы создаем QAction. Каждый объект QMenu имеет одно или несколько объектов действий QAction.

~~~
QMenu *file;
file = menuBar()->addMenu("&File");
~~~

Создаем объект QMenu.

~~~
file->addAction(quit);
~~~

Добавляем действие к меню используя метод addAction.

~~~
connect(quit, &QAction::triggered, qApp, QApplication::quit)
~~~

Когда мы выбираем этот пункт меню, то приложение завершает работу.

~~~
#include "simple_menu.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  SimpleMenu window;

  window.resize(350, 250);
  window.setWindowTitle("Simple menu");
  window.show();

  return app.exec();
}
~~~

Файл main.

## Иконки, ярлыки, разделители ##

В следующем примере, мы расширим наше приложение. Мы добавим иконки к меню, ярлыки и разделители.

~~~
#pragma once

#include <QMainWindow>
#include <QApplication>

class AnotherMenu : public QMainWindow {

  public:
    AnotherMenu(QWidget *parent = nullptr);
};
~~~

Заголовочный файл примера.

~~~
#include <QMenu>
#include <QMenuBar>
#include "another_menu.h"

AnotherMenu::AnotherMenu(QWidget *parent)
    : QMainWindow(parent) {

  QPixmap newpix("new.png");
  QPixmap openpix("open.png");
  QPixmap quitpix("quit.png");

  auto *newa = new QAction(newpix, "&New", this);
  auto *open = new QAction(openpix, "&Open", this);
  auto *quit = new QAction(quitpix, "&Quit", this);
  quit->setShortcut(tr("CTRL+Q"));

  QMenu *file = menuBar()->addMenu("&File");
  file->addAction(newa);
  file->addAction(open);
  file->addSeparator();
  file->addAction(quit);

  qApp->setAttribute(Qt::AA_DontShowIconsInMenus, false);

  connect(quit, &QAction::triggered, qApp, &QApplication::quit);
}
~~~

В нашем примере, у нас есть одно меню с тремя действиями. Только действие выхода из приложения что-то делает, если мы вибираем его. Так же мы создаем разделитель и "горячую клавишу" CTRL+Q, которая прерывает выполнение приложения.

~~~
QPixmap newpix("new.png");
QPixmap openpix("open.png");
QPixmap quitpix("quit.png");
~~~

Эти изображения мы используем в меню. Обратите внимание, что в некоторых окружениях, изображения в меню могут не отображаться.

~~~
auto *newa = new QAction(newpix, "&New", this);
auto *open = new QAction(openpix, "&Open", this);
auto *quit = new QAction(quitpix, "&Quit", this);
~~~

В это коде мы используем конструктор QAction с объектами QPixmap в качестве первого аргумента.

~~~
quit->setShortcut(tr("CTRL+Q"));
~~~

Здесь мы создаем и "горячую клавишу". Нажимая клавиатурную комбинацию, мы запускаем действие выхода из приложения.

~~~
file->addSeparator();
~~~

Мы создаем разделитель. Разделитель - это горизонтальная линия, которая позволяет нам группировать пункты меню в некоторые логические группы.

~~~
qApp->setAttribute(Qt::AA_DontShowIconsInMenus, false);
~~~

В некоторых окружениях, иконки меню не показывается по умолчанию. В таком случае, мы можем отключить атрибут Qt::AA_DontShowIconsInMenus.

~~~
#include "another_menu.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  AnotherMenu window;

  window.resize(350, 250);
  window.setWindowTitle("Another menu");
  window.show();

  return app.exec();
}
~~~

Главный файл.

## Меню с возможностью отметки ##

В следующем примере, мы создадим меню с возможностью отмечать пункты. Это действия чекбокса. Переключать будем видимость статусной строки.

~~~
#pragma once

#include <QMainWindow>
#include <QApplication>

class Checkable : public QMainWindow {

  Q_OBJECT

  public:
    Checkable(QWidget *parent = nullptr);

  private slots:
    void toggleStatusbar();

  private:
    QAction *viewst;
};
~~~

Заголовочный файл примера.

~~~
#include <QMenu>
#include <QMenuBar>
#include <QStatusBar>
#include "checkable.h"

Checkable::Checkable(QWidget *parent)
    : QMainWindow(parent) {

  viewst = new QAction("&View statusbar", this);
  viewst->setCheckable(true);
  viewst->setChecked(true);

  QMenu *file = menuBar()->addMenu("&File");
  file->addAction(viewst);

  statusBar();

  connect(viewst, &QAction::triggered, this, &Checkable::toggleStatusbar);
}

void Checkable::toggleStatusbar() {

  if (viewst->isChecked()) {

      statusBar()->show();
  } else {

      statusBar()->hide();
  }
}
~~~

Меню с возможностью отметки, переключает видимость строки статуса.

~~~
viewst = new QAction("&View statusbar", this);
viewst->setCheckable(true);
viewst->setChecked(true);
~~~

Мы создали команду и сделали ее отмечаемой с помощью метода setCheckable. Метод setChecked отмечает ее.

~~~
if (viewst->isChecked()) {

    statusBar()->show();
} else {

    statusBar()->hide();
~~~

Внутри метода toggleStatusbar, мы определяем, если пункт меню отмечен и прячем или показывает строку статуса соответственно.

~~~
#include "checkable.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Checkable window;

  window.resize(350, 250);
  window.setWindowTitle("Checkable menu");
  window.show();

  return app.exec();
}
~~~

Это главный файл.

## Qt5 QToolBar ##

Класс QToolBar реализует передвигаемую панель для быстрого доступа к набору различных действий приложения.

~~~
#pragma once

#include <QMainWindow>
#include <QApplication>

class Toolbar : public QMainWindow {

  Q_OBJECT

  public:
    Toolbar(QWidget *parent = nullptr);
};
~~~

Заголовочный файл примера.

~~~
#include <QToolBar>
#include <QIcon>
#include <QAction>
#include "toolbar.h"

Toolbar::Toolbar(QWidget *parent)
    : QMainWindow(parent) {

  QPixmap newpix("new.png");
  QPixmap openpix("open.png");
  QPixmap quitpix("quit.png");

  QToolBar *toolbar = addToolBar("main toolbar");
  toolbar->addAction(QIcon(newpix), "New File");
  toolbar->addAction(QIcon(openpix), "Open File");
  toolbar->addSeparator();

  QAction *quit = toolbar->addAction(QIcon(quitpix),
      "Quit Application");

  connect(quit, &QAction::triggered, qApp, &QApplication::quit);
}
~~~

Для создания панели инструментов, мы наследуемся от QMainWindow класса.

~~~
QToolBar *toolbar = addToolBar("main toolbar");
~~~

Метод addToolBar для создает и панель инструментов и возвращает указатель на нее.

~~~
toolbar->addAction(QIcon(newpix), "New File");
toolbar->addAction(QIcon(openpix), "Open File");
toolbar->addSeparator();
~~~

Добавляем для действия и разделитель на панель инструментов.

~~~
#include "toolbar.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Toolbar window;

  window.resize(350, 250);
  window.setWindowTitle("QToolBar");
  window.show();

  return app.exec();
}
~~~

Это главный файл приложения.

## Скелет приложения Qt5 ##

В конце этой части учебника по C++ Qt5, мы создадим скелет приложения. Этот пример большей частью основан на виджете QMainWindow.

~~~
#pragma once

#include <QMainWindow>
#include <QApplication>

class Skeleton : public QMainWindow {

  Q_OBJECT

  public:
    Skeleton(QWidget *parent = nullptr);
};
~~~

Заголовочный файл для примера.

~~~
#include <QToolBar>
#include <QIcon>
#include <QAction>
#include <QMenu>
#include <QMenuBar>
#include <QStatusBar>
#include <QTextEdit>
#include "skeleton.h"

Skeleton::Skeleton(QWidget *parent)
    : QMainWindow(parent) {

  QPixmap newpix("new.png");
  QPixmap openpix("open.png");
  QPixmap quitpix("quit.png");

  auto *quit = new QAction("&Quit", this);

  QMenu *file = menuBar()->addMenu("&File");
  file->addAction(quit);

  connect(quit, &QAction::triggered, qApp, &QApplication::quit);

  QToolBar *toolbar = addToolBar("main toolbar");
  toolbar->addAction(QIcon(newpix), "New File");
  toolbar->addAction(QIcon(openpix), "Open File");
  toolbar->addSeparator();

  QAction *quit2 = toolbar->addAction(QIcon(quitpix),
      "Quit Application");
  connect(quit2, &QAction::triggered, qApp, &QApplication::quit);

  auto *edit = new QTextEdit(this);

  setCentralWidget(edit);

  statusBar()->showMessage("Ready");
}
~~~

Здесь мы создаем меню, панель инструментов и строку статуса.

~~~
auto *edit = new QTextEdit(this);

setCentralWidget(edit);
~~~

Мы создаем QTextEdit виджет и располагаем его в центрральной части QMainWindow виджета.

~~~
#include "skeleton.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Skeleton window;

  window.resize(450, 350);
  window.setWindowTitle("Application skeleton");
  window.show();

  return app.exec();
}
~~~

Это главный файл.

В этой части учебника по Qt5, мы изучили меню и панели инструментов.

