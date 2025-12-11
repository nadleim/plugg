# KAPITEL 9: ARV

Nu ska jag ge dig en grundlig och pedagogisk genomgång av kapitel 9, som introducerar ett av de mest kraftfulla koncepten i objektorienterad programmering: **arv** (inheritance). Detta kapitel bygger vidare på klasskoncepten från kapitel 8 och ger dig verktyg för att skapa flexibla, återanvändbara klasshierarkier.

---

## 🌟 ÖVERSIKT OCH MOTIVATION

### Vad är arv och varför behöver vi det?

Tänk dig att du ska skapa klasser för olika typer av anställda i ett företag. Du kanske har vanliga anställda, chefer, säljare och deltidsanställda. Alla dessa typer delar gemensamma egenskaper som namn, personnummer och lön, men de har också sina unika egenskaper. Utan arv skulle du behöva kopiera och klistra samma kod i varje klass, vilket skulle vara ineffektivt och svårt att underhålla.

**Arv** (inheritance) löser detta problem genom att låta dig skapa nya klasser baserade på befintliga klasser. Den nya klassen **ärver** (inherit) medlemmarna från den befintliga klassen och kan sedan lägga till eller modifiera funktionalitet. Detta är som när ett barn ärver egenskaper från sina föräldrar men också utvecklar sin egen unika personlighet.

### Grundläggande terminologi

I arvssammanhang använder vi specifika termer:

**Superclass** (superklass) är den befintliga klassen som andra klasser ärver från. Detta kallas ibland också för **parent class** (förälderaklass) eller **base class** (basklass) i andra programmeringsspråk.

**Subclass** (subklass) är den nya klassen som ärver från superklassen. Detta kallas ibland också för **child class** (barnklass) eller **derived class** (härledd klass).

En **direkt superklass** är den superklass som en subklass explicit ärver från. En **indirekt superklass** är varje klass ovanför den direkta superklassen i klasshierarkin. Till exempel, om klass C ärver från klass B, som i sin tur ärver från klass A, så är B den direkta superklassen till C, medan A är en indirekt superklass.

### Java's klasshierarki och Object-klassen

En fundamental princip i Java är att **varje klass** antingen direkt eller indirekt ärver från klassen **Object** (i paketet java.lang). Object är roten i hela Javas klasshierarki. Detta innebär att alla klasser automatiskt har tillgång till Object-klassens metoder, som toString(), equals() och hashCode().

När du skapar en klass utan att explicit ange vilken klass den ärver från, ärver den automatiskt från Object. Det här är anledningen till att alla dina klasser har haft en toString()-metod även när du inte explicit har deklarerat den.

### Single inheritance i Java

Java stödjer **single inheritance** (enkel arv), vilket innebär att varje klass kan ärva från exakt en direkt superklass. Detta skiljer sig från språk som C++ som tillåter **multiple inheritance** (multipelt arv), där en klass kan ärva från flera direkta superklasser.

Även om Java inte stödjer multipelt arv av klasser, kan du uppnå liknande funktionalitet genom **interfaces** (gränssnitt), vilket du kommer att lära dig mer om i kapitel 10. Detta designval gjordes för att undvika de komplexa problem som kan uppstå med multipelt arv.

---

## 🏗️ SUPERKLASSER OCH SUBKLASSER

### Is-a relationship

Arv representerar en **is-a relationship** (är-ett-förhållande). Detta betyder att ett objekt av en subklass också kan behandlas som ett objekt av dess superklass. Till exempel:

- En bil **är ett** fordon
- En hund **är ett** djur
- En cirkel **är en** form
- En student **är en** person

Detta förhållande går endast i en riktning. En bil är ett fordon, men alla fordon är inte bilar. En hund är ett djur, men alla djur är inte hundar.

**Jämförelse med Python:** I Python använder du parenteser för att ange superklassen: `class Dog(Animal):`. I Java använder du nyckelordet `extends`: `class Dog extends Animal`.

### Kontrast med has-a relationship

Det är viktigt att skilja is-a från **has-a relationship** (har-ett-förhållande), som representerar **komposition** (composition) som du lärde dig i kapitel 8.

