# 📚 Kapitel 7: Introduction to Classes and Objects
## En Omfattande Sammanfattning

---

## 🎯 KAPITELÖVERSIKT

Detta kapitel introducerar objektorienterad programmering i Java genom att visa hur du skapar egna klasser och objekt. Du kommer att lära dig skillnaden mellan en klass (ritningen) och ett objekt (det faktiska exemplaret), samt hur du bygger klasser med instansvariabler och metoder.

**Centralt tema:** Från att använda färdiga klasser till att skapa dina egna anpassade klasser som blir nya datatyper i ditt program.

---

## 📖 SEKTION 7.1: INTRODUKTION

### Varför är detta viktigt?

Hittills har du använt färdiga klasser som någon annan har skapat - till exempel:
- `System.out` för att skriva ut text
- `Scanner` för att läsa input från användaren
- `String` för att hantera text

Nu ska du lära dig att skapa dina egna klasser! Detta är en av Java's största styrkor - språket är **extensible** (utbyggbart), vilket betyder att du kan skapa nya typer efter behov.

### Kapitlets struktur

Kapitlet innehåller flera viktiga case studies:
1. **Account-klassen** - En bankkonto-klass som modellerar verkliga bankoperationer
2. **Card shuffling** - Simulation av kortblandning
3. **GradeBook (två versioner)** - Hantering av studentbetyg med arrayer

---

## 🏗️ SEKTION 7.2: INSTANCE VARIABLES, SET METHODS OCH GET METHODS

### 7.2.1 Account-klassen med Instance Variable

#### Vad är en instance variable?

Tänk på ett objekt som en låda som bär med sig information under hela sin livstid. **Instance variables** (instansvariabler) är den information som objektet "kommer ihåg".

**Exempel:** Om vi har en Account-klass för bankkonton, så skulle varje konto-objekt behöva komma ihåg sitt namn och sitt saldo.

```java
public class Account {
    private String name; // instance variable
    
    // Metoder följer här...
}
```

#### 🔑 Nyckelbegrepp: Instance Variable
- Deklareras **inuti klassen** men **utanför metoderna**
- Varje objekt får sin **egen kopia** av varje instance variable
- Existerar så länge objektet existerar (från skapelse till destruktion)
- Initialiseras automatiskt till standardvärden om inget annat anges

#### Access Modifiers: public vs private

**`private`** - Det viktigaste ordet för instance variables!
- Gör variabeln tillgänglig **endast** för metoderna i samma klass
- Andra klasser kan inte direkt komma åt den
- Detta kallas **information hiding** (informationsgömning)

**`public`** - För metoder som ska vara tillgängliga utifrån
- Låter andra klasser använda metoden
- Används för metoderna som är klassens "gränssnitt" mot omvärlden

```
┌─────────────────────────────┐
│     Account (klass)         │
│                             │
│  PRIVATE (gömd inuti):      │
│    - name                   │
│    - balance                │
│                             │
│  PUBLIC (tillgänglig):      │
│    + setName()              │
│    + getName()              │
│    + deposit()              │
└─────────────────────────────┘
```

#### Set och Get Methods - Varför behövs de?

Eftersom instance variables är `private`, behöver vi metoder för att komma åt dem utifrån:

**Get-metod (accessor):** Hämtar värdet
```java
public String getName() {
    return name; // returnerar värdet av instance variable
}
```

**Set-metod (mutator):** Ändrar värdet
```java
public void setName(String name) {
    this.name = name; // tilldelar nytt värde
}
```

#### 🔍 Viktigt: this-referensen

När en parameter har samma namn som en instance variable använder vi `this` för att skilja dem åt:

```java
public void setName(String name) {
    this.name = name;
    //     ↑       ↑
    //  instance  parameter
    //  variable
}
```

**Vad betyder `this`?**
- `this` är en referens till det aktuella objektet
- `this.name` betyder "detta objektets name-variable"
- `name` (utan this) betyder parametern

**Visualisering:**
```
┌────────────────────────┐
│  myAccount-objektet    │
│                        │
│  name = "Jane"    ←────┼── this.name pekar hit
│                        │
└────────────────────────┘

När vi anropar: myAccount.setName("John")
  → Parametern 'name' får värdet "John"
  → this.name (objektets variable) får värdet "John"
```

---

### 7.2.2 AccountTest - Driver Class

En **driver class** är en klass vars enda syfte är att testa en annan klass. Den innehåller `main`-metoden och skapar objekt för att demonstrera funktionalitet.

#### Skapa och använda ett objekt - Steg för steg

**Steg 1: Deklarera referensvariabeln**
```java
Account myAccount;
```
Detta skapar en variabel som **kan** referera till ett Account-objekt, men objektet finns inte än!

