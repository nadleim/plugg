# 📚 KAPITEL 10: OBJEKTORIENTERAD PROGRAMMERING - POLYMORFISM OCH INTERFACE
---
## 🎯 ÖVERSIKT AV KAPITLET

Kapitel 10 utvidgar din förståelse av objektorienterad programmering genom att introducera två kraftfulla koncept: **polymorfism** och **interface**. Dessa verktyg gör det möjligt att skriva kod som är mer flexibel, återanvändbar och lätt att underhålla. Kapitlet bygger på de arvsmekanismer du lärde dig i Kapitel 9 och visar hur du kan "programmera i det generella" istället för "i det specifika".

---

## 🔑 DEL 1: POLYMORFISM - GRUNDLÄGGANDE KONCEPT

### 1.1 Vad är Polymorfism?

**Polymorfism** betyder bokstavligen "många former" och är förmågan att använda samma metodanrop på olika typer av objekt och få olika resultat beroende på objektets typ.

**🎪 Analogiexempel:** Tänk dig en djurpark där du har olika djur (Fish, Frog, Bird) som alla ärver från klassen Animal. Alla djur har en `move()`-metod, men varje djur rör sig på sitt unika sätt:

- En fisk simmar tre fot
- En groda hoppar fem fot
- En fågel flyger tio fot

När programmet säger "move" till varje djur, vet varje djur automatiskt hur det ska röra sig - **det är polymorfism i praktiken**.

**🔹 Nyckelprincip:**  
Polymorfism låter dig använda en superklassvariabel för att anropa metoder på både superklassobjekt och subklassobjekt. Detta gör det möjligt att "programmera i det generella" - du skriver kod som fungerar med superklassen, men den kan hantera alla subklasser automatiskt.

### 1.2 Fördelar med Polymorfism

**✅ Extensibilitet (Utbyggbarhet):**

- Nya klasser kan läggas till med minimal eller ingen ändring av befintlig kod
- De nya klasserna "pluggas in" i systemet
- Endast kod som direkt använder de nya klasserna behöver modifieras

**✅ Enklare underhåll:**

- Generell kod som fungerar med många objekttyper
- Mindre duplicerad kod
- Lättare att testa och felsöka

**📌 Viktigt exempel:**  
Device drivers (drivrutiner) är ett utmärkt exempel. Ett operativsystem använder polymorfism för att kommunicera med olika enheter (skrivare, skannrar, tangentbord). Varje enhet har sin egen implementering, men operativsystemet använder samma interface för alla.

---

## 🏗️ DEL 2: ABSTRAKTA KLASSER OCH METODER

### 2.1 Abstrakta Klasser

**Definition:**  
En **abstrakt klass** är en klass som **inte kan instansieras** (du kan inte skapa objekt av den direkt). Den finns för att tjäna som en basmodell för andra klasser att ärva från.

**Syntax:**

java

```java
public abstract class Employee {
    // Kan innehålla både abstrakta och konkreta metoder
}
```

**🎨 Nyckelkarakteristik:**

- ❌ Kan **INTE** skapa objekt: `new Employee()` ger kompileringsfel
- ✅ Kan ha både abstrakta och konkreta (vanliga) metoder
- ✅ Kan ha instansvariabler
- ✅ Kan ha konstruktorer (används av subklasser)
- ✅ Definierar en gemensam design för subklasser

**📝 Viktigt att förstå:**  
I Python finns inte riktiga abstrakta klasser på samma sätt, men du kan använda `abc`-modulen för liknande funktionalitet. Java kräver explicit deklaration med `abstract`-nyckelordet.

### 2.2 Abstrakta Metoder

**Definition:**  
En **abstrakt metod** är en metod som **endast deklareras** men inte implementeras i superklassen. Den måste implementeras av varje konkret subklass.

**Syntax:**

java

```java
public abstract class Employee {
    // Abstrakt metod - ingen implementation
    public abstract double earnings();
}
```

**🔹 Viktiga regler:**
1. En abstrakt metod har **INGEN metodkropp** (ingen `{}` efter deklarationen)
2. Om en klass innehåller **minst en abstrakt metod**, måste klassen deklareras som `abstract`
3. Konkreta subklasser **MÅSTE** implementera alla abstrakta metoder från superklassen
4. Konstruktorer och `static`-metoder kan **INTE** vara abstrakta

