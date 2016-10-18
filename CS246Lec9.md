## GDB -GNU debugger

- part of GNU gcc suite
- allows interctive debugging
- usage:
  - `g++ -g main.cc -o main`
  - `gdb main`
  - do stuff…..!

## Constructors

- default constructor — compiler provides(no args)
- your can define your own — default desappears

### Consts & refs

```c++
struct student{
  const int id;	//student number should be read-only  const is good!
  int assns,mt,final;	//but const needs to be initialized
}
```

- so we want a const student number/id but every student(instance) should have their own value

- where do we initialize it to make this happen? how?

- what happens when an object is created?

  - space is allocated (memory)

    `student billy;`

  - fields are constructed

    `// billy's asn, mt, final, id`

  - constructor body runs

### Member initailzation list (MIL)

- allows us to initialize consts for each instance

  ```c++
  struct student{
    const int id;
    int assns, mt, final;
    student(int id, int mt, int final): id{id}, mt{mt}, final{final}
    {
    }
  };
  ```

- Guidelines

  - you can and should use this for any field initialization

    - not just const & refs
    - style — MIL init
    - constructor everything else

  - Fields listed in the MIL are initialized in the order they are decleared in the class, not the MIL

    ```c++
    struct stuent{
      const int id;
      int final, mid, assns;	//order different
      student(int id, int assns, int mt, int final): id{id}, assns{assns}, mt{mt}, final{final}
      {
      }
    };
    ```

  - efficiency — the MIL can be more efficient

    - e.g. fields that are objects will get default constructed and then potentially initialized (a second time) in the constructor body — 2x initialization — "wasteful"

  - love the MIL!

    - style
    - efficiency
    - well check on assignment!!!

  - what happens if a field is initialized twice?

    ```c++
    struct student{
      int mt = 0;
      .;
      .;
      student(...): int{int}...		//MIL wins
    };
    ```

## Consider:

```c++
student billy {50, 55, 60}; //constructor, assns 50, mt 55, final 60
student bobby = billy;
```

- copy fields (values for each field) from billy to bobby

- copy constructor — invoked when one object is initialized as a copy of another object (get if for free*)

  - Every class comes with

    - default constructor (default constructs all fields that are objects, takes no arguments, los if you define any constructor)
    - copy constructor (just copies all fields)
    - copy assignment operator
    - a destructor
    - a move constructor
    - a move assignment operator

  - e.g. copy constructor

    ```c++
    struct student {
      int assns, mt, final;
      student(int assns, int mt, int final):assns{assns}, mt{mt}, final{final}
      {
      }
      student(const student &other):assns{other.assns}, mt{other.mt}, final{other.final}	//equivlent to the default copy constructor
      {
      }
    };
    ```

  - dynamic memory is a problem

    - copying values will jest copy the address
      - i.e. copy will point to the orignals' memory

  - shallow copy — just copies values

    - we want a deep copy that works with dynamic memory

      - i.e. more than just a copy

    - e.g.

      ```c++
      struct Node{	//e.g. copy-value
        int data;
        Node *next;
        Node(int data, Node *next): data{data}, next{next}
        {
        }
      };

      Node *n = new Node{1, new Node{2, new Node{3, nullptr}}};
      Node m = *n;	//copy
      Node *p = new Node(*n);
      ```

    - fix i.e. deep - copy

      ```c++
      struct Node {
        int data;
        Node *next;
        Node(int data, Node *next):data{data}, next{next}
        {
        }
        Node(const Node &other): data{data}, 
        next{other.next? new Node{*(other.next)} : nullptr}	//recursive
        {
        }
      };
      ```

    - copy constructor is called when: 

      - when an object is initialized with another object
      - when an object is pass by value
      - when an  object is returned by a function
      - there are exceptions — will talk later

## bware of constructors take a single parameter

```c++
struct Node{
  ...;
  Node(int data):data{data}
  {
  }
};
//single-arg constructor can lead to implicit conversions
Node n(4);	//ok
Node n = 4; //what?	....ok - compiler create a temp

int f(Node n)
{
  ...;
}

f(4);	//ok - bad form, bad compiler
f(newNode);
//want constructor to be called explicatly!

struct Node{
  ...;
  explicit Node(int data): data{data},next{nullptr}
  {
  }
};

Node n(4); //ok
Node n = 4; //error
f(4) //error
```

## Destructors

- method that runs when an object is destroyed

  - stack — allocated -> out of scope
  - heap — allocated -> deleted it

- `~class()`

  - e.g.

    `~Node()`

    `~Student()`

- when an object is destroyed:

  - the destructor body runs (whatever object is destroyed)
  - the destructor are invoked for any fields that are objects
    - reverse of destration order
  - space is deallocated
  - classes have a default destructor — which just invlkes destructor for fields that are objects

  ```c++
  struct Node{
    ...;
    ~Node(){delete next;}
  };
  ```

  ​