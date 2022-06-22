https://zetcode.com/gui/qt5/files/

## Qt5 файлы и каталоги ##

В этой части учебника по Qt5 C++, мы будем работать с файлами и каталогами.

QFile, QDir и QFileInfo являются фундаметнальными классам для работы с файлами в Qt5. QFile обеспечивает интерфейс для чтения и записи файлов. QDir обеспечивает доступ к структуре катоалогов и их содержимому. QFileInfo обеспечивает системно-незоависимую информацию о файле, включая имя и расположение в файловой системе, время последнего доступа к файлу и время его изменения, права доступа и информацию о владельце файла.

## Размер файла в Qt5 ##

В следующем примере, мы определяем размер файла.

~~~
#include <QTextStream>
#include <QFileInfo>

int main(int argc, char *argv[]) {

  QTextStream out{stdout};

  if (argc != 2) {

    qWarning("Usage: file_size file");
    return 1;
  }

  QString filename = argv[1];

  QFile f{filename};

  if (!f.exists()) {

    qWarning("The file does not exist");
    return 1;
  }

  QFileInfo fileinfo{filename};

  qint64 size = fileinfo.size();

  QString str = "The size is: %1 bytes";

  out << str.arg(size) << endl;

  return 0;
}
~~~

Размер файла определяется с помощью метода size класса QFileInfo.

~~~
QString filename = argv[1];
~~~

Имя файла указано в аргументе при запуске программы.

~~~
QFile f{filename};

if (!f.exists()) {

  qWarning("The file does not exist");
  return 1;
}
~~~

Метод exits класса QFile проверяет, существует ли файл. Если нет, то выдаем предупреждение и завершаем программу.

~~~
QFileInfo fileinfo{filename};
~~~

Создаем экземпляр QFileInfo.

~~~
qint64 size = fileinfo.size();
~~~

Определяем размер файла с помощью метода size. Тип qint гарантирует, что размер переменной будет в 64 бита на любой платформе, где поддерживается Qt.

~~~
QString str = "The size is: %1 bytes";

out << str.arg(size) << endl;
~~~

Выводим в консоль.

~~~
$ ./filesize Makefile
The size is: 19993 bytes
~~~

## Чтение файлов в Qt5 ##

Прежде чем читать содержимое файла, необходимо его открыть для чтения. Затем создается входящий поток и данные из файла читаются из потока.

~~~
sky
blue
cloud
falcon
forest
lake
cup
bear
wolf
~~~

У нас есть пример текстового файла.

~~~
#include <QTextStream>
#include <QFile>

int main(void) {

  QTextStream out{stdout};

  QFile f{"words.txt"};

  if (!f.open(QIODevice::ReadOnly)) {

    qWarning("Cannot open file for reading");
    return 1;
  }

  QTextStream in{&f};

  while (!in.atEnd()) {

    QString line = in.readLine();
    out << line << endl;
  }

  return 0;
}
~~~

Этот пример читает данные их файла words.txt.

~~~
QFile f{"words.txt"};
~~~

Экземпляр QFile создан. QFile автоматически закрывает файл, когда он выходит из области видимости.

~~~
if (!f.open(QIODevice::ReadOnly)) {

  qWarning("Cannot open file for reading");
  return 1;
}
~~~

Метод open класса QFile открывает файл в режиме "только для чтения". Если метод не может открыть файл, то создается предупреждение и программа прерывается.

~~~
QTextStream in{&f};
~~~

Входящий поток создан. QTextStream получает дескриптор файла. Данные будут прочитаны из этого потока.

~~~
while (!in.atEnd()) {

  QString line = in.readLine();
  out << line << endl;
}
~~~

В цикле мы читаем файл по строкам до конца файла. Метод atEnd возвращает true, если достигнут коне файла и данных для чтения больше нет. Метод readLine читает одну строку из потока.

~~~
$ ./readfile 
sky
blue
cloud
falcon
forest
lake
cup
bear
wolf
~~~

## Запись в файл в Qt5 ##

Для записи в файл, необходимо сначала открыть файл в режиме записи, создать исходящий поток в файл, и использовать оператор записи в поток.

~~~
#include <QTextStream>
#include <QFile>

