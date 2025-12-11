# 📚 KORTFATTAT Kapitel 2-6 i Java How to Program

## **Kapitel 2 - Introduktion till Java-applikationer**

### 🎯 Kapitelöversikt

Kapitel 2 introducerar grunderna för att skriva Java-program som kan ta emot indata från användaren, utföra beräkningar och fatta beslut baserat på jämförelser. Detta är fundamentalt för all programmering och lägger grunden för mer avancerade koncept.

### 📝 **1. Ditt första Java-program - Att skriva ut text**

Ett Java-program börjar alltid sin exekvering från `main`-metoden. Varje Java-applikation måste ha följande struktur:

java

```java
public class MittProgram {
    public static void main(String[] args) {
        // Din kod här
    }
}
```

**Viktiga komponenter att förstå:**

- **`public`** - Gör klassen tillgänglig från överallt
- **`class`** - Definierar att vi skapar en klass
- **`main`** - Startpunkten för alla Java-applikationer
- **`String[] args`** - Tar emot kommandoradsargument (kommer användas senare)

### 💡 **2. Kommentarer - Dokumentera din kod**

Java erbjuder tre typer av kommentarer:

java

```java
// Enkelradskommentar - används för korta förklaringar

/* Traditionell kommentar
   kan sträcka sig över
   flera rader */

/** Javadoc-kommentar
 *  Används för att generera dokumentation
 *  automatiskt från koden
 */
```

Kommentarer ignoreras av kompilatorn men är kritiska för att göra koden förståelig för människor. Tänk på dem som anteckningar till dig själv och andra programmerare.

### 🔤 **3. Primitiva datatyper**

Java har åtta primitiva datatyper som är byggstenar för all datahantering:

|Datatyp|Storlek|Värdeområde|Användning|
|---|---|---|---|
|`byte`|8 bitar|-128 till 127|Små heltal, spara minne|
|`short`|16 bitar|-32,768 till 32,767|Medelstora heltal|
|`int`|32 bitar|-2,147,483,648 till 2,147,483,647|Standard för heltal|
|`long`|64 bitar|-9.2×10¹⁸ till 9.2×10¹⁸|Stora heltal|
|`float`|32 bitar|±3.4×10³⁸ (6-7 decimaler)|Decimaltal med lägre precision|
|`double`|64 bitar|±1.7×10³⁰⁸ (15 decimaler)|Standard för decimaltal|
|`char`|16 bitar|Unicode-tecken|Enstaka tecken|
|`boolean`|1 bit|true eller false|Logiska värden|

### 📥 **4. Input/Output med Scanner**

För att interagera med användaren använder vi `Scanner`-klassen:

java

```java
import java.util.Scanner;  // Importera Scanner-klassen

public class InputExample {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);  // Skapa Scanner-objekt
        
        System.out.print("Ange ditt namn: ");    // Prompt utan radbrytning
        String namn = input.nextLine();          // Läs hela raden
        
        System.out.print("Ange din ålder: ");
        int alder = input.nextInt();             // Läs heltal
        
        System.out.printf("Hej %s, du är %d år gammal%n", namn, alder);
    }
}
```

**Scanner-metoder att memorera:**

- `nextInt()` - läser ett heltal
- `nextDouble()` - läser ett decimaltal
- `nextLine()` - läser hela raden som text
- `next()` - läser ett ord (fram till mellanslag)

### 🧮 **5. Aritmetiska operatorer**

Java har fem grundläggande aritmetiska operatorer:

java

```java
int a = 10, b = 3;
int summa = a + b;        // Addition: 13
int differens = a - b;    // Subtraktion: 7
int produkt = a * b;      // Multiplikation: 30
int kvot = a / b;         // Heltalsdivision: 3 (inte 3.33!)
int rest = a % b;         // Modulo (rest): 1
```

**⚠️ Viktig observation om heltalsdivision:** När båda operanderna är heltal, blir resultatet också ett heltal. Decimaldelen försvinner!

java

```java
int x = 5 / 2;        // Resultat: 2 (inte 2.5)
double y = 5.0 / 2;   // Resultat: 2.5 (minst en operand är double)
```

### 🎯 **6. Operatorprioritet**

Precis som i matematik har operatorer en prioritetsordning:

1. **Högst prioritet:** `*`, `/`, `%` (vänster till höger)
2. **Lägre prioritet:** `+`, `-` (vänster till höger)

java

```java
int resultat = 2 + 3 * 4;     // 14 (inte 20!)
// Beräknas som: 2 + (3 * 4) = 2 + 12 = 14

int annatResultat = (2 + 3) * 4;  // 20
// Parenteser ändrar ordningen
```

### ⚖️ **7. Jämförelse- och likhetsoperatorer**

För att fatta beslut i program använder vi jämförelseoperatorer:

java

```java
int x = 5, y = 10;

// Likhetsoperatorer
if (x == y)  // Lika med
if (x != y)  // Inte lika med

// Jämförelseoperatorer
if (x < y)   // Mindre än
if (x > y)   // Större än
if (x <= y)  // Mindre än eller lika med
if (x >= y)  // Större än eller lika med
```

### 🧠 **8. Minneskonceptet**

När vi deklarerar variabler reserveras plats i datorns minne:

java

```java
int tal1;          // Reserverar plats för ett heltal
tal1 = 45;         // Lagrar värdet 45 på den platsen
int tal2 = 72;     // Deklarerar och initialiserar samtidigt
int summa = tal1 + tal2;  // Beräknar och lagrar 117
```

**Visualisering av minnet:**
```
Variabel | Värde | Minnesplats
---------|-------|------------
tal1     | 45    | 0x1234
tal2     | 72    | 0x1238
summa    | 117   | 0x123C
```

---

## **Kapitel 3 - Kontrollstrukturer**

### 🎯 Kapitelöversikt
Detta kapitel introducerar fundamentala koncept för att styra programflödet: algoritmer, pseudokod och de tre grundläggande kontrollstrukturerna som alla program bygger på.

### 🔄 **1. Algoritmer och problemlösning**

En **algoritm** är en steg-för-steg procedur för att lösa ett problem. Den specificerar:
1. Vilka åtgärder som ska utföras
2. I vilken ordning de ska utföras

**Exempel - Morgonrutinalgoritm:**
```
1. Vakna
2. Stäng av väckarklockan
3. Gå upp ur sängen
4. Duscha
5. Klä på dig
6. Ät frukost
7. Åk till jobbet/skolan
```

Ordningen är kritisk! Om du klär på dig före du duschar blir resultatet problematiskt.

### 📝 **2. Pseudokod**

Pseudokod är ett informellt språk som hjälper dig planera algoritmer utan att oroa dig för programmeringsspråkets syntax:
```
Pseudokod för att beräkna medelbetyg:
    Sätt total till 0
    Sätt räknare till 0
    
    Medan det finns fler betyg att läsa in
        Läs in nästa betyg
        Lägg till betyget till total
        Öka räknare med 1
    
    Beräkna medelvärde som total delat med räknare
    Visa medelvärdet
````

### 🏗️ **3. De tre fundamentala kontrollstrukturerna**

**All** programmering kan uttryckas med bara tre kontrollstrukturer:

#### **a) Sekvensstruktur**

Satser exekveras en efter en i ordning:

java

```java
int x = 5;        // Först
x = x * 2;        // Sedan
System.out.println(x);  // Sist
```

#### **b) Selektionsstruktur (val)**

Programmet väljer mellan alternativa vägar:

java

```java
if (temperature > 30) {
    System.out.println("Det är varmt!");
} else {
    System.out.println("Det är svalt.");
}
```

#### **c) Iterationsstruktur (upprepning)**

En uppsättning satser upprepas:

java

```java
while (counter < 10) {
    System.out.println(counter);
    counter++;
}
```

### 🔀 **4. if och if-else satser**

**Enkel if-sats:**

java

```java
if (villkor) {
    // Kod som körs om villkoret är sant
}
```

**if-else för två alternativ:**

java

```java
if (poang >= 60) {
    System.out.println("Godkänt!");
} else {
    System.out.println("Underkänt");
}
```

**Nästlade if-else för flera alternativ:**

java

```java
if (betyg >= 90) {
    betygsBokstav = 'A';
} else if (betyg >= 80) {
    betygsBokstav = 'B';
} else if (betyg >= 70) {
    betygsBokstav = 'C';
} else if (betyg >= 60) {
    betygsBokstav = 'D';
} else {
    betygsBokstav = 'F';
}
```

### 🔁 **5. while-loopen**

While-loopen upprepar kod så länge ett villkor är sant:

java

```java
int raknare = 1;
while (raknare <= 5) {
    System.out.println("Räknare: " + raknare);
    raknare++;
}
// Skriver ut: 1, 2, 3, 4, 5
```

**Viktigt att komma ihåg:**

- Villkoret testas FÖRE varje iteration
- Om villkoret är falskt från början körs loopen aldrig
- Se till att villkoret någon gång blir falskt (undvik oändliga loopar!)

### 📊 **6. Räknarstyrda loopar**

När vi vet exakt hur många gånger något ska upprepas:

java

```java
int total = 0;
int antalBetyg = 10;
int raknare = 1;

while (raknare <= antalBetyg) {
    System.out.print("Ange betyg " + raknare + ": ");
    int betyg = input.nextInt();
    total += betyg;
    raknare++;
}

double medelvarde = (double) total / antalBetyg;
```

### 🚩 **7. Vaktpostsstyrda loopar (Sentinel-controlled)**

När vi inte vet hur många gånger något ska upprepas använder vi en speciell "stoppsignal":

java

```java
int total = 0;
int antal = 0;
int betyg;

System.out.print("Ange betyg (-1 för att avsluta): ");
betyg = input.nextInt();

while (betyg != -1) {  // -1 är vaktposten
    total += betyg;
    antal++;
    
    System.out.print("Ange betyg (-1 för att avsluta): ");
    betyg = input.nextInt();
}

if (antal != 0) {
    double medelvarde = (double) total / antal;
    System.out.printf("Medelvärde: %.2f%n", medelvarde);
}
```

### ➕ **8. Sammansatta tilldelningsoperatorer**

Java erbjuder förkortningar för vanliga operationer:

java

```java
int x = 10;
x += 5;   // Samma som: x = x + 5;   (x blir 15)
x -= 3;   // Samma som: x = x - 3;   (x blir 12)
x *= 2;   // Samma som: x = x * 2;   (x blir 24)
x /= 4;   // Samma som: x = x / 4;   (x blir 6)
x %= 4;   // Samma som: x = x % 4;   (x blir 2)
```

### 🔢 **9. Inkrement- och dekrementoperatorer**

För att öka eller minska med 1:

java

```java
int a = 5;
a++;     // Postinkrement: använd a, öka sedan
++a;     // Preinkrement: öka först, använd sedan

int b = 10;
int c = b++;  // c får värdet 10, b blir 11
int d = ++b;  // b blir 12 först, d får värdet 12
```

**Minnesregel:**

- **Pre** (före): Ändra först, använd sedan
- **Post** (efter): Använd först, ändra sedan

---
## **Kapitel 4 - Kontrollstrukturer och Logiska operatorer**

### 🎯 Kapitelöversikt

Nu när vi behärskar grunderna från kapitel 3, är det dags att utöka vår verktygslåda med mer kraftfulla kontrollstrukturer. Tänk på det som att uppgradera från en enkel hammare till en komplett verktygslåda - varje verktyg har sitt specifika användningsområde där det är mest effektivt.

### 🔄 **1. for-loopen - När precision är nyckeln**

For-loopen är som en schweizisk armékniv för iteration - den samlar all loop-kontroll på ett ställe, vilket gör koden mer läsbar och mindre felbenägen.

java

```java
// Anatomin av en for-loop
for (initialisering; villkor; uppdatering) {
    // Kod som upprepas
}

// Konkret exempel: Räkna från 1 till 10
for (int i = 1; i <= 10; i++) {
    System.out.println("Räknare: " + i);
}
```

Låt oss bryta ner vad som händer steg för steg:

1. **Initialisering** (`int i = 1`) - Körs EN gång innan loopen startar
2. **Villkor** (`i <= 10`) - Kontrolleras före VARJE iteration
3. **Kroppen** körs om villkoret är sant
4. **Uppdatering** (`i++`) - Körs efter VARJE iteration
5. Gå tillbaka till steg 2

En vanlig användning är att summera värden:

java

```java
// Beräkna summan av alla jämna tal mellan 2 och 20
int summa = 0;
for (int tal = 2; tal <= 20; tal += 2) {
    summa += tal;  // 2 + 4 + 6 + ... + 20
}
System.out.println("Summa av jämna tal: " + summa);  // 110
```

### 💰 **2. Ränta-på-ränta - Ett praktiskt exempel**

Här ser vi kraften i for-loopen för att lösa verkliga problem. Tänk dig att du sätter in pengar på ett sparkonto:

java

```java
double grundbelopp = 1000.00;  // Startkapital
double rantesats = 0.05;        // 5% årlig ränta

System.out.println("År\tBelopp på kontot");

for (int ar = 1; ar <= 10; ar++) {
    // Formeln för ränta-på-ränta: A = P(1 + r)^n
    double belopp = grundbelopp * Math.pow(1.0 + rantesats, ar);
    
    // Visa resultatet snyggt formaterat
    System.out.printf("%d\t%,.2f kr%n", ar, belopp);
}
```

Detta exempel visar hur matematik och programmering arbetar tillsammans. `Math.pow()` är Javas sätt att hantera exponenter, något som saknas som en inbyggd operator.

### 🔁 **3. do-while - När du måste göra något minst en gång**

Ibland behöver vi garantera att något händer minst en gång, oavsett villkoret. Det är här do-while kommer in:

java

```java
// Exempel: Användarinmatning med validering
Scanner input = new Scanner(System.in);
int tal;

do {
    System.out.print("Ange ett tal mellan 1 och 10: ");
    tal = input.nextInt();
    
    if (tal < 1 || tal > 10) {
        System.out.println("Ogiltigt! Försök igen.");
    }
} while (tal < 1 || tal > 10);

System.out.println("Tack! Du angav: " + tal);
```

Skillnaden mellan while och do-while är subtil men viktig:

- **while**: Testar villkoret FÖRST, kanske kör aldrig
- **do-while**: Kör FÖRST, testar villkoret SEDAN, kör minst en gång

### 🎛️ **4. switch - När if-else blir för rörigt**

När du har många specifika värden att testa blir switch-satsen elegant:

java

```java
System.out.print("Ange betyg (A-F): ");
char betyg = input.next().charAt(0);
String kommentar;

switch (betyg) {
    case 'A':
    case 'a':  // Hanterar både stora och små bokstäver
        kommentar = "Utmärkt prestation!";
        break;
        
    case 'B':
    case 'b':
        kommentar = "Mycket bra arbete!";
        break;
        
    case 'C':
    case 'c':
        kommentar = "Bra jobbat!";
        break;
        
    case 'D':
    case 'd':
        kommentar = "Godkänt, men kan förbättras.";
        break;
        
    case 'F':
    case 'f':
        kommentar = "Tyvärr underkänt.";
        break;
        
    default:
        kommentar = "Ogiltigt betyg angivet.";
        break;
}

System.out.println(kommentar);
```

**Viktigt om switch:**

- Varje `case` behöver vanligtvis ett `break` för att undvika "fall-through"
- `default` fångar alla värden som inte matchar något case
- Flera cases kan dela samma kod (som A och a ovan)

### 🛑 **5. break och continue - Finkontrollen**

Dessa två nyckelord ger dig exakt kontroll över loopflödet:

**break - Avbryt helt:**

java

```java
// Hitta första talet delbart med både 3 och 7
for (int i = 1; i <= 100; i++) {
    if (i % 3 == 0 && i % 7 == 0) {
        System.out.println("Första talet delbart med både 3 och 7: " + i);
        break;  // Hoppa ut ur loopen direkt
    }
}
```

**continue - Hoppa över denna iteration:**

java

```java
// Skriv ut alla tal 1-10 utom 5 och 7
for (int i = 1; i <= 10; i++) {
    if (i == 5 || i == 7) {
        continue;  // Hoppa till nästa iteration
    }
    System.out.println(i);
}
// Skriver ut: 1, 2, 3, 4, 6, 8, 9, 10
```

### 🧠 **6. Logiska operatorer - Bygga komplexa villkor**

Logiska operatorer låter oss kombinera flera villkor till mer sofistikerade beslut:

#### **AND-operatorn (&&) - Båda måste vara sanna**

java

```java
int alder = 25;
boolean harKorkort = true;

if (alder >= 18 && harKorkort) {
    System.out.println("Du får hyra en bil!");
}
// Både ålder OCH körkort krävs
```

#### **OR-operatorn (||) - Minst en måste vara sann**

java

```java
String dag = "Lördag";
boolean semester = false;

if (dag.equals("Lördag") || dag.equals("Söndag") || semester) {
    System.out.println("Du behöver inte jobba idag!");
}
// Helg ELLER semester räcker
```

#### **NOT-operatorn (!) - Vänd på sanningsvärdet**

java

```java
boolean regnar = false;

