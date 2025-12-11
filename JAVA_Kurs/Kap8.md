# KAPITEL 8: KLASSER OCH OBJEKT – EN DJUPARE ANALYS

## 🎯 ÖVERSIKT AV KAPITLET

Kapitel 8 bygger vidare på de grundläggande klasskoncepten från kapitel 7 och introducerar mer avancerade tekniker för att skapa robusta, väl designade klasser. Kapitlet fokuserar på hur du skapar professionella klasser med god inkapsling, flexibla konstruktorer och väl genomtänkta klassmedlemmar.

---

## 📚 SEKTION 8.2: TIME CLASS CASE STUDY

### Grundläggande klassstruktur och public interface

Kapitlet börjar med en **Time1-klass** som representerar tid på dagen. Detta är ett utmärkt exempel på hur man designar en klass med tydlig separation mellan det publika gränssnittet och den privata implementationen.

**Nyckelbegrepp:**

- **Public interface** (publikt gränssnitt): De publika metoderna i en klass utgör dess publika gränssnitt eller publika tjänster. Detta är vad klienterna av klassen kan använda.
- **Private implementation** (privat implementation): De privata medlemmarna är inte tillgängliga för klassens klienter och utgör implementationsdetaljerna.

**Time1-klassens struktur:**

java

```java
public class Time1 {
    private int hour;    // 0-23
    private int minute;  // 0-59
    private int second;  // 0-59
    
    public void setTime(int h, int m, int s) {
        // Validering och inställning av tid
    }
    
    public String toUniversalString() {
        // Returnerar tid i format HH:MM:SS
    }
    
    public String toString() {
        // Returnerar tid i format hh:MM:SS AM/PM
    }
}
```

### String.format() metoden

En viktig metod som introduceras här är **String.format()**. Detta är en statisk metod i String-klassen som fungerar som `System.out.printf()`, men istället för att skriva ut direkt returnerar den en formaterad sträng.

**Exempel:**

java

```java
return String.format("%02d:%02d:%02d", hour, minute, second);
```

Detta skapar en sträng där varje heltal formateras med minst två siffror (med ledande nollor om nödvändigt).

### toString() metoden

Varje objekt i Java har en **toString()** metod som returnerar en strängrepresentation av objektet. Denna metod anropas _implicit_ när ett objekt används där en String förväntas (till exempel i println()).

---

## 🔒 SEKTION 8.3: CONTROLLING ACCESS TO MEMBERS

### Access modifiers

Java använder **access modifiers** (åtkomstmodifierare) för att kontrollera åtkomsten till klassmedlemmar:

1. **public**: Medlemmen är tillgänglig från vilken klass som helst
2. **private**: Medlemmen är endast tillgänglig inifrån den egna klassen

**Designprincip:** Instansvariabler ska nästan alltid vara private för att upprätthålla inkapsling. Publika metoder tillhandahåller ett kontrollerat gränssnitt för att manipulera dessa privata data.

---

## 🔍 SEKTION 8.4: this-REFERENSEN

### Vad är this?

Nyckelordet **this** är en referens till det aktuella objektet. Det används för att:

1. Referera till det aktuella objektets instansvariabler när de skuggas av lokala variabler
2. Anropa andra konstruktorer i samma klass
3. Explicit referera till det aktuella objektets metoder

### Användning av this för att undvika shadowing

När en metodparameter eller lokal variabel har samma namn som en instansvariabel, sägs parametern/variabeln **skugga** (shadow) instansvariabeln. Då måste du använda this för att referera till instansvariabeln.

**Exempel:**

java

```java
public class SimpleTime {
    private int hour;
    
    public void setHour(int hour) {
        this.hour = hour;  // this.hour refererar till instansvariabeln
                           // hour refererar till parametern
    }
}
```

**Jämförelse med Python:** I Python använder du explicit `self` för att referera till instansvariabler, medan i Java är this ofta implicit. Du behöver bara använda det explicit när det finns namnkonflikter.