**Is-a exempel (arv):**

- En bil är ett fordon ✓
- En anställd är en person ✓

**Has-a exempel (komposition):**

- En bil har en motor ✓
- En anställd har ett födelsedatum ✓
- En bil har hjul ✓

Det vore felaktigt att säga "en anställd är ett födelsedatum" eller "en bil är en motor". Dessa representerar komposition, inte arv.

### Klasshierarkier och UML-diagram

Klasshierarkier visualiseras ofta med **UML class diagrams** (UML-klassdiagram). I dessa diagram används pilar för att visa arvsrelationer, där pilen pekar från subklassen upp mot superklassen.

**Exempel på en universitetsklasshierarki:**

```
         CommunityMember (högst upp)
                |
         +------+------+
         |             |
      Employee      Student
         |
    +----+----+
    |         |
  Faculty   Staff
    |
    +----+----+
    |         |
 Teacher  Administrator
```

Här kan du följa pilarna uppåt för att tillämpa is-a relationen:

- En Teacher (lärare) är en Faculty (fakultetsmedlem)
- En Faculty är en Employee (anställd)
- En Employee är en CommunityMember (samhällsmedlem)
- En CommunityMember är ett Object (alla klasser i Java)

### Specialisering och generalisering

**Specialisering** innebär att subklassen är mer specifik än sin superklass. Den representerar en mindre, mer specialiserad grupp av objekt. Till exempel är "hund" mer specifikt än "djur", och "sportvagn" är mer specifikt än "fordon".

**Generalisering** går åt andra hållet. När du rör dig uppåt i klasshierarkin blir klasserna mer generella och representerar större grupper av objekt.

Denna princip är kraftfull eftersom den låter dig:

- Skriva generell kod som fungerar med superklassen
- Automatiskt hantera alla nuvarande och framtida subklasser
- Lägga till nya subklasser utan att ändra befintlig kod

---

## 🔐 PROTECTED ACCESS MODIFIER

### De tre åtkomstnivåerna för arv

Du känner redan till **public** och **private** från kapitel 8. Nu introducerar vi **protected**, som ger en mellannivå av åtkomst:

**public**: Medlemmen är tillgänglig överallt där programmet har en referens till ett objekt av klassen eller dess subklasser.

**protected**: Medlemmen kan nås av:

- Medlemmar i samma klass (som private)
- Medlemmar i subklasser (detta är nytt!)
- Medlemmar i andra klasser i samma paket (package access)

**private**: Medlemmen kan endast nås inom den egna klassen. Subklasser kan inte direkt komma åt private medlemmar.

### Hur subklasser hanterar superklassens medlemmar

När en subklass ärver från en superklass gäller följande:

**Public medlemmar i superklassen** blir public medlemmar i subklassen. De behåller sin public åtkomst och kan användas av vem som helst.

**Protected medlemmar i superklassen** blir protected medlemmar i subklassen. De kan användas av subklassen och dess subklasser, men inte av godtyckliga klasser utanför hierarkin.

**Private medlemmar i superklassen** ärvs inte direkt av subklassen. De existerar i subklassobjektet men kan endast nås genom public eller protected metoder som ärvts från superklassen.

Detta sista punkten är viktig att förstå. När du skapar ett subklassobjekt innehåller det faktiskt alla instansvariabler från hela klasshierarkin, inklusive private variabler från superklassen. Men subklassen kan inte direkt manipulera dessa private variabler – den måste använda ärvda metoder (som set- och get-metoder).

### Varför är detta viktigt för inkapsling?

Denna design skyddar **inkapslingen** (encapsulation). Om subklasser kunde direkt komma åt superklassens private variabler, skulle det bryta ned informationsdöljandet. Om en framtida subklass av din subklass också kunde komma åt dessa variabler, skulle åtkomsten spridas okontrollerat genom hierarkin.

Genom att tvinga subklasser att använda public eller protected metoder för att manipulera superklassens data bibehålls kontrollen och valideringen som dessa metoder tillhandahåller.

