# Принципы SOLID

## Классы

### SOLID

#### Single Responsibility Principle | Принцип единственной ответственности

> «Модуль должен иметь одну и только одну причину для изменения»

Отрывок из книги: Мартин Р. «Чистая архитектура. Искусство разработки программного обеспечения». Apple Books. 
Это старое определение. 

Класс `Book`, приведённый ниже, имеет две причины для изменения: 

- если зменится логика печати книги
- если изменится формат(структура) книги

```c++
class Book
{
public:
  void printBook();
  
  Page getPage(int pageNumber);
  void addPage(Page page);
  
  string getTitle();
  void setTitle(string title);
  
  string getAuthor();
  void setAuthor(string author)

private:
  vector<Page> _pages;
  string _title;
  string _authorName;
};
```

Есть более новое определение:

> «Модуль должен отвечать за одного и только за одного актора.»

Отрывок из книги: Мартин Р. «Чистая архитектура. Искусство разработки программного обеспечения». Apple Books.

Кому может понадобиться книга? Представим, что книга может понадобиться писателям, читателям и издателям. 
Все трое акторов будут использовать класс `Book`, т.е. в этом контексте у класса 3 причины изменения. 

Разобьем класс `Book` согласно принципу:

```c++
struct Book // Хранение данных
{
  vector<Page> _pages;
  string _title;
  string _authorName;
};

class BookWriter // Писатель
{
public:
  void beginBook(string title);
  void addPage(Book &book, Page page);
  Book getBook();
  
private:
  string _name;
  Book _book;
};

class BookReader // Читатель
{
public:
  void beginBook (Book book);
  Page nextPage();
  Page previousPage();
  void makeBookmark(int pageNumber);
  
private:
  Book _book;
  vector<int> bookmarks;
};

class BookPublisher
{
public:
  void printBook(Book book);
private:
  string _publisherName;
};
```

