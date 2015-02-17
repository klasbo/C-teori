
LF: C-programmering
===============
*Teoriøving i TTK4235 Tilpassede datasystemer*


1: Verktøykjeden
----------------

 1. *Fra Eksamen 2012, Oppgave 5c*:
   - Se også LF fra eksamen
   - Trinnene:
     - .c-fil
     - preprosessor
     - kompilator
     - (.S-fil)
     - assembler
     - .o-fil
     - lenker
     - kjørbart program
     - (lasting og kjøring)
   - Kompilering gjøres én gang per .c-fil (genererer én .o-fil for hver .c-fil). Lenking gjøres samlet for alle .o-filene.
   - Merk at noen kompilatorer hopper over assemblersteget, og genererer maskinkode direkte.


2: Filer og Moduler
-------------------

 1. Hva er forskjellen mellom deklarasjoner og definisjoner?
   - Deklarasjon:
     - Det kompilatoren trenger for å forstå et navn (eng: "identifier")
     - For type-sjekking
       - (Ved manglende deklarasjon av funksjoner vil kompilatoren forsøke å lage en implisitt deklasjon av typen `int()` (funksjon som tar et ukjent antall parametre, og returnerer en int))
     - Bruker ikke plass i minne
     - Kan eksistere så mange ganger som man vil (så lenge de ser like ut)
     - Ser slik ut (eksempel):
       - Funksjon: `int foo(int, double);`
       - Variabel: `extern int x;`
   - Definisjon:
     - Det som trengs av lenkeren
       - Kompilatoren generer symboler fra definisjoner
     - Bruker plass i minne (for funksjoner: generert kode, for variabler: minneplaseringen til variabelen)
     - Kan kun eksistere én gang
     - Definisjoner er også deklarasjoner
     - Ser slik ut (eksempel):
       - Funksjon: `int foo(int a, double b){/*kode...*/}`
       - Variabel: `int x;`

 1. Er det noen fundamental forskjell mellom headerfiler (*.h) og c-filer?
   - Nei.
   - Men det er en kulturell forskjell:
     - Deklarasjoner går i headerfiler
     - Definisjoner går i c-filer
   - (C++ -spesifikt: Templates må være i #include-erte filer, fordi instansiering må ha hele templatet)
 
 1. Hva gjør `static`:
   - På en variabel inni en funksjon
     - Beholder verdien mellom invokasjoner av funksjonen
     - (Dvs. ikke stack-allokert, variabelen har en fast minneplasering)
   - På en global variabel (dvs utenfor en funksjon) eller en funksjon
     - Symboler eksporteres ikke fra denne kompilerings-enheten (tenk "denne .c-filen")
       - (Strengt tatt så er symbolene lokale for objektfilen, og lenkeren velger å se bort i fra disse når den lenker sammen)
     - Variabler/funksjoner kan ikke refereres til fra andre kompilerings-enheter
       - Omtrentlig analogt til `private` i språk som har modulbegrep

 1. *Fra Eksamen 2012, Oppgave 5b*:
![](2014_kont_1b_klassediagram.png)
Hvilke mekanismer i programmeringsspråket C er egnet til å implementere den modulariseringen (oppdelingen), avhengigheten og synligheten som dette klassediagrammet beskriver?
  - Se også LF fra eksamen
  - Modularisering:
    - En .c-fil for hvert delsystem
  - Synlighet:
    - En .h-fil for delsystem B, hvor `extern int logg;` og `bool start();` er deklarert
    - `int teller` og `bool sjekk(int)` er deklarert `static` i c-filen
  - Avhengighet:
    - Delsystem A `#include`-erer delsystem B sin headerfil, slik at kompilatoren har de nødvendige deklarasjonene av navnene 
  
  

3: Warnings & Errors
--------------------

 1. Hvilket verktøy (preprosessor, kompilator, assembler, lenker) kommer disse feilmeldingene fra, og hva er den mest sannsynlige årsaken til feilmeldingen?
   - `error: 'x' undeclared (first use in this function)`
     - Kompilatoren. Variabelen `x` er brukt uten at den er deklarert
   - `warning: implicit declaration of function 'foo'`
     - Kompilatoren. Funksjonen `foo` er kalt uten at den er deklarert
     - Kompilatoren vil implisitt deklarere `foo` som en funksjon av typen `int()`
   - `undefined reference to 'foo'`
     - Lenkeren (når den skal generere et kjørbart program). Skjer hvis det er et symbol som mangler, gjerne ved at man har glemt å gi lenkeren en .o-fil eller et bibliotek
   - `fatal error: file.h: No such file or directory`
     - Preprosessoren. Filen finnes ikke (eller er feilstavet)

 1. Hva er en "Segmentation fault"?
   - Programmet prøver å aksessere minne den ikke eier, eller prøver å bruke minnet på feil vis (feks skrive over programkoden eller annet read-only-data)
     - "Segment" kommer fra disse forskjellige typene minne: Kjørbar kode, data, osv.
   - Skjer kun på hardware med minnebeskyttelse

4: C-kode
---------

 1. Hva er galt med denne koden?

    ```C
    int sum(int arr[], int size){
        int sum;
        for(int i = 0; i < size; i++){
            sum += arr[i];
        }
        return sum;
    }
    ```
    
    - Variabelen `sum` er uinitialisert, og vil derfor inneholde hva enn som lå i det minnet fra før.
    - Funksjonen vil derfor returnere søppeldata.

 1. Hva er forskjellen på `hello_1` og `hello_2`, og hvorfor vil en av disse ikke fungere?

    ```C
    int main(void){
        char * hello_1 = "hello";
        hello_1[1] = 'a';
        printf("%s\n", hello_1);
        
        char hello_2[] = "hello";
        hello_2[1] = 'a';
        printf("%s\n", hello_2);
    }
    ```
    - Alle streng-literaler (tekst i fnutter, eg `"hello"`), ligger i read-only-minne
    - `hello_1` peker derfor til en konstant som ligger i read-only-minne
      - Denne kan ikke skrives til
    - `hello_2` er et array som ligger på stacken
      - Denne har fast størrelse 6 (en byte for hver bokstav, og ett byte for 0-terminering)
        - Kompilatoren finner størrelsen implisitt ved å se på streng-literalen
      - Konstanten `"hello"` ligger i read-only-minne, og kopieres til arrayet når `main()` kalles.
      - Kopien er ikke read-only, og kan derfor skrives til.

 1. Hva skriver denne koden ut?

    ```C
    int main(void){
        int a = 42;
        int * p = &a;
        *p = 8;
        printf("a = %d\n", a);
    }
    ```
    - Skriver ut `a = 8`
    - Læringsmål:
      - Forstå at man kan skrive til et sted i minne via en peker
      - Forstå at variabler som ligger i dette minnet kan bli endret gjennom en peker (dvs en peker tar ikke en kopi)
      - Forstå at `*` betyr forskjellige ting i forskjellige kontekster:
        - `int *` er en variabeltype (peker-til-int)
        - `*p` er en operasjon utført på `p` (verdi-på-plassering p)

 1. Hvilken operasjon utfører funksjonen `rotate()`?
    
    - Dette er eksempelimplementasjonen av funksjonen i C++'s Standard Template Library
    - Ekvivalent med:
    
       ```Python
       def rotate(arr, first, n_first, last):
           return arr[:first] + arr[n_first:last] + arr[first:n_first] +  arr[last:]
       ```
      
    - Roterer `[n_first:last]` frem til posisjonen av `first`
    - Læringsmål:
      - Forstå `swap()` via pekere
        - Bytter verdien på to minneadresser
      - Forstå postfix `++`:
        - At dette gjøres *etter* funksjonskallet
        - At man kan inkrementere/dekrementere pekere for å traversere minne (iterere gjennom arrays)
      - At man kan sammenlikne pekere for å se om de refererer til samme minneområde
      - (`len()`-makroen og sizeof)
      - Det er ikke et mål å forstå akkurat hvordan denne implementasjonen av rotate fungerer
    - Utskrift:
    
        ```
        {1, 2, 3, 4, 5, 6, 7, 8, 9}
        {4, 5, 6, 7, 8, 9, 1, 2, 3}
        {6, 7, 8, 9, 1, 4, 5, 2, 3}
        {6, 7, 9, 1, 8, 4, 5, 2, 3}
        ```
      


 1. Hvorfor kræsjer dette programmet med en "Segmentation fault"?

    ```C
    int main(){
        int * p;
        printf("Value at p: %d\n", *p);
    }
    ```
    - `p` er uinitialisert, og vil derfor peke til en tilfeldig minneadresse
    - Sannsynligheten for at dette minnet er gyldig er minimal

 1. Hvilke av disse er gyldige?

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
    - I den øverste vil variabelen `int a = 42;` automatisk frigjøres når funksjonen returnerer (stack-allokering)
      - Funksjoner som kalles senere vil bruke dette minneområdet
      - Dermed vil det ikke nødvendigvis stå `42` når vi ser på dette minneområdet senere
    - I den nederste allokerer vi manuelt minne med `malloc` (heap-allokering)
      - `malloc` returnerer addressen til et nytt (ubrukt) minneområde av angitt størrelse
      - Dette minnet er ikke automatisk frigitt når funksjonen returnerer

      
      
      
      
