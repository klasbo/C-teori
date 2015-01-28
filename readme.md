Notater
=======

Legge ut LF?
    
Noe mer om symboler/segmenter? .text vs .data?
    
    
1: Verktøykjeden
================
Enten/eller (Tegn trinnene fra kildekode til kjørbart program)
 - Midtsemester 2008 oppg 5a-e
 - Eksamen 2012 oppg 5c

<br>

 - Marker også hvor objektfiler (*.o) kommer inn.

 
2: Filer og Moduler
===================

 - Hva er forskjellen mellom deklarasjoner og definisjoner?
 
 - Er det noen fundamental forskjell mellom headerfiler (*.h) og c-filer?

 - Hvordan kan man lage en modul ved bruk av flere filer?
   - Evt: Kont 2014 oppg 1b ("Hvilke mekanismer i C er egnet til å implementere modulariseringen...")
 
3: Warnings/Errors
==================

 - Hvilket verktøy (preprosessor, kompilator, assempler, lenker) kommer disse feilmeldingene fra?
   - Undefined reference to ...
   - file.h: No such file
   - variable "x" undeclared
   - Implicit declaration of function ...

 
4: C-kode
=========

 - Hva er galt med denne koden?

```C
int sum(int arr[], int size){
    int sum;
    for(int i = 0; i < size; i++){
        sum += arr[i];
    }
    return sum;
}
```

5: Pekere
=========

 - Hva skriver denne koden ut?
 
```C
int main(void){
    int a = 42;
    int * p = &a;
    *p = 8;
    printf("a = %d\n", a);
}
```

 - Hva skriver denne koden ut?
   - **[For "virkelighetsfjernt"?]**

```C
int a = 42;

int foo(int * p){
    int b = *p;
    b += 6;
    return b;
}

int main(void){
    printf("foo(&a) = %d\n", foo(&a));
    printf("a = %d\n", a);
    return 0;
}
```

 - Hvilken operasjon utfører funksjonen [`rotate()`](rotate.c)? 
   - **[For vanskelig?]**

   
 - Hvorfor kræsjer dette programmet med en "Segmentation fault"?
 
```C
int main(){
    int * p;
    printf("Value at p: %d\n", *p);
}
```

 - Hvilke av disse er gyldige?


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
 