### Performance-aspekt

Det finns endast _en kopia_ av varje metod per klass som delas av alla objekt. Varje objekt har dock sina _egna kopior_ av klassens instansvariabler. Metoderna använder implicit this för att bestämma vilket specifikt objekt som ska manipuleras.

---

## 🏗️ SEKTION 8.5: ÖVERLAGRADE KONSTRUKTORER

### Constructor overloading

**Constructor overloading** (konstruktor-överlagring) innebär att du deklarerar flera konstruktorer med olika signaturer (olika antal eller typer av parametrar). Detta ger flexibilitet i hur objekt initialiseras.

**Time2-klassens konstruktorer:**

java

```java
public class Time2 {
    private int hour;
    private int minute;
    private int second;
    
    // Konstruktor 1: Ingen parameter
    public Time2() {
        this(0, 0, 0);  // Anropar konstruktor med tre parametrar
    }
    
    // Konstruktor 2: En parameter (hour)
    public Time2(int hour) {
        this(hour, 0, 0);
    }
    
    // Konstruktor 3: Två parametrar (hour, minute)
    public Time2(int hour, int minute) {
        this(hour, minute, 0);
    }
    
    // Konstruktor 4: Tre parametrar (hour, minute, second)
    public Time2(int hour, int minute, int second) {
        if (hour < 0 || hour >= 24) {
            throw new IllegalArgumentException("hour must be 0-23");
        }
        if (minute < 0 || minute >= 60) {
            throw new IllegalArgumentException("minute must be 0-59");
        }
        if (second < 0 || second >= 60) {
            throw new IllegalArgumentException("second must be 0-59");
        }
        
        this.hour = hour;
        this.minute = minute;
        this.second = second;
    }
    
    // Konstruktor 5: Time2-objekt som parameter
    public Time2(Time2 time) {
        this(time.hour, time.minute, time.second);
    }
}
```

### Constructor chaining med this()

När du använder **this()** för att anropa en annan konstruktor, måste det vara den _första satsen_ i konstruktorn. Detta kallas **constructor chaining** och är en effektiv teknik för att undvika kodduplicering.

**Fördelar med constructor chaining:**

- Undviker kodduplicering
- Centraliserar valideringslogik till en konstruktor
- Gör koden lättare att underhålla

### throw-satsen

**throw** används för att indikera att ett problem har uppstått. När du kastar ett undantag (exception), avbryts normal exekvering och kontroll överförs till en exception handler.

**Exempel:**

java

```java
if (hour < 0 || hour >= 24) {
    throw new IllegalArgumentException("hour must be 0-23");
}
```

---

## 🏭 SEKTION 8.6: DEFAULT OCH NO-ARGUMENT CONSTRUCTORS

### Default constructor

Om du _inte_ deklarerar någon konstruktor i din klass, skapar kompilatorn automatiskt en **default constructor** (standardkonstruktor). Denna konstruktor:

- Har inga parametrar
- Initialiserar instansvariabler till deras standardvärden (0 för numeriska typer, false för boolean, null för referenser)

### No-argument constructor

Om du deklarerar _någon_ konstruktor i din klass, skapar kompilatorn _inte_ en default constructor. Om du då vill kunna skapa objekt utan argument måste du explicit deklarera en **no-argument constructor** (konstruktor utan argument).

**Viktig skillnad:**

- **Default constructor**: Skapas automatiskt av kompilatorn om ingen konstruktor finns
- **No-argument constructor**: Explicit deklarerad konstruktor utan parametrar

**Exempel:**

java

```java
public class MyClass {
    private int value;
    
    // Explicit no-argument constructor
    public MyClass() {
        value = 10;  // Anpassad initialisering
    }
    
    // Konstruktor med parameter
    public MyClass(int value) {
        this.value = value;
    }
}
```

---