int main(void) {

  QTextStream out{stdout};

  QString filename = "distros.txt";
  QFile f{filename};

  if (f.open(QIODevice::WriteOnly)) {

    QTextStream out{&f};
    out << "Xubuntu" << endl;
    out << "Arch" << endl;
    out << "Debian" << endl;
    out << "Redhat" << endl;
    out << "Slackware" << endl;

  } else {

    qWarning("Could not open file");
  }

  return 0;
}
~~~

В примере, именя пяти дистрибутивов Linux записываются в файл с именем distrost.txt.

~~~
QString filename = "distros.txt";
QFile f{filename};
~~~

Объект класса QFile создан с именем файла.

~~~
if (f.open(QIODevice::WriteOnly)) {
~~~

С помощью метода open, открываем файл в режиме "только запись".

~~~
QTextStream out{&f};
~~~

Эта строка создает объект класса QTextStream, которые оперирует дескриптором файла. Другими словами, поток данных будет записан в файл.

~~~
out << "Xubuntu" << endl;
out << "Arch" << endl;
out << "Debian" << endl;
out << "Redhat" << endl;
out << "Slackware" << endl;
~~~

Данные записываются с помощью оператора <<.

~~~
$ ./writefile
$ cat distros.txt 
Xubuntu
Arch
Debian
Redhat
Slackware
~~~

## Копирование файла в Qt ##

Когда мы копируем файл, мы создаем точную копию файла с другим именем или с другим расположением в файловой системе.

~~~
#include <QTextStream>
#include <QFile>

int main(int argc, char *argv[]) {

  QTextStream out{stdout};

  if (argc != 3) {

      qWarning("Usage: copyfile source destination");
      return 1;
  }

  QString src = argv[1];

  if (!QFile{src}.exists()) {

      qWarning("The source file does not exist");
      return 1;
  }

  QString dest(argv[2]);

  QFile::copy(src, dest);

  return 0;
}
~~~

Этот пример создает копию файла с помощью метода copy класса QFile/

~~~
if (argc != 3) {

    qWarning("Usage: copyfile source destination");
    return 1;
}
~~~

Программа принимает два параметра, если они отсутствуют, то программа завершается с предупреждением.

~~~
QString src = argv[1];
~~~

Из аргументов командной строки программа получает имя исходного файла.

~~~
if (!QFile{src}.exists()) {

    qWarning("The source file does not exist");
    return 1;
}
~~~

Мы проверяем существование исходного файла с помщью метода exits класса QFile. Если он не существует , мы прерываем выполнение программы с предупреждением.

~~~
QString dest(argv[2]);
~~~

Получаем из аругментов командной строки имя файла-назначения.

~~~
QFile::copy(src, dest);
~~~

Исходный файл скопирован с помощью метода copy класса QFile. Первый параметр - имя исходного файла, второй параметр - имя файла-назначения.

## Владелец файла и группа в Qt5 ##

У каждого файла есть владелец. Каждый владелец состоит в группе пользователей, для большей управляемости и защиты файлов.

~~~
#include <QTextStream>
#include <QFileInfo>

int main(int argc, char *argv[]) {

  QTextStream out{stdout};

  if (argc != 2) {

      qWarning("Usage: owner file");
      return 1;
  }

  QString filename = argv[1];

  QFileInfo fileinfo{filename};

  QString group = fileinfo.group();
  QString owner = fileinfo.owner();

  out << "Group: " << group << endl;
  out << "Owner: " << owner << endl;

  return 0;
}
~~~

Этот пример печатает владельца и группу заданного файла.

~~~
QFileInfo fileinfo{filename};
~~~

Экземпляр класса QFileInfo создан. Его параметром явялется заданное имя файла в командной строке.

~~~
QString group = fileinfo.group();
~~~

С помощью метода group класса QFileInfo получили группу.

~~~
QString owner = fileinfo.owner();
~~~

Получили владельца файла с помощью метода owner класса QFileInfo.

~~~
$ touch myfile
$ ./ownergroup myfile
Group: janbodnar
Owner: janbodnar
~~~

## Последнее чтение, последнее изменение файла в Qt5 ##

Файлы хранят информацию о времени последнего чтения и изменения. Для получения этой информации, мы используем QFileInfo класс.

~~~
#include <QTextStream>
#include <QFileInfo>
#include <QDateTime>

int main(int argc, char *argv[]) {

  QTextStream out{stdout};

  if (argc != 2) {

      qWarning("Usage: file_times file");
      return 1;
  }

  QString filename = argv[1];

  QFileInfo fileinfo{filename};

  QDateTime last_rea = fileinfo.lastRead();
  QDateTime last_mod = fileinfo.lastModified();

  out << "Last read: " << last_rea.toString() << endl;
  out << "Last modified: " << last_mod.toString() << endl;

  return 0;
}
~~~

Пример печатает на экране время последнегоо чтения и изменения 6 января 2022 года.

~~~
QFileInfo fileinfo{filename};
~~~

объект типа QFile Info создан.

~~~
QDateTime last_rea = fileinfo.lastRead();
~~~

Метод lastRead возвращает даут и время, когда последний раз файл был прочитан (когда было обращние к файлу).

~~~
QDateTime last_mod = fileinfo.lastModified();
~~~

Метод lastModified возвращает дату и время, когда последний раз файл был изменен.

~~~
$ ./filetimes Makefile 
Last read: Sun Dec 6 12:46:11 2020
Last modified: Sun Dec 6 12:46:10 2020
~~~

## Работа с каталогами в Qt5 ##

Класс QDir содержит методы для работы с каталогами.

~~~
#include <QTextStream>
#include <QDir>

int main(void) {

  QTextStream out{stdout};
  QDir dir;

  if (dir.mkdir("mydir")) {

    out << "mydir successfully created" << endl;
  }

  dir.mkdir("mydir2");

  if (dir.exists("mydir2")) {

    dir.rename("mydir2", "newdir");
  }

  dir.mkpath("temp/newdir");

  return 0;
}
~~~

В этом примере, мы используем четыре метода для работы с каталогами.

~~~
if (dir.mkdir("mydir")) {

  out << "mydir successfully created" << endl;
}
~~~

Метод mkdir создает каталог. В случае успешного создания, он возвращает true.

~~~
if (dir.exists("mydir2")) {

  dir.rename("mydir2", "newdir");
}
~~~

Метод exists проверяет существование каталога. Метод rename переименовывает католог.

~~~
dir.mkpath("temp/newdir");
~~~

Метод mkpath создает новый катало и все необходимые родительские каталоги за один вызов.

## Специальные каталоги в Qt% ##

В файловой системе есть специальные каталоги, например для экземпляров домашнего каталога и каталога root. Класс QDir используется для получения пути к специальным каталогам.

~~~
#include <QTextStream>
#include <QDir>

int main(void) {

  QTextStream out{stdout};

  out << "Current path:" << QDir::currentPath() << endl;
  out << "Home path:" << QDir::homePath() << endl;
  out << "Temporary path:" << QDir::tempPath() << endl;
  out << "Rooth path:" << QDir::rootPath() << endl;

  return 0;
}
~~~

Этот пример печатает четыре специальных каталога.

~~~
out << "Current path:" << QDir::currentPath() << endl;
~~~

Текущйи рабочий каталог возвращает метод currentPath класса QDir.

~~~
out << "Home path:" << QDir::homePath() << endl;
~~~

Метод homePath класса QDir возвращает домашний каталог пользователя.

~~~
out << "Temporary path:" << QDir::tempPath() << endl;
~~~

Метод tempPath класса QDir возвращает каталог для временных файлов.

~~~
out << "Rooth path:" << QDir::rootPath() << endl;
~~~

Каталог root возвращает метод rootPath класса QDir

## Путь к файлу в Qt5 ##

Файлы идентифицируются по имени и пути. Путь состоит из имени файла, базового имени и суффикса.

~~~
#include <QTextStream>
#include <QFileInfo>

int main(int argc, char *argv[]) {

  QTextStream out{stdout};

  if (argc != 2) {

      out << "Usage: file_times file" << endl;
      return 1;
  }

  QString filename = argv[1];

  QFileInfo fileinfo{filename};

  QString absPath = fileinfo.absoluteFilePath();
  QString baseName = fileinfo.baseName();
  QString compBaseName = fileinfo.completeBaseName();
  QString fileName = fileinfo.fileName();
  QString suffix = fileinfo.suffix();
  QString compSuffix = fileinfo.completeSuffix();

  out << "Absolute file path: " << absPath << endl;
  out << "Base name: " << baseName << endl;
  out << "Complete base name: " << compBaseName << endl;
  out << "File name: " << fileName << endl;
  out << "Suffix: " << suffix << endl;
  out << "Whole suffix: " << compSuffix << endl;

  return 0;
}
~~~

В этом примере, мы используем несколько  методов для печати пути к фалу ичастей его имени.

~~~
QFileInfo fileinfo{filename};
~~~

Путь к файлу определяем с помощью QFileInfo класса.

~~~
QString absPath = fileinfo.absoluteFilePath();
~~~

Метод adsolutePath возвращает абсолютный путь к файлу, включая имя файла.

~~~
QString baseName = fileinfo.baseName();
~~~

Метод baseName возвращает базовое имя файла - имя файла без пути к нему.

~~~
QString compBaseName = fileinfo.completeBaseName();
~~~

Метод completeBaseName возвращает полное базовое имя файла - все символы, но не включая последнюю точку.

~~~
QString fileName = fileinfo.fileName();
~~~

Метод fileName возвращает имя файла, которое включает в себя базовое имя и расширение.

~~~
QString suffix = fileinfo.suffix();
~~~

Метод suffix возвращает конец имеи файла, после (но не включая) последней точки.

~~~
QString compSuffix = fileinfo.completeSuffix();
~~~

Окончание имени файла может состоять из несколькоих частей; метод completeSuffix возвращает все символы после (но не ключая) первой точки.

~~~
$ ./filepath ~/Downloads/qt-everywhere-opensource-src-5.5.1.tar.gz
Absolute file path: /home/janbodnar/Downloads/qt-everywhere-opensource-src-5.5.1.tar.gz
Base name: qt-everywhere-opensource-src-5
Complete base name: qt-everywhere-opensource-src-5.5.1.tar
File name: qt-everywhere-opensource-src-5.5.1.tar.gz
Suffix: gz
Whole suffix: 5.1.tar.gz
~~~

## Права доступа к файлам в Qt5 ##

Файлы в файловой системе имеют защиту. У файла есть флаги, которые определяют, кто может получить к ним доступ и кто может их изменять. QFile::permissions метод возвращает перечисление флагов файла.

~~~
#include <QTextStream>
#include <QFile>

int main(int argc, char *argv[]) {

  QTextStream out{stdout};

  if (argc != 2) {

      out << "Usage: permissions file" << endl;
      return 1;
  }

  QString filename = argv[1];

  auto ps = QFile::permissions(filename);

  QString fper;

  if (ps & QFile::ReadOwner) {

      fper.append('r');
  } else {
      fper.append('-');
  }

  if (ps & QFile::WriteOwner) {

      fper.append('w');
  } else {
      fper.append('-');
  }

  if (ps & QFile::ExeOwner) {

      fper.append('x');
  } else {
      fper.append('-');
  }

  if (ps & QFile::ReadGroup) {

      fper.append('r');
  } else {
      fper.append('-');
  }

  if (ps & QFile::WriteGroup) {

      fper.append('w');
  } else {
      fper.append('-');
  }

  if (ps & QFile::ExeGroup) {

      fper.append('x');
  } else {
      fper.append('-');
  }

  if (ps & QFile::ReadOther) {

      fper.append('r');
  } else {
      fper.append('-');
  }

  if (ps & QFile::WriteOther) {

      fper.append('w');
  } else {
      fper.append('-');
  }

  if (ps & QFile::ExeOther) {

      fper.append('x');
  } else {
      fper.append('-');
  }

  out << fper << endl;

  return 0;
}
~~~

В этом примере создается UNIX-подобный список прав доступа для указанного файла. Бывает три вида возможных пользователей: владалец, группа к которой он принадлежит, и остальные пользователи, которые именуют другими. Первые три позиции принадлежат владельцу файла, следующие три позиции принадлежат группе владельца, а последние три - остальным пользователям. Существует четыре вида прав, чтение (r), запись или изменение (w), исполнение (x) и нет права (-).

~~~
auto ps = QFile::permissions(filename);
~~~

С помощью метода permissions класса QFile мы получаем список флагов разрешений.

~~~
QString fper;
~~~

Эта строка динамически строится на основе полученных разрешений.

~~~
if (ps & QFile::ReadOwner) {

    fper.append('r');
} else {
    fper.append('-');
}
~~~

Мы используем оператор & для определения в возвращенном списке прав наличия флага QFile::ReadOwner

~~~
$ ./permissions Makefile
rw-rw-r--
~~~

Владелец и группа пользователей с ним имеют право читать и модифицировать файл. Остальные пользователи имеют право читать файл. Так как файл неисполняемый, ни у кого нет прав на его исполнение.

## Список содержимого каталога в Qt5 ##

В следующем примеремы отобразим соедржимое выбранного каталога.

~~~
#include <QTextStream>
#include <QFileInfo>
#include <QDir>

int main(int argc, char *argv[]) {

  QTextStream out{stdout};

  if (argc != 2) {

      qWarning("Usage: list_dir directory");
      return 1;
  }

  QString directory = argv[1];

  QDir dir{directory};

  if (!dir.exists()) {

      qWarning("The directory does not exist");
      return 1;
  }

  dir.setFilter(QDir::Files | QDir::AllDirs);
  dir.setSorting(QDir::Size | QDir::Reversed);

  QFileInfoList list = dir.entryInfoList();

  int max_size = 0;

  for (QFileInfo finfo: list) {

      QString name = finfo.fileName();
      int size = name.size();

      if (size > max_size) {

          max_size = size;
      }
  }

  int len = max_size + 2;

  out << QString("Filename").leftJustified(len).append("Bytes") << endl;

  for (int i = 0; i < list.size(); ++i) {

    QFileInfo fileInfo = list.at(i);
    QString str = fileInfo.fileName().leftJustified(len);
    str.append(QString("%1").arg(fileInfo.size()));
    out << str << endl;
  }

  return 0;
}

~~~

Для печати содержимого мы используем метод entryInfoList класса QDir. Список файлов будет обратно отсортирован по размеру файлов. Там две колонки, первая сожержит имя файла, а вторая его размер.

~~~
QDir dir{directory};
~~~

Объект QDir создан с заданным именем каталога.

~~~
dir.setFilter(QDir::Files | QDir::AllDirs);
~~~

Метод setFilter указывает, какие файлы будут возвращены методом entryInfoListю

~~~
dir.setSorting(QDir::Size | QDir::Reversed);
~~~

Метод setSorting указывает порядок сортирвки, который будет использовать метод entryInfoList.

~~~
QFileInfoList list = dir.entryInfoList();
~~~

Метод entryInfoList возвращает список объектов типа QFileInfo для все файлов и каталогов, отфильрованный и отсортированный методами фильтрации и сортировки. QFileInfoList является синонимом QList<QFileINfo>.

~~~
for (QFileInfo finfo: list) {

  QString name = finfo.fileName();
  int size = name.size();

  if (size > max_size) {

      max_size = size;
  }
}
~~~

Перебирая в цикле список, мы определяем файл с самым большим размером, эта информация нам понадобится для организации аккуратного вывода.

~~~
int len = max_size + 2;
~~~

Добавляем два пробела к длине колонки.

~~~
out << QString("Filename").leftJustified(len).append("Bytes") << endl;
~~~

Здесь мы печатаем имена колонок. Метод leftJustified возвращает строку заданной длины, данная строка выровнена по левому краю, а отступ заполнен символом пробела справа.

~~~
for (int i = 0; i < list.size(); ++i) {

  QFileInfo fileInfo = list.at(i);
  QString str = fileInfo.fileName().leftJustified(len);
  str.append(QString("%1").arg(fileInfo.size()));
  out << str << endl;
}
~~~

Проходя по списку файлой мы печатаем их имена и размеры. Первая колонка выровнена по левому краю и отступ заполнен пробелами в необходимом количестве; вторая колонка просто добалена к онцу строки.

~~~
$ ./list_dir .
Filename      Bytes
list_dir.pro  291
list_dir.cpp  1092
..            4096
.             4096
list_dir.o    10440
list_dir      19075
Makefile      28369
~~~

Это пример вывода.

В этой главе, мы поработали с файлами и катологами в Qt5.