**Steg 2: Skapa objektet med `new`**
```java
myAccount = new Account();
```
- `new` ber systemet om minne för objektet
- `Account()` anropar konstruktorn som initialiserar objektet
- Referensen till det nya objektet lagras i `myAccount`

**Steg 3: Använda objektet**
```java
myAccount.setName("Jane Green");
String theName = myAccount.getName();
```

#### 📊 Visualisering av objektskapelse

```
Före: myAccount = null
       ┌──────────────┐
       │  myAccount   │
       │     null     │
       └──────────────┘

Efter: myAccount = new Account();
       ┌──────────────┐           ┌─────────────────┐
       │  myAccount   │──────────→│  Account-objekt │
       │              │           │  name = null    │
       └──────────────┘           └─────────────────┘

Efter: myAccount.setName("Jane");
       ┌──────────────┐           ┌─────────────────┐
       │  myAccount   │──────────→│  Account-objekt │
       │              │           │  name = "Jane"  │
       └──────────────┘           └─────────────────┘
```

#### Scanner input - Läsa användarinput

```java
Scanner input = new Scanner(System.in);
System.out.print("Enter name: ");
String theName = input.nextLine(); // Läser hela raden
myAccount.setName(theName);
```

**Skillnad mellan nextLine() och next():**
- `nextLine()` - Läser alla tecken fram till newline (Enter-knapp)
- `next()` - Läser till första whitespace (mellanslag, tab, newline)

---

### 7.2.3 Kompilera och köra program med flera klasser

När du har flera klasser i samma mapp:

```bash
# Kompilera alla Java-filer på en gång:
javac *.java

# Eller individuellt:
javac Account.java AccountTest.java

# Kör programmet (den klass som har main):
java AccountTest
```

#### Default Package

Klasser i samma mapp tillhör automatiskt **default package**. De kan använda varandra utan `import`-satser!

---

### 7.2.4 UML Class Diagram

**UML (Unified Modeling Language)** är ett visuellt språk för att modellera klasser och objekt.

#### Struktur av ett UML-klassdiagram

```
┌─────────────────────────┐
│      Account            │  ← Klassnamn (fet stil)
├─────────────────────────┤
│  - name: String         │  ← Attribut (instance variables)
├─────────────────────────┤    - = private
│  + setName(name:String):void  │  ← Operationer (metoder)
│  + getName():String     │    + = public
└─────────────────────────┘
```

**Symboler:**
- **Minus (-)** före attribut = `private`
- **Plus (+)** före operation = `public`
- **Kolon (:)** används för att visa typ
- **Void** visas inte för metoder som inte returnerar något (i vissa varianter)

---

### 7.2.5 Varför använder vi inte static för Account's metoder?

**Viktigt att förstå:**

```java
// static metod (som main):
public static void main(String[] args) {
    // Kan köras utan att skapa ett objekt
}

// Instance metod (som getName):
public String getName() {
    return name; // Behöver ett objekt att arbeta med
}
```

- **`static` metoder** tillhör klassen själv - ingen objektspecifik data
- **Instance metoder** tillhör varje enskilt objekt - använder objektets data

`main` är static eftersom JVM måste kunna köra den utan att först skapa ett objekt!

---

### 7.2.6 Software Engineering med private och public

#### Fördelar med information hiding

**🛡️ Inkapsling (Encapsulation):**

Tänk på ett konto-objekt som en säker låda:

```
      ╔════════════════════════════╗
      ║   PUBLIC INTERFACE         ║
      ║   +setName()               ║
      ║   +getName()               ║
      ║   +deposit()               ║
      ╠════════════════════════════╣
      ║   PROTECTED DATA           ║
      ║   -name                    ║
      ║   -balance                 ║
      ╚════════════════════════════╝
```

**Varför är detta bra?**

1. **Kontroll:** Set-metoder kan validera data innan den sparas
   ```java
   public void setBalance(double balance) {
       if (balance >= 0) {  // Validering!
           this.balance = balance;
       } else {
           System.out.println("Balance cannot be negative!");
       }
   }
   ```

2. **Flexibilitet:** Du kan ändra intern implementation utan att påverka användare
   - Kanske vill du senare lagra namnet som förnamn + efternamn separat
   - Get-metoden kan fortfarande returnera hela namnet
   - Användare märker ingen skillnad!

3. **Säkerhet:** Ingen kan sätta ogiltiga värden direkt

---

## 🎯 SEKTION 7.3: DEFAULT OCH EXPLICIT INITIALISERING

### Automatisk initialisering av instance variables

**Skillnad mellan lokala variabler och instance variables:**

#### Lokala variabler (i metoder):
```java
public void someMethod() {
    int x;  // INTE automatiskt initialiserad!
    // System.out.println(x);  // ← FEL! Compile error
}
```