## 🎛️ SEKTION 8.7: SET OCH GET METODER

### Terminologi

**Set-metoder** kallas även:

- **Mutator methods** (muteringsmetoder) – eftersom de ändrar värden
- Vanligtvis har namnen på formen `setVariableName()`

**Get-metoder** kallas även:

- **Accessor methods** (åtkomstmetoder) eller **query methods** (frågemetoder)
- Vanligtvis har namnen på formen `getVariableName()`

### Predicate methods

En **predicate method** (predikatsmetod) är en speciell typ av query method som testar om ett villkor är sant eller falskt och returnerar ett boolean-värde. Dessa metoder har ofta namn som börjar med "is" eller "has".

**Exempel:**

java

```java
public boolean isEmpty() {
    return size == 0;
}

public boolean isValidAge(int age) {
    return age >= 0 && age <= 120;
}
```

### Best practices för set/get-metoder

Set-metoder bör innehålla valideringslogik för att säkerställa att data förblir konsistent. Get-metoder bör returnera kopior av föränderliga objekt för att skydda intern data.

---

## 🧩 SEKTION 8.8: COMPOSITION

### Vad är composition?

**Composition** (komposition) innebär att en klass har referenser till objekt av andra klasser som medlemmar. Detta kallas ibland för en **has-a relationship** (har-ett-förhållande).

**Exempel med Employee och Date:**

java

```java
public class Date {
    private int month;
    private int day;
    private int year;
    
    public Date(int month, int day, int year) {
        this.month = month;
        this.day = day;
        this.year = year;
    }
    
    public String toString() {
        return String.format("%d/%d/%d", month, day, year);
    }
}

public class Employee {
    private String firstName;
    private String lastName;
    private Date birthDate;  // Employee "has-a" Date
    private Date hireDate;   // Employee "has-a" Date
    
    public Employee(String firstName, String lastName, 
                    Date birthDate, Date hireDate) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.birthDate = birthDate;
        this.hireDate = hireDate;
    }
    
    public String toString() {
        return String.format("%s, %s Hired: %s Birthday: %s",
            lastName, firstName, hireDate, birthDate);
    }
}
```

**Användning:**

java

```java
Date birth = new Date(7, 24, 1949);
Date hire = new Date(3, 12, 1988);
Employee employee = new Employee("Bob", "Blue", birth, hire);
System.out.println(employee);  // Implicit toString()-anrop
```

### Designprinciper för composition

Composition är ett kraftfullt verktyg för att bygga komplexa klasser från enklare byggstenar. Det främjar återanvändning av kod och skapar tydliga relationer mellan klasser.

---

## 🏷️ SEKTION 8.9: enum TYPES

### Vad är enum?

En **enum type** (enum-typ) är en speciell typ av klass som definierar en uppsättning namngivna konstanter. Alla enum-typer är referenstyper.

### Grundläggande enum-deklaration

**Egenskaper:**

- enum-konstanter är implicit **final** (kan inte ändras)
- enum-konstanter är implicit **static** (delas av alla instanser)
- Du kan inte skapa objekt av en enum-typ med `new`

**Enkel enum:**

java

```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

### Avancerad enum med konstruktorer och metoder

En enum kan innehålla konstruktorer, instansvariabler och metoder, precis som en vanlig klass:

java

```java
public enum Book {
    // enum-konstanter med argument till konstruktorn
    JHTP("Java How to Program", "2018"),
    CHTP("C How to Program", "2016"),
    IW3HTP("Internet & World Wide Web How to Program", "2012"),
    CPPHTP("C++ How to Program", "2017"),
    VBHTP("Visual Basic How to Program", "2014"),
    CSHARPHTP("Visual C# How to Program", "2017");
    
    // Instansvariabler
    private final String title;
    private final String copyrightYear;
    
    // Konstruktor (implicit private)
    Book(String title, String copyrightYear) {
        this.title = title;
        this.copyrightYear = copyrightYear;
    }
    