if (!regnar) {  // Om det INTE regnar
    System.out.println("Perfekt väder för picknick!");
}
```

### ⚡ **7. Kortslutningsutvärdering - Effektivitet i praktiken**

Java är smart och slutar utvärdera så fort svaret är klart:

java

```java
// Exempel på kortslutning med &&
int x = 5;
int y = 0;

// Detta kraschar INTE även om vi dividerar med noll!
if (y != 0 && x / y > 2) {
    System.out.println("Villkoret är sant");
}
// Java ser att y != 0 är falskt och hoppar över resten

// Exempel på kortslutning med ||
String namn = null;

// Detta kraschar INTE även om namn är null!
if (namn == null || namn.isEmpty()) {
    System.out.println("Namnet saknas eller är tomt");
}
// Java ser att namn == null är sant och behöver inte kolla resten
```

Detta är inte bara en optimering - det kan förhindra programkrascher!

---

## **Kapitel 5 - Metoder**

### 🎯 Kapitelöversikt

Metoder är programmeringens sätt att organisera kod i återanvändbara byggblock. Tänk på dem som recept i en kokbok - varje recept (metod) har ingredienser (parametrar), instruktioner (metodkroppen) och ger ett resultat (returvärde).

### 🏗️ **1. Varför metoder är fundamentala**

Föreställ dig att du skriver samma kod om och om igen:

java

```java
// UTAN metoder - repetitiv och svårunderhållen kod
System.out.println("*******************");
System.out.println("*   VÄLKOMMEN!    *");
System.out.println("*******************");

// ... senare i programmet ...

System.out.println("*******************");
System.out.println("*     HEJDÅ!      *");
System.out.println("*******************");
```

Med metoder blir det elegantare:

java

```java
public static void visaRubrik(String text) {
    System.out.println("*******************");
    System.out.printf("*   %-13s *%n", text);
    System.out.println("*******************");
}

// Nu kan vi använda metoden
visaRubrik("VÄLKOMMEN!");
visaRubrik("HEJDÅ!");
```

### 📦 **2. Metodernas anatomi**

Varje metod har flera viktiga delar:

java

```java
public static returtyp metodNamn(parametertyp1 param1, parametertyp2 param2) {
    // Metodkropp
    return värde;  // Om returtyp inte är void
}
```

Låt oss dissekera en konkret metod:

java

```java
public static double beraknaCirckelArea(double radie) {
    // Validera indata
    if (radie < 0) {
        System.out.println("Fel: Radien kan inte vara negativ!");
        return 0;
    }
    
    // Beräkna och returnera arean
    double area = Math.PI * radie * radie;
    return area;
}
```

**Förklaring av delarna:**

- `public` - Metoden kan anropas från andra klasser
- `static` - Tillhör klassen, inte ett specifikt objekt (mer om detta i kapitel 7)
- `double` - Returtypen, metoden ger tillbaka ett decimaltal
- `beraknaCirckelArea` - Metodens namn (använd beskrivande namn!)
- `double radie` - Parameter som metoden tar emot
- `return area` - Skickar tillbaka resultatet

### 🔄 **3. Metoder som bygger på varandra**

En av de kraftfullaste aspekterna med metoder är hur de kan använda varandra:

java

```java
// Grundläggande matematiska metoder
public static int kvadrat(int tal) {
    return tal * tal;
}

public static int kub(int tal) {
    return tal * kvadrat(tal);  // Använder kvadrat-metoden!
}

public static double pythagoras(int a, int b) {
    // Beräkna hypotenusan: c = √(a² + b²)
    int summaKvadrater = kvadrat(a) + kvadrat(b);
    return Math.sqrt(summaKvadrater);
}
```

### 🎲 **4. Slumptal och Math-klassen**

Java erbjuder kraftfulla matematiska verktyg genom Math-klassen:

java

```java
// Simulera tärningskast
public static int kastaTarning() {
    // Math.random() ger 0.0 <= värde < 1.0
    // Multiplicera med 6: 0.0 <= värde < 6.0
    // Casta till int: 0, 1, 2, 3, 4, eller 5
    // Lägg till 1: 1, 2, 3, 4, 5, eller 6
    return (int)(Math.random() * 6) + 1;
}

// Mer avancerat: Slumptal inom ett intervall
public static int slumptal(int min, int max) {
    int intervall = max - min + 1;
    return (int)(Math.random() * intervall) + min;
}
```

### 🔍 **5. Metoders räckvidd (Scope)**

Variabler lever bara inom sitt scope - det område där de är deklarerade:

java

```java
public static void demonstreraScope() {
    int x = 10;  // x existerar bara inom denna metod
    
    if (x > 5) {
        int y = 20;  // y existerar bara inom if-blocket
        System.out.println(x + y);  // Fungerar - båda är tillgängliga här
    }
    
    // System.out.println(y);  // FEL! y existerar inte här
}

public static void annanMetod() {
    // System.out.println(x);  // FEL! x från förra metoden existerar inte här
    int x = 30;  // Detta är en HELT ANNAN variabel x
}
```

### 🔁 **6. Metodöverlagring (Method Overloading)**

Java låter dig ha flera metoder med samma namn, så länge parametrarna skiljer sig:

java

```java
// Alla dessa metoder kan samexistera!
public static int max(int a, int b) {
    return (a > b) ? a : b;
}

public static double max(double a, double b) {
    return (a > b) ? a : b;
}

public static int max(int a, int b, int c) {
    return max(max(a, b), c);  // Använder den tvåparameters-versionen!
}

// Java väljer automatiskt rätt version baserat på argumenten:
int heltal = max(5, 3);           // Använder int-versionen
double decimal = max(5.5, 3.2);   // Använder double-versionen
int treTal = max(5, 3, 8);        // Använder treparameters-versionen
```

### 🏃 **7. Rekursion - Metoder som anropar sig själva**

Rekursion är ett fascinerande koncept där en metod löser problem genom att dela upp dem i mindre delar:

java

```java
// Fakultet: n! = n × (n-1) × (n-2) × ... × 1
public static long fakultet(int n) {
    // Basfall - när ska rekursionen sluta?
    if (n <= 1) {
        return 1;
    }
    
    // Rekursivt fall - metoden anropar sig själv med ett mindre problem
    return n * fakultet(n - 1);
}

// Hur det fungerar för fakultet(5):
// fakultet(5) = 5 * fakultet(4)
//             = 5 * 4 * fakultet(3)
//             = 5 * 4 * 3 * fakultet(2)
//             = 5 * 4 * 3 * 2 * fakultet(1)
//             = 5 * 4 * 3 * 2 * 1
//             = 120
```

---

## **Kapitel 6 - Arrayer och ArrayLists**

### 🎯 Kapitelöversikt

Arrayer och ArrayLists låter oss hantera samlingar av data effektivt. Istället för att ha hundratals separata variabler kan vi organisera relaterad data i strukturerade samlingar.

### 📊 **1. Arrayer - Den grundläggande datastrukturen**

En array är som en rad numrerade lådor där varje låda kan hålla ett värde:

java

````java
// Deklarera och skapa en array
int[] betyg = new int[5];  // Skapar 5 "lådor" för heltal

// Arrayer indexeras från 0!
betyg[0] = 85;  // Första elementet
betyg[1] = 92;  // Andra elementet
betyg[2] = 76;  // Tredje elementet
betyg[3] = 88;  // Fjärde elementet
betyg[4] = 94;  // Femte elementet

// Alternativ: Initiera direkt
int[] betyg2 = {85, 92, 76, 88, 94};
```

**Visualisering av arrayen:**
```
Index:  [0] [1] [2] [3] [4]
Värde:  85  92  76  88  94
````

### 🔄 **2. Bearbeta arrayer med loopar**

Arrayer och loopar är bästa vänner:

java

```java
public static void analyseraBetyg(int[] betyg) {
    int summa = 0;
    int hogsta = betyg[0];
    int lagsta = betyg[0];
    
    // Enhanced for-loop - perfekt för att gå igenom alla element
    for (int ettBetyg : betyg) {
        summa += ettBetyg;
        
        if (ettBetyg > hogsta) {
            hogsta = ettBetyg;
        }
        if (ettBetyg < lagsta) {
            lagsta = ettBetyg;
        }
    }
    
    double medel = (double) summa / betyg.length;
    
    System.out.printf("Antal betyg: %d%n", betyg.length);
    System.out.printf("Medelvärde: %.2f%n", medel);
    System.out.printf("Högsta betyg: %d%n", hogsta);
    System.out.printf("Lägsta betyg: %d%n", lagsta);
}
```

### 🎲 **3. Kortblandning - Ett praktiskt exempel**

Låt oss skapa en kortlek och blanda den:

java

```java
public class Kortlek {
    private static final String[] FARG = {"Hjärter", "Ruter", "Klöver", "Spader"};
    private static final String[] VALOR = {"Ess", "2", "3", "4", "5", "6", "7", 
                                           "8", "9", "10", "Knekt", "Dam", "Kung"};
    
    public static String[] skapaKortlek() {
        String[] kortlek = new String[52];
        int index = 0;
        
        // Skapa alla 52 kort
        for (String farg : FARG) {
            for (String valor : VALOR) {
                kortlek[index++] = valor + " i " + farg;
            }
        }
        
        return kortlek;
    }
    
    public static void blandaKortlek(String[] kortlek) {
        Random slump = new Random();
        
        // Fisher-Yates shuffle algoritm
        for (int i = kortlek.length - 1; i > 0; i--) {
            int j = slump.nextInt(i + 1);
            
            // Byt plats på kort i och j
            String temp = kortlek[i];
            kortlek[i] = kortlek[j];
            kortlek[j] = temp;
        }
    }
}
```

### 📈 **4. Flerdimensionella arrayer**

Tänk på dessa som tabeller eller rutnät:

java

```java
// En 3x4 matris (3 rader, 4 kolumner)
int[][] matris = new int[3][4];

// Eller initiera direkt som en tabell
int[][] multiplikationstabell = {
    {1,  2,  3,  4},
    {2,  4,  6,  8},
    {3,  6,  9, 12}
};

// Bearbeta en 2D-array
public static void skrivUtTabell(int[][] tabell) {
    for (int rad = 0; rad < tabell.length; rad++) {
        for (int kol = 0; kol < tabell[rad].length; kol++) {
            System.out.printf("%4d", tabell[rad][kol]);
        }
        System.out.println();  // Ny rad efter varje tabellrad
    }
}
```

### 🔄 **5. ArrayList - Den flexibla storebror**

ArrayLists kan ändra storlek dynamiskt, till skillnad från vanliga arrayer:

java

```java
import java.util.ArrayList;

public static void demonstreraArrayList() {
    // Skapa en ArrayList för strängar
    ArrayList<String> shoppinglista = new ArrayList<>();
    
    // Lägg till element
    shoppinglista.add("Mjölk");
    shoppinglista.add("Bröd");
    shoppinglista.add("Ägg");
    
    // Infoga på specifik position
    shoppinglista.add(1, "Smör");  // Infogar som element nummer 2
    
    // Ta bort element
    shoppinglista.remove("Bröd");
    
    // Kontrollera om något finns
    if (shoppinglista.contains("Mjölk")) {
        System.out.println("Glöm inte mjölken!");
    }
    
    // Gå igenom listan
    for (String vara : shoppinglista) {
        System.out.println("- " + vara);
    }
    
    // Få storleken
    System.out.println("Antal varor: " + shoppinglista.size());
}
```

**Jämförelse Array vs ArrayList:**

|Egenskap|Array|ArrayList|
|---|---|---|
|Storlek|Fast vid skapande|Kan växa/krympa|
|Prestanda|Snabbare|Lite långsammare|
|Syntax|`int[] arr`|`ArrayList<Integer> list`|
|Primitiva typer|Stöds direkt|Måste använda wrapper-klasser|
|När använda|När storlek är känd|När storlek varierar|

### 🎯 **6. Vanliga arrayalgoritmer**

**Linjärsökning - När ordning inte spelar roll:**

java

```java
public static int linjarSokning(int[] arr, int sokVarde) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == sokVarde) {
            return i;  // Returnera index där värdet hittades
        }
    }
    return -1;  // Värdet hittades inte
}
```

**Binärsökning - När arrayen är sorterad:**

java

```java
public static int binarSokning(int[] arr, int sokVarde) {
    int vanster = 0;
    int hoger = arr.length - 1;
    
    while (vanster <= hoger) {
        int mitt = (vanster + hoger) / 2;
        
        if (arr[mitt] == sokVarde) {
            return mitt;  // Hittad!
        } else if (arr[mitt] < sokVarde) {
            vanster = mitt + 1;  // Sök i högra halvan
        } else {
            hoger = mitt - 1;  // Sök i vänstra halvan
        }
    }
    
    return -1;  // Inte hittad
}
```

### 💡 **7. Praktisk tillämpning - Frekvensanalys**

Låt oss bygga ett program som analyserar tärningskast:

java

```java
public static void analyseraTarningskast(int antalKast) {
    int[] frekvens = new int[7];  // Index 0 används inte, 1-6 för tärningssidor
    Random slump = new Random();
    
    // Kasta tärningen många gånger
    for (int i = 0; i < antalKast; i++) {
        int kast = slump.nextInt(6) + 1;  // 1-6
        frekvens[kast]++;
    }
    
    // Visa resultat som histogram
    System.out.println("Sida\tFrekvens\tHistogram");
    System.out.println("----\t--------\t---------");
    
    for (int sida = 1; sida <= 6; sida++) {
        System.out.printf("%d\t%d\t\t", sida, frekvens[sida]);
        
        // Rita stjärnor proportionellt mot frekvensen
        int stjarnor = frekvens[sida] * 50 / antalKast;
        for (int j = 0; j < stjarnor; j++) {
            System.out.print("*");
        }
        System.out.println();
    }
}
```

---

## 🎓 Sammanfattande tankar och kopplingar

Nu har vi gått igenom grundpelarna i Java-programmering. Låt mig visa hur allt hänger ihop:

**Kapitel 2** gav oss de atomära byggstenarna - variabler, operatorer och grundläggande I/O. Som att lära sig alfabetet innan man kan skriva meningar.

**Kapitel 3** introducerade kontrollstrukturer - if, while och de fundamentala sätten att styra programflöde. Detta är grammatiken som låter oss bygga meningsfulla program.

**Kapitel 4** utökade vår kontroll med for, switch och logiska operatorer. Nu kunde vi skriva mer eleganta och effektiva lösningar.

**Kapitel 5** lärde oss att organisera kod i återanvändbara metoder. Detta är nyckeln till att skriva underhållbar kod som kan växa utan att bli ohanterlig.

**Kapitel 6** gav oss kraften att hantera samlingar av data med arrayer och ArrayLists. Plötsligt kunde vi bearbeta hundratals eller tusentals datapunkter lika enkelt som en enda variabel.

Tillsammans bildar dessa kapitel en solid grund för all vidare Java-programmering. Med dessa verktyg kan du redan lösa många verkliga programmeringsproblem. Det kommande - objektorientering, arv, polymorfism - bygger vidare på denna grund, men kärnan finns redan här.



# 📚 OMFATTANDE SAMMANFATTNING: KAPITEL 2-6

## **KAPITEL 2 - INTRODUKTION TILL JAVA-APPLIKATIONER**

### 🎯 2.1 Introduktion - Vad är Java-programmering?

Detta kapitel introducerar dig till grunderna i Java-programmering. Du kommer att lära dig hur man skriver enkla program som visar meddelanden på skärmen, läser input från användaren, utför beräkningar och fattar beslut. Kapitlet lägger grunden för allt du kommer att lära dig senare i kursen.

### 📝 2.2 Ditt Första Java-Program - Hello World

**Att förstå ett enkelt program:**

java

```java
// Fig. 2.1: Welcome.java  
// Text-printing program.

public class Welcome {
    // main method begins execution of Java application
    public static void main(String[] args) {
        System.out.println("Welcome to Java Programming!");
    } // end method main
} // end class Welcome
```

Låt oss analysera varje del av detta program:

**Kommentarer (Lines 1-2):** Kommentarer är text som hjälper människor förstå koden, men som ignoreras av kompilatorn. End-of-line kommentarer börjar med `//` och fortsätter till slutet av raden. De används för att dokumentera koden och göra den lättare att förstå.

**Klassdeklaration (Line 4):** Varje Java-program består av minst en klass. Nyckelordet `public` betyder att klassen är tillgänglig för andra klasser. Nyckelordet `class` introducerar en klassdefinition. Klassnamnet `Welcome` är en identifierare som följer Javas namnkonventioner (börjar med stor bokstav, använder camelCase för flera ord).

**Metoddeklaration `main` (Line 6):** Detta är startpunkten för varje Java-applikation. När du kör programmet med kommandot `java Welcome`, startar JVM (Java Virtual Machine) och letar efter metoden `main` för att börja exekveringen. Signaturen måste alltid vara exakt: `public static void main(String[] args)`.

**Utskrift till skärmen (Line 7):** `System.out.println()` är en metod som skriver ut sitt argument till standardutmatningen (vanligtvis terminalen eller kommandofönstret) och lägger sedan till en ny rad.

**Kompilering och Körning:**

För att kompilera programmet:

bash

```bash
javac Welcome.java
```

Detta skapar en fil `Welcome.class` som innehåller bytecode (JVMs maskinkod).

