# C++

## Basics

- Close syntax to the C language, allied to an Object Oriented approach
- .h files - headers (class definition)
- .cpp files - code (class implementation)
- To compile: `g++ -o exefile file1.cpp file2.cpp`

## Minimal Working Example

program.h:

```c++
#ifndef PROGRAM_H
#define PROGRAM_H

int main(int argc, char *argv[]);

class Program {
    protected:
        int a;
    public:
        Program(int a);
        virtual void overrideMe();
};

#endif
```

program.cpp:

```c++
#include "program.h"
#include <iostream>

int main(int argc, char *argv[]) {
    Program p = Program(0);
    p.overrideMe();
    return 0;
}

Program::Program(int a) {
    this->a = a;
}

void Program::overrideMe() {
    std::cout << a << ": This will always call the implementation furthest down in the hierarchy\n";
}
```

## Inheritance

New main:

```c++
int main(int argc, char *argv[]) {
    Program *p = new Child(); // non-pointer ref would not hold Child info, just Program
    p->overrideMe();
    delete p;
    return 0;
}
```

child.h:

```c++
#ifndef CHILD_H
#define CHILD_H

#include "program.h"

class Child : public Program {
    public:
        Child();
        virtual void overrideMe();
};

#endif
```

child.cpp:

```c++
#include "child.h"
#include <iostream>

Child::Child() : Program(1) {
}

void Child::overrideMe() {
    std::cout << a << ": This is overriden\n";
}
```

## Input/Output

### Console I/O

```c++
#include <iostream>
std::out << 0 << ": Please insert day month year:\n";
int day, month, year;
std::cin >> day >> month >> year;
```

### File I/O

```c++
#include <iostream>
#include <fstream>
#include <string>

int main() {
    std::ofstream outFile("file.txt");
    outFile << "hello\n";
    outFile.close();

    std::ifstream inFile("file.txt");
    std::string line;
    getline(inFile, line);
    inFile.close();

    std::cout << "Read: " << line << std::endl;
    return 0;
}
```