**💡 Varför använda abstrakta metoder?**
- Tvingar subklasser att implementera specifik funktionalitet
- Garanterar att alla subklasser har samma metodsignatur
- Skapar ett "kontrakt" som subklasser måste följa

### 2.3 Konkreta vs Abstrakta Klasser

**Konkreta klasser:**
- Kan instansieras (skapa objekt)
- Innehåller endast kompletta metodimplementationer
- Exempel: `String`, `ArrayList`, `SalariedEmployee`

**Abstrakta klasser:**
- Kan INTE instansieras
- Kan innehålla abstrakta metoder
- Kan innehålla konkreta metoder
- Exempel: `Employee`, `Shape`, `Animal`

---

## 💼 DEL 3: CASE STUDY - LÖNESYSTEM MED POLYMORFISM

Detta är kapitel 10s största och viktigaste exempel som demonstrerar alla koncept i praktiken.

### 3.1 Problemformulering

Ett företag betalar sina anställda på olika sätt:
1. **SalariedEmployee** - Fast veckolön oavsett arbetstimmar
2. **HourlyEmployee** - Timlön med övertidsersättning (1.5x efter 40 timmar)
3. **CommissionEmployee** - Provision baserad på försäljning
4. **BasePlusCommissionEmployee** - Grundlön + provision

**🎯 Mål:** Skapa ett system som beräknar lön polymorfiskt för alla anställda.

### 3.2 Klasshierarkin
```
           Employee (abstrakt)
          /        |        \
         /         |         \
  Salaried   Hourly    Commission
  Employee   Employee   Employee
                            |
                            |
                  BasePlusCommission
                      Employee
````

### 3.3 Abstract Superclass Employee

**🔹 Innehåll:**

java

```java
public abstract class Employee {
    private final String firstName;
    private final String lastName;
    private final String socialSecurityNumber;
    
    // Konstruktor
    public Employee(String firstName, String lastName, String ssn) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.socialSecurityNumber = ssn;
    }
    
    // Get-metoder för alla fält
    public String getFirstName() { return firstName; }
    public String getLastName() { return lastName; }
    public String getSocialSecurityNumber() { return socialSecurityNumber; }
    
    // toString - konkret implementation
    @Override
    public String toString() {
        return String.format("%s %s%nsocial security number: %s",
            getFirstName(), getLastName(), getSocialSecurityNumber());
    }
    
    // Abstrakt metod - måste implementeras av subklasser
    public abstract double earnings();
}
```

**📌 Viktiga observationer:**

- `final` på instansvariabler = de kan inte ändras efter konstruktion
- `toString()` är konkret - alla subklasser kan använda den
- `earnings()` är abstrakt - varje anställdtyp beräknar lön olika

### 3.4 Konkreta Subklasser

**A) SalariedEmployee**

java

```java
public class SalariedEmployee extends Employee {
    private double weeklySalary;
    
    public SalariedEmployee(String firstName, String lastName, 
                           String ssn, double salary) {
        super(firstName, lastName, ssn);
        
        if (salary < 0.0) {
            throw new IllegalArgumentException("Salary must be >= 0.0");
        }
        this.weeklySalary = salary;
    }
    
    @Override
    public double earnings() {
        return getWeeklySalary();  // Enkel - bara returnera lönen
    }
    
    @Override
    public String toString() {
        return String.format("salaried employee: %s%n%s: $%,.2f",
            super.toString(), "weekly salary", getWeeklySalary());
    }
}
```

**B) HourlyEmployee**

java

```java
public class HourlyEmployee extends Employee {
    private double wage;    // Timlön
    private double hours;   // Arbetade timmar
    
    @Override
    public double earnings() {
        if (getHours() <= 40) {
            return getWage() * getHours();
        } else {
            // Övertid betalar 1.5x efter 40 timmar
            return 40 * getWage() + (getHours() - 40) * getWage() * 1.5;
        }
    }
}
```

**C) CommissionEmployee**

java

```java
public class CommissionEmployee extends Employee {
    private double grossSales;       // Totalförsäljning
    private double commissionRate;   // Provisionssats
    
    @Override
    public double earnings() {
        return getCommissionRate() * getGrossSales();
    }
}
```

**D) BasePlusCommissionEmployee**

java

```java
public class BasePlusCommissionEmployee extends CommissionEmployee {
    private double baseSalary;
    