För att köra programmet:

bash

```bash
java Welcome
```

JVM laddar `.class`-filen, verifierar bytecoden och kör programmet.

### 🔧 2.3 Modifiera Ditt Första Program

**Skriva ut på samma rad:**

java

```java
System.out.print("Welcome to ");
System.out.print("Java Programming!");
```

Outputen blir: `Welcome to Java Programming!` (på en rad)

**Escape-sekvenser:**

Escape-sekvenser börjar med backslash `\` och används för speciella tecken:

|Escape-sekvens|Beskrivning|Exempel|
|---|---|---|
|`\n`|Ny rad (newline)|`"Hello\nWorld"` → två rader|
|`\t`|Horisontell tab|`"Name:\tJohn"` → indenterad|
|`\r`|Carriage return|Återställer till radens början|
|`\\`|Backslash-tecken|`"C:\\Users"` → `C:\Users`|
|`\"`|Citattecken|`"He said \"Hi\""` → `He said "Hi"`|

java

````java
System.out.println("Welcome\nto\nJava\nProgramming!");
```
Output:
```
Welcome
to
Java
Programming!
````

### 🖨️ 2.4 Formaterad Utskrift med printf

Metoden `System.out.printf` ger dig mer kontroll över hur data visas.

**Grundläggande syntax:**

java

```java
System.out.printf("format string", argument1, argument2, ...);
```

**Format specifiers:**

|Specifier|Datatyp|Exempel|
|---|---|---|
|`%s`|String|`"Hello"`|
|`%d`|Decimal integer (int, byte, short, long)|`42`|
|`%f`|Floating-point (float, double)|`3.14159`|
|`%c`|Character (char)|`'A'`|
|`%b`|Boolean|`true`|
|`%n`|Plattformsoberoende ny rad|(ny rad)|

**Exempel:**

java

```java
System.out.printf("%s%n%s%n", "Welcome to", "Java Programming!");
```

Detta skriver ut "Welcome to" följt av en ny rad, sedan "Java Programming!" följt av en ny rad.

**Varför använd `%n` istället för `\n`?** `%n` är plattformsoberoende. På Windows använder systemet `\r\n` för nya rader, medan Unix/Linux använder `\n`. Med `%n` hanterar Java detta automatiskt.

### 💻 2.5 En Mer Komplex Applikation - Addition av Två Tal

Detta är ett längre exempel som demonstrerar många viktiga koncept:

java

```java
// Fig. 2.7: Addition.java
// Addition program that inputs two numbers then displays their sum.
import java.util.Scanner;

public class Addition {
    public static void main(String[] args) {
        // create a Scanner to obtain input from the command window
        Scanner input = new Scanner(System.in);
        
        System.out.print("Enter first integer: "); // prompt
        int number1 = input.nextInt(); // read first number from user
        
        System.out.print("Enter second integer: "); // prompt  
        int number2 = input.nextInt(); // read second number from user
        
        int sum = number1 + number2; // add numbers, then store total in sum
        
        System.out.printf("Sum is %d%n", sum); // display sum
    } // end method main
} // end class Addition
```

**Körningsexempel:**
```
Enter first integer: 45
Enter second integer: 72
Sum is 117
````

#### 2.5.1 Import-deklarationer

**Line 3: `import java.util.Scanner;`**

Java har tusentals färdiga klasser organiserade i paket. Ett paket är en samling relaterade klasser. För att använda en klass från ett annat paket måste du importera den.

`java.util` är paketet, och `Scanner` är klassen vi vill använda. Detta säger åt kompilatorn var den ska leta efter `Scanner`-klassen.

**Java API (Application Programming Interface):** Detta är en enorm samling av färdiga klasser som du kan använda. Exempel på paket:

- `java.lang` - grundläggande klasser (importeras automatiskt)
- `java.util` - verktygsklasser (Scanner, ArrayList, etc.)
- `java.io` - input/output för filer
- `java.math` - matematiska operationer med hög precision

#### 2.5.2 Variabler och Scanner-objektet

**Line 8: `Scanner input = new Scanner(System.in);`**

Detta är en komplex rad som gör flera saker:

1. **Variabeldeklaration**: `Scanner input` - Vi deklarerar en variabel som heter `input` av typen `Scanner`
2. **Objektskapande**: `new Scanner(System.in)` - Vi skapar ett nytt Scanner-objekt
3. **System.in**: Detta är standard input-strömmen (vanligtvis tangentbordet)
4. **Tilldelning**: `=` - Vi tilldelar det nya objektet till variabeln `input`

**Vad är en variabel?** En variabel är en namngiven plats i datorns minne där ett värde kan lagras. Tänk på det som en etikett på en låda där du kan stoppa in saker.

**Variabler måste ha:**

- Ett namn (identifierare)
- En typ (vilken sorts data det kan innehålla)
- Ett värde (efter initialisering)

#### 2.5.3 Promptande användare för input

**Lines 10-11:**

java

```java
System.out.print("Enter first integer: "); // prompt
int number1 = input.nextInt(); // read first number from user
```

**Line 10** visar en prompt - ett meddelande som talar om för användaren vad de ska göra. Vi använder `print` (inte `println`) så att cursorn stannar på samma rad.

**Line 11** gör två saker:

1. Deklarerar en variabel `number1` av typen `int`
2. Läser ett heltal från användaren med `input.nextInt()` och lagrar det i `number1`

**Datatypen `int`:**

- Står för "integer" (heltal)
- Värdeområde: -2,147,483,648 till +2,147,483,647
- Tar 4 bytes (32 bitar) i minnet

**Andra primitiva datatyper:**

|Typ|Storlek|Värdeområde|Användning|
|---|---|---|---|
|`byte`|1 byte|-128 till 127|Små heltal, spara minne|
|`short`|2 bytes|-32,768 till 32,767|Mindre heltal|
|`int`|4 bytes|-2.1 miljarder till 2.1 miljarder|Standard för heltal|
|`long`|8 bytes|-9.2×10¹⁸ till 9.2×10¹⁸|Mycket stora heltal|
|`float`|4 bytes|~7 decimala siffrors precision|Decimaltal|
|`double`|8 bytes|~15 decimala siffrors precision|Standard för decimaltal|
|`char`|2 bytes|Unicode-tecken (0 till 65,535)|Enskilda tecken|
|`boolean`|~1 bit|`true` eller `false`|Logiska värden|

#### 2.5.4 och 2.5.5 Läsa flera värden

**Lines 13-14:**

java

```java
System.out.print("Enter second integer: ");
int number2 = input.nextInt();
```

Detta följer samma mönster som för det första talet.

#### 2.5.6 Beräkningar med Variabler

**Line 17: `int sum = number1 + number2;`**

Detta är en **tilldelningssats** (assignment statement) som:

1. Deklarerar en ny variabel `sum` av typen `int`
2. Beräknar värdet av uttrycket `number1 + number2`
3. Lagrar resultatet i `sum`

**Uttryck (Expressions):** Ett uttryck är någon del av koden som har ett värde. Exempel:

- `5` - är ett uttryck (värde: 5)
- `number1 + number2` - är ett uttryck (värde: summan)
- `input.nextInt()` - är ett uttryck (värde: det användaren skriver in)

#### 2.5.7 Visa Resultatet

**Line 19: `System.out.printf("Sum is %d%n", sum);`**

`%d` är en placeholder för ett heltal (d står för "decimal integer"). När programmet körs ersätts `%d` med värdet av `sum`.

**Exempel med fler format specifiers:**

java

````java
String name = "Alice";
int age = 25;
double height = 1.68;

System.out.printf("Name: %s%nAge: %d%nHeight: %.2f meters%n", 
                  name, age, height);
```

Output:
```
Name: Alice
Age: 25
Height: 1.68 meters
````

**Fältbredd och justering:**

java

```java
int num = 42;
System.out.printf("%5d%n", num);   // "   42" (höger justerad, 5 tecken bred)
System.out.printf("%-5d%n", num);  // "42   " (vänster justerad, 5 tecken bred)

double value = 123.456789;
System.out.printf("%.2f%n", value);     // "123.46" (2 decimaler)
System.out.printf("%8.2f%n", value);    // "  123.46" (totalt 8 tecken, 2 decimaler)
System.out.printf("%,10.2f%n", 1234.5); // "  1,234.50" (komma-separator)
```

### 💾 2.6 Minneskonceptet - Hur Variabler Fungerar

När du deklarerar och använder variabler är det viktigt att förstå vad som händer i datorns minne.

**Exempel:**

java

```java
int number1 = 45;
```

Detta gör följande:
1. Reserverar 4 bytes minne för en `int`
2. Etiketterar denna plats som "number1"
3. Lagrar värdet 45 där

**Visualisering av minnet:**
```
┌────────────┬──────┐
│ number1    │  45  │
└────────────┴──────┘
Adress: 0x1000 (exempel)
````

**Värdet ersätts (är destruktivt):**

java

````java
int number1 = 45;  // number1 innehåller 45
number1 = 72;      // 45 är borta, number1 innehåller nu 72
```

Efter andra tilldelningen:
```
┌────────────┬──────┐
│ number1    │  72  │  (45 är förstört och kan inte återställas)
└────────────┴──────┘
````

**Kopiera värden:**

java

````java
int number1 = 45;
int number2 = number1;  // Kopierar 45 till number2
number1 = 100;          // Ändrar number1, number2 påverkas INTE
```

Resultat:
```
┌────────────┬──────┐
│ number1    │ 100  │
└────────────┴──────┘

┌────────────┬──────┐
│ number2    │  45  │  (fortfarande 45!)
└────────────┴──────┘
````

### 🧮 2.7 Aritmetiska Operatorer - Komplett Guide

#### Grundläggande Operatorer

**Addition (+):**

java

```java
int sum = 5 + 3;  // sum = 8
```

**Subtraktion (-):**

java

```java
int difference = 10 - 4;  // difference = 6
```

**Multiplikation (*):**

java

```java
int product = 6 * 7;  // product = 42
```

**Division (/):**

java

```java
int quotient = 20 / 3;      // quotient = 6 (HELTALSDIVISION!)
double quotient2 = 20.0 / 3;  // quotient2 = 6.666...
```

**⚠️ Kritiskt att förstå - Heltalsdivision:** När båda operanderna är heltal, blir resultatet ett heltal (decimaldelen kastas bort, inte avrundas!):

java

```java
int result1 = 7 / 2;       // = 3 (inte 3.5!)
int result2 = 5 / 2;       // = 2 (inte 2.5!)
int result3 = 1 / 2;       // = 0 (inte 0.5!)

double result4 = 7.0 / 2;   // = 3.5 (minst en operand är double)
double result5 = 7 / 2.0;   // = 3.5
double result6 = (double)7 / 2;  // = 3.5 (typkonvertering)
```

**Remainder/Modulo (%):** Ger resten efter division:

java

```java
int remainder1 = 17 % 5;   // = 2 (17 = 3*5 + 2)
int remainder2 = 10 % 3;   // = 1 (10 = 3*3 + 1)
int remainder3 = 15 % 5;   // = 0 (jämnt delbart)
```

**Praktiska användningar av modulo:**

- Kontrollera om tal är jämnt/udda: `number % 2 == 0` → jämnt
- Hitta sista siffran: `number % 10`
- Implementera cykliska strukturer (t.ex. klockor: `(hour + 5) % 24`)

#### Operatorprioritet (Precedence)

Java följer matematiska prioritetsregler:

**Prioritetsordning (högst till lägst):**

1. **Parenteser** `()`
2. **Multiplikation, Division, Modulo** `*`, `/`, `%` (samma prioritet, vänster-till-höger)
3. **Addition, Subtraktion** `+`, `-` (samma prioritet, vänster-till-höger)

**Exempel på beräkningsordning:**

java

```java
int result = 2 + 3 * 4;
// Steg 1: 3 * 4 = 12 (multiplikation först)
// Steg 2: 2 + 12 = 14
// Resultat: 14
```

**Med parenteser:**

java

```java
int result = (2 + 3) * 4;
// Steg 1: (2 + 3) = 5 (parenteser först)
// Steg 2: 5 * 4 = 20
// Resultat: 20
```

**Komplext exempel - Polynom y = ax² + bx + c:**

java

```java
int a = 2, b = 3, c = 7, x = 5;
int y = a * x * x + b * x + c;

// Steg-för-steg utvärdering:
// 1. a * x = 2 * 5 = 10
// 2. 10 * x = 10 * 5 = 50
// 3. b * x = 3 * 5 = 15
// 4. 50 + 15 = 65
// 5. 65 + 7 = 72
// Resultat: y = 72
```

**Förståelse av associativitet (vänster-till-höger):**

java

```java
int result = 20 - 10 - 5;
// Steg 1: 20 - 10 = 10 (vänstra operationen först)
// Steg 2: 10 - 5 = 5
// Resultat: 5 (INTE 20 - (10 - 5) = 15)
```

#### Praktiska Tips för Aritmetik

**Använd parenteser för tydlighet:**

java

```java
// Fungerar men kan vara otydligt:
int result = a * x * x + b * x + c;

// Tydligare med onödiga parenteser:
int result = (a * x * x) + (b * x) + c;
```

**Försiktig med heltalsdivision i beräkningar:**

java

```java
// Beräkna medelvärde:
int total = 100;
int count = 3;

int avg1 = total / count;        // = 33 (heltalsdivision)
double avg2 = (double)total / count;  // = 33.333... (korrekt)
```

**Compound assignment operatorer (förhandsvisning):** Dessa behandlas mer i kapitel 3, men här är en snabb intro:

java

```java
int x = 10;
x = x + 5;  // x blir 15
x += 5;     // Kortare sätt att skriva samma sak, x blir 20
```

### 🔍 2.8 Besluts-tagande - if-satser och Jämförelseoperatorer

#### Relationella och Jämförelseoperatorer

Java har sex operatorer för att jämföra värden:

|Operator|Algebraisk notation|Betydelse|Exempel|Resultat|
|---|---|---|---|---|
|`==`|=|Lika med|`5 == 5`|`true`|
|`!=`|≠|Inte lika med|`5 != 3`|`true`|
|`>`|>|Större än|`5 > 3`|`true`|
|`<`|<|Mindre än|`5 < 3`|`false`|
|`>=`|≥|Större än eller lika med|`5 >= 5`|`true`|
|`<=`|≤|Mindre än eller lika med|`5 <= 3`|`false`|

**⚠️ Vanligt misstag:**

java

```java
// FEL - tilldelning, inte jämförelse!
if (x = 5) { ... }  // Kompileringsfel

// RÄTT - jämförelse
if (x == 5) { ... }  // Jämför x med 5
```

#### if-satsen - Grundläggande Syntax

java

```java
if (condition) {
    // Kod som körs om condition är true
}
```

**Exempel:**

java

```java
int grade = 75;

if (grade >= 60) {
    System.out.println("Passed!");
}
```

**Om condition är `true`**: Koden i krullparenteserna körs **Om condition är `false`**: Koden hoppas över

#### Komplett Exempel - Jämföra Två Tal

java

```java
import java.util.Scanner;

public class Comparison {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        
        System.out.print("Enter first integer: ");
        int number1 = input.nextInt();
        
        System.out.print("Enter second integer: ");
        int number2 = input.nextInt();
        
        if (number1 == number2) {
            System.out.printf("%d == %d%n", number1, number2);
        }
        
        if (number1 != number2) {
            System.out.printf("%d != %d%n", number1, number2);
        }
        
        if (number1 < number2) {
            System.out.printf("%d < %d%n", number1, number2);
        }
        
        if (number1 > number2) {
            System.out.printf("%d > %d%n", number1, number2);
        }
        
        if (number1 <= number2) {
            System.out.printf("%d <= %d%n", number1, number2);
        }
        
        if (number1 >= number2) {
            System.out.printf("%d >= %d%n", number1, number2);
        }
    }
}
```

**Körningsexempel 1:**
```
Enter first integer: 777
Enter second integer: 777
777 == 777
777 <= 777
777 >= 777
```

**Körningsexempel 2:**
```
Enter first integer: 1000
Enter second integer: 2000
1000 != 2000
1000 < 2000
1000 <= 2000
````

#### Viktiga Detaljer om if-satser

**Krullparenteser:**

java

```java
// En sats - krullparenteser är valfria men rekommenderas
if (x > 5)
    System.out.println("x is greater than 5");

// Flera satser - krullparenteser KRÄVS
if (x > 5) {
    System.out.println("x is greater than 5");
    x = 0;
}
```

**Indentering:** Indentering påverkar inte hur koden körs, men gör den lättare att läsa:

java

```java
// Svår att läsa:
if(x>5){System.out.println("Greater");y=x*2;}

// Lättare att läsa:
if (x > 5) {
    System.out.println("Greater");
    y = x * 2;
}
```

### 📋 2.9 Sammanfattning av Kapitel 2

**Vad du har lärt dig:**

1. **Grundläggande programstruktur**: Klasser, main-metoden, kommentarer
2. **Output**: `println`, `print`, `printf` med format specifiers
3. **Input**: Scanner-klassen för att läsa från tangentbordet
4. **Variabler**: Deklaration, initialisering, tilldelning
5. **Datatyper**: Primitiva typer (int, double, char, boolean, etc.)
6. **Aritmetik**: +, -, *, /, % och prioritetsregler
7. **Beslut**: if-satser och jämförelseoperatorer
8. **Minnesmodell**: Hur variabler lagras och ändras

