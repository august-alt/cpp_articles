# Пользовательский виджет в Qt5

В этой части обучающего курса по Qt5, мы создадим пользовательский виджет.

Большинство наборов инструментов обычно предоставляют только самые распространенные виджеты, такие как кнопки, текстовые виджеты или ползунки (слайдеры). Но, ни один набор инструментов не предосталяет все возможные виджеты.. Программисты должны сами создавать такие виджеты. Они делают это, используя инструменты для рисования, доступные в этом наборе инструментов. Есть две возможности: программист может модифицировать или расширить сузествующий виджет или он может создать свой виджет с нуля.

## Горящий виджет

В следующем примере мы создадим пользовательский горящий виджет. Это твиджет вы могли видеть в таких приложениях, как Nero или K3B. Виджет будет создан с нуля.

~~~
#pragma once

#include <QWidget>
#include <QSlider>
#include <QFrame>
#include "widget.h"

class Application : public QFrame {

  Q_OBJECT

  public:
    Application(QWidget *parent = nullptr);

  private:
    QSlider *slider;
    Widget *widget;

    void initUI();
};
~~~

Это заголовочный файл для главного окна примера.

~~~
#include <QVBoxLayout>
#include <QHBoxLayout>
#include "application.h"

Application::Application(QWidget *parent)
    : QFrame(parent) {

  initUI();
}

void Application::initUI() {

  const int MAX_VALUE = 750;

  slider = new QSlider(Qt::Horizontal , this);
  slider->setMaximum(MAX_VALUE);
  slider->setGeometry(50, 50, 170, 30);

  widget = new Widget(this);
  connect(slider, &QSlider::valueChanged, widget, &Widget::setValue);

  auto *vbox = new QVBoxLayout(this);
  auto *hbox = new QHBoxLayout();

  vbox->addStretch(1);
  hbox->addWidget(widget, 0);
  vbox->addLayout(hbox);

  setLayout(vbox);
}
~~~

Здесь мы строим главное окно приложения.

~~~
connect(slider, &QSlider::valueChanged, widget, &Widget::setValue);
~~~

Когда мы передвигаем ползунок, вызывается слот valueChanged.

~~~
#pragma once

#include <QFrame>

class Widget : public QFrame {

  Q_OBJECT;

  public:
    Widget(QWidget *parent = nullptr);

  public slots:
    void setValue(int);

  protected:
    void paintEvent(QPaintEvent *e);
    void drawWidget(QPainter &qp);

  private:
    int cur_width;

    static constexpr int DISTANCE = 19;
    static constexpr int LINE_WIDTH = 5;
    static constexpr int DIVISIONS = 10;
    static constexpr float FULL_CAPACITY = 700;
    static constexpr float MAX_CAPACITY = 750;
};
~~~

Это заголовочный файл пользовательского горящего виджета.

~~~
private:
  int cur_width;
~~~

Переменная cur_width содержит текущее значение слайдера. Это значение используется, когда рисуется горящий виджет.

~~~
static constexpr int DISTANCE = 19;
static constexpr int LINE_WIDTH = 5;
static constexpr int DIVISIONS = 10;
static constexpr float FULL_CAPACITY = 700;
static constexpr float MAX_CAPACITY = 750;
~~~

Это важные константы. DISTANCE -  это дистанция чисел на шкале от верхнего уровня границы родителя. LINE_WIDTH - ширина вертикальной линии. DIVISIONS - количество частей на шкале. FULL_CAPACITY - размер медиа. После того, как он достигнут, возникает переполнение. Это показыватеся красным цветом. MAX_CAPACITY - это максимальная вместимость носителя.

~~~
#include <QtGui>
#include "widget.h"

const int PANEL_HEIGHT = 30;

Widget::Widget(QWidget *parent)
    : QFrame(parent), cur_width(0) {

  setMinimumHeight(PANEL_HEIGHT);
}

