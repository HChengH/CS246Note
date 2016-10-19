## Note: implicit conversion from cin to bool

- i.e. cin knows status of last op

  ```c++
  int main(){
    int i;
    while(true)
    {
      cin >> i;
      if(!cin) break;		//replaces cin.fail()
      out << i <<endl;	//cin = True success
    }						//cin = false fail
  }
  ```

## Aside:

- 10 operators `<<`  `>>` have a different meaning in c
  - in c, there are bit shift operators
    - `a >> b` right shift
    - `LHS >>/<< RHS` variable means bit shift
    - `cin << / cout >> variable (iostreams)`
      - `cin >> x >> y >> z`
        - value from cin assigned to x… return cin
        - value from cin assigned to y… return cin

## Example

- e.g. simplify

  ```c++
  int main(){
    int i;
    while(true)
    {
      if(!(cin>>i))break;
      cout << i << endl;
    }
  }
  ```

- e.g. last time

  ```c++
  int main(){
    int i;
    while(cin >> i)
    {
      cout << i << endl;
    }
  }
  ```

- e.g. read all ints with EOF -skip invalid input

  ```c++
  int main(){
    int i;
    while(true){
      if(!(cin >> i)){
        if(cin.eof())break;	//i.e. done
        cin.clear();			//clear fail flag/bit
        cin.ignore();			//discard data(invalid)
      }
      else{
        cout << i << endl;
      }
    }
  }
  ```

- e.g. reading strings

  - string is a type, `std::string`

  - need to include string `#include<string>`

    ```c++
    int main(){
      string s;
      cin >> s;
      //stream into a string(ignores leading white space, read characters until next whitespace read a word)
      cout << s;
    }
    ```

  - if you want to read whitespace:

    - `getline(cin,s); //s is a string(reads to next newline)`

## I/O manipulators

- `cout << 95 << endl; 		//print 95`

- `cout << hex << 95 << endl;    //print 5f`

  - I/O manipulators change the behaviour of the stream
    - stay in effect until "overwritten" by another manipulator
    - `std::hex` — convert all subsequent output to hex
    - `std::dec` — convert all subsequent output to dec
    - `std::endl`	

- stream abstraction applies to "all" data

  - e.g. files — read from a file — `ifstream` — read from file

    ​						 — `ofstream` — output to file

  - e.g. in c:

    ```c
    #include <stdio.h>
    int main(){
      char s[256];
      FILE *file = fopen("suite.txt", t);
      while(true)
      {
        if(feof(file)) break;
        printf("%\n", s);
      }
      fclose(file);
    }
    ```

  - e.g. in c++:

    ```c++
    #include <fstream>
    #include <iostream>
    int main(){
      //initialize and opens file(for reading)
      ifstream file("suite.txt");
      string s;
      while(file >> s){
        cout << s << endl;
      }
    }		//implicit close here -ifstream goes out of scope
    ```

  - e.g. string — use streams to read from/write to strings

    - `#include<sstream>`

      - `std::istringstream` — read from string

      - `std::ostringstream` — write to string

        ```c++
        int li = 5;
        int hi = 65;
        ostringstream ss;
        ss << "Enter a number between" << li << "and" <<hi;
        string s = ss.str();
        cout << s << endl;
        ```

  - e.g. convert a string to a numbr

    ```c++
    int n;
    while(true)
    {
      cout << "Enter a number" << endl;
      string s;
      cin >> s;
      istringstream ss(s);
      if(ss >> n)break;
      cout <<"I Said,";
    }
    cout << n << endl;
    ```

  - e.g. reunited

    ```c++
    int main(){
      string s;
      while(cin >> s){
        istringstream ss(s);
        int n;
        if(ss >> n) cout << n << endl;
      }
    }
    ```

## Strings

- in c

  - an array of characters(`char *`, `char[]`), terminated with null(`\0`)
  - explicity manage memory, if a string grows, need to allocated more memory
  - risky — easy to overwrite `\0` , easy to corrupt memory

- in c++

  - string are a type
  - grow as needed — no manager
  - safer — no memory corruption
  - much easier to use

  ```c++
  #include <string>
  std::string

  string s1;
  string s2;

  s1 == s2;	//compare the contents
  s1 != s2; 	//inequality
  s2 < s3; 	//comparision
  s3 <= s4; 	//lexographic
  s1.length();//length
  s1 = s2+s3;	//concatenation
  s4 +=s5; 	//concatenation
  ```

  - c-style character access

    ```c
    string s{"abc"};

    s[0]->a;
    s[1]->b;
    s[2]->c;
    ```

    - note: not null-terminated
    - more — course notes

## Default function parameters

- make function parameter optional, or supply a default

  ```c++
  void printSuiteFile(string file = "suite.txt")
  {
    ifstream file{name};
    string s;
    while(file >> s)
    {
      cout << s << endl;
    }
  }
  ```

  - `printSuiteFile();`
  - `printSuiteFile("suite2.txt")`

  ​    

  - can have multiple default parameters
  - can mix default/non default in your parameters

  ​    

  - e.g.

    ```c++
    void func(int a, int b, int c = 5){}
    func(a,b,c);
    func(a,b);

    void func2(int a = 5, int b = 6, int c){}
    func2(7, 3, 4);		//ok
    func2(5);			//c = 5 ok.....
    func2(1,3);			//I give up
    						//-ambiguous(avoid)
    						//-default parameters go at the end 
    						//   of the parameter list
    ```

    ​