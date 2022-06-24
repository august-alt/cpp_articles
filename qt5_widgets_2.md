[исходник](https://zetcode.com/gui/qt5/widgets2/)

# Виджеты Qt II

В этой части обучающего курса по Qt5, мы продолжим говорить о виджетах в Qt5. Мы рассмотрим следующие виджеты: QCheckBox, QListWidget, QProgressBar, QPixmap, QSplitter, и QTableWidget.

## QCheckBox

Виджет QCheckBox имеет два состояния: включено и выключено. Это отметка c текстовой меткой. Если checkbox включен, что в ставится отметка в виде галочки.

В нашем примере, мы отобразим chekcbox  в окне. Если checkbox  включен, заголовок окна отобразится, в противном случае, он будет спрятанным.

~~~
#pragma once

#include <QWidget>

class CheckBox : public QWidget {
    
  Q_OBJECT

  public:
    CheckBox(QWidget *parent = 0);

  private slots:
    void showTitle(int);
};
~~~

Это заголовочный файл для нашего примера.

~~~
#include <QCheckBox>
#include <QHBoxLayout>
#include "checkbox.h"

CheckBox::CheckBox(QWidget *parent)
    : QWidget(parent) {

  QHBoxLayout *hbox = new QHBoxLayout(this);
  
  QCheckBox *cb = new QCheckBox("Show Title", this);
  cb->setCheckState(Qt::Checked);
  hbox->addWidget(cb, 0, Qt::AlignLeft | Qt::AlignTop);

  connect(cb, &QCheckBox::stateChanged, this, &CheckBox::showTitle);
}

void CheckBox::showTitle(int state) {
    
  if (state == Qt::Checked) {
    setWindowTitle("QCheckBox");
  } else {
    setWindowTitle(" ");
  }
}
~~~

Мы отображаем checkbox в окне и соединяем его с слотом showTitle.

>cb->setCheckState(Qt::Checked);

Когда приложение запускается, checkbox включен.

~~~
void CheckBox::showTitle(int state) {

  if (state == Qt::Checked) {
    setWindowTitle("QCheckBox");
  } else {
    setWindowTitle(" ");
  }
}
~~~

Мы определяем состояние checkbox и вызываем setWindowTitle соотвествующе.

~~~
#include <QApplication>
#include "checkbox.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv); 
    
  CheckBox window;

  window.resize(250, 150);
  window.setWindowTitle("QCheckBox");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## QListWidget

Виджет QListWidget используется для отображения списка элементов. В нашем примере, мы продемострируем как добавлять, переименовывать и удалять элементы из списка.

~~~
#pragma once

#include <QWidget>
#include <QPushButton>
#include <QListWidget>

class ListWidget : public QWidget {
    
  Q_OBJECT

  public:
    ListWidget(QWidget *parent = 0);

  private slots:
    void addItem();
    void renameItem();
    void removeItem();
    void clearItems();

  private:
    QListWidget *lw;
    QPushButton *add;
    QPushButton *rename;
    QPushButton *remove;
    QPushButton *removeAll; 
};
~~~

Это заголовочный файл примера.

~~~
#include "listwidget.h"
#include <QVBoxLayout>
#include <QInputDialog>

ListWidget::ListWidget(QWidget *parent)
    : QWidget(parent) {

  QVBoxLayout *vbox = new QVBoxLayout();
  vbox->setSpacing(10);

  QHBoxLayout *hbox = new QHBoxLayout(this);

  lw = new QListWidget(this);
  lw->addItem("The Omen"); 
  lw->addItem("The Exorcist");
  lw->addItem("Notes on a scandal");
  lw->addItem("Fargo");
  lw->addItem("Capote");

  add = new QPushButton("Add", this);
  rename = new QPushButton("Rename", this);
  remove = new QPushButton("Remove", this);
  removeAll = new QPushButton("Remove All", this);

  vbox->setSpacing(3);
  vbox->addStretch(1);
  vbox->addWidget(add);
  vbox->addWidget(rename);
  vbox->addWidget(remove);
  vbox->addWidget(removeAll);
  vbox->addStretch(1);

  hbox->addWidget(lw);
  hbox->addSpacing(15);
  hbox->addLayout(vbox);
  
  connect(add, &QPushButton::clicked, this, &ListWidget::addItem);
  connect(rename, &QPushButton::clicked, this, &ListWidget::renameItem);
  connect(remove, &QPushButton::clicked, this, &ListWidget::removeItem);
  connect(removeAll, &QPushButton::clicked, this, &ListWidget::clearItems);
  
  setLayout(hbox);
}

void ListWidget::addItem() {
    
  QString c_text = QInputDialog::getText(this, "Item", "Enter new item");
  QString s_text = c_text.simplified();
  
  if (!s_text.isEmpty()) {
      
    lw->addItem(s_text);
    int r = lw->count() - 1;
    lw->setCurrentRow(r);
  }
}

void ListWidget::renameItem() {
    
  QListWidgetItem *curitem = lw->currentItem();
  
  int r = lw->row(curitem);
  QString c_text = curitem->text();
  QString r_text = QInputDialog::getText(this, "Item", 
      "Enter new item", QLineEdit::Normal, c_text);
  
  QString s_text = r_text.simplified();
  
  if (!s_text.isEmpty()) {
      
    QListWidgetItem *item = lw->takeItem(r);
    delete item;
    lw->insertItem(r, s_text);
    lw->setCurrentRow(r);
  }
}

void ListWidget::removeItem() {
    
  int r = lw->currentRow();

  if (r != -1) {
      
    QListWidgetItem *item = lw->takeItem(r);
    delete item;
  }
}

void ListWidget::clearItems(){
    
  if (lw->count() != 0) {
    lw->clear();
  }
}
~~~

Мы отображаем список и четыре кнопки. Мы будем использовать эти кнопки для добавления переименования и удаления элементов из списка.

~~~
lw = new QListWidget(this);
lw->addItem("The Omen"); 
lw->addItem("The Exorcist");
lw->addItem("Notes on a scandal");
lw->addItem("Fargo");
lw->addItem("Capote);
~~~

Виджет QListWidget создан и заполнен четрыьмя элементами.

~~~
void ListWidget::addItem() {
    
  QString c_text = QInputDialog::getText(this, "Item", "Enter new item");
  QString s_text = c_text.simplified();
  
  if (!s_text.isEmpty()) {
      
    lw->addItem(s_text);
    int r = lw->count() - 1;
    lw->setCurrentRow(r);
  }
}
~~~

Метод addItem добавляет элемент в виджет. Метод показывает диалог ввода. Диолог возвращает строковое значение. Мы удлаяем возможные пробелы, используя метод simplified. Если возвращаемое значение не пустое, мы добавляем его в виджет, в конец списка. В окнце, мы выделяем вставленный элемент с помощью setCurrentRow метод.

~~~
void ListWidget::renameItem() {
    
  QListWidgetItem *curitem = lw->currentItem();
  
  int r = lw->row(curitem);
  QString c_text = curitem->text();
  QString r_text = QInputDialog::getText(this, "Item", 
      "Enter new item", QLineEdit::Normal, c_text);
  
  QString s_text = r_text.simplified();
  
  if (!s_text.isEmpty()) {
      
    QListWidgetItem *item = lw->takeItem(r);
    delete item;
    lw->insertItem(r, s_text);
    lw->setCurrentRow(r);
  }
}
~~~

Переименование элемента состоит из нескольких шагов. Сначала мы должны получить текущий элемент с помощью метода currrentItem. Далее мы получаемтекст элемента и строку, где элемент расположен. Текст элемента отображаем в диалоге QInputDialog. Строка, которую возвратил диалог, обрабатывается с помощью метода simpified, для удаления возможных пробелов. Далее, мы удаляем страый метод с помощью метода takeItem, и заменяем его с помощью insertItem метода. Мы удаляем элемент с помощью takeItem метода, потому, что удаленные элементы более не управляются Qt. В конце, с помощью метода setCurrentRow мы выбираем новый элемент.

~~~
void ListWidget::removeItem() {
    
  int r = lw->currentRow();

  if (r != -1) {
      
    QListWidgetItem *item = lw->takeItem(r);
    delete item;
  }
}
~~~

Метод removeItem удаляет указанные элемент из списка. Сначала, мы получим текущую выбранную строку с помощью метода currentRow. (Он возвратит -1, если больше строк нет). Текущий выбранный элемент удален с помощью takeItem метода.

~~~
void ListWidget::clearItems(){
    
  if (lw->count() != 0) {
    lw->clear();
  }
}
~~~

Метод clear удаляет все элементы из списка.

~~~
#include <QApplication>
#include "listwidget.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);
    
  ListWidget window;

  window.setWindowTitle("QListWidget");
  window.show();
  
  return app.exec();
}
~~~

Это main файл.

## QProgressBar

QProgressBar используется для информирования пользователя о выполнении операции.

~~~
#pragma once

#include <QWidget>
#include <QProgressBar>
#include <QPushButton>

class ProgressBarEx : public QWidget {
    
  Q_OBJECT
  
  public:
    ProgressBarEx(QWidget *parent = 0);

  private:
    int progress;
    QTimer *timer;
    QProgressBar *pbar; 
    QPushButton *startBtn;
    QPushButton *stopBtn;
    static const int DELAY = 200;
    static const int MAX_VALUE = 100;
    
    void updateBar();
    void startMyTimer();
    void stopMyTimer();
};
~~~

Это заголовочный файл примера.

~~~
#include <QProgressBar>
#include <QTimer>
#include <QGridLayout>
#include "progressbar.h"

ProgressBarEx::ProgressBarEx(QWidget *parent)
    : QWidget(parent) {
        
  progress = 0; 
  timer = new QTimer(this);
  connect(timer, &QTimer::timeout, this, &ProgressBarEx::updateBar);

  QGridLayout *grid = new QGridLayout(this);
  grid->setColumnStretch(2, 1);
         
  pbar = new QProgressBar();
  grid->addWidget(pbar, 0, 0, 1, 3);

  startBtn = new QPushButton("Start", this);
  connect(startBtn, &QPushButton::clicked, this, &ProgressBarEx::startMyTimer);
  grid->addWidget(startBtn, 1, 0, 1, 1);
  
  stopBtn = new QPushButton("Stop", this);
  connect(stopBtn, &QPushButton::clicked, this, &ProgressBarEx::stopMyTimer);
  grid->addWidget(stopBtn, 1, 1);
}

void ProgressBarEx::startMyTimer() {
  
  if (progress >= MAX_VALUE) {
      
      progress = 0;
      pbar->setValue(0);
  }
    
  if (!timer->isActive()) {
      
    startBtn->setEnabled(false); 
    stopBtn->setEnabled(true); 
    timer->start(DELAY);
  }
}

void ProgressBarEx::stopMyTimer() {
    
  if (timer->isActive()) {
      
    startBtn->setEnabled(true);
    stopBtn->setEnabled(false);
    timer->stop();
  }
}

void ProgressBarEx::updateBar() {
    
  progress++;
  
  if (progress <= MAX_VALUE) {
      
    pbar->setValue(progress);
  } else {
      
    timer->stop();
    startBtn->setEnabled(true); 
    stopBtn->setEnabled(false); 
  }
}
~~~

В примере, у нас есть QProgressBar и две кнопки. ОДнак кнопка запускает таймер, который обновляет progress bar. Другая кнопка останавливает таймер.

~~~
timer = new QTimer(this);
connect(timer, &QTimer::timeout, this, &ProgressBarEx::updateBar);
~~~

QTimer используется для управления QProgressBar виджетом.

~~~
pbar = new QProgressBar();
~~~

Экземпляр QProgressBar создан. Минимальное и максимальное значение от 0 до 99.

~~~
if (!timer->isActive()) {
    
  startBtn->setEnabled(false); 
  stopBtn->setEnabled(true); 
  timer->start(DELAY);
}
~~~

В зависимости от состояния progress bar, кнопки включены или отключены. Это реализовано с помощью метода setEnabled.

~~~
void ProgressBarEx::updateBar() {
    
  progress++;
  
  if (progress <= MAX_VALUE) {
      
    pbar->setValue(progress);
  } else {
      
    timer->stop();
    startBtn->setEnabled(true);
    stopBtn->setEnabled(false); 
  }
}
~~~

Процесс выполнения сохраняется в переменной progress. Метод setValue обновляет текущее значение progress bar.

~~~
#include <QApplication>
#include "progressbar.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);
    
  ProgressBarEx window;

  window.resize(250, 150);
  window.setWindowTitle("QProgressBar");
  window.show();

  return app.exec();
}
~~~

