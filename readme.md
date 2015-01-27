Verktøykjeden (preprosessor, kompilator, lenker)
    eksamensoppg

Filtyper (.c, .h, .o, kjørbar)
    Moduler i C?
        hva går hvor .h /.c
        deklarasjon vs definisjon

Symboler og segmenter (hvis han har noe om det? Må spørre.)

Warnings/errors (hvor kommer de fra? kompilator eller lenker? hvilken linje i eksempelet?)

Grunnleggende C-syntaks

Enkle(!) pekereksempler.
    return peker fra fn
    

    
Notater
=======

LF?
    
    
1: Verktøykjeden
================
Enten/eller
 - Midtsemester 2008 oppg 5a-e
 - Eksamen 2012 oppg 5c

<br>

 - Marker også hvor objektfiler (*.o) kommer inn.

 
2: Filtyper
===========

 
 - Hva er forskjellen mellom deklarasjoner og definisjoner?
 
 - Er det noen fundamental forskjell mellom headerfiler (*.h) og c-filer?
 
 - **[?]** Hva gjør `static`:
   - På en variabel i en funksjon
   - På en variabel utenfor en funksjon
   
 - Hvordan kan man lage en modul ved bruk av flere filer?
 
3: Warnings/Errors
==================

 - Hvilket verktøy (preprosessor, kompilator, assempler, lenker) kommer disse feilmeldingene fra?
   - Undefined reference
   - No such file
   - Undeclared
   - Implicit declaration
   
 - **[?]** Segfault

 
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




 - Hvilke av disse er gyldige?

**Ta med static?**
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
 