**Nyckellärdom:**

- Java är case-sensitive
- Varje sats slutar med semikolon
- Heltalsdivision truncar, inte avrundar
- Använd `==` för jämförelse, inte `=`
- Alla variabler måste deklareras med typ
- Import-satser behövs för klasser från andra paket

---
## **KAPITEL 3 - KONTROLLSTRUKTURER**

### 📚 3.1 Introduktion till Kontrollstrukturer

I kapitel 2 lärde du dig grunderna i Java-programmering med sekventiell exekvering där varje sats körs en gång i ordning. Nu ska vi lära oss kontrollstrukturer som tillåter programmet att fatta beslut och upprepa instruktioner. Detta är vad som gör program riktigt kraftfulla eftersom de kan anpassa sitt beteende och hantera upprepade uppgifter utan att du behöver skriva samma kod om och om igen.

### 🎯 3.2 Algoritmer - Receptet för Problemlösning

En algoritm är en serie steg som beskriver hur man löser ett problem, precis som ett recept i en kokbok. Innan du skriver kod bör du planera din algoritm. Till exempel, en algoritm för att bestämma det största av tre tal skulle vara:

1. Anta att det första talet är störst
2. Om det andra talet är större än ditt antagna största tal, uppdatera det största talet
3. Om det tredje talet är större än ditt antagna största tal, uppdatera det största talet
4. Nu har du det största talet

### 📝 3.3 Pseudokod - Skriva Algoritmer

Pseudokod är ett sätt att beskriva algoritmer med vanligt språk blandat med programmeringsliknande struktur. Det är inte riktig kod men tillräckligt strukturerat för att lätt kunna översättas till kod. Exempel:

```
Prompt användaren för två tal
Input första talet
Input andra talet
Beräkna summan
Display summan
```

Pseudokod hjälper dig att tänka igenom logiken innan du fastnar i syntaxdetaljer.

### 🔀 3.4 Kontrollstrukturer - De Tre Byggstenarna

Java har tre typer av kontrollstrukturer som kan kombineras för att skapa alla program du behöver:

**Sekvens:** Satser körs en efter en i ordning (detta har du redan sett)

**Selektion:** Programmet väljer mellan olika vägar baserat på villkor. Java har tre typer:

- `if` - enkelselektion
- `if...else` - dubbelselektion
- `switch` - multiselektion

**Iteration (loopar):** Satser upprepas så länge ett villkor är sant. Java har fyra typer:

- `while` - testa villkor först
- `do...while` - testa villkor sist
- `for` - räknarbaserad upprepning
- enhanced `for` - för att iterera genom samlingar

Alla dessa strukturer har en ingång och en utgång, vilket gör dem lätta att kombinera och resonera om.

### 🔍 3.5 if Enkelselektionssats

Vi såg en introduktion till `if` i kapitel 2. Låt oss fördjupa oss:

java

```java
if (studentGrade >= 60) {
    System.out.println("Passed");
}
```

**Hur det fungerar:**

1. Java utvärderar villkoret i parenteserna (`studentGrade >= 60`)
2. Om resultatet är `true`, körs koden i krullparenteserna
3. Om resultatet är `false`, hoppas koden över och programmet fortsätter efter if-satsen

**UML Aktivitetsdiagram:** I UML visualiseras detta som en diamant (beslutspunkt) med två vägar ut. Detta hjälper dig att se programmets flöde visuellt.

### 🔄 3.6 if...else Dubbelselektionssats

Ofta vill du göra olika saker beroende på om ett villkor är sant eller falskt:

java

```java
if (grade >= 60) {
    System.out.println("Passed");
}
else {
    System.out.println("Failed");
}
```

**Viktigt att förstå:** Exakt en av dessa två block kommer att köra, aldrig båda.

#### 3.6.1 Nästlade if...else Satser

Du kan ha if...else inuti andra if...else för att hantera flera villkor:

java

```java
if (grade >= 90) {
    System.out.println("A");
}
else if (grade >= 80) {
    System.out.println("B");
}
else if (grade >= 70) {
    System.out.println("C");
}
else if (grade >= 60) {
    System.out.println("D");
}
else {
    System.out.println("F");
}
```

Notera hur Java testar varje villkor i ordning tills ett är sant. När ett villkor är sant körs den koden och resten hoppas över. Detta är effektivt eftersom det undviker onödiga tester.

#### 3.6.2 Det Hängande else-problemet

När du har nästlade if-satser utan krullparenteser kan det bli tvetydigt vilket if ett else hör till:

java

```java
// Otydlig kod:
if (x > 5)
    if (y > 5)
        System.out.println("x and y are > 5");
else
    System.out.println("x is <= 5");  // FEL ANTAGANDE!
```

Java kopplar else till närmaste if, så else ovan hör till `if (y > 5)`, inte `if (x > 5)`. För att undvika förvirring, använd alltid krullparenteser:

java

```java
// Tydlig kod:
if (x > 5) {
    if (y > 5) {
        System.out.println("x and y are > 5");
    }
}
else {
    System.out.println("x is <= 5");
}
```

#### 3.6.4 Den Ternära Operatorn (?:)

För enkla if...else kan du använda den ternära operatorn för mer kompakt kod:

java

```java
// Traditionell if...else:
int result;
if (grade >= 60) {
    result = 1;
}
else {
    result = 0;
}

// Ternär operator:
int result = (grade >= 60) ? 1 : 0;
```

Syntaxen är: `(villkor) ? värde_om_sant : värde_om_falskt`

Ett annat exempel:

java

```java
System.out.println(studentGrade >= 60 ? "Passed" : "Failed");
```

Använd detta när det gör koden tydligare, men undvik att nästla ternära operatorer eftersom det blir svårläst.

### 🔁 3.7 while Iterationssats - Din Första Loop

En loop upprepar kod så länge ett villkor är sant:

java

```java
int counter = 1;

while (counter <= 10) {
    System.out.printf("%d ", counter);
    counter++;
}
```

**Hur while fungerar:**
1. Utvärdera villkoret (`counter <= 10`)
2. Om `true`, kör loopens kropp
3. Gå tillbaka till steg 1
4. Om `false`, fortsätt efter loopen

Output: `1 2 3 4 5 6 7 8 9 10`

**Viktiga delar av en loop:**
- **Initialisering:** `int counter = 1;` - sätt startvärde
- **Villkor:** `counter <= 10` - när ska loopen fortsätta?
- **Uppdatering:** `counter++` - ändra kontrollvariabeln så loopen kan sluta

Om du glömmer uppdateringen får du en oändlig loop som aldrig slutar!

### 📊 3.8 Formulera Algoritmer - Räknarstyrd Iteration

Låt oss se hur man använder loopar för att lösa verkliga problem. Vi ska beräkna klassgenomsnitt för tio studenter.

**Pseudokod:**
```
Sätt total till noll
Sätt räknare till noll

Medan räknare är mindre än tio
    Prompt användaren för nästa betyg
    Input betyget
    Lägg till betyget till total
    Öka räknaren med ett

Sätt genomsnittet till totalen dividerat med tio
Skriv ut genomsnittet
````

**Java-implementation:**

java

```java
import java.util.Scanner;

public class ClassAverage {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        
        int total = 0;
        int gradeCounter = 1;
        
        while (gradeCounter <= 10) {
            System.out.print("Enter grade: ");
            int grade = input.nextInt();
            total = total + grade;
            gradeCounter = gradeCounter + 1;
        }
        
        int average = total / 10;
        System.out.printf("Class average is %d%n", average);
    }
}
```

**Vad händer steg för steg:** Första iterationen: gradeCounter är 1, användaren anger kanske 85, total blir 85, gradeCounter blir 2. Andra iterationen: gradeCounter är 2, användaren anger kanske 90, total blir 175, gradeCounter blir 3. Detta fortsätter tills gradeCounter blir 11, då är villkoret `gradeCounter <= 10` falskt och loopen slutar.

### 🛡️ 3.9 Sentinel-styrd Iteration - Flexibel Looping

Vad om du inte vet i förväg hur många gånger loopen ska köra? Du kan använda ett sentinel-värde, ett speciellt värde som signalerar slutet på input.

**Exempel:** Beräkna genomsnitt för okänt antal betyg, där -1 signalerar slutet:

java

```java
import java.util.Scanner;

public class ClassAverageSentinel {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        
        int total = 0;
        int gradeCounter = 0;
        
        System.out.print("Enter grade or -1 to quit: ");
        int grade = input.nextInt();
        
        while (grade != -1) {
            total = total + grade;
            gradeCounter = gradeCounter + 1;
            
            System.out.print("Enter grade or -1 to quit: ");
            grade = input.nextInt();
        }
        
        if (gradeCounter != 0) {
            double average = (double) total / gradeCounter;
            System.out.printf("Class average is %.2f%n", average);
        }
        else {
            System.out.println("No grades were entered");
        }
    }
}
```

**Viktiga punkter:**

- Vi läser första värdet FÖRE loopen
- Loopen fortsätter så länge värdet INTE är sentinel (-1)
- Vi läser nästa värde i SLUTET av loopen
- Vi kontrollerar division med noll genom att testa om `gradeCounter != 0`
- Vi använder `double` för genomsnittet så vi får decimalresultat

**Varför casta till double?** `(double) total / gradeCounter` konverterar total till double före divisionen, vilket ger flyttalsdivision istället för heltalsdivision.

### 🎯 3.10 Nästlade Kontrollstrukturer - Mer Komplex Logik

Du kan placera en kontrollstruktur inuti en annan. Här är ett exempel som analyserar tentamensresultat:

java

```java
int passes = 0;
int failures = 0;
int studentCounter = 1;

while (studentCounter <= 10) {
    System.out.print("Enter result (1 = pass, 2 = fail): ");
    int result = input.nextInt();
    
    if (result == 1) {
        passes = passes + 1;
    }
    else {
        failures = failures + 1;
    }
    
    studentCounter = studentCounter + 1;
}

System.out.printf("Passed: %d%nFailed: %d%n", passes, failures);

if (passes > 8) {
    System.out.println("Bonus to instructor!");
}
```

Här har vi en if...else inuti en while-loop. Detta är kraftfullt eftersom vi både itererar genom studenter OCH fattar beslut för varje student.

### ➕ 3.11 Sammansatta Tilldelningsoperatorer - Skriv Mindre, Gör Samma

Java erbjuder kortare syntax för vanliga operationer:

java

```java
// Långt sätt:
counter = counter + 1;
total = total + value;
price = price * 1.05;

// Kort sätt med sammansatta operatorer:
counter += 1;  // samma som counter = counter + 1
total += value;  // samma som total = total + value
price *= 1.05;  // samma som price = price * 1.05
```

**Alla sammansatta operatorer:**

|Operator|Exempel|Motsvarar|
|---|---|---|
|`+=`|`x += 5`|`x = x + 5`|
|`-=`|`x -= 3`|`x = x - 3`|
|`*=`|`x *= 2`|`x = x * 2`|
|`/=`|`x /= 4`|`x = x / 4`|
|`%=`|`x %= 3`|`x = x % 3`|

Detta gör koden mer koncis och ibland tydligare när avsikten är att modifiera en variabel baserat på sitt eget värde.

### 🔢 3.12 Inkrement och Dekrement Operatorer

För att öka eller minska ett värde med ett, har Java särskilda operatorer:

**Inkrement (++):**

java

```java
int counter = 5;
counter++;  // counter blir 6
++counter;  // counter blir 7
```

**Dekrement (--):**

java

```java
int counter = 5;
counter--;  // counter blir 4
--counter;  // counter blir 3
```

**Prefix vs Postfix - När gör skillnaden?**

När operatorn används i ett större uttryck spelar placeringen roll:

java

```java
int c = 5;
int d;

// Postfix (c++): använd värdet, sedan öka
d = c++;  // d blir 5, sedan ökar c till 6
System.out.println("d: " + d);  // Output: d: 5
System.out.println("c: " + c);  // Output: c: 6

// Prefix (++c): öka först, sedan använd värdet
c = 5;  // återställ
d = ++c;  // c ökar till 6, sedan tilldelas d värdet 6
System.out.println("d: " + d);  // Output: d: 6
System.out.println("c: " + c);  // Output: c: 6
```

**Minnesregel:**

- **Prefix (++x):** "Öka först, använd sedan"
- **Postfix (x++):** "Använd först, öka sedan"

I vanliga situationer som loopar spelar det ingen roll:

java

```java
// Dessa är identiska i en loop:
for (int i = 0; i < 10; i++) { ... }
for (int i = 0; i < 10; ++i) { ... }
```

De flesta Java-programmerare föredrar postfix eftersom det ser mer naturligt ut, men prefix kan vara marginellt snabbare i vissa situationer.

### 📦 3.13 Primitiva Typer - En Komplett Översikt

Vi har nämnt primitiva typer tidigare, men här är en komplett sammanfattning:

**Heltalstyper:**

|Typ|Storlek|Min värde|Max värde|Användning|
|---|---|---|---|---|
|`byte`|8 bits|-128|127|Spara minne med små tal|
|`short`|16 bits|-32,768|32,767|Mindre heltal|
|`int`|32 bits|-2,147,483,648|2,147,483,647|Standard för heltal|
|`long`|64 bits|-9,223,372,036,854,775,808|9,223,372,036,854,775,807|Mycket stora tal|

**Flyttalstyper:**

|Typ|Storlek|Precision|Användning|
|---|---|---|---|
|`float`|32 bits|~7 signifikanta siffror|Mindre decimaltal|
|`double`|64 bits|~15 signifikanta siffror|Standard för decimaltal|

**Andra typer:**

|Typ|Storlek|Värden|Användning|
|---|---|---|---|
|`char`|16 bits|Unicode-tecken '\u0000' till '\uFFFF'|Enskilda tecken|
|`boolean`|Ej specificerad|`true` eller `false`|Logiska värden|

**Exempel på literaler:**

java

```java
int regularInt = 1000000;
int readableInt = 1_000_000;  // Understreck för läsbarhet (Java 7+)

long bigNumber = 1234567890123L;  // L suffix för long

float price = 19.99F;  // F suffix för float
double precise = 3.14159;  // Ingen suffix behövs för double

char letter = 'A';
char unicode = '\u0041';  // Också 'A'

boolean isValid = true;
```

**Val av datatyp:** Använd `int` för heltal och `double` för decimaltal om du inte har specifika skäl att välja annat. Detta är Javas standarder och vad de flesta programmerare förväntar sig.

### 📚 3.14 Sammanfattning av Kapitel 3

I detta kapitel lärde du dig grunderna för kontrollflöde i Java. Du kan nu skriva program som fattar beslut med `if` och `if...else`, upprepar operationer med `while`-loopar, och använder räknare och sentinel-värden för att kontrollera iteration. Du lärde dig också om sammansatta operatorer och inkrement/dekrement operatorer som gör din kod mer koncis.

**Nyckelpunkter att komma ihåg:**

- Kontrollstrukturer låter program fatta beslut och upprepa kod
- Använd alltid krullparenteser för tydlighet, även när de är valfria
- Var försiktig med oändliga loopar, se till att loopvillkoret blir falskt någon gång
- Testa för division med noll innan du delar
- Sammansatta operatorer gör koden kortare men inte nödvändigtvis tydligare
- Prefix och postfix ++ gör samma sak i isolering, men skiljer sig i uttryck

---
## **KAPITEL 4 - KONTROLLSTRUKTURER**

### 📚 4.1 Introduktion - Komplettera Verktygslådan

I kapitel 3 lärde du dig grunderna för kontrollflöde med `if`, `if...else` och `while`. Nu ska vi komplettera din verktygslåda med resten av Javas kontrollstrukturer. Du kommer att lära dig `for`-loopen som är perfekt för räkningsuppgifter, `do...while` som garanterar minst en iteration, `switch` för att välja bland många alternativ, och de logiska operatorerna som låter dig kombinera flera villkor. Vi avslutar med att se hur alla dessa strukturer passar ihop i det stora sammanhanget av strukturerad programmering.

### 🔢 4.2 Essentialer för Räknarstyrd Iteration

Innan vi dyker in i `for`-loopen, låt oss formalisera vad som krävs för räknarstyrd iteration. Varje räknarstyrd loop behöver fyra element:

**De fyra essentiella delarna:**

1. **Kontrollvariabel** - en variabel som räknar iterationerna (ofta kallad `counter`, `i`, eller `index`)
2. **Initialt värde** - var räknaren startar (ofta noll eller ett)
3. **Inkrement/dekrement** - hur mycket räknaren ändras varje iteration (ofta plus eller minus ett)
4. **Loop-fortsättningsvillkor** - när loopen ska fortsätta köra (ofta `counter < limit`)

Här är ett exempel med `while` som visar alla fyra delarna tydligt:

java

```java
int counter = 1;                    // 1. Kontrollvariabel med 2. initialt värde