### super-nyckelordet för att komma åt dolda medlemmar

Om en subklass överskriver (override) en superklassmetod men fortfarande vill anropa superklassversionen, kan den använda nyckelordet **super** följt av en punkt och metodnamnet:

java

```java
public class Subclass extends Superclass {
    @Override
    public void someMethod() {
        super.someMethod();  // Anropar superklassens version
        // Lägg till subklassens unika beteende här
    }
}
```

Detta är analogt med hur du använder **this** för att referera till det aktuella objektet, men **super** refererar specifikt till superklassens medlemmar.

---

## 💼 PRAKTISKT EXEMPEL: COMMISSION EMPLOYEE HIERARKI

Kapitlet använder ett genomgående exempel med anställda för att illustrera olika aspekter av arv. Låt mig följa detta exempel steg för steg för att visa dig hur arv fungerar i praktiken.

### Scenario

Företaget har två typer av säljare:

**CommissionEmployee** (provisionsanställd): Får betalt baserat på en procentandel av sin försäljning.

**BasePlusCommissionEmployee** (basprovisionsanställd): Får en baslön plus en procentandel av sin försäljning.

Eftersom BasePlusCommissionEmployee har allt som en CommissionEmployee har **plus** en baslön, är detta ett perfekt scenario för arv.

### Exempel 1: CommissionEmployee som ärver från Object

Först skapar vi klassen CommissionEmployee som direkt ärver från Object:

java

```java
public class CommissionEmployee extends Object {
    private final String firstName;
    private final String lastName;
    private final String socialSecurityNumber;
    private double grossSales;        // Bruttointäkter
    private double commissionRate;    // Provisionssats
    
    // Konstruktor
    public CommissionEmployee(String firstName, String lastName, 
                            String socialSecurityNumber,
                            double grossSales, double commissionRate) {
        // Validering av argument skulle göras här
        this.firstName = firstName;
        this.lastName = lastName;
        this.socialSecurityNumber = socialSecurityNumber;
        this.grossSales = grossSales;
        this.commissionRate = commissionRate;
    }
    
    // Set- och get-metoder för grossSales och commissionRate
    
    // Beräkna lön
    public double earnings() {
        return commissionRate * grossSales;
    }
    
    // toString-metod för strängrepresentation
    @Override
    public String toString() {
        return String.format(
            "%s: %s %s%n%s: %s%n%s: %.2f%n%s: %.2f",
            "commission employee", firstName, lastName,
            "social security number", socialSecurityNumber,
            "gross sales", grossSales,
            "commission rate", commissionRate);
    }
}
```

Notera att vi använder **@Override** annotationen före toString(). Detta är inte strikt nödvändigt, men det är en **best practice** som hjälper kompilatorn att kontrollera att du verkligen överskriver en metod från superklassen och inte av misstag skapar en ny metod med fel signatur.

### Exempel 2: BasePlusCommissionEmployee utan arv

Nästa steg i kapitlets progression är att skapa BasePlusCommissionEmployee **utan** att använda arv, för att visa hur mycket kod som skulle dupliceras:

java

```java
public class BasePlusCommissionEmployee {
    private final String firstName;
    private final String lastName;
    private final String socialSecurityNumber;
    private double grossSales;
    private double commissionRate;
    private double baseSalary;  // Den enda nya variabeln!
    
    // Konstruktor med alla sex parametrar
    public BasePlusCommissionEmployee(String firstName, String lastName,
                                    String socialSecurityNumber,
                                    double grossSales, double commissionRate,
                                    double baseSalary) {
        // Samma validering och initialisering som CommissionEmployee
        // PLUS baseSalary
    }
    
    // Samma set- och get-metoder som CommissionEmployee
    // PLUS setBaseSalary och getBaseSalary
    
    public double earnings() {
        return baseSalary + (commissionRate * grossSales);
    }
    
    @Override
    public String toString() {
        // Nästan identisk med CommissionEmployee's toString
        // PLUS baseSalary
    }
}
```

