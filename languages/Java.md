# Java

## Basics

- Close syntax to the C language, allied to an Object Oriented approach
- .java files - code (class implementation)
- To compile: `javac -cp .:lib/lib1.jar *.java`
- To run: `java -cp .:lib/lib1.jar MainClass`

## Minimal Working Example

Program.java:
```java
public class Program {

    protected int a;

    public Program(int a) {
        this.a = a;
    }

    public static void main(String[] args) {
        Program p = new Program(0);
    }

    public void overrideMe() {
        System.out.println(a + ": This will be overridden");
    }
}
```

## Inheritance

Child.java:

```java
public class Child extends Program {

    public Child() {
        super(1);
    }

    @Override
    public void overrideMe() {
        System.out.println(a + " ": This overrides the parent method");
    }
}
```

## Conditionals & Loops

Besides those found in C, we have:

```java
for (Object o : myObjectList) {
}
```

## Variables

```java
int i = 1;
float f = 2.4f;
double d = 2.6;
String s = "text";
Integer ii = 1;
Float ff = 2.4f;
Double dd = 2.6;
Arrays and Lists

LinkedList<Integer> ll = new LinkedList<Integer>();
ArrayList<String> al = new ArrayList<String>();
al.add("Hello");
al.add("World");
al.remove(1); // Removes "World"
System.out.println(al.size()); // 1
int[] ia = new int[10]; // 10 zeros
ia[0] = 0;
ia[1] = 1;
System.out.println(ia.length); // 10
```

## Input/Output

### Console I/O

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
try {
    String line = br.readLine();
} catch (IOException e) {
    e.printStackTrace();
}

Integer number = Integer.parseInt(line);

System.out.println("The number was: " + number);
```

### File I/O

```java
FileInputStream f = new FileInputStream("file.txt");
BufferedReader br = new BufferedReader(new InputStreamReader(f));
// (...)
f.close();

FileOutputStream of = new FileOutputStream("file.txt");
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(of));
// (...)
of.close();
```