while (counter <= 10) {              // 4. Loop-fortsättningsvillkor
    System.out.printf("%d ", counter);
    counter++;                       // 3. Inkrement
}
```

Det som gör `for`-loopen så praktisk är att den samlar alla fyra dessa delar på ett ställe, vilket gör koden mer kompakt och lättare att förstå.

### 🔄 4.3 for Iterationssats - Räknarloopens Mästare

`for`-loopen är Javas mest populära loop för situationer där du vet i förväg hur många gånger du vill upprepa något. Den är särskilt elegant eftersom alla loopens kontrolldelar finns i headern.

**Grundläggande syntax:**

java

```java
for (initialisering; villkor; inkrement) {
    // Kod som upprepas
}
```

Samma exempel som ovan, men med `for`:

java

```java
for (int counter = 1; counter <= 10; counter++) {
    System.out.printf("%d ", counter);
}
```

Output: `1 2 3 4 5 6 7 8 9 10`

**Vad händer steg för steg:**

1. **Initialisering** (`int counter = 1`) körs EN gång när loopen börjar. Här deklareras och initialiseras kontrollvariabeln.
2. **Villkoret** (`counter <= 10`) testas. Om `true`, fortsätt till steg 3. Om `false`, hoppa över loopen helt.
3. **Loopens kropp** körs (utskriften).
4. **Inkrement** (`counter++`) körs efter loopens kropp.
5. Gå tillbaka till steg 2 och testa villkoret igen.

**Viktigt att förstå om scope:** När du deklarerar kontrollvariabeln i `for`-headern (`int counter = 1`), existerar den variabeln BARA inuti loopen. Efter loopen är variabeln borta:

java

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);  // i existerar här
}
System.out.println(i);  // KOMPILERINGSFEL! i existerar inte här
```

Om du behöver använda kontrollvariabeln efter loopen, deklarera den före:

java

```java
int i;
for (i = 0; i < 5; i++) {
    System.out.println(i);
}
System.out.println(i);  // Fungerar, i har värdet 5
```

**Konvertera mellan while och for:**

Nästan alla `for`-loopar kan skrivas om som `while`:

java

```java
// for-version:
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}

// Motsvarande while-version:
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}
```

Men `for` är ofta tydligare när du vet exakt hur många iterationer du vill ha.

### 🎨 4.4 Exempel med for-satsen

Låt oss se några praktiska variationer av `for`-loopen som visar dess flexibilitet.

**Räkna uppåt:**

java

```java
for (int i = 1; i <= 100; i++) {
    // Räknar från 1 till 100
}
```

**Räkna nedåt:**

java

```java
for (int i = 100; i >= 1; i--) {
    // Räknar från 100 till 1
}
```

Observera att villkoret ändras till `>=` när vi räknar nedåt. Ett vanligt misstag är att skriva `i <= 1` vilket ger en oändlig loop!

**Räkna med olika steg:**

java

```java
for (int i = 7; i <= 77; i += 7) {
    // Räknar 7, 14, 21, 28, ... 77
}

for (int i = 20; i >= 2; i -= 2) {
    // Räknar 20, 18, 16, ... 4, 2
}
```

**Varierande värden:**

java

```java
for (int i = 2; i <= 20; i += 3) {
    // Output: 2, 5, 8, 11, 14, 17, 20
}
```

### 4.4.1 Summera Jämna Tal

Här är ett komplett exempel som använder en `for`-loop för att summera de jämna talen från två till tjugo:

java

```java
public class Sum {
    public static void main(String[] args) {
        int total = 0;
        
        for (int number = 2; number <= 20; number += 2) {
            total += number;
        }
        
        System.out.printf("Sum is %d%n", total);
    }
}
```

Output: `Sum is 110`

Observera hur vi kan använda en sammansatt operator (`+=`) både för att öka kontrollvariabeln (`number += 2`) och för att ackumulera summan (`total += number`). Detta gör koden koncis och tydlig.

### 4.4.2 Ränta på Ränta Beräkningar

Ett mer avancerat exempel som visar hur pengar växer med sammansatt ränta:

java

```java
public class Interest {
    public static void main(String[] args) {
        double principal = 1000.0;  // Startkapital
        double rate = 0.05;         // 5% ränta
        
        System.out.printf("%s%20s%n", "Year", "Amount on deposit");
        
        for (int year = 1; year <= 10; year++) {
            double amount = principal * Math.pow(1.0 + rate, year);
            System.out.printf("%4d%,20.2f%n", year, amount);
        }
    }
}
```

**Förklaring av nya koncept:**

`Math.pow(base, exponent)` beräknar basen upphöjt till exponenten. Formeln för sammansatt ränta är: `amount = principal × (1 + rate)^year`

Format specifier `%,20.2f` betyder:
- `%f` - floating-point tal
- `20` - totalt 20 tecken bred kolumn (höger justerad)
- `.2` - två decimaler
- `,` - använd tusentalsavgränsare (komma i vissa länder, mellanslag i andra)

Output:
```
Year   Amount on deposit
   1            1,050.00
   2            1,102.50
   3            1,157.63
   4            1,215.51
   5            1,276.28
   6            1,340.10
   7            1,407.10
   8            1,477.46
   9            1,551.33
  10            1,628.89
````

**Varning om flyttal i beräkningar:** Eftersom datorer representerar decimaltal approximativt kan små avrundningsfel uppstå. För penningberäkningar i produktionssystem bör du använda `BigDecimal`-klassen istället för `double`, men för lärandesyften är `double` tillräckligt bra.

### 🔁 4.5 do...while Iterationssats

Ibland vill du garantera att loopens kropp körs minst en gång, oavsett vad villkoret är. Det är då `do...while` är perfekt.

**Syntax:**

java

```java
do {
    // Kod som upprepas
} while (villkor);
```

Notera att semikolon KRÄVS efter villkoret i `do...while`, till skillnad från `while` och `for`.

**Exempel:**

java

```java
int counter = 1;

do {
    System.out.printf("%d ", counter);
    counter++;
} while (counter <= 10);
```

Output: `1 2 3 4 5 6 7 8 9 10`

**Skillnaden mellan while och do...while:**

java

```java
// while - kroppen kanske aldrig körs:
int x = 11;
while (x <= 10) {
    System.out.println(x);  // Körs ALDRIG
    x++;
}

// do...while - kroppen körs minst en gång:
int y = 11;
do {
    System.out.println(y);  // Skriver ut 11 en gång
    y++;
} while (y <= 10);
```

**När använda do...while:** Använd det när du vet att du behöver köra koden minst en gång. Klassiska exempel är menysystem där du vill visa menyn minst en gång, eller validering av input där du ber om input tills användaren ger giltiga data:

java

```java
Scanner input = new Scanner(System.in);
int number;

do {
    System.out.print("Enter a positive number: ");
    number = input.nextInt();
} while (number <= 0);

System.out.println("You entered: " + number);
```

Detta garanterar att användaren blir tillfrågad minst en gång och fortsätter tills ett positivt tal anges.

### 🔀 4.6 switch Multiselektionssats

När du har många möjliga värden att välja bland blir nästlade `if...else`-satser snabbt röriga. `switch`-satsen erbjuder en tydligare lösning.

**Grundläggande syntax:**

java

```java
switch (uttryck) {
    case värde1:
        // Kod för värde1
        break;
    case värde2:
        // Kod för värde2
        break;
    default:
        // Kod om inget värde matchar
        break;
}
```

**Viktigt om switch:** Uttrycket måste vara av typen `byte`, `short`, `int`, `char`, `String` eller en `enum` (inte `long`, `float` eller `double`).

**Komplett exempel - Bokstavsbetyg:**

java

```java
Scanner input = new Scanner(System.in);
int grade;

System.out.print("Enter grade (0-100): ");
grade = input.nextInt();

int letterGrade = grade / 10;

switch (letterGrade) {
    case 10:
    case 9:
        System.out.println("A");
        break;
    case 8:
        System.out.println("B");
        break;
    case 7:
        System.out.println("C");
        break;
    case 6:
        System.out.println("D");
        break;
    default:
        System.out.println("F");
        break;
}
```

**Förklaring av fall-through:** Observera att `case 10:` och `case 9:` inte har någon kod mellan sig. Detta kallas "fall-through" och betyder att båda fallen kör samma kod (utskriften av "A"). När Java når `case 10:` och inte hittar en `break`, fortsätter den till nästa `case` och kör den koden.

**Varför break är viktigt:**

java

```java
// UTAN break - fel beteende:
switch (day) {
    case 1:
        System.out.println("Monday");
    case 2:
        System.out.println("Tuesday");
    case 3:
        System.out.println("Wednesday");
}

// Om day är 1, skriver programmet ut:
// Monday
// Tuesday
// Wednesday
// (alla fall efter matchningen körs!)
```

java

```java
// MED break - korrekt beteende:
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
}

// Om day är 1, skriver programmet bara ut:
// Monday
```

**Default-fallet:** `default` är valfritt men rekommenderat. Det fångar alla värden som inte matchar något annat fall, precis som ett sista `else` i en if...else-kedja.

### 🚪 4.7 break och continue Satser

Dessa satser ger dig mer kontroll över loopflödet, men de kan göra koden svårare att förstå om de används för mycket.

### 4.7.1 break-satsen

`break` får en loop eller `switch` att sluta omedelbart. Exekveringen fortsätter efter loopen.

java

````java
for (int count = 1; count <= 10; count++) {
    if (count == 5) {
        break;  // Avsluta loopen när count är 5
    }
    System.out.printf("%d ", count);
}
System.out.println("\nBroke out of loop at count = 5");
```

Output:
```
1 2 3 4
Broke out of loop at count = 5
````

**Praktisk användning:**

java

```java
// Hitta första primtalet i en lista:
for (int num : numbers) {
    if (isPrime(num)) {
        System.out.println("First prime: " + num);
        break;  // Vi hittade det, ingen anledning att fortsätta
    }
}
```

### 4.7.2 continue-satsen

`continue` hoppar över resten av loopens kropp för den aktuella iterationen och går vidare till nästa iteration.

java

```java
for (int count = 1; count <= 10; count++) {
    if (count == 5) {
        continue;  // Hoppa över utskriften när count är 5
    }
    System.out.printf("%d ", count);
}
```

Output: `1 2 3 4 6 7 8 9 10` (observera att 5 saknas)

**Praktisk användning:**

java

```java
// Bearbeta bara positiva tal:
for (int num : numbers) {
    if (num <= 0) {
        continue;  // Hoppa över negativa och noll
    }
    // Bearbeta bara positiva tal här
    processPositiveNumber(num);
}
```

**Varning om overanvändning:** Både `break` och `continue` kan göra loopflödet svårt att följa. Många programmerare föredrar att strukturera loopar så att dessa satser inte behövs. De kan dock vara mycket användbara i vissa situationer.

### 🧮 4.8 Logiska Operatorer - Kombinera Villkor

Ofta behöver du testa flera villkor samtidigt. Logiska operatorer låter dig kombinera enkla villkor till mer komplexa.

Java har sex logiska operatorer:

- `&&` - Conditional AND (kortslutande)
- `||` - Conditional OR (kortslutande)
- `!` - Logical NOT
- `&` - Boolean logical AND (icke-kortslutande)
- `|` - Boolean logical inclusive OR (icke-kortslutande)
- `^` - Boolean logical exclusive OR

### 4.8.1 Conditional AND (&&)

`&&` returnerar `true` bara om BÅDA operanderna är `true`.

**Sanningstabell:**

|uttryck1|uttryck2|uttryck1 && uttryck2|
|---|---|---|
|false|false|false|
|false|true|false|
|true|false|false|
|true|true|true|

**Exempel:**

java

```java
int grade = 85;

if (grade >= 60 && grade <= 100) {
    System.out.println("Valid passing grade");
}
```

Detta kontrollerar att betyget är både minst sextig OCH högst hundra. Båda villkoren måste vara sanna.

**Mer komplext exempel:**

java

```java
int age = 25;
boolean hasLicense = true;

if (age >= 18 && hasLicense) {
    System.out.println("Can drive");
}
```

### 4.8.2 Conditional OR (||)

`||` returnerar `true` om MINST EN av operanderna är `true`.

**Sanningstabell:**

|uttryck1|uttryck2|uttryck1 \| uttryck2|
|---|---|---|
|false|false|false|
|false|true|true|
|true|false|true|
|true|true|true|

**Exempel:**

java

```java
int day = 7;

if (day == 6 || day == 7) {
    System.out.println("Weekend!");
}
```

Om dag är sex ELLER sju, är det helg. Bara ett av villkoren behöver vara sant.

### 4.8.3 Kortslutningsutvärdering

Detta är en mycket viktig optimering som Java gör automatiskt.

Med `&&`: Om första operanden är `false`, är hela uttrycket `false` oavsett vad andra operanden är, så Java testar aldrig andra operanden.

java

```java
int x = 5;

// Andra delen testas ALDRIG eftersom x > 10 är falskt:
if (x > 10 && x < 20) {
    System.out.println("Between 10 and 20");
}
```

Med `||`: Om första operanden är `true`, är hela uttrycket `true` oavsett vad andra operanden är, så Java testar aldrig andra operanden.

java

```java
int x = 5;

// Andra delen testas ALDRIG eftersom x < 10 är sant:
if (x < 10 || x > 20) {
    System.out.println("Outside 10-20 range");
}
```

**Varför är detta användbart?**

java

```java
// Säker division - undvik division med noll:
if (denominator != 0 && numerator / denominator > 1) {
    // Säkert, för om denominator är 0 testas aldrig divisionen
}

// FARLIGT utan kortslutning:
if (numerator / denominator > 1 && denominator != 0) {
    // Kan krascha om denominator är 0!
}
```

Ordningen på villkoren spelar roll! Sätt alltid det säkraste eller snabbaste testet först.

### 4.8.6 Logical NOT (!)

`!` inverterar ett booleskt värde.

java

```java
boolean found = false;

if (!found) {
    System.out.println("Not found");
}

// Motsvarar:
if (found == false) {
    System.out.println("Not found");
}
```

Men `!found` är mer idiomatiskt och lättare att läsa.

**Komplex användning:**

java

```java
if (!(grade >= 60)) {
    System.out.println("Failed");
}

// Men detta är tydligare:
if (grade < 60) {
    System.out.println("Failed");
}
```

När det är möjligt, skriv om villkoret utan NOT-operator för bättre läsbarhet.

### 4.8.7 Komplett Exempel med Logiska Operatorer

java

```java
import java.util.Scanner;

public class LogicalOperators {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        
        System.out.print("Enter gender (M/F): ");
        char gender = input.next().charAt(0);
        
        System.out.print("Enter age: ");
        int age = input.nextInt();
        
        System.out.print("Enter marital status (1=Single, 2=Married): ");
        int marital = input.nextInt();
        
        // Kombinera flera villkor:
        if ((gender == 'F' || gender == 'f') && age >= 65) {
            System.out.println("Eligible for senior discount");
        }
        
        if ((marital == 1) && (age < 25 || age > 70)) {
            System.out.println("High risk category for insurance");
        }
        