Detta fungerar, men problemet är uppenbart: vi duplicerar enorma mängder kod från CommissionEmployee. Om vi senare behöver ändra hur namn eller personnummer hanteras måste vi göra ändringen på två ställen. Detta bryter mot **DRY-principen** (Don't Repeat Yourself).

### Exempel 3: BasePlusCommissionEmployee med arv (första försöket)

Nu försöker vi använda arv, men här möter vi ett problem:

java

```java
public class BasePlusCommissionEmployee extends CommissionEmployee {
    private double baseSalary;
    
    public BasePlusCommissionEmployee(String firstName, String lastName,
                                    String socialSecurityNumber,
                                    double grossSales, double commissionRate,
                                    double baseSalary) {
        // Vi måste anropa superklassens konstruktor!
        super(firstName, lastName, socialSecurityNumber,
              grossSales, commissionRate);
              
        if (baseSalary < 0.0) {
            throw new IllegalArgumentException(
                "Base salary must be >= 0.0");
        }
        this.baseSalary = baseSalary;
    }
    
    // setBaseSalary och getBaseSalary metoder
    
    @Override
    public double earnings() {
        // PROBLEM: commissionRate och grossSales är private i superklassen!
        return baseSalary + (commissionRate * grossSales);  // FEL!
    }
    
    @Override
    public String toString() {
        // PROBLEM: Samma här - kan inte komma åt private variabler
        return String.format(
            "%s: %s %s%n%s: %s%n%s: %.2f%n%s: %.2f%n%s: %.2f",
            "base-salaried commission employee", firstName, lastName,
            // FEL på alla rader ovan!
            // ...
        );
    }
}
```

**Varför ger detta kompileringsfel?**

Metoderna earnings() och toString() försöker direkt komma åt superklassens private instansvariabler (commissionRate, grossSales, firstName, etc.). Men private medlemmar kan inte nås direkt av subklasser – detta är kärnan i inkapsling.

### super() för att anropa superklassens konstruktor

Notera användningen av **super()** på rad 8 i konstruktorn ovan. Detta är ett speciellt anrop som **måste** vara den första satsen i subklassens konstruktor. Det anropar superklassens konstruktor för att initialis era de ärvda instansvariablerna.

Om du inte explicit anropar super() försöker kompilatorn automatiskt anropa superklassens no-argument konstruktor. Om en sådan inte finns får du ett kompileringsfel.

**Viktigt att förstå:** Subklassen **ärver inte** konstruktorer från superklassen. Du måste explicit skriva en konstruktor i subklassen och anropa superklassens konstruktor med super().

---

## 🛡️ PROTECTED INSTANCE VARIABLES (EXEMPEL 4)

### Lösning med protected

Ett sätt att lösa problemet från exempel 3 är att ändra CommissionEmployee's instansvariabler från private till protected:

java

```java
public class CommissionEmployee {
    protected final String firstName;
    protected final String lastName;
    protected final String socialSecurityNumber;
    protected double grossSales;
    protected double commissionRate;
    
    // Resten av klassen oförändrad
}
```

Nu kompilerar BasePlusCommissionEmployee utan problem eftersom protected medlemmar kan nås direkt av subklasser:

java

```java
public class BasePlusCommissionEmployee extends CommissionEmployee {
    private double baseSalary;
    
    // Konstruktor samma som tidigare
    
    @Override
    public double earnings() {
        // Nu fungerar detta! commissionRate och grossSales är protected
        return baseSalary + (commissionRate * grossSales);
    }
    
    @Override
    public String toString() {
        // Direkt åtkomst till alla protected variabler fungerar nu
        return String.format(
            "%s: %s %s%n%s: %s%n%s: %.2f%n%s: %.2f%n%s: %.2f",
            "base-salaried commission employee", firstName, lastName,
            "social security number", socialSecurityNumber,
            "gross sales", grossSales,
            "commission rate", commissionRate,
            "base salary", baseSalary);
    }
}
```

Detta fungerar tekniskt sett, men det skapar flera problem ur perspektivet av god programvaruutveckling.

### Problemet med protected instansvariabler

**Problem 1: Bryter inkapsling** När instansvariabler är protected kan subklasser ändra dem direkt utan att gå genom valideringslogik. Om CommissionEmployee's setGrossSales()-metod innehåller validering för att säkerställa att värdet inte är negativt, kan denna validering kringgås om en subklass direkt ändrar den protected variabeln.

**Problem 2: Svårt att underhålla** Om du senare vill ändra hur data lagras internt (till exempel byta från en double till en BigDecimal), måste du ändra inte bara superklassen utan också alla subklasser som direkt använder variablerna.

**Problem 3: Exponerar implementation** Protected variabler blir en del av klassens publika kontrakt gentemot dess subklasser. Du kan inte ändra dem utan att potentiellt bryta subklasser.

---

## ✅ BÄSTA LÖSNINGEN: PRIVATE VARIABLER MED PUBLIC/PROTECTED METODER

### Exempel 5: Den rekommenderade lösningen

Den bästa lösningen är att behålla instansvariabler som private i superklassen och låta subklassen använda de ärvda public metoderna:

java

```java
public class CommissionEmployee {
    // Tillbaka till private - bästa praxis!
    private final String firstName;
    private final String lastName;
    private final String socialSecurityNumber;
    private double grossSales;
    private double commissionRate;
    
    // Konstruktor och alla metoder samma som tidigare
    
    // Public get-metoder
    public String getFirstName() { return firstName; }
    public String getLastName() { return lastName; }
    public String getSocialSecurityNumber() { 
        return socialSecurityNumber; 
    }
    public double getGrossSales() { return grossSales; }
    public double getCommissionRate() { return commissionRate; }
    
    // Public set-metoder med validering
    public void setGrossSales(double grossSales) {
        if (grossSales < 0.0) {
            throw new IllegalArgumentException(
                "Gross sales must be >= 0.0");
        }
        this.grossSales = grossSales;
    }
    
    public void setCommissionRate(double commissionRate) {
        if (commissionRate <= 0.0 || commissionRate >= 1.0) {
            throw new IllegalArgumentException(
                "Commission rate must be > 0.0 and < 1.0");
        }
        this.commissionRate = commissionRate;
    }
}
```

Nu kan BasePlusCommissionEmployee använda dessa metoder:

java

```java
public class BasePlusCommissionEmployee extends CommissionEmployee {
    private double baseSalary;
    
    // Konstruktor samma som tidigare
    
    @Override
    public double earnings() {
        // Använder ärvda get-metoder istället för direkt variabelåtkomst
        return getBaseSalary() + (getCommissionRate() * getGrossSales());
    }
    
    @Override
    public String toString() {
        // Använder ärvda get-metoder
        return String.format(
            "%s: %s %s%n%s: %s%n%s: %.2f%n%s: %.2f%n%s: %.2f",
            "base-salaried commission employee", 
            getFirstName(), getLastName(),
            "social security number", getSocialSecurityNumber(),
            "gross sales", getGrossSales(),
            "commission rate", getCommissionRate(),
            "base salary", getBaseSalary());
    }
}
```

### Varför är detta bättre?

**Bevarad inkapsling**: All validering i superklassens set-metoder respekteras automatiskt.

**Lös koppling (loose coupling)**: Subklassen är inte beroende av superklassens interna implementation. Om CommissionEmployee ändrar hur data lagras internt behöver BasePlusCommissionEmployee inte ändras.

**Enkel underhållning**: Om valideringslogik behöver uppdateras görs det endast på ett ställe – i superklassen.

**Tydligt kontrakt**: Det publika gränssnittet (de publika metoderna) definierar tydligt hur subklasser kan interagera med superklassen.

---

## 🏗️ KONSTRUKTORER I ARVSHIERARKIER

### Varför ärvs inte konstruktorer?

Konstruktorer har ett speciellt syfte: att initiera objekt av en specifik klass. Om konstruktorer ärvdes skulle en subklass automatiskt få alla superklassens konstruktorer, vilket ofta inte skulle vara meningsfullt eftersom subklassen har ytterligare instansvariabler som behöver initialiseras.

Istället måste varje klass i hierarkin deklarera sina egna konstruktorer som anropar lämpliga superklassenkonstruktorer.

### Konstruktorkedjan (constructor chaining)

När du skapar ett subklassobjekt sker en **konstruktorkedja** där konstruktorer anropas uppåt genom hierarkin:

1. Subklassens konstruktor anropar superklassens konstruktor (med super())
2. Superklassens konstruktor anropar sin superklasses konstruktor
3. Detta fortsätter ända upp till Object-klassen
4. Sedan exekveras konstruktorernas kod nerifrån och upp

**Exempel:**

java

```java
public class GrandParent {
    public GrandParent() {
        System.out.println("GrandParent constructor");
    }
}

public class Parent extends GrandParent {
    public Parent() {
        super();  // Implicit om inte explicit angivet
        System.out.println("Parent constructor");
    }
}

public class Child extends Parent {
    public Child() {
        super();  // Implicit om inte explicit angivet
        System.out.println("Child constructor");
    }
}

// Vid skapande av: Child c = new Child();
// Output:
// GrandParent constructor
// Parent constructor
// Child constructor
```

### Implicit vs explicit super()

Om du inte explicit skriver super() som första sats i en konstruktor, lägger kompilatorn automatiskt till ett anrop till superklassens no-argument konstruktor.

**Detta fungerar:**

java

```java
public class Subclass extends Superclass {
    public Subclass() {
        // Kompilatorn lägger till: super();
        // Övrig kod här
    }
}
```

**Detta ger kompileringsfel om Superclass inte har en no-argument konstruktor:**

java

```java
public class Superclass {
    private int value;
    
    // Ingen no-argument konstruktor!
    public Superclass(int value) {
        this.value = value;
    }
}

public class Subclass extends Superclass {
    public Subclass() {
        // FEL! Kompilatorn försöker anropa super() men den finns inte
    }
}
```

**Lösning:**

java

```java
public class Subclass extends Superclass {
    public Subclass() {
        super(0);  // Explicit anrop med nödvändigt argument
    }
}
```

---

## 🎯 CLASS OBJECT OCH DESS METODER

### Object - roten av allt

Klassen **Object** (i paketet java.lang) är superklassen för alla klasser i Java. Även om du inte explicit skriver `extends Object`, äger varje klass från Object automatiskt.

Object-klassen definierar flera metoder som alla klasser ärver. De viktigaste är:

**toString()**: Returnerar en strängrepresentation av objektet. Standardimplementationen returnerar klassnamnet följt av ett @ och objektets hashkod. Du bör nästan alltid överskriva denna metod.

**equals(Object obj)**: Jämför två objekt för likhet. Standardimplementationen jämför referenser (samma som == ), men du kan överskriva den för att jämföra innehåll.

**hashCode()**: Returnerar en numerisk hashkod för objektet, används i hash-baserade samlingar som HashMap.

**getClass()**: Returnerar ett Class-objekt som representerar objektets runtime klass.

**clone()**: Skapar och returnerar en kopia av objektet.

**finalize()**: Anropas av garbage collectorn innan objektet tas bort (deprecated och bör inte användas).

### Överskriva toString()

Det är nästan alltid en god idé att överskriva toString() för att ge användbar information:

java

```java
@Override
public String toString() {
    return String.format("%s: %s %s", 
        "Employee", firstName, lastName);
}
```

När du använder %s i printf eller println anropas automatiskt objektets toString()-metod.

---

## 🎨 COMPOSITION VS INHERITANCE

### När ska du använda arv?

Använd arv när du har en äkta **is-a relationship**:

- Subklassen är verkligen en typ av superklassen
- Subklassen behöver allt som superklassen har
- Det är logiskt att behandla subklassobjekt som superklassobjekt

### När ska du använda komposition?

Använd komposition när du har en **has-a relationship**:

- En klass behöver funktionalitet från en annan klass men är inte en typ av den klassen
- Du vill ha lös koppling mellan klasser
- Du vill kunna byta ut implementationen vid körning

### Exempel på felaktig användning av arv

Säg att du vill skapa en klass Stack (stapel) och Java redan har en klass ArrayList. Det kan vara frestande att skriva:

java

```java
// DÅLIG DESIGN!
public class Stack extends ArrayList {
    public void push(Object item) {
        add(item);
    }
    
    public Object pop() {
        return remove(size() - 1);
    }
}
```

**Problemet:** En Stack är inte en ArrayList – det är inte en is-a relation. En Stack **använder** en ArrayList för sin implementation. Detta exponerar också alla ArrayList-metoder (som add, set, remove på godtyckliga positioner) vilket bryter Stack-abstraktionen.

**Bättre design med komposition:**

java

```java
public class Stack {
    private ArrayList items = new ArrayList();  // Has-a relationship
    
    public void push(Object item) {
        items.add(item);
    }
    
    public Object pop() {
        return items.remove(items.size() - 1);
    }
    
    // Endast de metoder vi vill exponera
}
```

### Fördelar med komposition

**Lös koppling**: Du kan enkelt byta implementation (från ArrayList till LinkedList) utan att påverka klienter.

**Flexibilitet**: Du kan välja exakt vilken funktionalitet du vill exponera.

**Enklare att förstå**: Has-a relationer är ofta mer intuitiva än is-a relationer.

### Forwarding

När du använder komposition kan du **vidarebefordra** (forward) metodanrop till det komponerade objektet:

java

```java
public class ComposingClass {
    private ComposedClass composed = new ComposedClass();
    
    public void someMethod() {
        // Vidarebefordrar anropet till det komponerade objektet
        composed.someMethod();
    }
}
```

---

## 📋 SAMMANFATTNING AV NYCKELKONCEPT

### Fundamentala arvsbegrepp

**Inheritance** ger kodåteranvändning genom att låta nya klasser ärva medlemmar från befintliga klasser. Subklasser kan lägga till nya funktioner och överskriva ärvda metoder.

**Single inheritance** i Java innebär att varje klass ärver från exakt en direkt superklass, till skillnad från multipel arv i språk som C++.

**Klasshierarkin** i Java börjar med Object-klassen som är direkt eller indirekt superklass till alla andra klasser.

### Access control i arvshierarkier

**Private medlemmar** ärvs inte direkt av subklasser men finns i subklassobjektet och kan nås genom ärvda metoder.

**Protected medlemmar** kan nås av subklasser och klasser i samma paket, vilket ger en mellannivå mellan public och private.

**Public medlemmar** behåller sin public åtkomst i subklasser och kan nås överallt.

### Konstruktorer och super

**Konstruktorer ärvs inte** men varje subklassekonstruktor måste anropa en superklassekonstruktor, antingen explicit med super() eller implicit.

**super()** måste vara den första satsen i en subklassekonstruktor om den används explicit.

**Constructor chaining** sker automatiskt uppåt genom hierarkin från subklass till Object.

### Metod-överlagring och @Override

**@Override annotationen** hjälper kompilatorn att verifiera att du verkligen överskriver en superklassmetod och inte av misstag skapar en ny metod.

**Åtkomstkontroll vid överlagring** innebär att du inte kan göra en överskiven metod mer restriktiv än originalet (public kan inte bli protected eller private).

### Design-principer

**Favörisera komposition över arv** när relationen är has-a snarare än is-a.

**Använd private instansvariabler** även i superklasser för att bibehålla inkapsling.

**Tillhandahåll public eller protected metoder** för subklasser att använda istället för direkt variabelåtkomst.

**Is-a testet**: Om du kan säga "X is a Y" är arv lämpligt; om du säger "X has a Y" använd komposition.

---

Arv är ett kraftfullt verktyg som, när det används korrekt, dramatiskt förbättrar kodåteranvändning och underhållbarhet. Nyckeln är att förstå när arv är lämpligt (is-a relationer) och när komposition är bättre (has-a relationer), samt att alltid bibehålla god inkapsling även i arvshierarkier. Detta skapar flexibla, robusta klasshierarkier som är enkla att utöka och underhålla.