Это файл main.

## Виджет QPixmap

Виджет QPixmap один из виджетов, который работает с изображениями. Он оптимизирован для показа изображений на экране. В нашем примере кода, мы будем использовать QPixmap для показа изображения на экране.

~~~
#pragma once

#include <QWidget>

class Pixmap : public QWidget {
    
  public:
    Pixmap(QWidget *parent = 0);
};
~~~

Заголовочный файл для примера

~~~
#include <QPixmap>
#include <QLabel>
#include <QHBoxLayout>
#include "pixmap.h"

Pixmap::Pixmap(QWidget *parent)
    : QWidget(parent) {

  QHBoxLayout *hbox = new QHBoxLayout(this);
  
  QPixmap pixmap("bojnice.jpg");
  
  QLabel *label = new QLabel(this);
  label->setPixmap(pixmap);

  hbox->addWidget(label, 0, Qt::AlignTop);
}
~~~

Мы показываем изображение известного замка, расположенного в центре Словакии.

~~~
QPixmap pixmap("bojnice.jpg");

QLabel *label = new QLabel(this);
label->setPixmap(pixmap);
~~~

Мы создали pixmap внутри виджета QLabel.

~~~
#include <QApplication>
#include "pixmap.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);
    
  Pixmap window;

  window.setWindowTitle("QPixmap");
  window.show();
  
  return app.exec();
}
~~~