    // Accessor-metoder
    public String getTitle() {
        return title;
    }
    
    public String getCopyrightYear() {
        return copyrightYear;
    }
}
```

### Använda enum

**values() metoden:** Kompilatorn genererar automatiskt en statisk metod **values()** som returnerar en array med alla enum-konstanter i deklarationsordning:

java

```java
for (Book book : Book.values()) {
    System.out.printf("%-10s%-45s%s%n", 
        book, book.getTitle(), book.getCopyrightYear());
}
```

**EnumSet:** **EnumSet** är en specialiserad Set-implementation för enum-typer. Metoden **EnumSet.range()** skapar ett EnumSet som innehåller alla konstanter mellan två angivna konstanter:

java

```java
for (Book book : EnumSet.range(Book.JHTP, Book.CPPHTP)) {
    System.out.printf("%-10s%-45s%s%n", 
        book, book.getTitle(), book.getCopyrightYear());
}
```

### När ska du använda enum?

Använd enum när du har en fast uppsättning relaterade konstanter, som dagar i veckan, månader, kortfärger, eller statusvärden i ett spel.

---

## 🗑️ SEKTION 8.10: GARBAGE COLLECTION

### Hur fungerar garbage collection?

**Garbage collection** (skräpinsamling) är en automatisk minneshanteringsprocess i Java. JVM (Java Virtual Machine) återtar minne som används av objekt som inte längre refereras till.

**Nyckelbegrepp:**

- Ett objekt är **eligible for garbage collection** (berättigat för skräpinsamling) när det inte längre finns några referenser till det
- Garbage collection sker inte omedelbart – den kan ske när JVM bestämmer, eller kanske inte alls innan programmet avslutas
- Detta minskar risken för minnesläckor jämfört med språk som C och C++

### finalize() metoden

Varje klass ärver metoden **finalize()** från klassen Object. Denna metod kan anropas innan garbage collection sker, men detta är inte garanterat. I modern Java-programmering används finalize() sällan och är faktiskt **deprecated** (utfasad).

**Bättre alternativ:** Använd try-with-resources eller explicit resurshantering istället för att förlita dig på finalize().

---

## 🌐 SEKTION 8.11: static CLASS MEMBERS

### static variabler

En **static variable** (statisk variabel) representerar klassvid information som delas mellan alla objekt av klassen. Det finns endast _en kopia_ av varje statisk variabel, oavsett hur många objekt som skapas.

**Exempel:**

java

```java
public class Employee {
    private static int count = 0;  // Antal Employee-objekt
    private String firstName;
    private String lastName;
    
    public Employee(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        count++;  // Öka räknaren när ett nytt objekt skapas
    }
    
    public static int getCount() {
        return count;
    }
}
```

### Class scope

static variabler har **class scope** (klassgilltighetområde). De kan nås:

- Genom en referens till vilket objekt som helst av klassen
- Genom att kvalificera medlemsnamnet med klassnamnet och en punkt: `ClassName.memberName`

**Exempel:**

java

```java
System.out.println("Employee count: " + Employee.getCount());
```

### static metoder

En **static method** (statisk metod) kan anropas även när inga objekt av klassen har skapats. Statiska metoder:

- Kan _inte_ direkt komma åt instansvariabler eller instansmetoder
- Kan _inte_ använda **this**-referensen
- Anropas typiskt via klassnamnet: `ClassName.methodName()`

**Varför dessa begränsningar?** Eftersom en statisk metod kan anropas utan att något objekt existerar, finns det inget specifikt objekt vars instansvariabler skulle kunna användas.

**Exempel på användning:**

java

```java
public class MathUtils {
    public static int square(int x) {
        return x * x;
    }
    
    public static double average(int[] numbers) {
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        return (double) sum / numbers.length;
    }
}

