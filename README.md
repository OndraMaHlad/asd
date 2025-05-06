# Cvičení 10 - MVC

Cílem cvičení je implementace demonstrační aplikace v rámci MVC architektury webového serveru. Pro inspiraci již existuje obdobný problém v podobě tříd `Product`, `ProductController` a `ProductService`.

**Views (šablony) nalezente v Netbeans pod položkou "Other Sources" nebo přímo ve složce "src/main/resources".**

## Todo - úkolovníčk

Vaším úkolem je vytvořit jednoduchou webovou aplikaci, která bude schopna zaznamenávat a evidovat úkoly.

![todos](./img/todos.gif)

Základní součásti:

- `Todo` - entita Todo - evidovaný úkol
- `TodoController` - controller k řízení todos
- `TodoService` - služba poskytující přístup k todos
- `TodoRepository` - repozitář todos zajišťující perzistenci
- `TodoLinkedList` - datová struktura pro uchovávání todos v paměti

### Todo

- třída nebo záznam
- musí definovat minimálně složky
  - `id` - řetězec, string nebo UUID; automaticky generované; uživatel nezadává
  - `TodoPriority` - priorita todo, výčtový typ, minimálně 3 úrovně (High, Medium, Low)
  - `LocalDateTime` - datum a čas vzniku todo; uživatel nezadává, doplňuje se automaticky
  - `String text` - text todo
  - realizuje `Comparable`
    - todo s vyšší prioritou je řazeno dle comparable dopředu
    - pokud mají dvě toto stejnou prioritu, pak se řadi dle data a času vzniku todo

### TodoController

- existuje v DI kontejneru
- musí minimálně poskytnout následující akce
  - index - listing všech todo
  - create - vytvoření nového todo (dle parametrů ve formuláři)
  - remove - smazání todo (dle id)

### TodoService

- existuje v DI kontejneru
- pracuje s `TodoRepository` 
- nabízí minimálně operace
  - `void add(Todo)`
  - `List<Todo> getAll()`
  - `Todo get(id)`
  - `void remove(id)`
  - další metody dle potřeby
- poznámka - nyní je tato třída prakticky zbytečná a v reálu by mohla být vypuštěna

### TodoRepository

- existuje v DI kontejneru
- pracuje s `TodoLinkedList`
- zajišťuje trvalou perzistenci todos
  - využijte DI kontejner a anotací `@Init` a `@Destroy` pro realizaci metod pro načtení a uložení todos
  - aby se destroy zavolalo, ukončujte server přes odkaz `/shutdown`
- data uložte do textového čitelného formátu
- veřejné rozhraní navrhněte dle potřeby

### TodoLinkedList

- datová struktura pro uložení `Todo` v paměti pomocí jednosměrného spojového seznamu
- nabizí minimálně operace
  - `void Add(Todo)` - vloží prvek do spojového seznamu na příslušné místo; automaticky provádí řazení dle comparable
  - `Iterator<Todo> iterator()` - iterátor
  - další metody dle potřeby

### UML

![uml](./img/uml.png)

<!-- classDiagram
direction LR

class Todo {
    id
    priority
    datetime
    text
}

namespace Controller {
    class TodoController
}

namespace Model {
    class TodoRepository
    class TodoService
    class TodoLinkedList
    class TodoEntry
    class Todo

}

namespace LinkedList {
    class TodoLinkedList
    class TodoEntry
}

TodoController -- > TodoService
TodoService -- > TodoRepository
TodoRepository -- > TodoLinkedList

TodoLinkedList -- > "first" TodoEntry

TodoEntry -- > "next" TodoEntry
TodoEntry -- > "todo" Todo -->