        if (!((age >= 18) && (age <= 65))) {
            System.out.println("Not in standard working age range");
        }
    }
}
```

Detta visar hur du kan bygga sofistikerade villkor genom att kombinera operatorer.

### 🎯 4.9 Strukturerad Programmering - Allt Faller På Plats

Nu har du lärt dig alla Javas kontrollstrukturer. Låt oss se hur de passar ihop.

**Javas kontrollstrukturer:**

- Sekvens (satser körs i ordning)
- Selektion: `if`, `if...else`, `switch`
- Iteration: `while`, `do...while`, `for`

**Strukturerad programmering-teoremet:** Varje program kan konstrueras från bara tre strukturer: sekvens, selektion och iteration. Du behöver inga andra kontrollstrukturer!

**Varför är detta viktigt?** Det betyder att alla program kan byggas genom att kombinera dessa byggstenar på två sätt:

1. **Stacking** - placera strukturer efter varandra
2. **Nesting** - placera strukturer inuti andra strukturer

Detta ger oss ett bevisbart sätt att resonera om våra program.

**Exempel på nesting:**

java

```java
// En if inuti en while inuti en for:
for (int i = 0; i < 10; i++) {
    while (condition) {
        if (test) {
            // Kod här
        }
    }
}
```

Varje struktur har en tydlig ingång och utgång, vilket gör programflödet lätt att följa och resonera om.

### 📚 4.10 Sammanfattning av Kapitel 4

Nu har du komplett kunskap om alla Javas kontrollstrukturer. Du kan använda `for` för räkningsuppgifter, `do...while` när du garanterat vill köra koden minst en gång, `switch` för multiselection, och logiska operatorer för att kombinera villkor. Du förstår också `break` och `continue` för mer avancerad loopkontroll.

**Nyckelpunkter:**

- `for` är bäst när du vet antalet iterationer i förväg
- `do...while` garanterar minst en iteration
- `switch` är tydligare än många `if...else` för diskreta värden
- Logiska operatorer låter dig kombinera flera villkor
- Kortslutningsutvärdering är både en optimering och ett säkerhetsnät
- Alla program kan byggas från sekvens, selektion och iteration

**Best practices:**

- Använd krullparenteser konsekvent för läsbarhet
- Placera säkerhetstester först i logiska uttryck
- Välj den loop-typ som tydligast uttrycker din avsikt
- Undvik komplexa nästlade strukturer när möjligt
- Kommentera komplexa logiska villkor

---
## **KAPITEL 5 - METODER**

### 📚 5.1 Introduktion - Dela och Härska

Föreställ dig att du skulle skriva all kod för en komplex applikation i en enda main-metod. Den skulle bli tusentals rader lång, omöjlig att förstå och ännu svårare att underhålla. Metoder är Javas svar på detta problem. De låter dig dela upp ett stort problem i mindre, hanterbara delar som var och en löser ett specifikt delproblem. Detta kallas "divide and conquer" och är en fundamental princip inom programmering.

När du organiserar kod i metoder får du flera fördelar. Du kan återanvända samma kod på flera ställen utan att kopiera den. Du kan gömma komplexa detaljer bakom enkla namn som beskriver vad metoden gör. Du kan testa varje metod separat, vilket gör det lättare att hitta buggar. Och du kan låta olika programmerare arbeta på olika metoder samtidigt.

### 🔧 5.2 Programenheter i Java

I Java är metoder byggstenar som grupperas i klasser, och klasser grupperas i paket. Detta ger en hierarkisk organisation som gör det lätt att hitta och återanvända kod.

Tänk på det så här: Om ett program är en stad, är paket olika stadsdelar (som java.util för verktyg eller java.io för filhantering), klasser är byggnader i dessa stadsdelar, och metoder är rum i dessa byggnader där specifikt arbete utförs. Main-metoden är entrén där besökare (JVM) kommer in i ditt program.

Du har redan använt många färdiga metoder från Java API. När du skriver `System.out.println()`, anropar du en metod som någon annan har skrivit. När du skapar en Scanner med `input.nextInt()`, använder du en metod från java.util-paketet. Nu ska du lära dig att skriva dina egna metoder.

### 📐 5.3 static Metoder, static Variabler och Math-klassen

Innan vi dyker in i hur man skapar egna metoder, låt oss förstå static-konceptet genom att titta på Math-klassen, som är full av användbara matematiska metoder.

**Vad betyder static?**

När en metod är static betyder det att den tillhör klassen själv, inte till specifika objekt av klassen. Du kan anropa en static metod direkt genom att använda klassnamnet, utan att först skapa ett objekt. Det är därför du skriver `Math.sqrt(25)` och inte behöver skapa ett Math-objekt först.

**Användbara Math-metoder:**

Math-klassen innehåller metoder för vanliga matematiska operationer. Här är några av de mest användbara:

java

```java
// Absolutvärde - tar bort minustecknet
int result1 = Math.abs(-23);        // result1 = 23
double result2 = Math.abs(-5.7);    // result2 = 5.7

// Minsta och största värde
int min = Math.min(10, 20);         // min = 10
int max = Math.max(10, 20);         // max = 20

// Upphöjt till - pow(bas, exponent)
double squared = Math.pow(5, 2);    // squared = 25.0 (5²)
double cubed = Math.pow(2, 3);      // cubed = 8.0 (2³)

// Kvadratrot
double root = Math.sqrt(16);        // root = 4.0

// Avrundning
double ceil = Math.ceil(9.2);       // ceil = 10.0 (rundar uppåt)
double floor = Math.floor(9.8);     // floor = 9.0 (rundar nedåt)
long rounded = Math.round(9.5);     // rounded = 10 (närmaste heltal)

// Trigonometriska funktioner (vinklar i radianer)
double sine = Math.sin(0);          // sine = 0.0
double cosine = Math.cos(0);        // cosine = 1.0
```

**Math-konstanter:**

Math-klassen har också två viktiga matematiska konstanter som är deklarerade som `public static final`, vilket betyder att de är tillgängliga för alla, kan inte ändras, och tillhör klassen (inte objekt).

java

```java
double pi = Math.PI;        // 3.141592653589793
double e = Math.E;          // 2.718281828459045

// Användning i beräkningar:
double circumference = 2 * Math.PI * radius;
double area = Math.PI * Math.pow(radius, 2);
```

Nyckelordet `final` gör en variabel till en konstant. När du deklarerar något som final kan det inte ändras efter initialiseringen, vilket är perfekt för matematiska konstanter som aldrig ska förändras.

### 🛠️ 5.4 Deklarera Metoder - Skapa Dina Egna Byggstenar

Nu ska vi lära oss att skapa egna metoder. Låt oss börja med ett exempel som bestämmer det största av tre tal:

java

```java
import java.util.Scanner;

public class MaximumFinder {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        
        System.out.print("Enter three numbers separated by spaces: ");
        double number1 = input.nextDouble();
        double number2 = input.nextDouble();
        double number3 = input.nextDouble();
        
        // Anropa vår egen metod maximum
        double result = maximum(number1, number2, number3);
        
        System.out.println("Maximum is: " + result);
    }
    
    // Vår egen metod som returnerar största värdet
    public static double maximum(double x, double y, double z) {
        double maximumValue = x;  // Antag att x är störst
        
        if (y > maximumValue) {
            maximumValue = y;
        }
        
        if (z > maximumValue) {
            maximumValue = z;
        }
        
        return maximumValue;
    }
}
```

Låt oss bryta ner metoddeklarationen del för del:

**Metodhuvudet (Method Header):**

`public static double maximum(double x, double y, double z)`

Detta innehåller fem viktiga delar:

Först kommer `public`, vilket betyder att metoden kan anropas från andra klasser. Detta är access modifier och styr var metoden är synlig.

Sedan kommer `static`, vilket betyder att metoden tillhör klassen, inte specifika objekt. Det är därför main kan anropa den direkt utan att skapa ett MaximumFinder-objekt. I kapitel 7 lär du dig om icke-static metoder.

Sedan kommer returtypen `double`, som talar om vilken typ av värde metoden ger tillbaka. Om metoden inte returnerar något används nyckelordet `void` istället.

Efter returtypen kommer metodnamnet `maximum`. Metodnamn följer samma regler som variabelnamn och bör börja med liten bokstav och använda camelCase för flera ord.

Slutligen kommer parameterlistan i parenteser `(double x, double y, double z)`. Detta är de värden som metoden behöver för att göra sitt jobb. Varje parameter måste ha en typ och ett namn. Om metoden inte behöver någon information är parenteserna tomma.

**Metodkroppen:**

Kroppen innehåller de satser som utför metodens uppgift. I vårt exempel använder vi en lokal variabel `maximumValue` för att hålla reda på det största värdet vi hittat hittills. Lokala variabler existerar bara medan metoden körs och försvinner när metoden returnerar.

**Return-satsen:**

`return maximumValue;` skickar tillbaka resultatet till den kod som anropade metoden. Värdet som returneras måste matcha metodens deklarerade returtyp. När Java når en return-sats avslutas metoden omedelbart och kontroll går tillbaka till anroparen.

### 🔄 5.5 Anropa och Använda Metoder

Det finns tre sätt att anropa metoder i Java, och vilket du använder beror på om metoden är static eller inte, och var den finns:

**Anropa metoder i samma klass:**

När du anropar en static metod från en annan static metod i samma klass (som när main anropar maximum), använder du bara metodnamnet:

java

```java
double result = maximum(num1, num2, num3);
```

**Anropa static metoder från andra klasser:**

För att anropa en static metod från en annan klass använder du klassnamnet följt av punkt och metodnamnet:

java

```java
double root = Math.sqrt(25);  // Math-klassen, sqrt-metoden
```

**Anropa icke-static metoder:**

För att anropa en icke-static metod behöver du ett objekt. Du använder objektvariabelns namn följt av punkt och metodnamnet. Detta lär du dig mer om i kapitel 7:

java

```java
Scanner input = new Scanner(System.in);
int value = input.nextInt();  // input är objektet, nextInt är metoden
```

**Metodanropets anatomi:**

När du skriver `maximum(number1, number2, number3)`, kallar vi number1, number2 och number3 för argument. Detta är de faktiska värden som skickas till metoden. Inuti metoden kallas x, y och z för parametrar. Detta är variablerna som tar emot värdena.

Tänk på parametrar som platshållare och argument som de faktiska värden som fyller platserna. När metoden anropas kopierar Java värdena från argumenten till parametrarna. Detta kallas pass-by-value och är mycket viktigt att förstå.

### 🔀 5.6 Metodanropsstack och Aktiveringsrecord

För att förstå hur Java håller reda på metodanrop behöver vi förstå metodanropsstacken. En stack är en datastruktur som fungerar som en hög tallrikar där du bara kan lägga till eller ta bort från toppen. Detta kallas LIFO (Last-In, First-Out) eftersom det sista som läggs till är det första som tas bort.

**Vad händer när en metod anropas:**

När main anropar maximum händer flera saker bakom kulisserna. Java måste komma ihåg var i main-metoden den ska återvända till när maximum är klar. Den måste också skapa utrymme för maximums parametrar och lokala variabler. All denna information lagras i något som kallas en stack frame eller activation record.

Föreställ dig att du läser en bok och kommer till en fotnot. Du lägger i ett bokmärke där du är, läser fotnoten, och återvänder sedan till platsen där bokmärket var. Metodanropsstacken fungerar på samma sätt. När maximum anropas, "bokmärker" Java platsen i main, skapar en ny stack frame för maximum, och kör maximums kod. När maximum returnerar, tar Java bort dess stack frame och återvänder till den bokmärkta platsen i main.

**Stack frames innehåller:**

Varje stack frame innehåller returadress, som talar om var programmet ska fortsätta när metoden är klar. Den innehåller också alla metodens parametrar och lokala variabler. När metoden returnerar tas hela stack framen bort och alla dess lokala variabler försvinner.

Detta förklarar varför lokala variabler i en metod inte är synliga i andra metoder. Varje metod har sin egen stack frame med sina egna variabler, helt isolerade från andra metoders variabler.

**Stack overflow:**

Om du har för många metodanrop samtidigt (till exempel genom oändlig rekursion där en metod anropar sig själv utan att någonsin sluta), kan stacken bli full. Detta kallas stack overflow och är ett fel som får programmet att krascha.

### 🔄 5.7 Argumentpromotion och Casting

Java är flexibel när det kommer till att konvertera mellan datatyper vid metodanrop, men det finns regler för när detta är tillåtet.

**Automatisk promotion:**

När du anropar en metod kan Java automatiskt konvertera argument till en "bredare" typ. Detta kallas widening conversion eller promotion och görs automatiskt eftersom ingen information går förlorad:

java

```java
public static double square(double number) {
    return number * number;
}

// Anrop med int argument:
int value = 5;
double result = square(value);  // Java konverterar 5 till 5.0 automatiskt
```

Java kan automatiskt konvertera i denna ordning (från smalare till bredare): byte → short → int → long → float → double

**Narrowing conversion kräver casting:**

För att konvertera åt andra hållet (från bredare till smalare typ) måste du explicit använda en cast operator eftersom information kan gå förlorad:

java

```java
public static int truncate(int number) {
    return number;
}

double value = 7.8;
int result = truncate((int) value);  // Måste casta, result blir 7
```

Observera att decimaldelen kastas bort utan avrundning. Om du försöker skicka en double till en metod som förväntar sig en int utan casting får du ett kompileringsfel.

### 📦 5.8 Java API Paket

Java API är organiserat i paket, som är samlingar av relaterade klasser. Att förstå viktiga paket hjälper dig att hitta rätt verktyg för jobbet.

**Vanliga paket du kommer använda:**

Paketet java.lang importeras automatiskt i alla Java-program och innehåller fundamentala klasser som String, Math, System och wrapperklasser som Integer och Double.

Paketet java.util innehåller verktygsklasser som Scanner för input, ArrayList för dynamiska arrayer, och klasser för datum och tid.

Paketet java.io innehåller klasser för input och output, särskilt för att läsa och skriva filer.

Paketet java.text innehåller klasser för formatering av tal och datum för olika språk och kulturer.

För att använda klasser från dessa paket (förutom java.lang) måste du importera dem i början av din fil. Du kan importera en specifik klass:

java

```java
import java.util.Scanner;  // Importera bara Scanner
```

Eller alla klasser i ett paket:

java

```java
import java.util.*;  // Importera alla klasser från java.util
```

Det första sättet är att föredra eftersom det gör det tydligt exakt vilka klasser programmet använder.

### 🎲 5.9 Fallstudie - Slumptalsgenerering med SecureRandom

Många program behöver generera slumptal, från spel som kastar tärningar till vetenskapliga simuleringar. Java tillhandahåller klassen SecureRandom för att generera högkvalitativa slumptal.

**Grundläggande användning:**

java

```java
import java.security.SecureRandom;

public class RandomNumbers {
    public static void main(String[] args) {
        SecureRandom randomNumbers = new SecureRandom();
        
        // Generera tio slumptal mellan 1 och 6 (tärningskast)
        for (int counter = 1; counter <= 10; counter++) {
            int face = 1 + randomNumbers.nextInt(6);
            System.out.printf("%d ", face);
        }
    }
}
```

Metoden nextInt(n) genererar ett slumptal från noll upp till men inte inkluderande n. För att få tal mellan ett och sex lägger vi till ett. Formeln är: `minimum + randomNumbers.nextInt(räckvidd)` där räckvidd är maximum minus minimum plus ett.

**Varför SecureRandom istället för Random?**

Java har också en enklare Random-klass, men SecureRandom är bättre eftersom den använder mer sofistikerade algoritmer som ger mer oförutsägbara tal. För spel och simuleringar är SecureRandom det rekommenderade valet. För säkerhetskritiska applikationer som kryptering är SecureRandom ett måste.

**Simulera tärningskast:**

java

```java
public class DiceSimulation {
    public static void main(String[] args) {
        SecureRandom random = new SecureRandom();
        int[] frequency = new int[7];  // Räkna frekvens för 1-6
        
        // Kasta tärning 6000 gånger
        for (int roll = 1; roll <= 6000; roll++) {
            int face = 1 + random.nextInt(6);
            frequency[face]++;
        }
        
        // Visa resultat
        System.out.println("Face\tFrequency");
        for (int face = 1; face <= 6; face++) {
            System.out.printf("%4d%10d%n", face, frequency[face]);
        }
    }
}
```

Med många kast borde varje sida dyka upp ungefär lika ofta (cirka tusen gånger var). Detta demonstrerar "law of large numbers" - ju fler gånger du upprepar ett slumpmässigt experiment, desto närmare teoretisk förväntning kommer resultaten.

### 🎰 5.10 Fallstudie - Tärningsspel med enum

Låt oss använda våra nya kunskaper för att skapa ett komplett tärningsspel kallat Craps. Detta kommer också introducera enum-typer, som låter dig definiera dina egna typer med namngivna konstanter.

**Spelets regler:**

En spelare kastar två tärningar. På första kastet vinner spelaren om summan är sju eller elva, och förlorar om summan är två, tre eller tolv. Alla andra summor blir spelarens point. Spelaren fortsätter kasta tills hen antingen kastar samma summa igen (vinner) eller kastar en sjua (förlorar).

**Definiera enum för spelstatus:**

java

```java
public enum Status {
    CONTINUE, WON, LOST
}
```

En enum är en speciell typ som kan ha bara ett begränsat antal värden. Detta är mycket bättre än att använda siffror eller strängar eftersom kompilatorn kan kontrollera att du bara använder giltiga värden. Om du skrev `Status.WIN` istället för `Status.WON` skulle kompilatorn fånga felet, medan felstavade strängar inte upptäcks förrän programmet körs.

**Komplett implementation:**

java

```java
import java.security.SecureRandom;

public class Craps {
    private static final SecureRandom randomNumbers = new SecureRandom();
    
    private enum Status {CONTINUE, WON, LOST}
    
    private static final int SNAKE_EYES = 2;
    private static final int TREY = 3;
    private static final int SEVEN = 7;
    private static final int YO_LEVEN = 11;
    private static final int BOX_CARS = 12;
    
    public static void main(String[] args) {
        int myPoint = 0;
        Status gameStatus;
        
        int sumOfDice = rollDice();  // Första kastet
        
        switch (sumOfDice) {
            case SEVEN:
            case YO_LEVEN:
                gameStatus = Status.WON;
                break;
            case SNAKE_EYES:
            case TREY:
            case BOX_CARS:
                gameStatus = Status.LOST;
                break;
            default:
                gameStatus = Status.CONTINUE;
                myPoint = sumOfDice;
                System.out.printf("Point is %d%n", myPoint);
                break;
        }
        
        while (gameStatus == Status.CONTINUE) {
            sumOfDice = rollDice();
            
            if (sumOfDice == myPoint) {
                gameStatus = Status.WON;
            }
            else if (sumOfDice == SEVEN) {
                gameStatus = Status.LOST;
            }
        }
        
        if (gameStatus == Status.WON) {
            System.out.println("Player wins");
        }
        else {
            System.out.println("Player loses");
        }
    }
    