#### Instance variables:
```java
public class Example {
    private int number;      // Automatiskt 0
    private double decimal;  // Automatiskt 0.0
    private boolean flag;    // Automatiskt false
    private String text;     // Automatiskt null
}
```

### 📋 Tabell över defaultvärden för primitiva typer

| Datatyp   | Defaultvärde |
|-----------|--------------|
| `byte`    | 0            |
| `short`   | 0            |
| `int`     | 0            |
| `long`    | 0L           |
| `float`   | 0.0f         |
| `double`  | 0.0          |
| `char`    | '\u0000'     |
| `boolean` | false        |

### Reference types (objekt):
- **Alla** reference types får defaultvärdet `null`
- `null` betyder "refererar inte till något objekt"

### Explicit initialisering

Du kan ge egna startvärden direkt i deklarationen:

```java
public class Account {
    private String name = "Unknown";     // Explicit värde
    private double balance = 0.0;        // Explicit värde
    private int accountNumber;           // Får default 0
}
```

---

## 🏗️ SEKTION 7.4: CONSTRUCTORS

### Vad är en Constructor?

En **constructor** är en speciell metod som:
- Har **samma namn som klassen**
- **Ingen returtyp** (inte ens `void`)
- Anropas **automatiskt** när du skapar ett objekt med `new`
- Används för att **initialisera** objektets instance variables

### 7.4.1 Deklarera en Constructor

```java
public class Account {
    private String name;
    
    // Constructor
    public Account(String name) {
        this.name = name;
    }
    
    // Metoder...
}
```

**Hur den fungerar:**
```java
Account account1 = new Account("Jane Green");
//                     ↑
//              Anropar constructor med "Jane Green"
```

### Visualisering av constructor-anrop

```
1. Deklaration:
   Account account1;
   
   account1 → null


2. Anrop av new Account("Jane"):
   - Minne allokeras för nytt Account-objekt
   - Constructor körs automatiskt
   - name initialiseras till "Jane"
   
   account1 → [Account: name="Jane"]


3. Nu är objektet redo att användas!
```

---

### 7.4.2 Default Constructor vs Custom Constructor

#### Default Constructor

Om du **inte** skriver någon constructor, skapar Java automatiskt en **default constructor**:

```java
public class Account {
    private String name;
    // Ingen constructor skriven
}

// Java skapar automatiskt:
// public Account() { }
```

Då kan du skapa objekt så här:
```java
Account myAccount = new Account(); // Tomma parenteser
```

#### När försvinner default constructor?

**VIKTIGT:** Så fort du skriver **någon** egen constructor, försvinner default constructor!

```java
public class Account {
    private String name;
    
    public Account(String name) {  // Custom constructor
        this.name = name;
    }
}

// NU FUNGERAR INTE:
// Account acc = new Account();  // ← FEL! Finns ingen sådan constructor

// MEN DETTA FUNGERAR:
Account acc = new Account("Jane"); // ✓ OK
```

---

### 7.4.3 UML med Constructor

```
┌─────────────────────────────────────┐
│           Account                   │
├─────────────────────────────────────┤
│ - name: String                      │
├─────────────────────────────────────┤
│ «constructor»                       │  ← Markerar constructor
│ + Account(name: String)             │
│ + setName(name: String): void       │
│ + getName(): String                 │
└─────────────────────────────────────┘
```

**Observera:** Constructors listas före vanliga metoder i UML.

---

## 💰 SEKTION 7.5: ACCOUNT CLASS MED BALANCE

Nu utökar vi Account-klassen med ett saldo (balance).

### 7.5.1 Account Class med balance Instance Variable

```java
public class Account {
    private String name;
    private double balance;  // Ny instance variable
    
    // Constructor med två parametrar
    public Account(String name, double balance) {
        this.name = name;
        
        // Validering av balance
        if (balance > 0.0) {
            this.balance = balance;
        }
        // Om balance <= 0, förblir balance 0.0 (default)
    }
    
    // Metod för att sätta in pengar
    public void deposit(double depositAmount) {
        if (depositAmount > 0.0) {
            balance = balance + depositAmount;
        }
    }
    
    public double getBalance() {
        return balance;
    }
    
    // Övriga metoder...
}
```

#### 🎯 Nyckelkoncept: Validering i Constructor

**Varför validera?**
- Vi vill inte att ett konto ska ha negativt saldo från start
- Om ogiltig data kommer in, använd default (0.0)
- Detta är ett exempel på **defensive programming**