void Widget::setValue(int value)
{
  cur_width = value;
  repaint();
}

void Widget::paintEvent(QPaintEvent *e) {

  QPainter qp(this);
  drawWidget(qp);

  QFrame::paintEvent(e);
}

void Widget::drawWidget(QPainter &qp) {

  QString num[] = { "75", "150", "225", "300", "375", "450",
    "525", "600", "675" };

  int asize = sizeof(num)/sizeof(num[1]);

  QColor redColor(255, 175, 175);
  QColor yellowColor(255, 255, 184);

  int width = size().width();

  int step = (int) qRound((double)width / DIVISIONS);
  int till = (int) ((width / MAX_CAPACITY) * cur_width);
  int full = (int) ((width / MAX_CAPACITY) * FULL_CAPACITY);

  if (cur_width >= FULL_CAPACITY) {

    qp.setPen(yellowColor);
    qp.setBrush(yellowColor);
    qp.drawRect(0, 0, full, 30);
    qp.setPen(redColor);
    qp.setBrush(redColor);
    qp.drawRect(full, 0, till-full, PANEL_HEIGHT);

  } else if (till > 0) {

    qp.setPen(yellowColor);
    qp.setBrush(yellowColor);
    qp.drawRect(0, 0, till, PANEL_HEIGHT);
  }

  QColor grayColor(90, 80, 60);
  qp.setPen(grayColor);

  for (int i=1; i <=asize; i++) {

    qp.drawLine(i*step, 0, i*step, LINE_WIDTH);
    QFont newFont = font();
    newFont.setPointSize(8);
    setFont(newFont);

    QFontMetrics metrics(font());

    int w = metrics.horizontalAdvance(num[i-1]);
    qp.drawText(i*step-w/2, DISTANCE, num[i-1]);
  }
}
~~~

Здесь мы рисуем пользовательский виджет. Мы рисуем прямоугольник, вертикальные линии и числа.

~~~
void Widget::setValue(int value)
{
  cur_width = value;
  repaint();
}
~~~

Мысохраняем выбранное значение в переменную cur_width и перерисовываем виджет.

~~~
void Widget::paintEvent(QPaintEvent *e) {

  QPainter qp(this);
  drawWidget(qp);

  QFrame::paintEvent(e);
}
~~~

Отрисовку пользовательского виджета делегируем методу drawWidget.

~~~
QString num[] = { "75", "150", "225", "300", "375", "450",
  "525", "600", "675" };
~~~

Мы используем эти числа для построения шкалы Горящего виджета.

~~~
int width = size().width();
~~~

мы получаем ширину виджета. Ширина пользовательского виджета динамическая. Ее размер может быть изменен пользователем.

~~~
int till = (int) ((width / MAX_CAPACITY) * cur_width);
int full = (int) ((width / MAX_CAPACITY) * FULL_CAPACITY);
~~~

Мы используем переменную width для трансформации, между значениями шкалы и единицами измерения пользовательского виджета.

~~~
qp.setPen(redColor);
qp.setBrush(redColor);
qp.drawRect(full, 0, till-full, PANEL_HEIGHT);
~~~

Эти три линии рисуют прямоугольник, показывающий процесс записи.

~~~
qp.drawLine(i*step, 0, i*step, LINE_WIDTH);
~~~

Здесь мы рисуем маленькие вертикальные линии.

~~~
QFontMetrics metrics(font());

int w = metrics.horizontalAdvance(num[i-1]);
qp.drawText(i*step-w/2, DISTANCE, num[i-1]);
~~~

Здесь мы рисуем числа на шкале. Для определения позиции чисел, мы должныполучить ширину строки.

~~~
#include <QApplication>
#include "application.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);
  Application window;

  window.resize(370, 200);
  window.setWindowTitle("The Burning widget");
  window.show();

  return app.exec();
}
~~~

Это файл main.

В этой части обучающего курса по Qt5, мы создали пользовательский виджет.