    @Override
    public double earnings() {
        // Återanvänd superklassens earnings + lägg till grundlön
        return getBaseSalary() + super.earnings();
    }
}
```

### 3.5 Polymorf Bearbetning

**Det kraftfulla i polymorfism:**

java

```java
public class PayrollSystemTest {
    public static void main(String[] args) {
        // Skapa olika typer av anställda
        SalariedEmployee salariedEmployee = 
            new SalariedEmployee("John", "Smith", "111-11-1111", 800.00);
        HourlyEmployee hourlyEmployee = 
            new HourlyEmployee("Karen", "Price", "222-22-2222", 16.75, 40);
        CommissionEmployee commissionEmployee = 
            new CommissionEmployee("Sue", "Jones", "333-33-3333", 10000, .06);
        BasePlusCommissionEmployee basePlusEmployee = 
            new BasePlusCommissionEmployee("Bob", "Lewis", "444-44-4444", 5000, .04, 300);
        
        // Polymorf array - alla är Employee-typer!
        Employee[] employees = new Employee[4];
        employees[0] = salariedEmployee;
        employees[1] = hourlyEmployee;
        employees[2] = commissionEmployee;
        employees[3] = basePlusEmployee;
        
        // Processar POLYMORFISKT
        for (Employee currentEmployee : employees) {
            System.out.println(currentEmployee);  // Anropar rätt toString()
            
            // Specialbehandling för BasePlusCommissionEmployee
            if (currentEmployee instanceof BasePlusCommissionEmployee) {
                BasePlusCommissionEmployee employee = 
                    (BasePlusCommissionEmployee) currentEmployee;
                
                employee.setBaseSalary(1.10 * employee.getBaseSalary());
                System.out.printf("new base salary: $%,.2f%n", 
                    employee.getBaseSalary());
            }
            
            // Polymorf anrop - rätt earnings() körs automatiskt!
            System.out.printf("earned $%,.2f%n%n", currentEmployee.earnings());
        }
    }
}
```

**🔑 Nyckelkoncept:**

**Dynamic Binding (Dynamisk Bindning):**

- Vid kompilering vet inte Java vilken `earnings()`-metod som ska köras
- Vid körning bestäms detta baserat på objektets **faktiska typ**
- Detta kallas även **late binding** (sen bindning)

### 3.6 Operatorn instanceof och Downcasting

**instanceof-operatorn:**

java

```java
if (currentEmployee instanceof BasePlusCommissionEmployee) {
    // Sant om currentEmployee ÄR en BasePlusCommissionEmployee
    // Eller en subklass till BasePlusCommissionEmployee
}
```

**Downcasting:**

java

```java
// Casta från superklasstyp till subklasstyp
BasePlusCommissionEmployee employee = 
    (BasePlusCommissionEmployee) currentEmployee;
```

**⚠️ Varning:**  
Downcasting är farligt! Använd alltid `instanceof` först för att kontrollera typen, annars kan du få `ClassCastException` vid körning.

**🔹 Varför behövs downcasting?**

- En `Employee`-referens kan bara anropa `Employee`-metoder
- För att komma åt `BasePlusCommissionEmployee`-specifika metoder (som `getBaseSalary()`) måste vi casta

### 3.7 getClass() och Få Objektets Klassnamn

java

```java
for (Employee e : employees) {
    System.out.printf("Employee is a %s%n", e.getClass().getName());
}
```

**Output:**
```
Employee is a SalariedEmployee
Employee is a HourlyEmployee
Employee is a CommissionEmployee
Employee is a BasePlusCommissionEmployee
````

---

## 🔌 DEL 4: INTERFACES (GRÄNSSNITT)

### 4.1 Vad är ett Interface?

Ett **interface** är ett kontrakt som specificerar **vad** som kan göras, men inte **hur** det görs. Det är en samling av metodsignaturer utan implementation.

**🎯 Jämförelse med Python:** I Python finns inte interfaces på samma sätt, men du kan använda `ABC` (Abstract Base Class) med `@abstractmethod` för liknande funktionalitet. Java's interface är mer strikt och formellt.

**Syntax:**

java

```java
public interface Payable {
    double getPaymentAmount();  // Ingen implementation
}
```

### 4.2 Interface vs Abstrakt Klass

