# Рисование в Qt5

В этой части обучающего курса по программированию на Qt5 мы будем немного рисовать.

Класс QPainter, это инструмент для рисования в Qt5. Рисование выполняется с помощью метода paintEvent.

## Линии

В первом примере, мы нарисуем немного линий в клиентской области окна.

~~~
#pragma once

#include <QWidget>

class Lines : public QWidget {

  public:
    Lines(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *event);
    void drawLines(QPainter *qp);
};
~~~

Это заголовочный файл

~~~
#include <QPainter>
#include "lines.h"

Lines::Lines(QWidget *parent)
    : QWidget(parent)
{ }

void Lines::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);
  
  QPainter qp(this);
  drawLines(&qp);
}

void Lines::drawLines(QPainter *qp) {
  
  QPen pen(Qt::black, 2, Qt::SolidLine);  
  qp->setPen(pen);
  qp->drawLine(20, 40, 250, 40);

  pen.setStyle(Qt::DashLine);
  qp->setPen(pen);
  qp->drawLine(20, 80, 250, 80);

  pen.setStyle(Qt::DashDotLine);
  qp->setPen(pen);
  qp->drawLine(20, 120, 250, 120);

  pen.setStyle(Qt::DotLine);
  qp->setPen(pen);
  qp->drawLine(20, 160, 250, 160);

  pen.setStyle(Qt::DashDotDotLine);
  qp->setPen(pen);
  qp->drawLine(20, 200, 250, 200);

  QVector<qreal> dashes;
  qreal space = 4;

  dashes << 1 << space << 5 << space;

  pen.setStyle(Qt::CustomDashLine);
  pen.setDashPattern(dashes);
  
  qp->setPen(pen);
  qp->drawLine(20, 240, 250, 240);
}
~~~

Мы рисуем шесть линий в окне, каждая линия имеет свой стиль рисования.

~~~
void Lines::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);
  
  QPainter qp(this);
  drawLines(&qp);
}
~~~

Метод paintEvent вызывается, когда виджет обновляется. Здесь мы создаем объект QPainter и рисуем. Поскольку мы не используем объект QPaintEvent, мы подавляем предупреждение компилятора макросом Q_UNUSED. Рисование делегировано методу drawLines.

~~~
QPen pen(Qt::black, 2, Qt::SolidLine);
qp->setPen(pen);
~~~

Создаем объект QPen. Ручка для рисования сплошная, толщиной 2 пикселя и имеет черный цвет. Ручка используется для рисования линий и фигур. Ручка передается объекту с помощью setPen метода.

~~~
qp->drawLine(20, 40, 250, 40);
~~~

Метод setStyle класса QPen устанавливает стиль рисования Qt::DashLine.

~~~
#include <QApplication>
#include "lines.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  Lines window;
  
  window.resize(280, 270);
  window.setWindowTitle("Lines");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## Цвета

Цвет представлен комбинацией интенсивности Красного, Зеленого и Голубого (RGB) компонентов. Значения RGB могут быть в диапазоне от 0 до 255. В следующем примере, мы нарисуем девять прямоугольников, закрашенных разными цветами.

~~~
#pragma once 

#include <QWidget>

class Colours : public QWidget {
    
  public:
    Colours(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *e);
    
  private:
    void doPainting();  
};
~~~

Это заголовочный файл.

~~~
#include <QPainter>
#include "colours.h"

Colours::Colours(QWidget *parent)
    : QWidget(parent)
{ }

void Colours::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);  
  
  doPainting();
}

void Colours::doPainting() {
    
  QPainter painter(this);
  painter.setPen(QColor("#d4d4d4"));

  painter.setBrush(QBrush("#c56c00"));
  painter.drawRect(10, 15, 90, 60);

  painter.setBrush(QBrush("#1ac500"));
  painter.drawRect(130, 15, 90, 60);

  painter.setBrush(QBrush("#539e47"));
  painter.drawRect(250, 15, 90, 60);
  
  painter.setBrush(QBrush("#004fc5"));
  painter.drawRect(10, 105, 90, 60);

  painter.setBrush(QBrush("#c50024"));
  painter.drawRect(130, 105, 90, 60);

  painter.setBrush(QBrush("#9e4757"));
  painter.drawRect(250, 105, 90, 60);

  painter.setBrush(QBrush("#5f3b00"));
  painter.drawRect(10, 195, 90, 60);

  painter.setBrush(QBrush("#4c4c4c"));
  painter.drawRect(130, 195, 90, 60);

  painter.setBrush(QBrush("#785f36"));
  painter.drawRect(250, 195, 90, 60);
}
~~~