**Flödesschema för validation:**
```
      ┌───────────────────┐
      │  Skapa Account    │
      │  balance = -100   │
      └─────────┬─────────┘
                │
                ↓
      ┌─────────────────────┐
      │  if (balance > 0.0) │
      └─────────┬───────────┘
                │
         ┌──────┴──────┐
         ↓             ↓
      FALSE          TRUE
         │             │
         ↓             ↓
    balance = 0.0   balance = -100
    (default)       this.balance = -100
```

---

### 7.5.2 Använda den utökade Account-klassen

```java
public class AccountTest {
    public static void main(String[] args) {
        Account account1 = new Account("Jane Green", 50.00);
        Account account2 = new Account("John Blue", -7.53);
        
        // Visa initial balances
        System.out.printf("%s balance: $%.2f%n", 
            account1.getName(), account1.getBalance());
        // Output: Jane Green balance: $50.00
        
        System.out.printf("%s balance: $%.2f%n",
            account2.getName(), account2.getBalance());
        // Output: John Blue balance: $0.00  (pga validering!)
        
        // Läs in och sätt in belopp
        Scanner input = new Scanner(System.in);
        System.out.print("Enter deposit amount for account1: ");
        double depositAmount = input.nextDouble();
        
        account1.deposit(depositAmount);
        
        // Visa uppdaterade balances
        System.out.printf("%s balance: $%.2f%n",
            account1.getName(), account1.getBalance());
    }
}
```

---

## 🎴 SEKTION 7.6: CASE STUDY - CARD SHUFFLING OCH DEALING

Detta är en mer avancerad case study som visar hur man kan modellera kortspel.

### Koncept

**Problem:** Simulera en kortlek med 52 kort som ska blandas och delas ut.

**Lösning:** Använd en array för att representera korten och implementera shuffle-algoritm.

### Representation av kort

```java
// Varje kort representeras av två delar:
String[] faces = {"Ace", "Deuce", "Three", "Four", "Five", "Six",
                  "Seven", "Eight", "Nine", "Ten", "Jack", "Queen", "King"};
String[] suits = {"Hearts", "Diamonds", "Clubs", "Spades"};
```

**Strukturering:**
- 13 valörer × 4 färger = 52 kort
- Använd tvådimensionell array eller Card-objekt

### Blandningsalgoritm

**Fisher-Yates shuffle** (eller liknande):
```
För varje position i från 0 till 51:
    1. Generera slumptal mellan i och 51
    2. Byt plats på element i och det slumpade elementet
```

**Visualisering:**
```
Start: [Ace♥, 2♥, 3♥, ..., K♠]

Iteration 1 (i=0):
  Slumptal = 25
  Byt plats på position 0 och 25
  [7♦, 2♥, 3♥, ..., Ace♥, ..., K♠]

Iteration 2 (i=1):
  Slumptal = 48
  Byt plats på position 1 och 48
  [7♦, Q♠, 3♥, ..., 2♥, ..., K♠]

... och så vidare för alla 52 positioner
```

### Pedagogisk poäng

Detta exempel visar:
- **Arrays** för att lagra många relaterade objekt
- **Random-klassen** för slumptalsgenerering
- **Algoritmer** för att manipulera data
- **Modellering** av verkliga koncept i kod

---

## 📊 SEKTION 7.7: CASE STUDY - GRADEBOOK MED EN ARRAY

### Problem

Skapa en klass som kan:
- Lagra betyg för 10 studenter på ett prov
- Beräkna klassgenomsnittet
- Hitta lägsta och högsta betyget
- Visa betygsdistribution

### 7.7.1 GradeBook-klassen

```java
public class GradeBook {
    private String courseName;
    private int[] grades;  // Array av betyg
    
    // Constructor
    public GradeBook(String courseName, int[] grades) {
        this.courseName = courseName;
        this.grades = grades;  // Lagrar referens till arrayen
    }
    
    // Visa alla betyg
    public void outputGrades() {
        System.out.println("The grades are:");
        for (int student = 0; student < grades.length; student++) {
            System.out.printf("Student %d: %d%n", student + 1, grades[student]);
        }
    }
    
    // Beräkna genomsnitt
    public double getAverage() {
        int total = 0;
        
        for (int grade : grades) {  // Enhanced for
            total += grade;
        }
        
        return (double) total / grades.length;
    }
    
    // Hitta lägsta betyget
    public int getMinimum() {
        int lowGrade = grades[0];  // Anta första är lägst
        
        for (int grade : grades) {
            if (grade < lowGrade) {
                lowGrade = grade;
            }
        }
        
        return lowGrade;
    }
    
    // Hitta högsta betyget
    public int getMaximum() {
        int highGrade = grades[0];  // Anta första är högst
        
        for (int grade : grades) {
            if (grade > highGrade) {
                highGrade = grade;
            }
        }
        
        return highGrade;
    }
    
    // Visa betygsdistribution som stapeldiagram
    public void outputBarChart() {
        System.out.println("Grade distribution:");
        
        int[] frequency = new int[11];  // 0-9, 10-19, ..., 100
        
        for (int grade : grades) {
            ++frequency[grade / 10];
        }
        
        for (int count = 0; count < frequency.length; count++) {
            if (count == 10) {
                System.out.printf("%5d: ", 100);
            } else {
                System.out.printf("%02d-%02d: ", count * 10, count * 10 + 9);
            }
            
            for (int stars = 0; stars < frequency[count]; stars++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
    
    // Huvudmetod som kör alla analyser
    public void processGrades() {
        outputGrades();
        System.out.printf("%nClass average is %.2f%n", getAverage());
        System.out.printf("Lowest grade is %d%nHighest grade is %d%n%n",
            getMinimum(), getMaximum());
        outputBarChart();
    }
}
```