|**Aspekt**|**Interface**|**Abstrakt Klass**|
|---|---|---|
|**Instansiering**|Nej|Nej|
|**Instansvariabler**|Nej (endast konstanter)|Ja|
|**Metodimplementation**|Endast `default`, `static`, `private`|Ja, alla typer|
|**Multipelt arv**|Ja (implementera flera)|Nej (ärv endast en)|
|**Använd när**|Specificera beteende för orelaterade klasser|Dela gemensam implementation|

### 4.3 Deklarera och Implementera Interface

**Deklarera interface:**

java

```java
public interface Payable {
    // Implicit: public abstract
    double getPaymentAmount();
}
```

**Implementera interface:**

java

```java
public class Invoice implements Payable {
    // MÅSTE implementera alla metoder från Payable
    @Override
    public double getPaymentAmount() {
        return quantity * pricePerItem;
    }
}
```

**🔹 Viktiga regler:**

1. Alla metoder i interface är implicit `public abstract` (före Java 8)
2. Alla konstanter är implicit `public static final`
3. En klass kan implementera **flera** interfaces
4. En klass som implementerar ett interface måste implementera alla dess abstrakta metoder

### 4.4 Case Study: Payable Hierarchy

**Designidé:**  
Både anställda (Employee) och fakturor (Invoice) kan betalas, men på olika sätt. Vi skapar ett `Payable`-interface för att hantera båda polymorfiskt.

**Interface Payable:**

java

```java
public interface Payable {
    double getPaymentAmount();  // Hur mycket ska betalas?
}
```

**Modifierad Employee-klass:**

java

```java
public abstract class Employee implements Payable {
    // Instansvariabler och metoder som tidigare...
    
    public abstract double earnings();
    
    // Implementera Payable interface
    @Override
    public double getPaymentAmount() {
        return earnings();  // För anställda är betalning = earnings
    }
}
```

**Invoice-klass:**

java

```java
public class Invoice implements Payable {
    private String partNumber;
    private String partDescription;
    private int quantity;
    private double pricePerItem;
    
    // Konstruktor, get/set-metoder...
    
    @Override
    public double getPaymentAmount() {
        return getQuantity() * getPricePerItem();  // Kvantitet × pris
    }
}
```

**Polymorf bearbetning:**

java

```java
Payable[] payableObjects = new Payable[4];

payableObjects[0] = new Invoice("01234", "seat", 2, 375.00);
payableObjects[1] = new Invoice("56789", "tire", 4, 79.95);
payableObjects[2] = new SalariedEmployee("John", "Smith", "111-11-1111", 800.00);
payableObjects[3] = new SalariedEmployee("Lisa", "Barnes", "888-88-8888", 1200.00);

// Processar polymorfiskt
for (Payable currentPayable : payableObjects) {
    System.out.printf("%s%npayment due: $%,.2f%n%n",
        currentPayable.toString(),
        currentPayable.getPaymentAmount());
}
```

**🎯 Kraft i detta:**

- `Invoice` och `Employee` är **helt orelaterade** klasser
- Men båda kan behandlas som `Payable` objekt
- Samma kod hanterar båda typerna polymorfiskt

---

## ⚙️ DEL 5: JAVA SE 8 & 9 INTERFACE-FÖRBÄTTRINGAR

### 5.1 Default Methods (Java SE 8)

**Vad är det?**  
`default`-metoder är konkreta metoder i interfaces med faktisk implementation.

**Syntax:**

java

```java
public interface Payable {
    double getPaymentAmount();
    
    // Default method
    default void printPayment() {
        System.out.printf("Payment amount: $%,.2f%n", getPaymentAmount());
    }
}
```

**🔹 Fördelar:**

- Lägga till nya metoder i interfaces utan att bryta befintlig kod
- Klasser som implementerar interfacet får `default`-implementationen gratis
- Kan overridas om nödvändigt

**💡 Varför introducerades detta?** Före Java 8, om du lade till en metod i ett interface, måste alla klasser som implementerade interfacet uppdateras. Detta bröt backåtkompatibilitet. `default`-metoder löser detta problem.

### 5.2 Static Methods (Java SE 8)

**Vad är det?**  
Statiska hjälpmetoder direkt i interfaces.

**Syntax:**

java

```java
public interface MathOperations {
    static double add(double a, double b) {
        return a + b;
    }
}

// Använd så här:
double result = MathOperations.add(5, 3);
```

**🔹 Fördelar:**

