## Operator overloading

- defining operators(+,*,-) for types you create

  ```c++
  struct Vec{
    int x,y;
  }
  ```

  - add 2 vectors using +, multiply vector by scalar using *

    ```c++
    Vec v1 = {1,2};
    Vec v2 = {3,4};
    Vec v3 = v1 + v2;	//{4,6}
    Vec v4 = v3 * 5;	//{20,30}

    Vec operator+(const Vec &v1, const Vec &v2){
      Vec v{v1.x + v2.x, v1.y + v2,y};
      return v;
    }

    //e.g. Vec v4 = v3 * 5;
    Vec operator*(const Vec &v, const int k){
      return {v.x*k, v.y*k};	
      //c++ compiler infers that you want to return a Vec
      //works because it can be instantiated like this
    }

    //e.g. Vec v4 = 5 * v3;
    Vec operator*(const int k, const Vec &v){
      return v*k;
    }
    ```

## Overloading << and >>

```c++
struct Grade{
  int theGrade;
}

//e.g. cin >> grade;
//	  cout << grade;

//e.g. cout<< gradde << x << "something" <<endl;
ostream &operator<<(ostream &out, const Grade &g){
  out << g.theGrade << "%";
  return out;
}

//e.g cin >> grade >> x >> y
istream &operator>>(istream &in, Grade &g){
  in >> g.theGrade;
  if(g.theGrade < 0){
    g.theGrade = 0;
  }
  if(g.theGrade > 100){
    g.theGrade = 100;
  }
  return in;
}
```

## Preprocessor

- transforms your program before compiler sees it

  ```c++
  #...... //preprocessor directive, e.g. #includeside note:
  ```

- Side note:

  - if you ant to include old C-headers

    ```c++
    #include <stdio.h>    ->    #include<cstdio>
    //	  c-style					c++-style
    ```

- `#define VARIABLE VALUE`

  - all instances of variable are replaced with VALUE

  - find replace

    ```c++
    //c-style(avoid to use)
    #define MAX 10
    int x[MAX];

    //c++-style
    const int max = 10;
    int x[max];
    ```

  - e.g. conditional compilation

    ```c++
    #define BBOS 1
    #define IOS 2
    #define OS BBOS

    #if OS == BBOS
    long long int key;
    #elif OS == IOS
    short int key;
    #endif

    cout << key;
    ```

  - e.g.

    ```c++
    #if 0 //eval false
    ...;
    #endif
    // do this to comment out code
    ```

  - e.g. directives from command line

    ```c++
    int main(){
      cout << x << endl;//x is not define
    }

    g++ -Dx=10 defines.cc -o defines
    ```

  - e.g. include /exclude Debug statements

    ```c++
    int main(){
      #ifdef DEBUG
      cout << "setting x=1"<<endl;
      #endif
      int x = 1;
      while(x < 10){
        ++x;
      }
      #ifdef DEBUG
      cout << "x is " << x << endl;
      #endif
    }
    ```

    - `g++ debug.cc -o debug`
    - `g++ -DDEBUG debug.cc -o debug`

## Separate compilation

- split your programs into composable modules, each module should have:

  - interface(header -.h file)
    - type definitions
    - function prototypes
  - implementation(source -.cc file)
    - full definition for every function
  - declaration -assert existence(header)
  - definition -full implementation(source) -allocates memory

  ```c++
  //vec.h -function prototypes, type definitions
  struct Vec{
    int x,y;
  }
  Vec operator+(const Vec &v1, const Vec &v2);
  Vec operator*(const Vec &v, const int k);
  Vec operator*(const int k, const Vec &v);
  ```

  ​    

  ```c++
  //main.cc -use a vector
  #include <vec.h>
  int main(){
    Vec v1{1,2};
    Vec v2{5,6};
    Vec v3 = v1 + v2;
  }
  ```

  ​    

  ```c++
  //vec.cc -implementations
  #include <vec.h>
  Vec operator+(const Vec &v1, const Vec &v2)
  {
    return {v1.x + v2.x, v1.y + v2.y};
  }

  Vec operator*(const Vec &v1, const int k)
  {
    return {v1.x * k, v1.y * k};
  }

  Vec operator*(const int k, const Vec &v1)
  {
    return v1*k;
  }
  ```

  - `g++14 -c filename.cc`  compile but not linking...
  - `g++14 filename.o main.o -0 main` will compile

  ​