### Viktiga koncept i detta exempel

#### 1️⃣ Array som instance variable
```java
private int[] grades;
```
- Arrayen lagras som en **referens**
- Alla metoder i klassen kan komma åt den
- Längden kan variera mellan olika GradeBook-objekt

#### 2️⃣ Enhanced for loop
```java
for (int grade : grades) {
    total += grade;
}
```
Detta betyder: "För varje int som vi kallar grade i grades-arrayen..."

#### 3️⃣ Algoritm för att hitta min/max
```
1. Anta att första elementet är min/max
2. Gå igenom resten av arrayen
3. Om du hittar något mindre/större, uppdatera min/max
```

#### 4️⃣ Frequency array för distribution
```java
int[] frequency = new int[11];  // Räknare för varje intervall

for (int grade : grades) {
    ++frequency[grade / 10];  // Vilket intervall tillhör betyget?
}
```

**Exempel:**
```
Betyg: 87 → 87 / 10 = 8 → frequency[8]++
Betyg: 100 → 100 / 10 = 10 → frequency[10]++
Betyg: 65 → 65 / 10 = 6 → frequency[6]++
```

---

### 7.7.2 Test Class

```java
public class GradeBookTest {
    public static void main(String[] args) {
        int[] gradesArray = {87, 68, 94, 100, 83, 78, 85, 91, 76, 87};
        
        GradeBook myGradeBook = new GradeBook(
            "CS101 Introduction to Java Programming", gradesArray);
        
        System.out.printf("Welcome to the grade book for%n%s%n%n",
            myGradeBook.getCourseName());
        
        myGradeBook.processGrades();
    }
}
```

**Output:**
```
Welcome to the grade book for
CS101 Introduction to Java Programming

The grades are:
Student 1: 87
Student 2: 68
Student 3: 94
Student 4: 100
Student 5: 83
Student 6: 78
Student 7: 85
Student 8: 91
Student 9: 76
Student 10: 87

Class average is 84.90
Lowest grade is 68
Highest grade is 100

Grade distribution:
00-09:
10-19:
20-29:
30-39:
40-49:
50-59:
60-69: *
70-79: **
80-89: ****
90-99: **
100: *
```

---

## 📈 SEKTION 7.8: CASE STUDY - GRADEBOOK MED TVÅDIMENSIONELL ARRAY

### Utökat problem

Nu vill vi hantera:
- 10 studenter
- 3 prov vardera
- Beräkna genomsnitt per student
- Hitta lägsta/högsta totalt
- Visa distribution

### Tvådimensionella arrays - Koncept

En 2D-array är en "array av arrays" - tänk på den som en tabell:

```
           Prov 1   Prov 2   Prov 3
Student 1  [87]     [96]     [70]
Student 2  [68]     [87]     [90]
Student 3  [94]     [100]    [90]
...
```

### Deklaration och initialisering

```java
int[][] grades = {
    {87, 96, 70},    // Student 1's betyg
    {68, 87, 90},    // Student 2's betyg
    {94, 100, 90},   // Student 3's betyg
    // ... fler studenter
};
```

### Accessera element

```java
grades[0][0]  // Student 1, Prov 1 → 87
grades[0][1]  // Student 1, Prov 2 → 96
grades[1][0]  // Student 2, Prov 1 → 68
```

**Generellt:**
```java
grades[rad][kolumn]
grades[studentIndex][provIndex]
```

---

### 7.8.1 GradeBook med 2D Array