Мы рисуем девять прямоугольников разными цветами, внешний прямоугольник серый.

~~~
painter.setBrush(QBrush("#c56c00"));
painter.drawRect(10, 15, 90, 60);
~~~

Класс QBrush объявляет шаблоны заполнения фигур, нарисованных QPainter. Метод drawRect рисует прямоугольник. Он рисует прямоугольник от верхнего левого угла с координатами x,y с заданной шириной и высотой. Мы используем шестнадцатеричное представление для обозначения цвета.

~~~
#include <QApplication>
#include "colours.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  Colours window;
  
  window.resize(360, 280);
  window.setWindowTitle("Colours");
  window.show();
  
  return app.exec();
}
~~~

Это файл main.

## Шаблоны (паттерны)

Следующий пример кода похож на предыдущий. В этот раз мы заполним прямоугольники с помощью разных шаблонов.

~~~
#pragma once

#include <QWidget>

class Patterns : public QWidget {
    
  public:
    Patterns(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *e);

  private:
    void doPainting();
};
~~~

Это заголовочный файл.

~~~
#include <QApplication>
#include <QPainter>
#include "patterns.h"

Patterns::Patterns(QWidget *parent)
    : QWidget(parent)
{ }

void Patterns::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);  
  
  doPainting();
}

void Patterns::doPainting() {
    
  QPainter painter(this);
  painter.setPen(Qt::NoPen);

  painter.setBrush(Qt::HorPattern);
  painter.drawRect(10, 15, 90, 60);

  painter.setBrush(Qt::VerPattern);
  painter.drawRect(130, 15, 90, 60);

  painter.setBrush(Qt::CrossPattern);
  painter.drawRect(250, 15, 90, 60);
  
  painter.setBrush(Qt::Dense7Pattern);
  painter.drawRect(10, 105, 90, 60);

  painter.setBrush(Qt::Dense6Pattern);
  painter.drawRect(130, 105, 90, 60);

  painter.setBrush(Qt::Dense5Pattern);
  painter.drawRect(250, 105, 90, 60);

  painter.setBrush(Qt::BDiagPattern);
  painter.drawRect(10, 195, 90, 60);

  painter.setBrush(Qt::FDiagPattern);
  painter.drawRect(130, 195, 90, 60);

  painter.setBrush(Qt::DiagCrossPattern);
  painter.drawRect(250, 195, 90, 60);
}
~~~

Мы рисуем девять прямоугольников разными кистями с разными шаблонами.

~~~
painter.setBrush(Qt::HorPattern);
painter.drawRect(10, 15, 90, 60);
~~~

Мы рисуем прямоугольник с указанным заполнением. Qt::HorPattern - константа, успользуемая для создания шаблона горизонтальных линий.

~~~
#include <QDesktopWidget>
#include <QApplication>
#include "patterns.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  Patterns window;
  
  window.resize(350, 280);
  window.setWindowTitle("Patterns");
  window.show();

  return app.exec();
}
~~~

Это main файл.

## Прозрачные треугольники

Прозрачность - это возможность видеть через материал. Самый легкий путь понять, что такое прозрачность, это представить кусок стекла или воды. Технически, лучи света могут пройти через стекло и поэтому, мы можем видеть объекты за стеклом.

В компьютерной графике, мы можем реализовать эффект прозрачности, используя альфа композитинг. Это процесс комбинирования образа с его фоном для создания эффекта частичной прозрачности. Этот процесс использует альфа канал.

~~~
#pragma once

#include <QWidget>

class TransparentRectangles : public QWidget {

  public:
    TransparentRectangles(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *event);
    void doPainting();
};
~~~

Это заголовочный файл.

~~~
#include <QApplication>
#include <QPainter>
#include <QPainterPath>
#include "transparent_rectangles.h"

TransparentRectangles::TransparentRectangles(QWidget *parent)
    : QWidget(parent)
{ }

void TransparentRectangles::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);
  
  doPainting();
}

void TransparentRectangles::doPainting() {
    
  QPainter painter(this);
  
  for (int i=1; i<=11; i++) {
    painter.setOpacity(i*0.1);
    painter.fillRect(50*i, 20, 40, 40, Qt::darkGray);
  }    
}
~~~

Этот пример рисует десять прямоугольников с разными уровнями прозрачности

