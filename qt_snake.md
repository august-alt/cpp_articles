# Змейка в Qt5

В этой части обучающиего курс а по Qt5, мы создадим клон игры Змейка.

## Змейка

Змейка - это классическая видео игра, Она была создана вконце 70-х годов. Позже, она была перенесена на PC. В этой игре игрок управляет змейкой. Цель - съесть как можно больше яблок. Каждый раз, когда змея съедает яблоко, ее тело становится длиннее. Змейка должна избегать стен и собственного тела. Иногда, эту игру называли Nibbles.

## Разработка

Размер каждого из сегментов змеи составляет 10 пикселей. Змея контролируется с помощью клавиш перемешения курсора. В начале игры, змея состоит из трех сегментов. Когда игра закончена, сообщение "Game Over" высвечивается в середине игрового поля.

~~~
#include <QWidget>
#include <QKeyEvent>

class Snake : public QWidget {

  public:
      Snake(QWidget *parent = nullptr);

  protected:
      void paintEvent(QPaintEvent *);
      void timerEvent(QTimerEvent *);
      void keyPressEvent(QKeyEvent *);

  private:
      QImage dot;
      QImage head;
      QImage apple;

      static const int B_WIDTH = 300;
      static const int B_HEIGHT = 300;
      static const int DOT_SIZE = 10;
      static const int ALL_DOTS = 900;
      static const int RAND_POS = 29;
      static const int DELAY = 140;

      int timerId;
      int dots;
      int apple_x;
      int apple_y;

      int x[ALL_DOTS];
      int y[ALL_DOTS];

      bool leftDirection;
      bool rightDirection;
      bool upDirection;
      bool downDirection;
      bool inGame;

      void loadImages();
      void initGame();
      void locateApple();
      void checkApple();
      void checkCollision();
      void move();
      void doDrawing();
      void gameOver(QPainter &);
};
~~~

Это заголовочный файл.

~~~
static const int B_WIDTH = 300;
static const int B_HEIGHT = 300;
static const int DOT_SIZE = 10;
static const int ALL_DOTS = 900;
static const int RAND_POS = 29;
static const int DELAY = 140;
~~~

Константы B_WiDTH и B_HEIGHT содержат размеры игрового поля. Константа DOT_SIZE содержит размер яблока и сегмента змеи. Константа ALL_DOTS содержит максимальное количество возможных клеток на поле 900 = (300*300)/(10*10). Константа RAND_POS используется для вычисления случайной позиции для яблока. Константа DELAY используется для установки скорости игры.

~~~
int x[ALL_DOTS];
int y[ALL_DOTS];
~~~

Эти два массива содержат x и y координаты всех сегментов змеи.

~~~
#include <QPainter>
#include <QTime>
#include "snake.h"

Snake::Snake(QWidget *parent) : QWidget(parent) {

    setStyleSheet("background-color:black;");
    leftDirection = false;
    rightDirection = true;
    upDirection = false;
    downDirection = false;
    inGame = true;

    setFixedSize(B_WIDTH, B_HEIGHT);
    loadImages();
    initGame();
}

void Snake::loadImages() {

    dot.load("dot.png");
    head.load("head.png");
    apple.load("apple.png");
}

void Snake::initGame() {

    dots = 3;

    for (int z = 0; z < dots; z++) {
        x[z] = 50 - z * 10;
        y[z] = 50;
    }

    locateApple();

    timerId = startTimer(DELAY);
}

void Snake::paintEvent(QPaintEvent *e) {

    Q_UNUSED(e);

    doDrawing();
}

void Snake::doDrawing() {

    QPainter qp(this);

    if (inGame) {

        qp.drawImage(apple_x, apple_y, apple);

        for (int z = 0; z < dots; z++) {
            if (z == 0) {
                qp.drawImage(x[z], y[z], head);
            } else {
                qp.drawImage(x[z], y[z], dot);
            }
        }

    } else {

        gameOver(qp);
    }
}

void Snake::gameOver(QPainter &qp) {

    QString message = "Game over";
    QFont font("Courier", 15, QFont::DemiBold);
    QFontMetrics fm(font);
    int textWidth = fm.horizontalAdvance(message);

    qp.setFont(font);
    int h = height();
    int w = width();

    qp.translate(QPoint(w/2, h/2));
    qp.drawText(-textWidth/2, 0, message);
}

void Snake::checkApple() {

    if ((x[0] == apple_x) && (y[0] == apple_y)) {

        dots++;
        locateApple();
    }
}

void Snake::move() {

    for (int z = dots; z > 0; z--) {
        x[z] = x[(z - 1)];
        y[z] = y[(z - 1)];
    }

    if (leftDirection) {
        x[0] -= DOT_SIZE;
    }

    if (rightDirection) {
        x[0] += DOT_SIZE;
    }

    if (upDirection) {
        y[0] -= DOT_SIZE;
    }

    if (downDirection) {
        y[0] += DOT_SIZE;
    }
}