```java
public class GradeBook {
    private String courseName;
    private int[][] grades;  // 2D array
    
    public GradeBook(String courseName, int[][] grades) {
        this.courseName = courseName;
        this.grades = grades;
    }
    
    // Visa alla betyg i tabellformat
    public void outputGrades() {
        System.out.println("The grades are:");
        System.out.print("            ");  // Mellanslag för rubrik
        
        // Skriv ut kolumnrubriker (Test 1, Test 2, Test 3)
        for (int test = 0; test < grades[0].length; test++) {
            System.out.printf("Test %d  ", test + 1);
        }
        System.out.println("Average");
        
        // För varje student
        for (int student = 0; student < grades.length; student++) {
            System.out.printf("Student %2d", student + 1);
            
            // Skriv ut studentens betyg för varje prov
            for (int test : grades[student]) {
                System.out.printf("%8d", test);
            }
            
            // Beräkna och visa studentens genomsnitt
            double average = getAverage(grades[student]);
            System.out.printf("%9.2f%n", average);
        }
    }
    
    // Beräkna genomsnitt för EN students betyg
    public double getAverage(int[] setOfGrades) {
        int total = 0;
        
        for (int grade : setOfGrades) {
            total += grade;
        }
        
        return (double) total / setOfGrades.length;
    }
    
    // Hitta lägsta betyget bland ALLA betyg
    public int getMinimum() {
        int lowGrade = grades[0][0];
        
        for (int[] studentGrades : grades) {
            for (int grade : studentGrades) {
                if (grade < lowGrade) {
                    lowGrade = grade;
                }
            }
        }
        
        return lowGrade;
    }
    
    // Liknande för getMaximum och outputBarChart...
    
    public void processGrades() {
        outputGrades();
        System.out.printf("%nLowest grade is %d%n", getMinimum());
        System.out.printf("Highest grade is %d%n%n", getMaximum());
        outputBarChart();
    }
}
```

### Nyckelkoncept: Nestlade loopar

För att gå igenom alla element i en 2D-array använder vi **nestlade loopar**:

```java
// Traditionell for-loop version
for (int rad = 0; rad < grades.length; rad++) {
    for (int kol = 0; kol < grades[rad].length; kol++) {
        System.out.printf("%d ", grades[rad][kol]);
    }
    System.out.println();  // Ny rad efter varje studentrad
}

// Enhanced for-loop version
for (int[] studentGrades : grades) {  // För varje studentrad
    for (int grade : studentGrades) {  // För varje betyg i den raden
        System.out.printf("%d ", grade);
    }
    System.out.println();
}
```

**Visualisering av traversering:**
```
Yttre loop (rad 0):  [87, 96, 70]
  Inre loop: 87 → 96 → 70

Yttre loop (rad 1):  [68, 87, 90]
  Inre loop: 68 → 87 → 90

Yttre loop (rad 2):  [94, 100, 90]
  Inre loop: 94 → 100 → 90
```

---

### 7.8.2 Test Program

```java
public class GradeBookTest {
    public static void main(String[] args) {
        int[][] gradesArray = {
            {87, 96, 70},
            {68, 87, 90},
            {94, 100, 90},
            {100, 81, 82},
            {83, 65, 85},
            {78, 87, 65},
            {85, 75, 83},
            {91, 94, 100},
            {76, 72, 84},
            {87, 93, 73}
        };
        
        GradeBook myGradeBook = new GradeBook(
            "CS101 Introduction to Java Programming", gradesArray);
        
        System.out.printf("Welcome to the grade book for%n%s%n%n",
            myGradeBook.getCourseName());
        
        myGradeBook.processGrades();
    }
}
```

---

## 🎓 SEKTION 7.9: WRAP-UP & SUMMERING

### Vad du har lärt dig i detta kapitel

#### 1️⃣ **Klasser och Objekt**
- En klass är en ritning (blueprint) för objekt
- Ett objekt är en instans av en klass
- Varje objekt har sina egna kopior av instance variables

#### 2️⃣ **Instance Variables**
- Deklareras i klassen men utanför metoderna
- Automatiskt initialiserade till defaultvärden
- Bör vara `private` för inkapsling

#### 3️⃣ **Metoder**
- **Set-metoder** (mutators): Ändrar instance variables
- **Get-metoder** (accessors): Hämtar värden
- Bör vara `public` för att användas utifrån

#### 4️⃣ **Constructors**
- Speciella metoder för att initialisera objekt
- Samma namn som klassen
- Anropas automatiskt vid `new`
- Ingen returtyp

#### 5️⃣ **Information Hiding**
- Göm implementation details med `private`
- Exponera interface med `public` metoder
- Ger kontroll, flexibilitet och säkerhet

#### 6️⃣ **Arrays som Instance Variables**
- Kan lagra många relaterade värden
- En eller tvådimensionella arrays
- Användbart för att modellera listor och tabeller

---

## 🔑 NYCKELTERMER OCH DEFINITIONER