- Samla relaterade hjälpmetoder med interfacet
- Ingen separat "helper class" behövs

### 5.3 Functional Interfaces (Java SE 8)

**Definition:**  
Ett **functional interface** är ett interface med **exakt EN abstrakt metod** (kan ha flera `default` och `static` metoder).

**Exempel:**

java

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);  // En abstrakt metod
    
    // Kan ha default och static metoder
    default Comparator<T> reversed() { /* ... */ }
}
```

**🎯 Varför viktigt?** Functional interfaces används med **lambdas** (kommer i senare kapitel):

java

```java
// Istället för att skriva en hel klass:
Comparator<String> comp = (s1, s2) -> s1.compareToIgnoreCase(s2);
```

**Vanliga functional interfaces:**

- `ChangeListener` - Hantera UI-förändringar
- `Comparator` - Jämföra objekt
- `Runnable` - Definiera parallella uppgifter

### 5.4 Private Methods (Java SE 9)

**Vad är det?**  
Privata hjälpmetoder i interfaces för att dela kod mellan `default`-metoder.

**Syntax:**

java

```java
public interface Calculator {
    default int addAndDouble(int a, int b) {
        return doubleValue(a + b);  // Använd private helper
    }
    
    default int subtractAndDouble(int a, int b) {
        return doubleValue(a - b);  // Återanvänd samma helper
    }
    
    // Private helper method
    private int doubleValue(int value) {
        return value * 2;
    }
}
```

**🔹 Fördelar:**

- Undvik kod duplicering mellan `default`-metoder
- Håll implementation-detaljer privata

---

## 🏛️ DEL 6: FINAL METHODS OCH KLASSER

### 6.1 Final Methods

**Definition:**  
En `final`-metod kan **INTE** overridas av subklasser.

**Syntax:**

java

```java
public class Employee {
    public final double calculateTax(double income) {
        return income * 0.25;
    }
}

public class Manager extends Employee {
    // KOMPILERINGSFEL - kan inte override final metod
    @Override
    public double calculateTax(double income) {
        return income * 0.30;
    }
}
```

**🔹 När använda:**

- När du vill garantera att metoden aldrig ändras
- Säkerhetsrelaterad funktionalitet
- Performance-optimering (kompilatorn kan inline:a metoden)

**📌 Implicit final:**

- Alla `private`-metoder är implicit `final`
- Alla `static`-metoder är implicit `final`

### 6.2 Final Classes

**Definition:**  
En `final`-klass kan **INTE** ärvas.

**Syntax:**

java

```java
public final class String {
    // Kan inte skapa subklasser till String
}
```

**🔹 Användningsfall:**

- Säkerhet (förhindra ondskefull subklassing)
- Immutability (som `String`)
- API-design (förhindra oväntad extension)

---

## 🎨 DEL 7: DESIGN PRINCIPLES - INTERFACE VS IMPLEMENTATION INHERITANCE

### 7.1 "Program to an Interface, Not an Implementation"

Detta är en fundamental designprincip i objektorienterad programmering.

**Implementation Inheritance (extends):**

- **Tight coupling** - subklasser är starkt kopplade till superklassen
- Bra för små hierarchier under en persons kontroll
- Svårt att modifiera när hierarchin växer

**Interface Inheritance (implements):**

- **Loose coupling** - klasser är frikopplade
- Mer flexibelt - lätt att byta implementationer
- Bättre för stora, underhållna system

### 7.2 Exempel: Redesigna Employee Hierarchy med Interface

**Problem med arv:** Om vi vill lägga till olika pensionsplaner (401K, IRA) till varje Employee-typ, får vi en explosion av klasser:

- `SalariedEmployeeWith401K`
- `SalariedEmployeeWithIRA`
- `HourlyEmployeeWith401K`
- `HourlyEmployeeWithIRA`
- osv...

**Lösning med Composition + Interface:**

java

```java
// Interface för compensation
public interface CompensationModel {
    double earnings();
}

// Olika implementationer
public class SalariedCompensationModel implements CompensationModel {
    private double weeklySalary;
    
    @Override
    public double earnings() {
        return weeklySalary;
    }
}

public class HourlyCompensationModel implements CompensationModel {
    private double wage;
    private double hours;
    
    @Override
    public double earnings() {
        if (hours <= 40) {
            return wage * hours;
        } else {
            return 40 * wage + (hours - 40) * wage * 1.5;
        }
    }
}

