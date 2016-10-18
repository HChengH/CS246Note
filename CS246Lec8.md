## Some problems

```c++
//Vec.h
struct Vec{
  int x,y;
}

Vec operator+(const &v1, const &v2);
```

```c++
//Vec.cc
#include <Vec.h>
//implementations
```

```c++
//main.cc
#include <Vec.h>
int main(){
  Vec v1;
  Vec v2 {5,0};
  Vec v3 = v1 + v2;
  ....;
  return 0;
}
```

### Issue 

- won't compile!
  - Vec - missing main
  - main- missing vec
- solution
  - separate compilation
  - `g++14 -c main.cc //(main.o)`
  - `g++14 -c vec.cc //(vec.o)`
  - `g++14 main.o vec.o -O main`

| preprocessing                      | ——- > | compiler                    | ——--> | linker                               |
| ---------------------------------- | ----- | --------------------------- | ----- | ------------------------------------ |
| scan source files, do # directives |       | compiles files individually |       | puts -object files together into exe |
| source                             |       | object                      |       | binary exec                          |

- what about headers?
  - `Vec.h`
  - don't compile

## Global variables

- i.e. a variable that is shared across source files

  ```c++
  //abc.h
  int globalNum; //declaration + definition
  ```

  ```c++
  //main.cc
  #include <abc.h> //gets a version of globalNum
  ```

  ```c++
  //abc.cc
  #include <abc.h> //gets its own version
  ```

  - multiple definitions
    - not shared
    - oops

  ```c++
  //abc.h
  extern int globalNum; //declaration(shared)
  ```

  ```c++
  //main.cc
  #include <abc.h>
  int globalNum = 5; //definition
  				   //one source file only
  ```

  ```c++
  //abc.cc
  #include <abc.h>
  .;
  .;
  .;
  cout << globalNum <<endl; //5
  ```

- e.g.linear algebra module (`lectures/c++/separate/example2`)

  ```c++
  //linalg.h
  #include "Vec.h"
  ....;
  //linalg.cc
  #include "linalg.h"
  #include "Vec.h"
  ...;
  //main.cc
  #include "linalg.h"
  #include "Vec.h"
  int main(){
    Vec v;
    .;
    .;
    .;
    
    return 0;
  }
  ```

  - mult copes of vec.h included

    - main gets 2 copies
    - linalg gets 2 copies

  - at best, we still have a conflict

    - linalg + main both need vec.h
    - cannot have mult definitions of (e.g.) struct vec

  - Fix?

    - be very careful…(this won't work)

    - include guard -mechanism to prevent headers from being included > 1 time

    - e.g. include guard

      ```c++
      //Vec.h
      #ifdef _VEC_H_...	//include guard (header huard)
      #define _VEC_H_...
      struct vec{
        int x,y;
      }
      #endf				//...
      ```

    - Rules:

      - always use include guards in every header file
      - don't put `using namespace `in header files

## GDB

- `g++14 -g main.cc -o main`

## Classes

- big "innovation" in oop

- c++ ~ c add classes

- way of adding functions to structures

  - data + behaviour

- class — structure type that can potentially have functions

- object — instance of a class

- member functions — functions asspciated with a class

- C++ has a class keyword — later

  - represent classes as structs — OK!

  ```c++
  struct student{
    int assns,mt,final;
    
  	float grade(){
    	return 0.40 * assns + 0.20 * mt + 0.40 * final;
  	}
  };
  ```

  ```c++
  // instantiate as normal
  student billy {15,45,98};
  ```

  - object "billy" is an instance of student, with his own fields

  - operate on instance "billy"

    - `cout << billy.grade(); // calculate for billys data`

  - Formally

    - method all have a hidden parameter, called ***this***, which is a pointer to the current instance of the class

      ```c++
      struct student{
        int assns, mt, final;
        float grade(){
          return this->assns*0.40 + this->mt*0.20 + this->final*0.40;
        }
      }
      ```

## Initializing objects

- `student billy {60,70,80}; // initializes billy, assign to the structs variables in order -ok simple and limited`

- method for handling initialization -constructor

  ```c++
  struct student{
    int assns,mt,final;
    float grade(){...}
    student(int assns, int mt, int final){
      //constructor
      this->assns = assns;
      this->mt = mt;
      this->final = final;
    }
  };
  ```

- `student billy {15, 45, 98};`

  - if a constructor exists, this init will use it
  - if it does not exist, will perform default assign(from earlier)

## Uniform initialzation

```c++
int x = 5;
string s = "hello";
// c type
```

```c++
int x(5);
string s("hello");
```

```c++
int x{5};
string s{"hello"};
// suggested for c++ 11 or higher
```

```c++
student *pJane = new student{98, 97, 105};
```

## Advantages of constructors

- default parameters — e.g. `student(int assns = 0, int mt = 0, int final = 0){}`

- overloading — i.e. multiple constructors

- sanity check — e.g. boundary checking

- other logic

  ```c++
  struct student{
    int assns, mt, final;
    float grade(){....}
    student(int assns = 0, int mt = 0, int final = 0){
     this->assns = assns;
     this->mt = mt;
     this->final = final
    }
  };

  student newKid; //0,0,0
  student billy{70,50}; //70,50,0
  student jane {90,90,80}; //90,90,80
  ```

  - if you don't specify a constructor, the compiler will create a default constructor

    - no arg constructor e.g. `student()`
    - all it does is default inilializes objects it contans

  - if you define and constructor on your own, no default is created

    ```c++
    struct vec{
      int x,y;
    };				//has a default constructor

    vec v;			//default init, use default constructor
    ```

    ```c++
    struct vec{
      int x,y;
      vec(int x, int y){
        this->x = x;
        this->y = y;
      }
    };

    vec v{2,4}; 	//OK -use constructor
    vec v2;			//error - no default constructor
    ```

  - Soln:

    - define no-arg constructor

      ```c++
      struct vec{
        int x,y;
        vec(){
          this->x = 0;
          this->y = 0;
        }
        vec(int x, int y){
          this->x = x;
          this->y = y;
        }
      };
      ```

    - define constructor with default that can handle 0 args

      ```c++
      struct vec{
        int x,y;
        vec(int x = 0, int y = 0){
          this->x = x;
          this->y = y;
        }
      };
      ```

## consts & refs in a struct

- issue — they have to be initialized

  - e.g.

    ```c++
    int z;
    struct mystruct{		//class
      const int myconst = 5;
      int &myRef = z;
    };
    ```

    ```c++
    Node student_rec;
    struct student{			//template
      int assns, mt, final;
      const string student_number = "20461625";
      Node &record = student_rec;
    };

    //student billy - has own student number
    ```

    ​