Это файл main.

## QSplitter

QSplitter повзоляет контролировать размер дочерних виджетов, передвигая границу между ними. В нашем примере, мы покажем три QFrame виджета, управляемых двумя QSplitter.

~~~
#pragma once

#include <QWidget>

class Splitter : public QWidget {
    
  public:
    Splitter(QWidget *parent = 0);
};
~~~

Заголовочный файл для примера.

~~~
#include <QFrame>
#include <QSplitter>
#include <QHBoxLayout>
#include "splitter.h"

Splitter::Splitter(QWidget *parent)
    : QWidget(parent) {
        
  QHBoxLayout *hbox = new QHBoxLayout(this);

  QFrame *topleft = new QFrame(this);
  topleft->setFrameShape(QFrame::StyledPanel);

  QFrame *topright = new QFrame(this);
  topright->setFrameShape(QFrame::StyledPanel);

  QSplitter *splitter1 = new QSplitter(Qt::Horizontal, this);
  splitter1->addWidget(topleft);
  splitter1->addWidget(topright);

  QFrame *bottom = new QFrame(this);
  bottom->setFrameShape(QFrame::StyledPanel);

  QSplitter *splitter2 = new QSplitter(Qt::Vertical, this);
  splitter2->addWidget(splitter1);
  splitter2->addWidget(bottom);
  
  QList<int> sizes({50, 100});
  splitter2->setSizes(sizes);

  hbox->addWidget(splitter2);
}
~~~