// Employee använder composition
public class Employee {
    private String firstName;
    private String lastName;
    private CompensationModel compensationModel;
    
    public Employee(String first, String last, CompensationModel model) {
        this.firstName = first;
        this.lastName = last;
        this.compensationModel = model;
    }
    
    // Enkelt byta compensation model!
    public void setCompensationModel(CompensationModel model) {
        this.compensationModel = model;
    }
    
    public double earnings() {
        return compensationModel.earnings();
    }
}
```

**🎯 Fördelar:**

1. **Flexibilitet** - Byt CompensationModel dynamiskt
2. **Ingen klasexplosion** - Endast en Employee-klass
3. **Enkel att utöka** - Nya CompensationModel-implementationer påverkar inte Employee
4. **Loose coupling** - Employee beror på interface, inte konkreta klasser

---

## 🔧 DEL 8: VIKTIGA TEKNISKA DETALJER

### 8.1 Allowed Assignments

**Tillåtna tilldelningar:**

java

```java
// 1. Superclass variable = subclass object (Alltid OK)
Employee e = new SalariedEmployee(...);  // ✅ Polymorphism

// 2. Subclass variable = subclass object (OK)
SalariedEmployee s = new SalariedEmployee(...);  // ✅

// 3. Interface variable = implementing object (OK)
Payable p = new Invoice(...);  // ✅ Polymorphism

// 4. Superclass variable = superclass object (OK om inte abstract)
// Employee e = new Employee(...);  // ❌ Employee är abstract
```

**INTE tillåtna utan explicit cast:**

java

```java
Employee e = new SalariedEmployee(...);
SalariedEmployee s = e;  // ❌ KOMPILERINGSFEL

// Måste casta:
SalariedEmployee s = (SalariedEmployee) e;  // ✅ Men farligt utan instanceof
```

### 8.2 @Override Annotation

**Vad gör den?**

java

```java
@Override
public double earnings() {
    return weeklySalary;
}
```

**🔹 Fördelar:**

1. Kompilatorn verifierar att metoden faktiskt overridar något
2. Fångar stavfel i metodnamn
3. Dokumenterar att metoden är avsedd att override:a

**Exempel på fel som fångas:**

java

```java
public abstract class Employee {
    public abstract double earnings();  // Observera namnet
}

public class SalariedEmployee extends Employee {
    @Override
    public double earnigns() {  // Stavfel: earnigns istället för earnings
        // KOMPILERINGSFEL tack vare @Override!
        return weeklySalary;
    }
}
```

### 8.3 Calling Methods from Constructors - Ett Djupare Problem

**Problem:** När du anropar en overridable metod från en konstruktor, kan subklassens version köras innan subklassens konstruktor har körts!

**Farligt exempel:**

java

```java
public class Employee {
    public Employee() {
        initialize();  // Farligt!
    }
    
    public void initialize() {
        // Employee's initialization
    }
}

public class SalariedEmployee extends Employee {
    private double salary;
    
    public SalariedEmployee(double salary) {
        super();  // Anropar Employee(), vilket anropar initialize()
        this.salary = salary;
    }
    
    @Override
    public void initialize() {
        // Försöker använda salary här
        // MEN salary är ännu inte initialiserat!
        System.out.println(salary);  // 0.0 (default värde)
    }
}
```

**✅ Lösning:**

- Använd `final` metoder i konstruktorer
- Eller använd `static` hjälpmetoder för validering
- Undvik att anropa overridable metoder från konstruktorer

### 8.4 Private Constructors

**Användningsfall:**

**1. Förhindra instansiering:**

java

```java
public class MathUtils {
    private MathUtils() {
        // Kan aldrig anropas utanför klassen
    }
    
    public static double add(double a, double b) {
        return a + b;
    }
}

// MathUtils m = new MathUtils();  // KOMPILERINGSFEL
```

**2. Factory Methods:**

java

```java
public class Employee {
    private String name;
    
    // Private constructor
    private Employee(String name) {
        this.name = name;
    }
    
    // Factory method
    public static Employee createSalariedEmployee(String name, double salary) {
        Employee e = new Employee(name);
        // Konfigurera som salaried...
        return e;
    }
    