~~~
painter.setOpacity(i*0.1);
~~~

Метод setOpacity устанавливает прозрачность. Значение должно быть в пределах от 0 до 1.0, где 0.0 - полная прозрачность, 1.0 - полная непрозрачность.

~~~
#include <QDesktopWidget>
#include <QApplication>
#include "transparent_rectangles.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  TransparentRectangles window;

  window.resize(630, 90);  
  window.setWindowTitle("Transparent rectangles");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## Пончик

А следующем примере мы создадим фигуру в форме пончика.

~~~
#pragma once

#include <QWidget>

class Donut : public QWidget {
    
  public:
    Donut(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *e);
    
  private:
    void doPainting();  
};
~~~

Это заголовочный файл.

~~~
#include <QApplication>
#include <QPainter>
#include "donut.h"

Donut::Donut(QWidget *parent)
    : QWidget(parent)
{ }

void Donut::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);

  doPainting();
}

void Donut::doPainting() {
  
  QPainter painter(this);

  painter.setPen(QPen(QBrush("#535353"), 0.5));
  painter.setRenderHint(QPainter::Antialiasing);

  int h = height();
  int w = width();

  painter.translate(QPoint(w/2, h/2));

  for (qreal rot=0; rot < 360.0; rot+=5.0 ) {
      painter.drawEllipse(-125, -40, 250, 80);
      painter.rotate(5.0);
  }
}
~~~

"Пончик" - усложненная геометрическая фигура, напоминающая съедобный пончик. Мы создаем ее, рисуем 72 повернутых эллипса.

~~~
painter.setRenderHint(QPainter::Antialiasing);
~~~

Мы будем присовать в режиме сглаживания. Рендеринг будет более качественным.

~~~
int h = height();
int w = width();

painter.translate(QPoint(w/2, h/2));
~~~

Эти линии идут от начала координат в середину окна. По умолчанию, оно находится по координатам 0,0. Другими словами, в левом верхнем углу. Двигая координатную систему, рисование будет намного легче.

~~~
for (qreal rot=0; rot < 360.0; rot+=5.0 ) {
    painter.drawEllipse(-125, -40, 250, 80);
    painter.rotate(5.0);
}
~~~

В этом цикле, мы рисуем 72 повернутых эллипсов.

~~~
#include <QDesktopWidget>
#include <QApplication>
#include "donut.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);
    
  Donut window;

  window.resize(350, 280);
  window.setWindowTitle("Donut");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## Фигуры

API Qt5 для рисования могут отрисовывать разные фигуры. Нижеследующий пример кода, покажет некоторые из них.

~~~
#pragma once

#include <QWidget>

class Shapes : public QWidget {
    
  public:
    Shapes(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *e);

  private:
    void doPainting();
};
~~~

Это заголовочный файл.

~~~
#include <QApplication>
#include <QPainter>
#include <QPainterPath>
#include "shapes.h"

Shapes::Shapes(QWidget *parent)
    : QWidget(parent)
{ }

void Shapes::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);

  doPainting();
}

void Shapes::doPainting() {
  
  QPainter painter(this);

  painter.setRenderHint(QPainter::Antialiasing);
  painter.setPen(QPen(QBrush("#888"), 1));
  painter.setBrush(QBrush(QColor("#888")));

  QPainterPath path1;

  path1.moveTo(5, 5);
  path1.cubicTo(40, 5,  50, 50,  99, 99);
  path1.cubicTo(5, 99,  50, 50,  5, 5);
  painter.drawPath(path1);  

  painter.drawPie(130, 20, 90, 60, 30*16, 120*16);
  painter.drawChord(240, 30, 90, 60, 0, 16*180);
  painter.drawRoundRect(20, 120, 80, 50);

  QPolygon polygon({QPoint(130, 140), QPoint(180, 170), QPoint(180, 140),
      QPoint(220, 110), QPoint(140, 100)});

  painter.drawPolygon(polygon);

  painter.drawRect(250, 110, 60, 60);

  QPointF baseline(20, 250);
  QFont font("Georgia", 55);
  QPainterPath path2;
  path2.addText(baseline, font, "Q");
  painter.drawPath(path2);

  painter.drawEllipse(140, 200, 60, 60);
  painter.drawEllipse(240, 200, 90, 60);
}
~~~

Мы рисуем девять разных фигур.

~~~
QPainterPath path1;

path1.moveTo(5, 5);
path1.cubicTo(40, 5,  50, 50,  99, 99);
path1.cubicTo(5, 99,  50, 50,  5, 5);
painter.drawPath(path1);
~~~