    public static int rollDice() {
        int die1 = 1 + randomNumbers.nextInt(6);
        int die2 = 1 + randomNumbers.nextInt(6);
        int sum = die1 + die2;
        
        System.out.printf("Player rolled %d + %d = %d%n", die1, die2, sum);
        return sum;
    }
}
```

**Viktiga koncept i detta exempel:**

Observera hur vi använder `private static final` för konstanter som SEVEN och SNAKE_EYES. Konstanter gör koden mer läsbar eftersom namnet förklarar betydelsen. Att se `case SEVEN:` är mycket tydligare än `case 7:`. Genom att göra dem private håller vi dem inom klassen, static betyder att de tillhör klassen, och final betyder att deras värden inte kan ändras.

Metoden rollDice är ett perfekt exempel på hur metoder främjar återanvändning. Istället för att skriva kod för att kasta tärning på flera ställen, skriver vi den en gång och anropar metoden när vi behöver den. Om vi senare vill ändra hur tärningskast fungerar behöver vi bara ändra på ett ställe.

### 🔭 5.11 Scope av Deklarationer - Var Variabler Lever

Scope bestämmer var i programmet en variabel kan användas. Detta är avgörande för att förstå och undvika buggar.

**Fyra scope-regler:**

En metodparameters scope är hela metodens kropp. Parametern existerar från när metoden anropas tills den returnerar.

En lokal variabels scope är från deklarationspunkten till slutet av blocket där den deklareras. Detta inkluderar variabler deklarerade i for-loopar, vars scope är bara loopen.

En kontrollvariabel i en for-loops header har scope endast inom den loopen. Efter loopen existerar variabeln inte längre.

En metods eller fields scope är hela klassens kropp. Du kan anropa metoder och använda fields var som helst i klassen.

**Exempel som demonstrerar scope:**

java

````java
public class Scope {
    private static int x = 1;  // Field - synlig i hela klassen
    
    public static void main(String[] args) {
        int x = 5;  // Lokal variabel som skuggar field x
        
        System.out.printf("Local x in main is %d%n", x);
        
        useLocalVariable();
        useField();
        useLocalVariable();
        useField();
        
        System.out.printf("Local x in main is %d%n", x);
    }
    
    public static void useLocalVariable() {
        int x = 25;  // Initialiseras varje gång metoden anropas
        
        System.out.printf("Local x entering useLocalVariable is %d%n", x);
        x++;
        System.out.printf("Local x exiting useLocalVariable is %d%n", x);
    }
    
    public static void useField() {
        System.out.printf("Field x entering useField is %d%n", x);
        x *= 10;
        System.out.printf("Field x exiting useField is %d%n", x);
    }
}
```

Output:
```
Local x in main is 5
Local x entering useLocalVariable is 25
Local x exiting useLocalVariable is 26
Field x entering useField is 1
Field x exiting useField is 10
Local x entering useLocalVariable is 25
Local x exiting useLocalVariable is 26
Field x exiting useField is 10
Field x exiting useField is 100
Local x in main is 5
````

**Varför skuggning sker:**

När både en lokal variabel och en field har samma namn, skuggar den lokala variabeln field inom sitt scope. I main skuggar lokal x field x. I useLocalVariable skuggar lokal x också field x. Men i useField, där ingen lokal x deklareras, refererar x till field.

Detta kan vara förvirrande, så det är bäst att undvika att ge lokala variabler samma namn som fields. Om du verkligen behöver komma åt en skuggad static field kan du använda klassnamnet: `Scope.x`.

### 🔄 5.12 Metodöverlagring - Samma Namn, Olika Jobb

Java tillåter dig att ha flera metoder med samma namn så länge de har olika parametrar. Detta kallas method overloading och är användbart när du vill utföra liknande operationer på olika typer av data.

**Exempel med Math.abs:**

Math-klassen har faktiskt fyra olika abs-metoder:

- `abs(int n)` för heltal
- `abs(long n)` för långa heltal
- `abs(float n)` för flyttal
- `abs(double n)` för dubbelprecision

Java väljer automatiskt rätt version baserat på argumentets typ:

java

```java
int intValue = Math.abs(-5);        // Anropar abs(int)
double doubleValue = Math.abs(-5.7); // Anropar abs(double)
```

**Eget exempel med överlagrade metoder:**

java

````java
public class MethodOverload {
    public static void main(String[] args) {
        System.out.printf("Square of integer 7 is %d%n", square(7));
        System.out.printf("Square of double 7.5 is %f%n", square(7.5));
    }
    
    public static int square(int intValue) {
        System.out.printf("Called square with int argument: %d%n", intValue);
        return intValue * intValue;
    }
    
    public static double square(double doubleValue) {
        System.out.printf("Called square with double argument: %f%n", 
                         doubleValue);
        return doubleValue * doubleValue;
    }
}
```

Output:
```
Called square with int argument: 7
Square of integer 7 is 49
Called square with double argument: 7.500000
Square of double 7.5 is 56.250000
````

**Hur kompilatorn väljer metod:**

Kompilatorn tittar på metodens signatur, som är kombinationen av namnet och parametrarnas antal, typer och ordning. Returtypen är inte en del av signaturen. Det betyder att du inte kan ha två metoder med samma namn och parametrar som bara skiljer sig åt i returtyp, eftersom kompilatorn inte skulle kunna avgöra vilken som ska anropas.

Metodöverlagring är särskilt användbart när samma koncept gäller för olika datatyper. Istället för att ha `squareInt` och `squareDouble`, har du bara `square` och låter kompilatorn välja rätt version.

### 📚 5.13 Sammanfattning av Kapitel 5

Du har nu lärt dig att skapa och använda metoder, som är fundamentala för att skriva välorganiserad och återanvändbar kod. Du förstår hur metodanropsstacken håller reda på metodanrop, hur argument skickas till metoder, och hur scope bestämmer var variabler kan användas. Du har sett hur SecureRandom kan generera slumptal och hur enum-typer kan göra kod tydligare. Du kan också överlagra metoder för att ge samma operation olika implementationer för olika datatyper.

**Viktigaste lärdomar:**

Metoder låter dig dela upp problem i mindre delar som är lättare att förstå och testa. Static metoder tillhör klassen och kan anropas utan att skapa objekt. Parametrar och lokala variabler existerar bara medan metoden körs. Pass-by-value betyder att metoder får kopior av argumenten, inte originalen. Scope-regler avgör var variabler kan användas. Metodöverlagring låter samma namn utföra liknande operationer på olika typer.

**Best practices att komma ihåg:**

Ge metoder beskrivande namn som förklarar vad de gör, inte hur de gör det. Håll metoder fokuserade på en uppgift så de blir lättare att förstå och återanvända. Använd konstanter istället för magic numbers för att göra kod mer läsbar. Undvik att ge lokala variabler samma namn som fields. Dokumentera komplexa metoder med kommentarer som förklarar vad de gör, vilka parametrar de tar, och vad de returnerar.

---
## **KAPITEL 6 - ARRAYS OCH ARRAYLIST**

### 📚 6.1 Introduktion - Från Enstaka Variabler till Samlingar

Hittills har du använt individuella variabler för att lagra data, där varje värde har sitt eget namn. Men föreställ dig att du ska hantera betyg för hundra studenter. Att skapa hundra separata variabler som grade1, grade2, grade3 och så vidare skulle vara en mardröm att hantera. Arrays löser detta problem genom att låta dig lagra många värden av samma typ under ett gemensamt namn. Du kommer åt varje enskilt värde genom dess position, vilket kallas index.

Arrays är den första datastrukturen du lär dig i Java, och de är fundamentala för programmering. En datastruktur är helt enkelt ett sätt att organisera data i datorns minne så att du kan arbeta med den effektivt. Tänk på en array som en rad lådor numrerade från noll och uppåt, där varje låda kan innehålla ett värde av samma typ. Detta gör det enkelt att organisera, komma åt och manipulera stora mängder relaterad data.

I detta kapitel kommer du också att lära dig om ArrayList, en mer flexibel variant som kan växa och krympa automatiskt efter behov. Men först måste vi förstå grunderna med vanliga arrays.

### 🔍 6.2 Primitiva Typer kontra Referenstyper

Innan vi dyker in i arrays måste vi förstå en fundamental skillnad i hur Java hanterar olika typer av data. Java delar upp alla typer i två kategorier som fungerar helt olika i minnet.

**Primitiva typer lagrar värden direkt:**

När du deklarerar en primitiv variabel som `int x = 5`, lagras värdet fem direkt i den minneslokation som hör till x. Om du kopierar variabeln med `int y = x`, skapas en ny kopia av värdet fem. Att ändra y påverkar inte x eftersom de är helt separata värden i separata minnesplatser. De åtta primitiva typerna är byte, short, int, long, float, double, char och boolean.

**Referenstyper lagrar adresser:**

När du arbetar med referenstyper som arrays, String eller objekt fungerar det annorlunda. Variabeln innehåller inte själva data utan en referens, som är en adress till var data finns någonstans i minnet. Det är som skillnaden mellan att ha en bok i handen kontra att ha ett bibliotekskort som talar om var boken står. Två variabler kan referera till samma array i minnet, vilket betyder att ändringar genom ena variabeln syns genom den andra.

Detta är avgörande att förstå eftersom det påverkar hur du använder arrays. När du skickar en array till en metod, skickar du inte en kopia av alla element, utan bara en referens till arrayen. Det betyder att metoden kan ändra arrayens innehåll, och dessa ändringar kommer att synas efter att metoden returnerat.

### 📦 6.3 Arrays - Samla Relaterad Data

En array är en grupp variabler som alla har samma typ och delar ett gemensamt namn. Du kommer åt varje individuell variabel, kallad ett element, genom att använda arrayens namn följt av ett index i hakparenteser.

**Viktiga fakta om arrays:**

Arrays är objekt i Java, vilket betyder att de skapas med new och är referenstyper. När du skapar en array bestämmer du dess längd, och denna längd kan aldrig ändras. Om du behöver mer utrymme måste du skapa en helt ny array. Varje array vet sin egen längd, som du kan få med arrayens length-instansvariabel.

**Arrayindexering börjar alltid på noll:**

Detta är en konvention som kommer från C-språket och används av de flesta moderna programmeringsspråk. Det första elementet har index noll, det andra har index ett, och så vidare. Det sista elementet i en array med tolv element har därför index elva. Detta kan vara förvirrande i början, men du vänjer dig snabbt. Det betyder att om du har en array med längd N, är giltiga index från noll till N minus ett.

**Visualisering av en array:**

Föreställ dig en array med namnet c och tolv element. Du kan tänka på den som en rad numrerade lådor:

```
Index:    0    1    2    3    4    5    6    7    8    9   10   11
       ┌────┬────┬────┬────┬────┬────┬────┬────┬────┬────┬────┬────┐
  c:   │-45 │  6 │  0 │ 62 │ -3 │  1 │6453│ 78 │  0 │-89 │1543│ 72 │
       └────┴────┴────┴────┴────┴────┴────┴────┴────┴────┴────┴────┘
```

Du kommer åt värdet minus fyrtiofem med `c[0]`, värdet sex med `c[1]`, och så vidare. Uttrycket `c[7]` har värdet sextitvå. Du kan använda dessa uttryck precis som vanliga variabler, så `c[0] + c[1] + c[2]` beräknar summan av de tre första elementen.

### 🏗️ 6.4 Deklarera och Skapa Arrays

Att arbeta med arrays involverar två steg som ofta kombineras men är viktigt att förstå separat.

**Deklarera en array-variabel:**

```java
int[] array;  // Deklarerar en referens till en int-array
```

Detta skapar en variabel som kan referera till en int-array, men skapar inte själva arrayen än. Variabeln array innehåller ännu inget användbart, tekniskt sett innehåller den värdet null som betyder att den inte pekar på något objekt.

**Skapa array-objektet:**

```java
array = new int[10];  // Skapar arrayen med tio element
```

Nu skapas faktiskt en array med tio int-element i minnet, och array-variabeln får en referens till denna array. Varje element initialiseras automatiskt till noll, vilket är standardvärdet för int. Olika typer har olika standardvärden: numeriska typer blir noll, boolean blir false, och referenstyper blir null.

**Kombinera deklaration och skapande:**

Den vanligaste stilen är att kombinera dessa två steg:

```java
int[] array = new int[10];  // Deklarera och skapa i ett steg
```

Detta är kortare och tydligare. Observera att hakparenteserna kan placeras efter typen eller efter variabelnamnet, men konventionen är att placera dem efter typen eftersom det tydligare visar att array är en array-typ.

**Storleken kan vara ett uttryck:**

Du kan använda vilket heltalsvärde som helst för att bestämma arrayens storlek:

```java
int size = 10;
int[] numbers = new int[size];  // Storlek bestäms vid körning

Scanner input = new Scanner(System.in);
System.out.print("How many grades? ");
int numGrades = input.nextInt();
int[] grades = new int[numGrades];  // Storlek från användaren
```

Detta är kraftfullt eftersom det låter dig skapa arrays vars storlek bestäms medan programmet körs, baserat på användarens behov eller data du läser från en fil.

### 🎨 6.5 Exempel med Arrays

Nu ska vi se hur arrays används i praktiken genom flera exempel som bygger på varandra.

### 6.5.1 Skapa och Initialisera en Array

```java
public class InitArray {
    public static void main(String[] args) {
        int[] array = new int[10];  // Skapa array med standardvärden
        
        System.out.printf("%s%8s%n", "Index", "Value");
        
        // Gå igenom arrayen med traditionell for-loop
        for (int counter = 0; counter < array.length; counter++) {
            System.out.printf("%5d%8d%n", counter, array[counter]);
        }
    }
}
```

Output visar att alla element initialiseras till noll:

```
Index   Value
    0       0
    1       0
    2       0
    ...
    9       0
```

Notera hur vi använder `array.length` i loop-villkoret. Detta är mycket bättre än att hårdkoda talet tio, eftersom om du senare ändrar arrayens storlek kommer loopen automatiskt att anpassa sig. Egenskapen length är en final instansvariabel som innehåller arrayens storlek, och den kan aldrig ändras.

### 6.5.2 Använda en Array Initializer

Ofta vet du vilka värden du vill ha i arrayen när du skapar den. Då kan du använda en array initializer:

```java
int[] array = {32, 27, 64, 18, 95, 14, 90, 70, 60, 37};
```

Detta skapar automatiskt en array med tio element och initialiserar dem till de angivna värdena. Kompilatorn räknar antalet värden i listan och skapar en array av lämplig storlek. Detta är mycket bekvämare än att först skapa arrayen och sedan tilldela varje element separat.

Array initializers kan bara användas när du deklarerar variabeln. Du kan inte senare skriva `array = {1, 2, 3}` för att tilldela nya värden, det måste göras vid deklarationen.

### 6.5.3 Beräkna Värden att Lagra

Du behöver inte ange värden direkt. Du kan beräkna dem med en loop:

```java
public class InitArray {
    public static void main(String[] args) {
        final int ARRAY_LENGTH = 10;
        int[] array = new int[ARRAY_LENGTH];
        
        // Fyll arrayen med jämna tal
        for (int counter = 0; counter < array.length; counter++) {
            array[counter] = 2 + 2 * counter;
        }
        
        // Visa innehållet
        System.out.printf("%s%8s%n", "Index", "Value");
        for (int counter = 0; counter < array.length; counter++) {
            System.out.printf("%5d%8d%n", counter, array[counter]);
        }
    }
}
```

Detta fyller arrayen med värdena två, fyra, sex, åtta och så vidare. Observera användningen av en final-variabel för längden. Detta är god praxis eftersom det ger längden ett beskrivande namn och gör det lätt att ändra på ett ställe om du senare vill ha en annan storlek.

### 6.5.4 Summera Element i en Array

Ett vanligt behov är att beräkna summan av alla element:

```java
int[] array = {87, 68, 94, 100, 83, 78, 85, 91, 76, 87};
int total = 0;

for (int counter = 0; counter < array.length; counter++) {
    total += array[counter];
}

System.out.printf("Total of array elements: %d%n", total);
```

Vi använder en ackumulatorvariabel total som startar på noll. För varje element lägger vi till elementets värde till totalen. Efter att ha gått igenom alla element innehåller total summan av alla värden. Detta mönster med en ackumulator är extremt vanligt vid arraybearbetning.

### 6.5.5 Visa Arraydata Grafiskt med Stapeldiagram

Arrays är perfekta för att lagra data som ska visualiseras. Här är ett exempel som visar frekvensfördelning av tal med asterisker:

```java
int[] array = {0, 0, 0, 0, 0, 0, 1, 2, 4, 2, 1};

System.out.println("Grade distribution:");

for (int counter = 0; counter < array.length; counter++) {
    // Skriv ut etiketten
    if (counter == 10) {
        System.out.printf("%5d: ", 100);
    }
    else {
        System.out.printf("%02d-%02d: ", counter * 10, counter * 10 + 9);
    }
    
    // Skriv ut stapel av asterisker
    for (int stars = 0; stars < array[counter]; stars++) {
        System.out.print("*");
    }
    
    System.out.println();
}
```

Detta skapar en visuell representation där antalet asterisker visar frekvensen. Yttre loopen går igenom varje element i arrayen, och inre loopen skriver ut så många asterisker som elementets värde anger. Detta är ett exempel på nästlade loopar där array-data styr hur många gånger inre loopen ska köra.

### 6.5.6 Använda Arrayelement som Räknare

Arrays är utmärkta för att räkna frekvenser av olika värden. Här är ett exempel som simulerar att kasta en tärning sextusen gånger och räknar hur ofta varje sida dyker upp:

```java
import java.security.SecureRandom;