| Term | Definition |
|------|------------|
| **Class** | Ritning för att skapa objekt; definierar attribut och beteenden |
| **Object** | En instans av en klass; har eget tillstånd och identitet |
| **Instance Variable** | Variabel som tillhör varje objekt; lagrar objektets tillstånd |
| **Instance Method** | Metod som arbetar med ett objekts data; kallas via objektreferens |
| **Constructor** | Speciell metod för att initialisera nytt objekt |
| **`this` reference** | Referens till det aktuella objektet |
| **Access Modifier** | Nyckelord som kontrollerar synlighet (`public`, `private`) |
| **Encapsulation** | Inkapsling av data och metoder i en sammanhållen enhet |
| **Information Hiding** | Göma implementation details med `private` |
| **Driver Class** | Klass som testar en annan klass; innehåller `main` |
| **Default Constructor** | Constructor utan parametrar som skapas automatiskt om ingen annan finns |
| **UML** | Unified Modeling Language; visuellt språk för att modellera klasser |
| **`null`** | Värde som betyder "refererar inte till något objekt" |

---

## 💡 VIKTIGA PROGRAMMERINGSPRINCIPER

### 🎯 Good Programming Practices

1. **Namngivning:**
   - Klasser: Börja med stor bokstav (`Account`, `GradeBook`)
   - Metoder och variabler: Börja med liten bokstav (`getName`, `balance`)
   - Använd camelCase

2. **Organisation:**
   - En public klass per .java-fil
   - Instance variables först i klassen
   - Sedan constructors
   - Sedan metoder

3. **Dokumentation:**
   - Kommentera syftet med varje klass
   - Förklara komplexa algoritmer

### 🛡️ Software Engineering Observations

1. **Inkapsling:**
   - Alltid `private` instance variables
   - `public` get/set-metoder för kontrollerad access

2. **Validering:**
   - Validera data i set-metoder och constructors
   - Förhindra ogiltiga tillstånd

3. **Separation of Concerns:**
   - Test-kod i separat driver class
   - Data och beteende tillsammans i klassen

### ⚠️ Common Pitfalls (Vanliga Fallgropar)

1. **Glömma `this`** när parameter har samma namn som instance variable
2. **Försöka använda default constructor** efter att ha skapat en custom constructor
3. **Förväxla lokala variabler med instance variables**
4. **Glömma validera indata** i constructors och set-metoder
5. **Göra instance variables `public`** istället för att använda get/set-metoder

---

## 📚 VIKTIGA SAMBAND OCH JÄMFÖRELSER

### Instance Variable vs Local Variable

| Aspekt | Instance Variable | Local Variable |
|--------|-------------------|----------------|
| **Var deklarerad** | I klassen, utanför metoder | I metod eller block |
| **Livstid** | Så länge objektet existerar | Så länge metoden kör |
| **Scope** | Alla metoder i klassen | Endast metoden/blocket |
| **Default värde** | Automatiskt initialiserad | INTE automatiskt initialiserad |
| **Synlighet** | Kan vara private/public | Alltid privat till metoden |

### Constructor vs Method

| Aspekt | Constructor | Vanlig Metod |
|--------|-------------|--------------|
| **Namn** | Exakt som klassen | Valfritt namn |
| **Returtyp** | Ingen (inte ens void) | Måste ha returtyp |
| **Syfte** | Initialisera nytt objekt | Utföra operation |
| **När kallad** | Automatiskt vid `new` | Explicit med `.metodnamn()` |
| **Kan överlagras** | Ja | Ja |

### Primitive Types vs Reference Types

| Aspekt | Primitive Type | Reference Type |
|--------|---------------|----------------|
| **Exempel** | int, double, boolean | String, Account, arrays |
| **Lagring** | Direkt värde | Referens (adress) till objekt |
| **Default** | 0, 0.0, false, etc. | null |
| **Jämförelse** | == jämför värden | == jämför referenser |
| **Null?** | Kan inte vara null | Kan vara null |

---

## 🧠 MENTALA MODELLER FÖR FÖRSTÅELSE

### Klass som Ritning

```
┌─────────────────────────────────┐
│       RITNING (Account)         │
│                                 │
│  Blueprint för bankkonton:      │
│  - Ska ha: name, balance        │
│  - Ska kunna: deposit, getName  │
└─────────────────────────────────┘
           │
           │ new (skapa objekt från ritning)
           ↓
    ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
    │  account1    │  │  account2    │  │  account3    │
    │ name="Jane"  │  │ name="John"  │  │ name="Bob"   │
    │ balance=50   │  │ balance=100  │  │ balance=0    │
    └──────────────┘  └──────────────┘  └──────────────┘
         OBJEKT           OBJEKT           OBJEKT
```

### Inkapsling som Skyddande Skal

