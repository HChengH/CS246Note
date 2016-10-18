## Overloading (functions)

- in c :

  ```c
    int negInt(int n){
      return -n;
    }
  ```

  ```c
    float negFloat(float n){
      return -n;
    }
  ```

  ```c
    bool negBool(bool b){
      return !b;
    }
  ```

- C++:

  - functions with different parameter lists can have same name

    ```c++
    int neg(int n){
      return -n;
    }
    ```

    ```c++
    float neg(float b){
      return -b;
    }
    ```

    ```c++
    bool neg(bool b){
      return !b;
    }
    ```

  - compiler uses the number and type of arguments to deide which version to call

    - they **may not** only differ on return type

  - "+" operator -overloaded strings, ints, floats

  - ">>" operator -overloaded strings, ints, floats, etc...

## Structures

- i.e. data structures, structs

  ```c++
    struct Node{
      int data;
      Node *next;	//in c, struct Node*..., 				 
      				//in c++,drop struct 					
      				//keyword wfter declear 
    };				// semi-colon 
  ```

## Constants

```c++
  	const int x = 5;
  // keyword
```

- makes variable invariant 
    - i.e. not changeable

```c++
  Node n1 {5, nullptr}; //intializing 
  //	    data   *next  //nullptr replaces NULL 
  					  //in c?
```

```c++
  Node n2 = n1;
  //		Copy
```

```c++
  const Node n3 = n1;
  //		Copy, but can never reassign
```

## Parameter Passing

- recall: 

  ```c++
    void inc(int x){
      x = x+1;
    }

    int n = 5;
    inc(n);
    cout << n;	//5call -by-value (pass-by-value)
  ```

  - call-by-value (pass-by-value)

    - inc gets a copy of the parameter, increments the copy
      - original is untouched

  - call-by-address (pointer)

    - pass a pointer to original parameter

      ```c++
        void inc(int *x){
          *x = *x+1;
        }

        int n = 5;
        inc(&n);
        cout << n; //6
      ```

    - n's address is passed by value

      - inc changes the value at that address

  - C++ also has another pointer-like type:

## References

- a variable that points to another variable(like an alias)

  ```c++
    int y = 10;
    int &z = y;
    //  & means reference
    //  (much like * means pointer)
    // z is an alias for y;
    //like : const *int z = &y;

    cout << y; //10
    cout << z; //10

    z = 12;
    cout << y; //12
    //z is really a ptr to y's memory address

    //in call cases, z behaves exactly like y
  ```

- Things you cannot do with references

  - cannot leave uninitialized (think: const int *)

  - must initialize it with something that has an address (i.e. an l value）

    ```c++
      	int a = 5;
      //L-value = R-value
    ```

  - e.g. 

    ```c++
      int &x = 3; //No! no address
      int &x = y+2; //No! no address
      int &x = y;
    ```

  - cannot create a pointer to a reference

    - reference does not have its own address

      ```c++
      int & *x; //Bad
      ```

    - but can have reference to a pointer

      ```c++
      int *&x; //OK -create an alias to a pointer
      ```

  - cannot have a reference to a reference

    ```c++
    int &&x; //Bad
    ```

  - cannot create an array of references

    ```c++
    int &r[3] = {a,b,c}; //BAD
    ```

## Refs as function parameters

```c++
void inc(int &x){	
  //pass anything t oa function, via pass by reference
  x=x+1;
}

int n = 5;
inc(n);
cout << n; // 6
```

- e.g.

  ```c++
  cin >> x;
  //  >> function
  // cin parameter1 
  // x parameter2
  // x int, cin suport strings, int, floats....

  std::istream& operator >>(std::istream &in, int &n);
  // returns cin& function>>    parameter1   parameter2
  //								&cin		   &x
  ```

- when to use pass-by-reference?

  ```c++
  int d(int n){	
    //pass-by-val makes a copy of parameter
    ...;
  }

  struct ReallyBig{...}; //image -500mb
  int f(ReallyBig rb){...} 
  //copy RB include 500mb of data, potentially bad

  int g(ReallyBig &rb){...}
  // pass-by-ref no copy
  // risk? exposed original data

  int h(const ReallyBig &rb){...}
  // const -read only
  ```

### advice

- prefer pass-by-const-ref

  - original data cannot be changed
  - pass ref to original, no copy required

- exceptions

  - if you **know** you want a copy, consider pass-by-value

    - e.g. need to manipulate data

  - if your parameters are rally small, do whatever

    ```c++
    void f(int n); //no performance different
    ```

### Strange case of const-ref

```c++
int f(int &n){...}
int g(const int &n){...}

f(5); //no address for 5, i.e. not an l-value, invalid
g(5); //ok, compiler allocates memory for a temp copy -
      //on function g() return, discard memory
```

## Dynamic memory allocation

- in c

  ```c
  int *p = malloc((...)* sizeof(int)); //allocates bytes
  ...;
  ...;
  free(p);							 //dealocates
  ```

- in c++

  ```c++
  struct Node{
    int data;
    Node *next;
  };

  Node *np = new Node; //dynamically allocate
  delete np;			 //deallocates
  ```

## Memory

|  Stack↓   |
| :-------: |
|    …..    |
| **Heap↑** |
| **Code**  |



- all local variables reside on the stack

  ```c
  int x = 5; //stack
  ```

- variables(on the stack) are deallocated when go out of scope

  - e.g. function returns

- allocated memory resides on the heap

  - remains allocated until deletes is called
  
    - if you don;t deallocate memory…. memory leak! BAD

    ```c++
    Node *np = new Node;
    ```

  | stack(Node *np) |
  | :-------------: |
  |       ...       |
  |       ...       |
  |  heap  (Node)   |

  ​

### arrays

```c++
Node *myNodes = new Node[10];
delete [] myNodes;
```

- do not mix up
  - new/delete
  - new[]/delete[]

    ​