QPainterPath - это объект, используемый для создания сложных фигур. Мы используем его для рисования кривых Безье.

~~~
painter.drawPie(130, 20, 90, 60, 30*16, 120*16);
painter.drawChord(240, 30, 90, 60, 0, 16*180);
painter.drawRoundRect(20, 120, 80, 50);
~~~

Это код рисует сектор, хорду и прямоугольник со скругленными углами.

~~~
QPolygon polygon({QPoint(130, 140), QPoint(180, 170), QPoint(180, 140),
    QPoint(220, 110), QPoint(140, 100)});

painter.drawPolygon(polygon);
~~~

Мы рисуем многоугольник с помощью метода drawPolygon. Многоугольник состоит из пяти точек.

~~~
QPointF baseline(20, 250);
QFont font("Georgia", 55);
QPainterPath path2;
path2.addText(baseline, font, "Q");
painter.drawPath(path2);
~~~

Qt5 позволяет создавать путь на базе символа.

~~~
painter.drawEllipse(140, 200, 60, 60);
painter.drawEllipse(240, 200, 90, 60);
~~~

Метод drawEllipse рисует эллипс и окружность одинаково хорошо. Окружность - частный случай эллипса. Параметры x и y  координаты начала прямоугольника с шириной и высотой ограничивающего эллипс.

~~~
#include <QDesktopWidget>
#include <QApplication>
#include "shapes.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  Shapes window;

  window.resize(350, 280);  
  window.setWindowTitle("Shapes");
  window.show();

  return app.exec();
}
~~~

Это main файл примера.

## Градиенты

В компьютерной графике, градиент это плавный переход от светлого к темному, от одного цвета к другому. В двухмерных графических программах, градиенты используются для создания цветных фонов и специальных эффектов также хорошо, как и для света и теней.

Следующий пример кода показывает как создавать линейные градиенты.

~~~
#pragma once

#include <QWidget>

class LinearGradients : public QWidget {

  public:
    LinearGradients(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *e);
    
  private:
    void doPainting();  
};
~~~

Это заголовочный файл.

~~~
#include <QApplication>
#include <QPainter>
#include "linear_gradients.h"

LinearGradients::LinearGradients(QWidget *parent)
    : QWidget(parent)
{ }

void LinearGradients::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);
  
  doPainting();
}  

void LinearGradients::doPainting() {
         
  QPainter painter(this);
  
  QLinearGradient grad1(0, 20, 0, 110);

  grad1.setColorAt(0.1, Qt::black);
  grad1.setColorAt(0.5, Qt::yellow);
  grad1.setColorAt(0.9, Qt::black);

  painter.fillRect(20, 20, 300, 90, grad1);

  QLinearGradient grad2(0, 55, 250, 0);

  grad2.setColorAt(0.2, Qt::black);
  grad2.setColorAt(0.5, Qt::red);
  grad2.setColorAt(0.8, Qt::black);

  painter.fillRect(20, 140, 300, 100, grad2);
}
~~~

В этом примере, мы рисуем два прямоугольника и заполняем их линейным градиентом.

~~~
QLinearGradient grad1(0, 20, 0, 110);
~~~

QLineGradient создает линейный градиент с помощью интерполяции области между двух точек из параметров.

~~~
grad1.setColorAt(0.1, Qt::black);
grad1.setColorAt(0.5, Qt::yellow);
grad1.setColorAt(0.9, Qt::black);
~~~

Цвета градиента определены используя стоп-точки. Метод setColorAt создает стоп-точки на заданных позициях с заданным цветом.

~~~
painter.fillRect(20, 20, 300, 90, grad1);
~~~

Мы заполняем прямоугольник градиентом.

~~~
#include <QApplication>
#include "linear_gradients.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  LinearGradients window;

  window.resize(350, 260);  
  window.setWindowTitle("Linear gradients");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## Радиальный градиент

Радиальный градиент - это смешение цветов и оттенков между двух окружностей.

~~~
#pragma once

#include <QWidget>

class RadialGradient : public QWidget {

  public:
    RadialGradient(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *e);
    
  private:
    void doPainting();  
};
~~~

Это заголовочный файл.

~~~
#include <QApplication>
#include <QPainter>
#include "radial_gradient.h"

RadialGradient::RadialGradient(QWidget *parent)
    : QWidget(parent)
{ }

void RadialGradient::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);
  
  doPainting();
}

