[исходник](https://zetcode.com/gui/qt5/breakoutgame/)

# Игра Breakout  в Qt5

В этой части обучающего курса по Qt5, мы создадим клон простой игры Breakout.

Игра Breakout - это аркадная игра, созданная Atari Inc. Игра была создана в 1976 году. В этой игре, игрок двигает веслом и отбивает мяч. Цель - уничтожить блоки в верхней части окна.

## Разработка

В нашей игре, мы имееем одно весло, один мяч и тридцать блоков. Таймер использутеся для создания цикл игры. Мы не работаем с углами, мы просто меняем направление, вверх, вниз, влево и вправо. Создание этого кода было вдохновлено игрой PyBreakout, которая была разработана с помощью библиотеки PyGame Натаном Доусоном.

Игра очень простая. Там не бонусов, уровней или очков. И так она проще для понимания.

Библиотека Qt5 была разработана для приложений. Тем не менее, ее можно использовать и для создания игр. Создание компьютерный игр, это отличный способ узнать больше про Qt5.

~~~
#include <QImage>
#include <QRect>

class Paddle {

  public:
    Paddle();
    ~Paddle();

  public:
    void resetState();
    void move();
    void setDx(int);
    QRect getRect();
    QImage & getImage();

  private:
    QImage image;
    QRect rect;
    int dx;
    static const int INITIAL_X = 200;
    static const int INITIAL_Y = 360;
};
~~~

Это заголовочный файл для объекта весла. Переменные INITIAL_X  и INITINAL_Y - константы, которые определяют изначальное положение весла.

~~~
#include <iostream>
#include "paddle.h"

Paddle::Paddle() {
    
  dx = 0;    
  image.load("paddle.png");

  rect = image.rect();
  resetState();
}

Paddle::~Paddle() {
    
 std::cout << ("Paddle deleted") << std::endl;
}

void Paddle::setDx(int x) {
  dx = x;
}

void Paddle::move() {
    
    int x = rect.x() + dx;
    int y = rect.top();
    
    rect.moveTo(x, y);
}

void Paddle::resetState() {
    
  rect.moveTo(INITIAL_X, INITIAL_Y);
}

QRect Paddle::getRect() {
    
  return rect;
}

QImage & Paddle::getImage() {
    
  return image;
}
~~~

Весло может двигаться влево или вправо.

~~~


Paddle::Paddle() {
    
  dx = 0;    
  image.load("paddle.png");

  rect = image.rect();
  resetState();
}
~~~

В конструкторе, мы инициализируем переменную dx и загружаем изображение весла. Мы получаем прамоугольник с изображеением и передвигаем изображение в начальную позицию.

~~~
void Paddle::move() {
    
    int x = rect.x() + dx;
    int y = rect.top();
    
    rect.moveTo(x, y);
}
~~~

Метод move передвигает прямоугольник с изображением весла. Направление передвижения управляется переменной dx.

~~~
void Paddle::resetState() {
    
  rect.moveTo(INITIAL_X, INITIAL_Y);
}
~~~

Метод resetState передвигает весло в начальную позицию.

~~~
#include <QImage>
#include <QRect>

class Brick {

  public:
    Brick(int, int);
    ~Brick();

  public:
    bool isDestroyed();
    void setDestroyed(bool);
    QRect getRect();
    void setRect(QRect);
    QImage & getImage();

  private:
    QImage image;
    QRect rect;
    bool destroyed;
};
~~~

Это заголовочный файл для объекта блока. Если блок уничтожен, то переменная destroyed устанавливается в true.

~~~
Brick::Brick(int x, int y) {
    
  image.load("brickie.png");
  destroyed = false;
  rect = image.rect();
  rect.translate(x, y);
}

Brick::~Brick() {

  std::cout << ("Brick deleted") << std::endl;
}

QRect Brick::getRect() {
    
  return rect;
}

void Brick::setRect(QRect rct) {
    
  rect = rct;
}

QImage & Brick::getImage() {
    
  return image;
}

bool Brick::isDestroyed() {
    
  return destroyed;
}

void Brick::setDestroyed(bool destr) {
    
  destroyed = destr;
}
~~~

Класс Brick представляет объект блока.

~~~
Brick::Brick(int x, int y) {
    
  image.load("brickie.png");
  destroyed = false;
  rect = image.rect();
  rect.translate(x, y);
}
~~~

Конструктор класса Brick загружает изображение, устанавливает флаг  destroyed, и передвигает изображение в начальную позицию.

~~~
bool Brick::isDestroyed() {
    
  return destroyed;
}
~~~

Объект класса Brick имеет флаг destroyed. Если флаг установлен, блок не отображается на экране.

~~~
#include <QImage>
#include <QRect>

class Ball {

  public:
    Ball();
    ~Ball();

  public:
    void resetState();
    void autoMove();
    void setXDir(int);
    void setYDir(int);
    int getXDir();
    int getYDir();
    QRect getRect();
    QImage & getImage();
  
  private:
    int xdir;
    int ydir;
    QImage image;
    QRect rect;
    static const int INITIAL_X = 230;
    static const int INITIAL_Y = 355;    
    static const int RIGHT_EDGE = 300;
};
~~~

Это заголовочный файл для объекта мяча. Переменные xdir и ydir хранят направление движения мяча.

~~~
#include <iostream>
#include "ball.h"

Ball::Ball() {

  xdir = 1;
  ydir = -1;

  image.load("ball.png");

  rect = image.rect();
  resetState();
}

Ball::~Ball() {
    
  std::cout << ("Ball deleted") << std::endl;
}

void Ball::autoMove() {
    
  rect.translate(xdir, ydir);

  if (rect.left() == 0) {
    xdir = 1;
  }

  if (rect.right() == RIGHT_EDGE) {
    xdir = -1;
  }

  if (rect.top() == 0) {
    ydir = 1;
  }
}

void Ball::resetState() {
    
  rect.moveTo(INITIAL_X, INITIAL_Y);
}

void Ball::setXDir(int x) {
    
  xdir = x;
}

void Ball::setYDir(int y) {
    
  ydir = y;
}

int Ball::getXDir() {
    
  return xdir;
}

int Ball::getYDir() {
    
  return ydir;
}

QRect Ball::getRect() {
    
  return rect;
}

QImage & Ball::getImage() {
    
  return image;
}
~~~

Класс Ball представляет объект мяча.

~~~
xdir = 1;
ydir = -1;
~~~

В начале, мяч двигается в северо-западном направлении.

~~~
void Ball::autoMove() {
    
  rect.translate(xdir, ydir);

  if (rect.left() == 0) {
    xdir = 1;
  }

  if (rect.right() == RIGHT_EDGE) {
    xdir = -1;
  }

  if (rect.top() == 0) {
    ydir = 1;
  }
}
~~~

Метод autoMove вызывается кажды игровой цикл для движения мяча на экране. Если он достигает границы, он изменяет направление. Если мяч проходит через нижний край, то игра заканчивается. 

~~~
#include <QWidget>
#include <QKeyEvent>
#include "ball.h"
#include "brick.h"
#include "paddle.h"

class Breakout : public QWidget {
    
  public:
    Breakout(QWidget *parent = 0);
    ~Breakout();

  protected:
    void paintEvent(QPaintEvent *);
    void timerEvent(QTimerEvent *);
    void keyPressEvent(QKeyEvent *);
    void keyReleaseEvent(QKeyEvent *);
    void drawObjects(QPainter *);
    void finishGame(QPainter *, QString);
    void moveObjects();

    void startGame();
    void pauseGame();
    void stopGame();
    void victory();
    void checkCollision();

  private:
    int x;
    int timerId;
    static const int N_OF_BRICKS = 30;
    static const int DELAY = 10;
    static const int BOTTOM_EDGE = 400;
    Ball *ball;
    Paddle *paddle;
    Brick *bricks[N_OF_BRICKS];
    bool gameOver;
    bool gameWon;
    bool gameStarted;
    bool paused;
};
~~~

Это заголовоный файл для объекта Breakout.

~~~
void keyPressEvent(QKeyEvent *);
void keyReleaseEvent(QKeyEvent *);
~~~

Весло контролирутся с помощью клавиш для перемещения курсора. В игре, мы слушаем события нажатия и отпускания клавиш.

~~~
int x;
int timerId;
~~~

Переменная x хранит текущую позицию весла. Переменная timerId используется для идентификации объекта таймера. Это необходимо для реализации паузы в игре.

~~~
static const int N_OF_BRICKS = 30;
~~~

Переменная-константа N_OF_BRICKS храният количество блоков в игре.

~~~
static const int DELAY = 10;
~~~

Константа DELAY  определяет скорость в игре.

~~~
static const int BOTTOM_EDGE = 400;
~~~

Когда мяч проходит нижнюю границу, игра заканчивается.

~~~
Ball *ball;
Paddle *paddle;
Brick *bricks[N_OF_BRICKS];
~~~

Игра состоит из мяча, весла и массива блоков.

~~~
bool gameOver;
bool gameWon;
bool gameStarted;
bool paused;
~~~

Эти четрые переменных отображают текущий статус игры.

~~~
#include <QPainter>
#include <QApplication>
#include "breakout.h"

Breakout::Breakout(QWidget *parent)
    : QWidget(parent) {
  
  x = 0;
  gameOver = false;
  gameWon = false;
  paused = false;
  gameStarted = false;
  ball = new Ball();
  paddle = new Paddle();

  int k = 0;
  
  for (int i=0; i<5; i++) {
    for (int j=0; j<6; j++) {
      bricks[k] = new Brick(j*40+30, i*10+50);
      k++; 
    }
  }  
}

Breakout::~Breakout() {
    
 delete ball;
 delete paddle;
 
 for (int i=0; i<N_OF_BRICKS; i++) {
   delete bricks[i];
 }
}

void Breakout::paintEvent(QPaintEvent *e) {
  
  Q_UNUSED(e);  
    
  QPainter painter(this);

  if (gameOver) {
  
    finishGame(&painter, "Game lost");    

  } else if(gameWon) {

    finishGame(&painter, "Victory");
  }
  else {
      
    drawObjects(&painter);
  }
}

void Breakout::finishGame(QPainter *painter, QString message) {
    
  QFont font("Courier", 15, QFont::DemiBold);
  QFontMetrics fm(font);
  int textWidth = fm.width(message);

  painter->setFont(font);
  int h = height();
  int w = width();

  painter->translate(QPoint(w/2, h/2));
  painter->drawText(-textWidth/2, 0, message);    
}

void Breakout::drawObjects(QPainter *painter) {
    
  painter->drawImage(ball->getRect(), ball->getImage());
  painter->drawImage(paddle->getRect(), paddle->getImage());

  for (int i=0; i<N_OF_BRICKS; i++) {
    if (!bricks[i]->isDestroyed()) {
      painter->drawImage(bricks[i]->getRect(), bricks[i]->getImage());
    }
  }      
}

void Breakout::timerEvent(QTimerEvent *e) {
    
  Q_UNUSED(e);  
    
  moveObjects();
  checkCollision();
  repaint();
}

void Breakout::moveObjects() {

  ball->autoMove();
  paddle->move();
}

void Breakout::keyReleaseEvent(QKeyEvent *e) {
    
    int dx = 0;
    
    switch (e->key()) {
        case Qt::Key_Left:
            dx = 0;
            paddle->setDx(dx);        
            break;       
            
        case Qt::Key_Right:
            dx = 0;
            paddle->setDx(dx);        
            break;    
    }
}

void Breakout::keyPressEvent(QKeyEvent *e) {
    
    int dx = 0;
    
    switch (e->key()) {
    case Qt::Key_Left:
        
        dx = -1;
        paddle->setDx(dx);
        
        break;
       
    case Qt::Key_Right:
    
        dx = 1;
        paddle->setDx(dx);        
        break;
    
    case Qt::Key_P:
    
        pauseGame();
        break;
        
    case Qt::Key_Space:

        startGame();
        break;        
                
    case Qt::Key_Escape:
        
        qApp->exit();
        break;
        
    default:
        QWidget::keyPressEvent(e);
    }
}

void Breakout::startGame() {
     
  if (!gameStarted) {
    ball->resetState();
    paddle->resetState();

    for (int i=0; i<N_OF_BRICKS; i++) {
      bricks[i]->setDestroyed(false);
    }
    
    gameOver = false; 
    gameWon = false; 
    gameStarted = true;
    timerId = startTimer(DELAY);  
  }      
}

void Breakout::pauseGame() {
    
  if (paused) {
      
    timerId = startTimer(DELAY);
    paused = false;
  } else {
      
    paused = true;
    killTimer(timerId); 
  }        
}

void Breakout::stopGame() {
    
  killTimer(timerId);    
  gameOver = true;      
  gameStarted = false;
}

void Breakout::victory() {
    
  killTimer(timerId);    
  gameWon = true;  
  gameStarted = false;    
}

void Breakout::checkCollision() {
  
  if (ball->getRect().bottom() > BOTTOM_EDGE) {
    stopGame();
  }

  for (int i=0, j=0; i<N_OF_BRICKS; i++) {
      
    if (bricks[i]->isDestroyed()) {
      j++;
    }
    
    if (j == N_OF_BRICKS) {
      victory();
    }
  }

  if ((ball->getRect()).intersects(paddle->getRect())) {

    int paddleLPos = paddle->getRect().left();  
    int ballLPos = ball->getRect().left();   

    int first = paddleLPos + 8;
    int second = paddleLPos + 16;
    int third = paddleLPos + 24;
    int fourth = paddleLPos + 32;

    if (ballLPos < first) {
      ball->setXDir(-1);
      ball->setYDir(-1);
    }

    if (ballLPos >= first && ballLPos < second) {
      ball->setXDir(-1);
      ball->setYDir(-1*ball->getYDir());
    }

    if (ballLPos >= second && ballLPos < third) {
       ball->setXDir(0);
       ball->setYDir(-1);
    }

    if (ballLPos >= third && ballLPos < fourth) {
       ball->setXDir(1);
       ball->setYDir(-1*ball->getYDir());
    }

    if (ballLPos > fourth) {
      ball->setXDir(1);
      ball->setYDir(-1);
    }
  }      
 
  for (int i=0; i<N_OF_BRICKS; i++) {
      
    if ((ball->getRect()).intersects(bricks[i]->getRect())) {

      int ballLeft = ball->getRect().left();  
      int ballHeight = ball->getRect().height(); 
      int ballWidth = ball->getRect().width();
      int ballTop = ball->getRect().top();  
  
      QPoint pointRight(ballLeft + ballWidth + 1, ballTop);
      QPoint pointLeft(ballLeft - 1, ballTop);  
      QPoint pointTop(ballLeft, ballTop -1);
      QPoint pointBottom(ballLeft, ballTop + ballHeight + 1);  

      if (!bricks[i]->isDestroyed()) {
        if(bricks[i]->getRect().contains(pointRight)) {
           ball->setXDir(-1);
        } 

        else if(bricks[i]->getRect().contains(pointLeft)) {
           ball->setXDir(1);
        } 

        if(bricks[i]->getRect().contains(pointTop)) {
           ball->setYDir(1);
        } 

        else if(bricks[i]->getRect().contains(pointBottom)) {
           ball->setYDir(-1);
        } 

        bricks[i]->setDestroyed(true);
      }
    }
  }
}
~~~

Это breakout.cpp файл, здесь логика игры.

~~~
int k = 0;
for (int i=0; i<5; i++) {
  for (int j=0; j<6; j++) {
    bricks[k] = new Brick(j*40+30, i*10+50);
    k++; 
  }
}
~~~

Конструктор класса Breakout, где мы инициализируем тридцать блоков.

~~~
void Breakout::paintEvent(QPaintEvent *e) {
  
  Q_UNUSED(e);  
    
  QPainter painter(this);

  if (gameOver) {
  
    finishGame(&painter, "Game lost");    

  } else if(gameWon) {

    finishGame(&painter, "Victory");
  }
  else {
      
    drawObjects(&painter);
  }
}
~~~

В зависимости от переменных gameOver и gameWon, мы заканчиваем игру с сообщением или рисуем объеткы игры в окне.

~~~
void Breakout::finishGame(QPainter *painter, QString message) {
    
  QFont font("Courier", 15, QFont::DemiBold);
  QFontMetrics fm(font);
  int textWidth = fm.width(message);

  painter->setFont(font);
  int h = height();
  int w = width();

  painter->translate(QPoint(w/2, h/2));
  painter->drawText(-textWidth/2, 0, message);    
}
~~~

Метод finishGame показывает финальное сообщение в центре окна. Это или "Game lost" или "Victory". Метод width, объекта QFontMetrics используется для вычисления ширины символа.

~~~
void Breakout::drawObjects(QPainter *painter) {
    
  painter->drawImage(ball->getRect(), ball->getImage());
  painter->drawImage(paddle->getRect(), paddle->getImage());

  for (int i=0; i<N_OF_BRICKS; i++) {
    if (!bricks[i]->isDestroyed()) {
      painter->drawImage(bricks[i]->getRect(), bricks[i]->getImage());
    }
  }      
}
~~~

Метод drawObjects используется для отрисовки объектов игры на экране: мяча, весла и блоков. Объекты представлены изображениями и метод drawImage рисует их в окне.

~~~
void Breakout::moveObjects() {

  ball->autoMove();
  paddle->move();
}
~~~

Метод moveObjects передвигает объекты мяча и весла. Вызывается их собственный метод move.

~~~
void Breakout::keyReleaseEvent(QKeyEvent *e) {
    
    int dx = 0;
    
    switch (e->key()) {
        case Qt::Key_Left:
            dx = 0;
            paddle->setDx(dx);        
            break;       
            
        case Qt::Key_Right:
            dx = 0;
            paddle->setDx(dx);        
            break;    
    }
}
~~~

Когда игрок отпускает клавишу перемещения курсора влево или вправо, мы устанавливаем переменную dx в ноль. Как следствие, весло останавливает движение.

~~~
void Breakout::keyPressEvent(QKeyEvent *e) {
    
    int dx = 0;
    
    switch (e->key()) {
    case Qt::Key_Left:
        
        dx = -1;
        paddle->setDx(dx);
        
        break;
       
    case Qt::Key_Right:
    
        dx = 1;
        paddle->setDx(dx);        
        break;
    
    case Qt::Key_P:
    
        pauseGame();
        break;
        
    case Qt::Key_Space:

        startGame();
        break;        
                
    case Qt::Key_Escape:
        
        qApp->exit();
        break;
        
    default:
        QWidget::keyPressEvent(e);
    }
}
~~~

В методе keyPressedEvent, мы слушаем события нажатия клавиш, нужных для нашей игры. Клавиши перемещения курсора влево или вправо передвигают объект весла. Они устанавливают переменную dx, которая складывается с координатой x  весла. Клавиша P устанавливает на паузу игру. Клавиша пробел начинает игру. Клавиша Esc завершает приложение.

~~~
void Breakout::startGame() {
     
  if (!gameStarted) {
    ball->resetState();
    paddle->resetState();

    for (int i=0; i<N_OF_BRICKS; i++) {
      bricks[i]->setDestroyed(false);
    }
    
    gameOver = false; 
    gameWon = false; 
    gameStarted = true;
    timerId = startTimer(DELAY);  
  }      
}
~~~

Метод startGame приводит в начальное состояние объект мяча и весла, они перемещаются на свои первоначальные позиции. В цикле, мы сбрасываем у каждого уничтоженного блока флаг destroyed в состояние false, и, таким образом они отображаются в окне. Переменные gameOver, gameWon и gameStarted получают свои первоначальные значения. В конце, запускаем таймер с помощью метода startTimer.

~~~
void Breakout::pauseGame() {
    
  if (paused) {
      
    timerId = startTimer(DELAY);
    paused = false;
  } else {
      
    paused = true;
    killTimer(timerId); 
  }        
}
~~~

Метод pauseGame используется для постановки на паузу и продолжения игры. Мы удаляем таймер с помощью метода killTimer. Для перезапуска, мы вызываем метод startTimer.

~~~
void Breakout::stopGame() {
    
  killTimer(timerId);    
  gameOver = true;      
  gameStarted = false;
}
~~~

В методе stopGame, мы удаляем таймер и устанавливаем необходимые флаги.

~~~
void Breakout::checkCollision() {
  
  if (ball->getRect().bottom() > BOTTOM_EDGE) {
    stopGame();
  }
...
}
~~~

В методе checkCollision, мы проверяем наличии стокновений в игре. Игра завершается, когда мяч достигает нижнего края.

~~~
for (int i=0, j=0; i<N_OF_BRICKS; i++) {
      
  if (bricks[i]->isDestroyed()) {
    j++;
  }
    
  if (j == N_OF_BRICKS) {
    victory();
  }
}
~~~

Мы проверяем, как много блоков уничтожены. Если все блок уничтожены, мы выигрываем игру.

~~~
if (ballLPos < first) {
  ball->setXDir(-1);
  ball->setYDir(-1);
}
~~~

Если мяч ударяется о первую часть весла, мы меняем направление мяча на северо-запад.

~~~
if(bricks[i]->getRect().contains(pointTop)) {
  ball->setYDir(1);
} 
~~~

Если мяч ударяет в нижнюю часть блока, то мы меняем направление мяча, он движется вниз.

~~~
#include <QApplication>
#include "breakout.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);  
    
  Breakout window;
  
  window.resize(300, 400);
  window.setWindowTitle("Breakout");
  window.show();

  return app.exec();
}
~~~

Это файл main.

Это была игра Breakout в Qt5.