    public static Employee createHourlyEmployee(String name, double wage) {
        Employee e = new Employee(name);
        // Konfigurera som hourly...
        return e;
    }
}
```

---

## 📊 DEL 9: PRAKTISKA TIPS OCH BEST PRACTICES

### 9.1 När Använda Vad?

**Använd Abstract Class när:**

- ✅ Klasser är tätt relaterade
- ✅ Du behöver dela instansvariabler
- ✅ Du behöver dela konkret implementation
- ✅ Du vill ha både abstrakta och konkreta metoder

**Använd Interface när:**

- ✅ Orelaterade klasser ska dela beteende
- ✅ Du vill ha multipelt "arv"
- ✅ Du specificerar endast kontrakt (vad, inte hur)
- ✅ Systemet ska vara maximalt flexibelt

**Modern trend:** Med Java 8+ `default` och `static` metoder, går interfaces att använda i nästan alla fall där du tidigare använde abstrakta klasser.

### 9.2 Common Pitfalls (Vanliga Misstag)

**❌ Misstag 1: Försöka instansiera abstrakt klass**

java

```java
Employee e = new Employee(...);  // KOMPILERINGSFEL
```

**❌ Misstag 2: Glömma implementera abstrakta metoder**

java

```java
public class SalariedEmployee extends Employee {
    // KOMPILERINGSFEL - måste implementera earnings()
}
```

**❌ Misstag 3: Downcast utan instanceof**

java

```java
Employee e = employees[0];
SalariedEmployee s = (SalariedEmployee) e;  // Kan krascha!

// Rätt:
if (e instanceof SalariedEmployee) {
    SalariedEmployee s = (SalariedEmployee) e;
}
```

**❌ Misstag 4: Överanvända polymorfism** Polymorfism är kraftfullt, men inte alltid rätt lösning. Ibland är enkel, explicit kod bättre.

### 9.3 Testing Tips

**Testa polymorf kod:**

java

```java
@Test
public void testPolymorphicProcessing() {
    Employee[] employees = {
        new SalariedEmployee("John", "Doe", "111-11-1111", 800),
        new HourlyEmployee("Jane", "Smith", "222-22-2222", 15, 40)
    };
    
    for (Employee e : employees) {
        double earnings = e.earnings();
        assertTrue(earnings > 0);  // Verifierar att alla fungerar
    }
}
```

---

## 🎓 SAMMANFATTNING OCH MINNESREGLER

### Polymorfism i 3 punkter:

1. **"Många former"** - Samma metodanrop, olika resultat
2. **Superklassreferens** - Använd superklassvariabler för att hantera subklassobjekt
3. **Runtime binding** - Java bestämmer vilken metod som körs vid körning, inte kompilering

### Interface i 3 punkter:

1. **Kontrakt** - Specificerar vad, inte hur
2. **Multipelt "arv"** - Implementera flera interfaces
3. **Loose coupling** - Klasser är frikopplade, maximalt flexibelt

### Abstrakta klasser i 3 punkter:

1. **Kan inte instansieras** - Bara för att ärva från
2. **Blandning** - Både abstrakta och konkreta metoder
3. **Gemensam design** - Definierar struktur för subklasser

### Design-princip att komma ihåg:

**"Program to an interface, not an implementation"**  
= Skriv kod som beror på interfaces/abstrakta klasser, inte konkreta klasser. Detta ger maximal flexibilitet.

---

## ✅ CHECKL ISTA - HAR DU FÖRSTÅTT?

Efter att ha läst detta kapitel bör du kunna:

- [ ]  Förklara vad polymorfism är och varför det är användbart
- [ ]  Skilja mellan abstrakta och konkreta klasser
- [ ]  Skapa och använda abstrakta metoder
- [ ]  Designa och implementera klasshierarkier med polymorfism
- [ ]  Använda `instanceof` och downcasting säkert
- [ ]  Deklarera och implementera interfaces
- [ ]  Förstå skillnaden mellan interface och abstract class
- [ ]  Använda Java SE 8+ interface-features (default, static)
- [ ]  Tillämpa principen "Program to an interface"
- [ ]  Använda `final` för metoder och klasser
- [ ]  Välja rätt verktyg (interface vs abstract class) för olika situationer

---

**Kapitel 10 är fundamentalt för att förstå modern Java-programmering. Polymorfism och interfaces är byggstenar i nästan all professionell Java-kod, och behärskning av dessa koncept är avgörande för din utveckling som programmerare!**