```
           YTTERVÄRLD
                ↓
    ╔═══════════════════════════╗
    ║    PUBLIC INTERFACE       ║
    ║    +setName()             ║  ← Kontrollerad ingång
    ║    +getName()             ║
    ║    +deposit()             ║
    ╠═══════════════════════════╣
    ║    PROTECTED DATA         ║
    ║    -name (private)        ║  ← Gömd och skyddad
    ║    -balance (private)     ║
    ╚═══════════════════════════╝
```

---

## 🎯 ÖVNINGSRÅD OCH TIPS

### Hur man närmar sig övningarna

1. **Läs problemet noga** - Identifiera vad som ska lagras (instance variables) och vad som ska göras (metoder)

2. **Designa innan du kodar:**
   - Rita UML-diagram
   - Lista instance variables
   - Lista metoder med parametrar och returtyper

3. **Bygg stegvis:**
   - Börja med klass-skal
   - Lägg till instance variables
   - Skapa constructor
   - Implementera en metod i taget
   - Testa efter varje metod

4. **Testa grundligt:**
   - Skapa test-klass med main
   - Testa edge cases (gränsvärden)
   - Testa invalid input

### Vanliga övningstyper

**Typ 1: Skapa klass från beskrivning**
- Exempel: Invoice-klassen, Car-klassen
- Fokus: Identifiera instance variables och metoder

**Typ 2: Utöka befintlig klass**
- Exempel: Lägg till withdraw() till Account
- Fokus: Förstå befintlig kod, validering

**Typ 3: Array-baserade klasser**
- Exempel: GradeBook-varianter
- Fokus: Array-hantering, algoritmer

---

## 🚀 NÄSTA STEG

### Vad kommer i Kapitel 8?

Kapitel 8 tar en **djupare titt på klasser och objekt**:
- Flera constructors i samma klass (overloading)
- Static vs instance members
- Composition (klasser som innehåller andra klasser)
- Enum types
- Garbage collection
- Package access

### Koppling till senare kapitel

- **Kapitel 9:** Arv - Klasser som bygger på andra klasser
- **Kapitel 10:** Polymorfism - Objekt som kan vara flera typer
- **Kapitel 16:** Generiska samlingar - Avancerade datastrukturer
- **Kapitel 17:** Lambdas och Streams - Modern Java-programmering

---

## ✅ SJÄLVTEST - Kan du svara på dessa?

1. Vad är skillnaden mellan en klass och ett objekt?
2. Varför ska instance variables vara `private`?
3. Vad händer när du anropar `new Account("Jane")`?
4. Varför behöver vi både get- och set-metoder?
5. Vad är default-värdet för en `String` instance variable?
6. Vad är skillnaden mellan en default constructor och en custom constructor?
7. När försvinner default constructor?
8. Vad gör `this`-nyckelordet?
9. Hur itererar man genom en tvådimensionell array?
10. Varför ska man validera data i constructors?

---

## 📖 SAMMANFATTNING I PUNKTFORM

**Klasser och Objekt:**
- Klass = ritning, Objekt = konkret instans
- Varje objekt har egen kopia av instance variables
- Skapas med `new` operator

**Instance Variables:**
- Deklareras i klassen, utanför metoder
- Automatiskt initialiserade
- Bör vara `private`

**Metoder:**
- Get-metoder hämtar värden
- Set-metoder ändrar värden (med validering!)
- Instance metoder arbetar med objektets data

**Constructors:**
- Samma namn som klassen, ingen returtyp
- Anropas automatiskt vid objektskapelse
- Kan ha parametrar för initialisering
- Default constructor försvinner vid custom constructor

**Inkapsling:**
- Private data + public metoder = kontrollerad access
- Information hiding ger flexibilitet och säkerhet
- Validering förhindrar ogiltiga tillstånd

**Arrays i Klasser:**
- Kan lagra relaterade data
- En- eller tvådimensionella
- Användbara för att modellera listor och tabeller

---

## 🎓 SLUTORD

Detta kapitel är **fundamentalt** för all framtida objektorienterad programmering i Java. Koncepten du har lärt dig här - klasser, objekt, inkapsling, constructors - är grunden för allt som kommer. Ta dig tid att verkligen förstå dessa koncept genom att:

1. **Koda själv** - Skriv om exemplen från grunden
2. **Experimentera** - Ändra kod och se vad som händer
3. **Rita diagram** - Visualisera objekt och deras relationer
4. **Förklara för andra** - Om du kan förklara det, förstår du det

Kom ihåg: Programmering är som att lära sig ett instrument - det krävs övning! Varje fel är en lärandemöjlighet.

**Lycka till med dina studier! 🚀**

---

**Skapad för:** Datavetenskap student  
**Baserad på:** Java How to Program, Late Objects, 11th Edition - Kapitel 7  
**Syfte:** Pedagogisk guide för djup förståelse och memorering