void Snake::checkCollision() {

    for (int z = dots; z > 0; z--) {

        if ((z > 4) && (x[0] == x[z]) && (y[0] == y[z])) {
            inGame = false;
        }
    }

    if (y[0] >= B_HEIGHT) {
        inGame = false;
    }

    if (y[0] < 0) {
        inGame = false;
    }

    if (x[0] >= B_WIDTH) {
        inGame = false;
    }

    if (x[0] < 0) {
        inGame = false;
    }

    if(!inGame) {
        killTimer(timerId);
    }
}

void Snake::locateApple() {

    QTime time = QTime::currentTime();
    qsrand((uint) time.msec());

    int r = qrand() % RAND_POS;
    apple_x = (r * DOT_SIZE);

    r = qrand() % RAND_POS;
    apple_y = (r * DOT_SIZE);
}

void Snake::timerEvent(QTimerEvent *e) {

    Q_UNUSED(e);

    if (inGame) {

        checkApple();
        checkCollision();
        move();
    }

    repaint();
}

void Snake::keyPressEvent(QKeyEvent *e) {

    int key = e->key();

    if ((key == Qt::Key_Left) && (!rightDirection)) {
        leftDirection = true;
        upDirection = false;
        downDirection = false;
    }

    if ((key == Qt::Key_Right) && (!leftDirection)) {
        rightDirection = true;
        upDirection = false;
        downDirection = false;
    }

    if ((key == Qt::Key_Up) && (!downDirection)) {
        upDirection = true;
        rightDirection = false;
        leftDirection = false;
    }

    if ((key == Qt::Key_Down) && (!upDirection)) {
        downDirection = true;
        rightDirection = false;
        leftDirection = false;
    }

    QWidget::keyPressEvent(e);
}
~~~

Это snake.cpp файл, содержащий логику игры.

~~~
void Snake::loadImages() {

    dot.load("dot.png");
    head.load("head.png");
    apple.load("apple.png");
}
~~~

В методе loadImages мы загружаем изображения для игры. Класс ImageIcon используется для отображения PNG изображений.

~~~
void Snake::initGame() {

    dots = 3;

    for (int z = 0; z < dots; z++) {
        x[z] = 50 - z * 10;
        y[z] = 50;
    }

    locateApple();

    timerId = startTimer(DELAY);
}
~~~

В методе initGame Мы содаем змею, случайно расположенное яблоко на игровом поле и запускаем таймер.

~~~
void Snake::checkApple() {

    if ((x[0] == apple_x) && (y[0] == apple_y)) {

        dots++;
        locateApple();
    }
}
~~~

Если яблоко сталкивается с головой, то мы увеличиваем количество сегментов змеи. Мы вызываем метод locateApple для размещения нового яблока на случайной позиции игрового поля.

В методе move у нас прасполагается ключевой алгоритм игры. Для его понимания, посмотрите, как змея двигается. Мы управляем головой змеи. Мы можем менять направление с помощью клавиш управления курсором. Остальные сегменты перемещаются на одну позицию вверх по цепи. Второй сегмент перемещатся на место первого, третий на место второго и так далее.

~~~
for (int z = dots; z > 0; z--) {
    x[z] = x[(z - 1)];
    y[z] = y[(z - 1)];
}
~~~

Этот код перемещает всю цепь.

~~~
if (leftDirection) {
    x[0] -= DOT_SIZE;
}
~~~

Эти строки кода двигают голову влево.

В методе checkCollizion, мы определяем, стокнулась ли змея со стеной или со своим телом.

~~~
for (int z = dots; z > 0; z--) {

    if ((z > 4) && (x[0] == x[z]) && (y[0] == y[z])) {
        inGame = false;
    }
}
~~~

Если змея сталкивается сегментом с головой, то игра заканчивается.

~~~
if (y[0] >= B_HEIGHT) {
    inGame = false;
}
~~~

Игра заканчивается, если змея стокнулась с нижней стеной игрового поля.

~~~
void Snake::timerEvent(QTimerEvent *e) {

    Q_UNUSED(e);

    if (inGame) {

        checkApple();
        checkCollision();
        move();
    }

    repaint();
}
~~~

Метод timerEvent формирует игровой цикл. При условии, что игра не закончилась  мы выполняем проверку стокновений и движение. Метод repaint вызывает перерисовку окна.

~~~
if ((key == Qt::Key_Left) && (!rightDirection)) {
    leftDirection = true;
    upDirection = false;
    downDirection = false;
}
~~~

Если нажата клавиша движения курсора влево, то устанавливаем переменную leftDirection в true. Эта переменная используется функцией move для изменения координат змеи. Обратите внимание, что когда змея поворачивается вправо, она не может немедленно повернуть налево.

~~~
#include <QApplication>
#include "snake.h"

int main(int argc, char *argv[]) {

  QApplication app(argc, argv);

  Snake window;

  window.setWindowTitle("Snake");
  window.show();

  return app.exec();
}
~~~

Это была игра змейка в Qt5. 