// Användning:
int result = MathUtils.square(5);
double avg = MathUtils.average(new int[]{1, 2, 3, 4, 5});
```

### När ska du använda static?

**Använd static variabler när:**

- Information ska delas mellan alla instanser av klassen
- Du vill räkna antal skapade objekt
- Du vill definiera konstanter (tillsammans med final)

**Använd static metoder när:**

- Metoden inte behöver komma åt instansvariabler
- Metoden utför en nyttofunktion (som Math.sqrt())
- Metoden är en factory-metod som skapar objekt

---

## 📦 SEKTION 8.12: static IMPORT

### Vad är static import?

**static import** låter dig importera statiska medlemmar från en klass så att du kan använda dem utan att kvalificera dem med klassnamnet.

### Två former av static import

**1. Single static import:** Importerar en specifik statisk medlem:

java

```java
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

public class Circle {
    public double calculateArea(double radius) {
        return PI * radius * radius;  // Ingen "Math." nödvändig
    }
    
    public double calculateRadius(double area) {
        return sqrt(area / PI);  // Ingen "Math." nödvändig
    }
}
```

**2. static import on demand:** Importerar alla statiska medlemmar från en klass:

java

```java
import static java.lang.Math.*;

public class Calculator {
    public double calculate(double x, double y) {
        return sqrt(pow(x, 2) + pow(y, 2));
    }
}
```

### Varningar om static import

**Använd med försiktighet!**

- Kan göra koden mindre läsbar genom att dölja varifrån metoder kommer
- Kan orsaka namnkonflikter om flera klasser har statiska medlemmar med samma namn
- Använd främst för välkända klasser som Math där avsikten är tydlig

---

## 🔒 SEKTION 8.13: final INSTANCE VARIABLES

### Vad betyder final?

Nyckelordet **final** specificerar att en variabel inte är modifierbar efter initialisering. För instansvariabler innebär detta att värdet måste sättas:

- I deklarationen, eller
- I varje konstruktor

**Exempel:**

java

```java
public class Employee {
    private final String socialSecurityNumber;  // Ska aldrig ändras
    private String firstName;
    private String lastName;
    
    public Employee(String ssn, String firstName, String lastName) {
        this.socialSecurityNumber = ssn;  // Initialiseras en gång
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    public String getSocialSecurityNumber() {
        return socialSecurityNumber;
    }
    
    // Ingen setSocialSecurityNumber() metod!
}
```

### Principle of least privilege

**Principle of least privilege** (principen om minsta privilegium) säger att kod endast ska ges den åtkomst och de privilegier som krävs för att utföra sin uppgift. Genom att deklarera variabler som final när de inte ska ändras, följer du denna princip.

### Kombinera final med static

När en final variabel initialiseras till samma värde för alla objekt bör den också vara static. Detta skapar en **klassspecifik konstant**:

java

```java
public class Circle {
    private static final double PI = 3.14159265358979323846;
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    public double getArea() {
        return PI * radius * radius;
    }
}
```

**Fördelar:**

- Endast en kopia av konstanten i minnet
- Tydligt visar att värdet är detsamma för alla instanser
- Kan nås som `Circle.PI` utan att skapa ett objekt

---

## 📦 SEKTION 8.14: PACKAGE ACCESS

### Vad är package access?

Om ingen access modifier (public eller private) anges för en metod eller variabel, får den **package access** (paketåtkomst). Detta innebär att medlemmen kan nås av alla klasser i samma paket.

**Exempel:**

java

```java
// PackageData.java
class PackageData {
    int number = 0;           // package access
    String string = "Hello";  // package access
    
