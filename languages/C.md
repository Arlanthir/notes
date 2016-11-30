# C

## Basics
- .h files - headers (function signatures)
- .c files - code (function bodies)
- To compile: `gcc -o exefile file1.c file2.c`

## Minimal Working Example

```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    printf("Hello World\n");
    return 0;
}
```

## Conditionals & Loops

```c
if (condition) {
} else if (condition) {
} else {
}

switch (number) {
    case 1:
        break;
    case 2:
    case 3:
        break;
    default:
}

while (condition) {
    // Change condition to false or break;
}

for (int i = 0; i < 10; ++i) {
    // Do stuff, continue; or break;
}

do {
} while (condition);
```

## Memory Allocation

Common basic types: `char`, `short`, `int`, `long`, `long long`, `float`, `double`

```c
#include <stdlib.h>

typedef struct {
    int x;
    int y;
} point;

point p;
p.x = 2;

point *p2 = malloc(sizeof(point));
p2->x = 3;
doThings(p2);
free(p2);
```

## Input/Output

```c
#include <stdio.h>
```

### Console I/O

```c
printf("Format tag with %d, %f, %lf, %c, %s", sInt, sFloat, sDouble, sChar, sCharArray);
int day, month, year;
scanf("%d/%d/%d", &day, &month, &year);
```

### File I/O

```c
FILE *fp = fopen("file.txt", "rw"); // r read, w write, a append, b binary
fprintf(fp, "hello");
char s[10];
fgets(s, 10, fp);
printf("Read: %s\n", s);
fclose(fp);
```

## Comments

```c
// Line Comments
/* Multi-line Comments */
```