public class RollDie {
    public static void main(String[] args) {
        SecureRandom randomNumbers = new SecureRandom();
        
        // Skapa array för att räkna frekvenser (index 0 används inte)
        int[] frequency = new int[7];
        
        // Kasta tärningen 6000 gånger
        for (int roll = 1; roll <= 6000; roll++) {
            int face = 1 + randomNumbers.nextInt(6);  // 1-6
            ++frequency[face];  // Använd värdet som index
        }
        
        System.out.printf("%s%10s%n", "Face", "Frequency");
        for (int face = 1; face < frequency.length; face++) {
            System.out.printf("%4d%10d%n", face, frequency[face]);
        }
    }
}
```

Tricket här är att använda tärningsvärdet som index i arrayen. När vi kastar en trea ökar vi frequency-elementet vid index tre. Detta är ett elegant sätt att räkna eftersom en enda operation både identifierar vad vi räknar och ökar räknaren. Observera att vi skapar arrayen med sju element men använder bara index ett till sex, vilket gör koden tydligare än att hantera offset-beräkningar.

### 🛡️ 6.6 Undantagshantering - När Saker Går Fel

Vad händer om du försöker komma åt ett array-element som inte finns? Java kastar ett undantag, vilket är ett objekt som innehåller information om felet.

**ArrayIndexOutOfBoundsException:**

Om du försöker använda ett ogiltigt index, till exempel negativt eller större än eller lika med arrayens längd, kastar Java ett ArrayIndexOutOfBoundsException. Detta är ett runtime-fel som avslutar programmet om det inte hanteras.

```java
int[] array = new int[5];  // Giltig index: 0-4
System.out.println(array[5]);  // KRASCH! Index utanför gränserna
```

**Hantera undantag med try-catch:**

Java tillhandahåller ett try-catch-statement för att fånga och hantera undantag så att programmet kan fortsätta köra:

```java
import java.util.Scanner;

public class ArrayExample {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        Scanner input = new Scanner(System.in);
        
        System.out.print("Enter array index: ");
        
        try {
            int index = input.nextInt();
            System.out.printf("Value at index %d is %d%n", 
                            index, array[index]);
        }
        catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Invalid index!");
            System.out.println("Error: " + e.toString());
        }
        
        System.out.println("Program continues...");
    }
}
```

Try-blocket innehåller kod som kan kasta ett undantag. Om ett undantag inträffar hoppar programmet omedelbart till catch-blocket, där du kan hantera felet på ett kontrollerat sätt. Undantagsobjektet e innehåller information om felet, och dess toString-metod ger en läsbar beskrivning. Efter catch-blocket fortsätter programmet normalt istället för att krascha.

### 🔄 6.7 Enhanced for-satsen - Enklare Iteration

När du bara behöver läsa varje element i en array i ordning erbjuder Java en enklare syntax kallad enhanced for eller for-each loop:

```java
int[] array = {87, 68, 94, 100, 83, 78, 85, 91, 76, 87};

for (int element : array) {
    System.out.printf("%d ", element);
}
```

Läs detta som "för varje int element i array". I varje iteration tar element automatiskt värdet av nästa array-element. Detta är mycket tydligare än en traditionell for-loop med räknare och index.

**När använda enhanced for:**

Enhanced for är perfekt när du behöver göra något med varje element men inte bryr dig om indexet. Den är kortare, tydligare och mindre benägen för off-by-one-fel. Du kan inte få ArrayIndexOutOfBoundsException eftersom Java hanterar indexeringen automatiskt.

**När inte använda enhanced for:**

Du kan inte använda enhanced for om du behöver ändra array-element, eftersom element-variabeln bara är en kopia. Du kan inte heller använda den om du behöver veta vilket index du är på, eller om du vill hoppa över vissa element eller gå igenom arrayen i en annan ordning än början till slut.

```java
// Detta ändrar INTE arrayen:
for (int element : array) {
    element = 0;  // Ändrar bara kopian, inte arrayen
}

// För att ändra array måste du använda index:
for (int i = 0; i < array.length; i++) {
    array[i] = 0;  // Detta fungerar
}
```

### 📤 6.8 Skicka Arrays till Metoder

Arrays kan skickas till metoder precis som andra parametrar, men eftersom arrays är referenstyper fungerar det annorlunda än med primitiva typer.

**Skicka en array:**

```java
public class PassArray {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5};
        
        System.out.println("Before modifyArray:");
        printArray(array);
        
        modifyArray(array);  // Skicka referens till arrayen
        
        System.out.println("After modifyArray:");
        printArray(array);  // Arrayen har ändrats!
    }
    
    public static void modifyArray(int[] array) {
        for (int counter = 0; counter < array.length; counter++) {
            array[counter] *= 2;  // Dubbla varje element
        }
    }
    
    public static void printArray(int[] array) {
        for (int element : array) {
            System.out.printf("%d ", element);
        }
        System.out.println();
    }
}
```

Output:

```
Before modifyArray:
1 2 3 4 5
After modifyArray:
2 4 6 8 10
```

När du skickar array till modifyArray skickar du inte en kopia av arrayen utan en referens till originalet. Metoden kan därför ändra arrayens innehåll, och dessa ändringar syns i main efter att metoden returnerat. Detta är både kraftfullt och farligt eftersom en metod kan ändra data som den inte äger, vilket kan leda till svåra buggar om man inte är försiktig.

**Pass-by-value för referensen:**

Det är viktigt att förstå att Java använder pass-by-value även för arrays, men det som kopieras är referensen, inte arrayen själv. Det betyder att metoden får en kopia av adressen till arrayen. Både original och kopia pekar på samma array i minnet, så ändringar genom metoden påverkar originalet.

### 🎲 6.9 Pass-by-Value kontra Pass-by-Reference

Detta är ett av de mest missförstådda koncepten i Java. Låt oss klargöra det en gång för alla.

**Java använder alltid pass-by-value:**

När du skickar ett argument till en metod kopierar Java alltid värdet. För primitiva typer betyder detta att värdet kopieras. För referenstyper betyder det att referensen kopieras, men objektet själv kopieras inte.

```java
public class PassTest {
    public static void main(String[] args) {
        int number = 5;
        int[] numbers = {1, 2, 3};
        
        System.out.println("Before: number = " + number);
        modifyPrimitive(number);
        System.out.println("After: number = " + number);  // Fortfarande 5
        
        System.out.println("Before: numbers[0] = " + numbers[0]);
        modifyArray(numbers);
        System.out.println("After: numbers[0] = " + numbers[0]);  // Ändrad!
    }
    
    public static void modifyPrimitive(int value) {
        value *= 2;  // Ändrar bara lokala kopian
    }
    
    public static void modifyArray(int[] array) {
        array[0] *= 2;  // Ändrar originalarrayen
    }
}
```

Med primitiva typer får metoden en kopia av värdet. Att ändra kopian påverkar inte originalet. Med arrays får metoden en kopia av referensen. Båda referenserna pekar på samma array, så ändringar genom metoden påverkar arrayen som den som anropade kan se.

### 🎯 6.10 Flerdimensionella Arrays

Hittills har vi arbetat med endimensionella arrays, som är som en rad värden. Men ibland behöver du organisera data i flera dimensioner, som en tabell med rader och kolumner.

**Tvådimensionella arrays:**

En tvådimensionell array är tekniskt sett en array av arrays. Varje element i den yttre arrayen är i sig en array. Du kan tänka på det som en tabell:

```java
int[][] table = new int[3][4];  // 3 rader, 4 kolumner
```

Detta skapar en struktur som kan visualiseras så här:

```
        Kolumn 0  Kolumn 1  Kolumn 2  Kolumn 3
Rad 0   [  0  ]   [  0  ]   [  0  ]   [  0  ]
Rad 1   [  0  ]   [  0  ]   [  0  ]   [  0  ]
Rad 2   [  0  ]   [  0  ]   [  0  ]   [  0  ]
```

Du kommer åt element med två index: `table[rad][kolumn]`. Första indexet anger raden, andra indexet anger kolumnen.

**Initialisera med nästlade initializerare:**

```java
int[][] table = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};
```

Varje inre lista representerar en rad. Detta skapar samma treradig, fyrkolumnig tabell som ovan men med angivna värden.

**Rader kan ha olika längd:**

Java tillåter att olika rader har olika antal element:

```java
int[][] ragged = {
    {1, 2},
    {3, 4, 5},
    {6, 7, 8, 9}
};
```

Detta kallas en ragged array. Rad noll har två element, rad ett har tre element, och rad två har fyra element. Detta är möjligt eftersom varje rad är sin egen separata array.

**Iterera genom tvådimensionella arrays:**

Du behöver nästlade loopar, en för rader och en för kolumner:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Traditionella for-loopar med index:
for (int row = 0; row < matrix.length; row++) {
    for (int col = 0; col < matrix[row].length; col++) {
        System.out.printf("%d ", matrix[row][col]);
    }
    System.out.println();  // Ny rad efter varje rad
}

// Enhanced for-loopar:
for (int[] row : matrix) {
    for (int element : row) {
        System.out.printf("%d ", element);
    }
    System.out.println();
}
```

Observera att `matrix.length` ger antalet rader, medan `matrix[row].length` ger antalet kolumner i den specifika raden. Detta är särskilt viktigt för ragged arrays där olika rader kan ha olika längd.

### 📝 6.11 Variabellängds Argumentlistor - Flexibla Metoder

Ibland vill du skriva en metod som kan ta olika antal argument. Java tillhandahåller varargs (variable-length argument lists) för detta:

```java
public class VarargsTest {
    public static double average(double... numbers) {
        double total = 0.0;
        
        // numbers behandlas som en array inuti metoden
        for (double d : numbers) {
            total += d;
        }
        
        return total / numbers.length;
    }
    
    public static void main(String[] args) {
        double d1 = average(1, 2, 3);        // Tre argument
        double d2 = average(4, 5, 6, 7, 8);  // Fem argument
        
        System.out.printf("d1 = %.1f%nd2 = %.1f%n", d1, d2);
    }
}
```

Tre punkter efter typen indikerar varargs. Java samlar automatiskt alla argument i en array. Du kan anropa metoden med vilket antal argument som helst, även noll. Inuti metoden behandlas numbers precis som en vanlig array.

**Regler för varargs:**

En metod kan bara ha en varargs-parameter, och den måste vara sista parametern i listan. Du kan ha andra parametrar före varargs:

```java
public static void printWithLabel(String label, int... values) {
    System.out.print(label + ": ");
    for (int value : values) {
        System.out.printf("%d ", value);
    }
    System.out.println();
}

// Använd:
printWithLabel("Numbers", 1, 2, 3, 4, 5);
```

### 💻 6.12 Kommandoradsargument - Input vid Start

När du startar ett Java-program från kommandoraden kan du skicka argument:

```bash
java MyProgram arg1 arg2 arg3
```

Dessa argument finns tillgängliga i String-arrayen som main-metoden tar emot:

```java
public class CommandLineTest {
    public static void main(String[] args) {
        if (args.length == 0) {
            System.out.println("No arguments provided");
            return;
        }
        
        System.out.println("Arguments received:");
        for (int i = 0; i < args.length; i++) {
            System.out.printf("args[%d] = %s%n", i, args[i]);
        }
    }
}
```

Om du kör `java CommandLineTest Hello World 123` kommer args-arrayen att innehålla tre strängar: "Hello", "World" och "123". Observera att allt är strängar även om det ser ut som ett tal. Om du behöver tal måste du konvertera:

```java
if (args.length > 0) {
    int number = Integer.parseInt(args[0]);  // Konvertera till int
}
```

### 🔧 6.13 Klassen Arrays - Kraftfulla Verktyg

Java tillhandahåller klassen Arrays i java.util-paketet med många användbara metoder för att arbeta med arrays.

**Fyll en array med samma värde:**

```java
import java.util.Arrays;

int[] values = new int[10];
Arrays.fill(values, 7);  // Alla element blir 7
```

**Sortera en array:**

```java
int[] numbers = {8, 4, 2, 9, 1, 6};
Arrays.sort(numbers);  // Sorterar i stigande ordning
// numbers är nu {1, 2, 4, 6, 8, 9}
```

**Jämföra arrays:**

```java
int[] array1 = {1, 2, 3};
int[] array2 = {1, 2, 3};
int[] array3 = {3, 2, 1};

boolean same1 = Arrays.equals(array1, array2);  // true
boolean same2 = Arrays.equals(array1, array3);  // false
```

Detta är viktigt eftersom att använda == på arrays jämför referenser, inte innehåll. Även om två arrays har samma element kommer `array1 == array2` att vara falskt om de är olika objekt.

**Kopiera arrays:**

```java
int[] source = {1, 2, 3, 4, 5};
int[] destination = Arrays.copyOf(source, source.length);

// Eller kopiera med System.arraycopy för mer kontroll:
int[] dest2 = new int[5];
System.arraycopy(source, 0, dest2, 0, source.length);
```

**Binärsökning:**

För sorterade arrays kan du använda binärsökning, som är mycket snabbare än att gå igenom varje element:

```java
int[] sortedArray = {1, 3, 5, 7, 9, 11, 13};
int index = Arrays.binarySearch(sortedArray, 7);  // index = 3

int notFound = Arrays.binarySearch(sortedArray, 8);  // negativt tal
```

Om värdet hittas returneras dess index. Om det inte hittas returneras ett negativt tal som indikerar var värdet skulle ha varit om det funnits. Kom ihåg att arrayen måste vara sorterad för att binärsökning ska fungera korrekt.

### 🌟 6.14 ArrayList - Dynamiska Arrays

Arrays har en fast storlek som inte kan ändras efter att de skapats. ArrayList är en samlingsklass som löser denna begränsning genom att automatiskt växa när du lägger till element.

**Skapa och använda ArrayList:**

```java
import java.util.ArrayList;

public class ArrayListTest {
    public static void main(String[] args) {
        // Skapa ArrayList för att lagra String-objekt
        ArrayList<String> items = new ArrayList<>();
        
        // Lägg till element i slutet
        items.add("red");
        items.add("yellow");
        
        // Infoga på specifik position
        items.add(1, "green");  // Sätter in "green" vid index 1
        
        // Visa storlek
        System.out.println("Size: " + items.size());
        
        // Visa innehåll med loop
        for (String item : items) {
            System.out.print(item + " ");
        }
        System.out.println();
        
        // Hämta specifikt element
        String first = items.get(0);
        System.out.println("First: " + first);
        
        // Ta bort element
        items.remove("yellow");  // Ta bort första förekomsten
        items.remove(0);          // Ta bort vid index 0
        
        // Kontrollera om element finns
        if (items.contains("red")) {
            System.out.println("Contains red");
        }
        
        // Töm listan
        items.clear();
        
        // Kontrollera om tom
        if (items.isEmpty()) {
            System.out.println("List is empty");
        }
    }
}
```

**Viktiga skillnader från arrays:**

ArrayList använder vinkelparenteser för att specificera typen av element, som `ArrayList<String>`. Detta kallas generics och säkerställer att du bara kan lägga till rätt typ av objekt. Du måste använda wrapper-klasser för primitiva typer, så `ArrayList<Integer>` istället för `ArrayList<int>`.

Du använder metoder istället för hakparenteser. För att komma åt element använder du `get(index)` istället för `array[index]`, och för att ändra element använder du `set(index, value)`. Storleken får du med `size()` istället för `length`.

**Autoboxing och unboxing:**

Java konverterar automatiskt mellan primitiva typer och deras wrapper-klasser:

```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(42);  // Autoboxing: int 42 blir Integer-objekt
int value = numbers.get(0);  // Unboxing: Integer-objekt blir int
```

Detta gör det bekvämt att arbeta med primitiva värden i collections även om tekniskt sett bara objekt kan lagras.

### 📚 6.15 Sammanfattning av Kapitel 6

Du har nu lärt dig att arbeta med arrays, som är fundamentala datastrukturer för att lagra och manipulera samlingar av data. Du förstår hur arrays är referenstyper och hur detta påverkar hur de skickas till metoder. Du kan använda både traditionella for-loopar och enhanced for-loopar för att iterera genom arrays. Du har sett flerdimensionella arrays för att representera tabelldata, och du känner till ArrayList som ett mer flexibelt alternativ till arrays.

**Viktigaste koncepten:**

Arrays är objekt som lagrar fasta antal element av samma typ. Indexering börjar på noll och du måste vara försiktig att inte använda ogiltiga index. När du skickar arrays till metoder skickas en referens, vilket betyder att metoden kan ändra originaldata. Enhanced for-loopen är enklare för att läsa element men ger inte index eller möjlighet att ändra. ArrayList växer automatiskt och erbjuder många bekväma metoder men är lite långsammare än vanliga arrays.

**Best practices:**

Använd enhanced for när du inte behöver index. Hantera alltid möjligheten för ogiltiga index med validering eller undantagshantering. Använd Arrays-klassens metoder för vanliga operationer som sortering och jämförelse. Välj ArrayList när du behöver dynamisk storlek, välj vanliga arrays för bättre prestanda eller när storleken är fix. Ge arrays beskrivande namn som förklarar vad de innehåller. Använd konstanter för array-storlekar så koden blir lättare att underhålla.

---