    public String toString() {
        return String.format("number: %d; string: %s", number, string);
    }
}

// PackageDataTest.java (i samma paket)
public class PackageDataTest {
    public static void main(String[] args) {
        PackageData packageData = new PackageData();
        
        // Direkt åtkomst till package-access medlemmar
        packageData.number = 77;
        packageData.string = "Goodbye";
        
        System.out.println(packageData);
    }
}
```

### Negativa aspekter av package access

**Varför är detta problematiskt?**

- Bryter inkapslingsprincipen
- Gör kod svårare att underhålla
- Kan leda till oväntade beroenden mellan klasser
- Svårare att spåra var variabler modifieras

**Best practice:** Använd alltid explicit access modifiers (public eller private). Detta gör din avsikt tydlig och förhindrar oavsiktlig package access.

---

## 💰 SEKTION 8.15: BIGDECIMAL FÖR MONETÄRA BERÄKNINGAR

### Problemet med double för pengar

Typen double använder flyttalsrepresentation som kan leda till avrundningsfel. Detta är oacceptabelt för finansiella beräkningar där precision är kritisk.

**Problem:**

java

```java
double amount = 0.1 + 0.2;
System.out.println(amount);  // Kan skriva ut 0.30000000000000004
```

### BigDecimal-klassen

**BigDecimal** (i paketet java.math) ger exakta beräkningar utan avrundningsfel. Alla beräkningar är exakta om du inte explicit specificerar avrundning.

### Skapa BigDecimal-objekt

**valueOf() metoden:**

java

```java
BigDecimal principal = BigDecimal.valueOf(1000.0);
BigDecimal rate = BigDecimal.valueOf(0.05);
```

**Konstanter:** BigDecimal innehåller konstanter för vanliga värden:

- `BigDecimal.ZERO` (0)
- `BigDecimal.ONE` (1)
- `BigDecimal.TEN` (10)

### BigDecimal-operationer

BigDecimal är immutable (oföränderlig), så operationer returnerar nya BigDecimal-objekt:

java

```java
// Addition
BigDecimal sum = value1.add(value2);

// Subtraktion
BigDecimal difference = value1.subtract(value2);

// Multiplikation
BigDecimal product = value1.multiply(value2);

// Division (kräver ofta avrundningsläge)
BigDecimal quotient = value1.divide(value2, RoundingMode.HALF_UP);

// Exponentiering
BigDecimal result = base.pow(exponent);
```

### Ränteberäkningsexempel

java

```java
import java.math.BigDecimal;
import java.text.NumberFormat;

public class Interest {
    public static void main(String[] args) {
        BigDecimal principal = BigDecimal.valueOf(1000.0);
        BigDecimal rate = BigDecimal.valueOf(0.05);
        
        System.out.printf("%s%20s%n", "Year", "Amount on deposit");
        
        for (int year = 1; year <= 10; year++) {
            // Beräkna (1 + rate)^year
            BigDecimal rateAddOne = BigDecimal.ONE.add(rate);
            BigDecimal amount = principal.multiply(
                rateAddOne.pow(year));
            
            // Formatera som valuta
            NumberFormat currency = NumberFormat.getCurrencyInstance();
            System.out.printf("%4d%20s%n", year, 
                currency.format(amount));
        }
    }
}
```

### Avrundning och MathContext

**MathContext** (java.math.MathContext) specificerar precision och avrundningsläge:

java

```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;

// Banker's rounding (avrundar till närmaste jämna tal)
MathContext mc = new MathContext(34, RoundingMode.HALF_EVEN);
BigDecimal result = value1.divide(value2, mc);

// Specificera antal decimaler
BigDecimal rounded = value.setScale(2, RoundingMode.HALF_UP);
```

### Formatera BigDecimal för utskrift

**NumberFormat** klassen formaterar numeriska värden som lokalspecifika strängar:

java

```java
NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
String formatted = currencyFormatter.format(bigDecimalValue);
// Exempel på output: "$1,234.56" (för amerikansk lokal)
```

### När ska du använda BigDecimal?

**Använd BigDecimal för:**

- Monetära beräkningar
- Finansiella applikationer
- Situationer där exakthet är kritisk
- När avrundningsfel är oacceptabla

**Använd double/float för:**

- Vetenskapliga beräkningar där approximationer är acceptabla
- Grafik och spel
- Situationer där prestanda är mer kritisk än exakthet

---

## 🎨 ÖVRIGA VIKTIGA KONCEPT

### Object.toString() metoden

Alla klasser i Java ärver från Object-klassen, vilket innebär att alla objekt har en toString() metod. Om du inte överlagrar (override) denna metod får du en standardimplementation som returnerar klassnamnet följt av objektets hashkod.

**Best practice:** Överlagra alltid toString() i dina klasser för att ge en meningsfull strängrepresentation:

java

```java
@Override
public String toString() {
    return String.format("Employee[name=%s %s, ssn=%s]", 
        firstName, lastName, socialSecurityNumber);
}
```

### Immutability (oföränderlighet)

En **immutable class** (oföränderlig klass) är en klass vars objekt inte kan ändras efter att de skapats. För att skapa en immutable class:

1. Deklarera alla instansvariabler som private och final
2. Tillhandahåll ingen set-metoder
3. Om klassen innehåller referenser till föränderliga objekt, returnera kopior i get-metoder

**Fördelar:**

- Trådsäkra (thread-safe)
- Enklare att resonera om
- Kan användas som nycklar i HashMap

**Exempel:**

java

```java
public final class ImmutablePoint {
    private final int x;
    private final int y;
    
    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() { return x; }
    public int getY() { return y; }
    