В примере, у нас есть три frame виджета, и два разделителя.

~~~
QSplitter *splitter1 = new QSplitter(Qt::Horizontal, this);
splitter1->addWidget(topleft);
splitter1->addWidget(topright);
~~~

Мы создали QSplitter виджет и два QFrame виджета.

~~~
QSplitter *splitter2 = new QSplitter(Qt::Vertical, this);
splitter2->addWidget(splitter1);
~~~

Мы можем добавить к одному QSptlitter другой.

~~~
QList<int> sizes({50, 100});
splitter2->setSizes(sizes);
~~~

С помощью метода setSize, мы задали размер дочерним QSPlitter виджетам.

~~~
#include <QDesktopWidget>
#include <QApplication>
#include "splitter.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);
    
  Splitter window;

  window.resize(350, 300);
  window.setWindowTitle("QSplitter");
  window.show();
  
  return app.exec();
}
~~~

В некоторых окружениях разделители не видны.

## QTableWidget

Виджет QTableWidget - уникальный, используется для табличных приложений. (Его также называет grid widget). Он один из самых сложных виджетов. Здесь мы только отобразим его в окне.

~~~
#pragma once

#include <QWidget>

class Table : public QWidget {
    
  public:
    Table(QWidget *parent = 0);
};
~~~

Это заголовочный файл для примера.

~~~
#include <QHBoxLayout>
#include <QTableWidget>
#include "table.h"

Table::Table(QWidget *parent)
    : QWidget(parent) {
        
  QHBoxLayout *hbox = new QHBoxLayout(this);

  QTableWidget *table = new QTableWidget(25, 25, this);

  hbox->addWidget(table);
}
~~~

Это пример показывает QTableWidget в окне.

~~~
QTableWidget *table = new QTableWidget(25, 25, this);
~~~

Здесь мы создаем QTableWidget размером 25 строк на 25 колонок.

~~~
#include <QApplication>
#include "table.h"

int main(int argc, char *argv[]) {
    
  QApplication app(argc, argv);
    
  Table window;

  window.resize(400, 250);
  window.setWindowTitle("QTableWidget");
  window.show();

  return app.exec();
}
~~~

В этой главе мы  описали несколько  других виджетов Qt5.