void RadialGradient::doPainting() {
  
  QPainter painter(this);
  
  int h = height();
  int w = width();

  QRadialGradient grad1(w/2, h/2, 80);

  grad1.setColorAt(0, QColor("#032E91"));
  grad1.setColorAt(0.3, Qt::white);
  grad1.setColorAt(1, QColor("#032E91"));

  painter.fillRect(0, 0, w, h, grad1);
}
~~~

Этот пример создает радиальный градиент, градиент распространяется от центра окна.

~~~
QRadialGradient grad1(w/2, h/2, 80);
~~~

QRadialGradient создает радиальный градиент, он интерполирует цвета между фокусной точкой и окружностью, окружающей ее. Параметры, это координаты центра окружности и ее радиус. Центральная точка находится в центре окружности.

~~~
grad1.setColorAt(0, QColor("#032E91"));
grad1.setColorAt(0.3, Qt::white);
grad1.setColorAt(1, QColor("#032E91"));
~~~

Метод setColorAt устанавливает конечный цвет.

~~~
painter.fillRect(0, 0, w, h, grad1);
~~~

Вся область окна заполнена радиальным градиентом.

~~~
#include <QApplication>
#include "radial_gradient.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  RadialGradient window;

  window.resize(300, 250);  
  window.setWindowTitle("Radial gradient");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## Эффект puff

В последнем примере этой главы учебного курса мы создадим эффект puff. Этот пример отобразит увеличивающийся текст, возрастающий из опредленной точки. Это очень частый эффект, который часто используется во флэш анимациях в сети Интернет.

~~~
#pragma once

#include <QWidget>

class Puff : public QWidget {

  public:
    Puff(QWidget *parent = 0);

  protected:
    void paintEvent(QPaintEvent *event);
    void timerEvent(QTimerEvent *event);

  private:
    int x;
    qreal opacity;
    int timerId;
    
    void doPainting();
};
~~~

В заголовочном файле мы определяем два обработчика: paint even обработчик и timer обработчик.

~~~
#include <QPainter>
#include <QTimer>
#include <QTextStream>
#include "puff.h"

Puff::Puff(QWidget *parent)
    : QWidget(parent) {
        
  x = 1;
  opacity = 1.0;
  timerId = startTimer(15);
}

void Puff::paintEvent(QPaintEvent *e) {
    
  Q_UNUSED(e);  
  
  doPainting();
}

void Puff::doPainting() {
  
  QPainter painter(this);
  QTextStream out(stdout);

  QString text = "ZetCode";

  painter.setPen(QPen(QBrush("#575555"), 1));

  QFont font("Courier", x, QFont::DemiBold);
  QFontMetrics fm(font);
  int textWidth = fm.width(text);

  painter.setFont(font);

  if (x > 10) {
    opacity -= 0.01;
    painter.setOpacity(opacity);
  }

  if (opacity <= 0) {
    killTimer(timerId);
    out << "timer stopped" << endl;
  }

  int h = height();
  int w = width();

  painter.translate(QPoint(w/2, h/2));
  painter.drawText(-textWidth/2, 0, text);
}

void Puff::timerEvent(QTimerEvent *e) {
    
  Q_UNUSED(e);
  
  x += 1;
  repaint();
}
~~~

Это файл puff.cpp

~~~
Puff::Puff(QWidget *parent)
    : QWidget(parent) {
        
  x = 1;
  opacity = 1.0;
  timerId = startTimer(15);
}
~~~

В конструкторе, мы запускаем таймер, и каждые 15 мс таймер генерирует событие.

~~~
void Puff::timerEvent(QTimerEvent *e) {
    
  Q_UNUSED(e);
  
  x += 1;
  repaint();
}
~~~

В методе timerEvent мы увеличиваем размер шрифта и перерисовываем виджет.

~~~
if (x > 10) {
  opacity -= 0.01;
  painter.setOpacity(opacity);
}
~~~

Если размер шрифта больше чем 10, мы постепенно увеличиваем прозрачность, и текст начинает исчезать.


~~~
if (opacity <= 0) {
  killTimer(timerId);
  out << "timer stopped" << endl;
}
~~~

Когда текст исчезнет совсем, мы удаляем таймер.

~~~
#include <QApplication>
#include "puff.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv); 

  Puff window;

  window.resize(350, 280);
  window.setWindowTitle("Puff");
  window.show();

  return app.exec();
}
~~~

Это файл main.

Это была глава о рисовании в Qt5.