    // Ingen set-metoder!
}
```

---

## 📊 SAMMANFATTNING AV NYCKELBEGREPP

### Access control och inkapsling

- **private**: Tillgänglig endast inom klassen
- **package access**: Tillgänglig för alla klasser i samma paket
- **public**: Tillgänglig överallt
- **Inkapsling**: Dölj implementationsdetaljer, exponera endast nödvändiga gränssnitt

### Konstruktor-relaterade koncept

- **Default constructor**: Skapas automatiskt om ingen konstruktor finns
- **No-argument constructor**: Explicit deklarerad konstruktor utan parametrar
- **Constructor overloading**: Flera konstruktorer med olika signaturer
- **Constructor chaining**: Använda this() för att anropa andra konstruktorer

### Metod-typer

- **Mutator methods** (set-metoder): Ändrar objektets tillstånd
- **Accessor methods** (get-metoder): Returnerar information utan att ändra tillstånd
- **Predicate methods**: Returnerar boolean-värden
- **static methods**: Kan anropas utan objekt

### Klassemedlemmar

- **Instance variables**: Varje objekt har sin egen kopia
- **static variables**: Delas mellan alla objekt av klassen
- **final variables**: Kan inte ändras efter initialisering
- **static final variables**: Klasskonstanter

### Avancerade koncept

- **this**: Referens till det aktuella objektet
- **Composition**: Has-a relationer mellan klasser
- **enum**: Uppsättningar av namngivna konstanter
- **Garbage collection**: Automatisk minneshantering
- **BigDecimal**: Exakta decimalberäkningar

---

## 🎯 DESIGNPRINCIPER

### Principle of least privilege

Ge endast den åtkomst som faktiskt behövs. Använd private som standard och exponera endast vad som är absolut nödvändigt.

### Information hiding

Dölj implementationsdetaljer bakom ett publikt gränssnitt. Klienter ska inte behöva veta hur en klass implementeras internt.

### Separation of concerns

Varje klass ska ha ett tydligt, väl definierat ansvar. Undvik klasser som försöker göra för mycket.

### DRY (Don't Repeat Yourself)

Använd constructor chaining och hjälpmetoder för att undvika kodduplicering.

---

Detta kapitel ger dig verktygen för att designa professionella, robusta klasser i Java. Koncepten bygger på varandra och skapar en solid grund för objektorienterad programmering. Genom att förstå och tillämpa dessa principer kan du skapa kod som är lättare att underhålla, mer flexibel och mindre benägen för buggar.