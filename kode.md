

Uninitialized variable
---------------------

```C
int sum(int arr[], int size){
    int sum;
    for(int i = 0; i < size; i++){
        sum += arr[i];
    }
    return sum;
}
```



Static variable
---------------

```C
#include <stdio.h>

void foo(void){
    static int a = 3;
    a++;
    printf("%d\n", a);
}

int main(void){
    foo();
    foo();
    foo();
}
```



Global variable
---------------

```C
#include <stdio.h>

int a;

void foo(void){
    a++;
    printf("%d\n", a);
}

int main(void){
    foo();
    foo();
    foo();
}
```



Storage classes
---------------

```C
int * fortytwo(void){
    static int a = 42;
    return &a;
}
```
```C
int * fortytwo(void){
    int a = 42;
    return &a;
}
```
```C
int * fortytwo(void){
    int * a = malloc(sizeof(int));
    *a = 42
    return a;
}
```





Stack frames
------------

```C
#include <stdio.h>

void foo(void){
    int a;
    printf("%d\n", a);
}

void bar(void){
    int a = 42;
}

int main(void){
    bar();
    foo();
}
```




Stack frames with optimization
------------------------------

 - As above, but with compiler optimization enabled


 

Undefined behaviour
-------------------

```C
#include <stdio.h>

int main(void){
    int a = 41;
    a = a++;
    printf("%d\n", a);
}
```





Undefined behaviour 2
---------------------

```C
int a = 41; a++; printf("%d\n", a);
```

```C
int a = 41; a++ & printf("%d\n", a);
```

```C
int a = 41; a++ && printf("%d\n", a);
```

```C
int a = 41; if (a++ < 42) printf("%d\n", a);
```

```C
int a = 41; a = a++; printf("%d\n", a);
```



Unspecified behaviour
---------------------

```C
#include <stdio.h>

int b(void){
    printf("3");
    return 3;
}

int c(void){
    printf("4");
    return 4;
}

int main(void){
    int a = b() + c();
    printf("%d\n", a);
}
```



Sizeof and word-boundaries
--------------------------

```C
#include <stdio.h>

struct X { int a; char b; int c; };

int main(void){
    printf("%zu\n", sizeof(int));
    printf("%zu\n", sizeof(char));
    printf("%zu\n", sizeof(struct X));
}
```







