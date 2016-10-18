```c++
struct Node{
  int data;
  Node *next;
}
Node *np = new Node;
//  stack    heap
.;
.;
.;
delete dp;
```

- stack-allocated ariables are auto deallocted when they go out of scope

  - e.g. function returns

- heap-allocated memory is never auto deallocated and must be deleted

  ```c++
  //stack-allocation
  Node getMeANode(){
    Node n; //stack -local
    return n; // makes a copy for caller, discards local copy
  }
  int main(){
    Node p = getMeANode(); //gets a copy- may have cost
  }
  ```

  ```c++
  //use pointers -avoid copy
  Node *getMeANode(){
    Node n;	//stack
    return &n;	//de-allocated, address is invalid -Crash
  }
  int main(){
    Node *p = getMeANode();
  }
  ```

  ```c++
  //correct
  Node *getMeANode(){
    Node *np = new Node; //heap-allocated
    return np;
  }
  int main(){
    Node *n = getMeANode();
    ...;
    ...;
    delete n;	//remember to deallocate!
  }
  ```