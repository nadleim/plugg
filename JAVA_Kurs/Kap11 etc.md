# 📚 OMFATTANDE SAMMANFATTNING: EXCEPTION HANDLING I JAVA

**Kapitel 11, Kapitel 6.6 & Oracle's Exception Guide**

---

## 📑 INNEHÅLLSFÖRTECKNING

### **DEL 1: GRUNDLÄGGANDE KONCEPT** ⚙️

- Vad är exceptions?
- Varför använda exception handling?
- Exception hierarchy

### **DEL 2: TRY-CATCH-FINALLY MEKANISMEN** 🔧

- try-block
- catch-block (inklusive multi-catch)
- finally-block

### **DEL 3: KASTA OCH ÅTERKASTA EXCEPTIONS** 🎯

- throw-statement
- throws-clause
- Rethrowing exceptions

### **DEL 4: STACK UNWINDING & DIAGNOSTIK** 🔍

- Stack unwinding process
- printStackTrace och getStackTrace
- getMessage och exception information

### **DEL 5: AVANCERADE KONCEPT** 🚀

- Chained exceptions
- Custom exception classes
- Preconditions & Postconditions
- Assertions
- try-with-resources

---

## DEL 1: GRUNDLÄGGANDE KONCEPT ⚙️

### 🎯 **Vad är en Exception?**

En **exception** är en indikation på ett problem som uppstår under ett programs körning. När något oväntat händer (t.ex. division med noll, ogiltig input, fil som inte hittas), "kastas" en exception.

**🔑 Nyckelord att förstå:**

- **Throwing an exception** = När en metod upptäcker ett problem den inte kan hantera
- **Catching an exception** = När du hanterar problemet i din kod
- **Throw point** = Den exakta platsen i koden där exception uppstår

**Exempel från Python-perspektiv:**

```python
# I Python:
try:
    result = 10 / 0  # ZeroDivisionError kastas
except ZeroDivisionError:
    print("Kan inte dela med noll!")
```

**Samma i Java:**

```java
try {
    int result = 10 / 0;  // ArithmeticException kastas
} 
catch (ArithmeticException e) {
    System.out.println("Kan inte dela med noll!");
}
```

### 🏗️ **Java Exception Hierarchy**

All exception-hantering i Java bygger på en klasshierarki:

```
Throwable (superklassen för ALLT som kan kastas)
├── Error (systemfel - fånga EJ!)
│   ├── OutOfMemoryError
│   └── StackOverflowError
│
└── Exception (programfel - dessa ska vi hantera!)
    ├── IOException (checked)
    ├── SQLException (checked)
    └── RuntimeException (unchecked)
        ├── ArithmeticException
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        └── InputMismatchException
```

**📌 Viktigt att förstå:**

1. **Throwable** är superklass för allt som kan kastas
2. **Error** = Allvarliga systemfel (minne slut, stack overflow). Program ska INTE fånga dessa!
3. **Exception** = Fel som program kan hantera
4. **RuntimeException** = Undertyp till Exception (unchecked exceptions)

### ✅ **Checked vs Unchecked Exceptions**

Detta är EN AV DE VIKTIGASTE SKILLNADERNA mot Python! Java delar upp exceptions i två kategorier:

#### **🔴 CHECKED EXCEPTIONS** (Kontrollerade)

- **MÅSTE** hanteras (catch) eller deklareras (throws)
- Kompilatorn tvingar dig att hantera dem
- Ärver från `Exception` men INTE från `RuntimeException`
- **Exempel:** IOException, SQLException, ClassNotFoundException

**Varför finns checked exceptions?** De representerar förväntade problem som kan hända även i korrekt skriven kod:

- Fil finns inte
- Nätverket är nere
- Database-anslutning misslyckas

**Exempel:**

```java
// KOMPILERINGSFEL om du inte hanterar IOException!
public void readFile() throws IOException {  // Måste deklarera!
    FileReader file = new FileReader("data.txt");
    // ...
}

// ELLER fånga det:
public void readFile() {
    try {
        FileReader file = new FileReader("data.txt");
    } 
    catch (IOException e) {
        System.out.println("Kunde inte läsa fil!");
    }
}
```

#### **🟢 UNCHECKED EXCEPTIONS** (Okontrollerade)

- Behöver INTE deklareras eller fångas (men kan!)
- Kompilatorn kontrollerar dem inte
- Ärver från `RuntimeException`
- **Exempel:** ArithmeticException, NullPointerException, ArrayIndexOutOfBoundsException

**Varför finns unchecked exceptions?** De representerar programmeringsfel som kan förhindras med korrekt kod:

- Division med noll (kontrollera före!)
- Null-referens (validera först!)
- Array index fel (kontrollera bounds!)

**Exempel:**

```java
// Inget kompileringsfel även utan try-catch
public int divide(int a, int b) {
    return a / b;  // Kan kasta ArithmeticException, men OK för kompilatorn
}

// MEN bättre att hantera:
public int divide(int a, int b) {
    if (b == 0) {
        throw new ArithmeticException("Kan inte dela med noll!");
    }
    return a / b;
}
```

### 🤔 **När ska vi använda Exception Handling?**

**✅ ANVÄND för:**

- **Synkrona fel** (uppstår när kod körs): division med noll, array bounds, null pointers
- **Förväntade problem:** fil saknas, network timeout, ogiltiga indata
- **Resource management:** stänga filer, databaser, network connections

**❌ ANVÄND INTE för:**

- **Asynkrona events:** thread notifications, GUI events (använd andra mekanismer)
- **Logikfel som kan förhindras:** använd if-satser istället för try-catch
- **Normal kontrollflöde:** använd INTE exceptions som "goto"

**Exempel på DÅLIG användning:**

```java
// DÅLIGT - använder exceptions som kontrollflöde
try {
    int i = 0;
    while (true) {
        System.out.println(array[i++]);
    }
} 
catch (ArrayIndexOutOfBoundsException e) {
    // Slut på array
}

// BRA - använd vanlig loop
for (int i = 0; i < array.length; i++) {
    System.out.println(array[i]);
}
```

---

## DEL 2: TRY-CATCH-FINALLY MEKANISMEN 🔧

### 📦 **try-Block**

**Syntax:**

```java
try {
    // Kod som KAN kasta exception
    // Endast denna kod skyddas
}
```

**🔑 Viktiga regler:**

1. Omsluter kod som kan kasta exceptions
2. Om exception kastas, **terminerar blocket omedelbart**
3. Lokala variabler i try-block försvinner när blocket avslutas
4. MÅSTE följas av minst en catch-block ELLER en finally-block

**Exempel:**

```java
try {
    int result = 10 / 0;  // Exception kastas HÄR
    System.out.println("Denna rad körs ALDRIG");  // Skip
    int x = 5;  // Skip
} 
catch (ArithmeticException e) {
    // Hoppar direkt hit!
    System.out.println("Hanterade exception");
}
// Fortsätter här efter catch
```

### 🎣 **catch-Block**

**Syntax:**

```java
catch (ExceptionType parameterName) {
    // Kod för att hantera exception
}
```

**🔑 Viktiga regler:**

1. Specificerar vilken typ av exception den fångar
2. **Exception-parametern** ger tillgång till exception-objektet
3. **Endast första matchande catch körs**
4. Ordningen spelar roll! (mer specifika först)

**Exempel med multiple catches:**

```java
try {
    // Kod som kan kasta olika exceptions
} 
catch (ArithmeticException e) {
    System.out.println("Matematiskt fel: " + e.getMessage());
} 
catch (InputMismatchException e) {
    System.out.println("Ogiltig input: " + e.getMessage());
} 
catch (Exception e) {  // "Catch-all" - sist!
    System.out.println("Något gick fel: " + e.getMessage());
}
```

**⚠️ VIKTIGT - Ordning måste vara från SPECIFIK till GENERELL:**

```java
// ❌ KOMPILERINGSFEL - generell före specifik
catch (Exception e) {  // Fångar ALLT
    // ...
}
catch (ArithmeticException e) {  // Denna nås ALDRIG - kompileringsfel!
    // ...
}

// ✅ KORREKT - specifik före generell
catch (ArithmeticException e) {  // Specifik först
    // ...
}
catch (Exception e) {  // Generell sist
    // ...
}
```

### 🎭 **Multi-catch (Java 7+)**

Istället för flera identiska catch-block kan du fånga flera exception-typer samtidigt:

**Syntax:**

```java
catch (ExceptionType1 | ExceptionType2 | ExceptionType3 e) {
    // Hantera alla tre typerna identiskt
}
```

**Exempel:**

```java
// FÖRE Java 7 (repetitivt):
try {
    // kod
} 
catch (IOException e) {
    System.err.println("Fel: " + e.getMessage());
} 
catch (SQLException e) {
    System.err.println("Fel: " + e.getMessage());
}

// JAVA 7+ (bättre):
try {
    // kod
} 
catch (IOException | SQLException e) {
    System.err.println("Fel: " + e.getMessage());
}
```

**📌 Multi-catch regler:**

- Exception-parametern är **final** (kan inte ändras)
- Typerna får inte ha arvsrelation (inte subklass av varandra)

### 🔒 **finally-Block**

**Syntax:**

```java
finally {
    // Kod som ALLTID körs
    // Perfekt för att frigöra resurser
}
```

**🔑 Nyckelkoncept - finally körs ALLTID:**

1. ✅ Om try-block lyckas
2. ✅ Om exception kastas och fångas
3. ✅ Om exception kastas men INTE fångas
4. ✅ Även om catch-block kastar ny exception
5. ✅ Även om return-statement finns i try/catch

**När används finally?**

- **Frigöra resurser:** stänga filer, databas-connections, nätverksanslutningar
- **Cleanup-kod:** återställa tillstånd
- **Logging:** alltid logga vad som hände

**Komplett exempel:**

```java
public void processFile() {
    FileReader file = null;
    
    try {
        file = new FileReader("data.txt");
        // Läs från fil
        int result = 10 / 0;  // Exception!
    } 
    catch (IOException e) {
        System.err.println("Fil-fel: " + e);
    } 
    catch (ArithmeticException e) {
        System.err.println("Matematik-fel: " + e);
        throw e;  // Kasta vidare!
    } 
    finally {
        // Körs ALLTID - även om exception kastas vidare!
        if (file != null) {
            try {
                file.close();  // Stäng filen!
            } 
            catch (IOException e) {
                System.err.println("Kunde inte stänga fil");
            }
        }
        System.out.println("Finally-block körde");
    }
    
    System.out.println("Efter try-catch-finally");
}
```

**⚠️ VARNING:** Undvik att kasta exceptions från finally-block!

```java
finally {
    throw new Exception();  // DÅLIGT - förstör original-exception!
}
```

### 🔄 **Termination Model**

Java använder **termination model** för exception handling:

**Vad betyder det?**

- När exception kastas, **terminerar try-block omedelbart**
- Kontroll returnerar ALDRIG till throw point
- Efter catch-block, fortsätter programmet EFTER try-catch

**Jämför med resumption model (används inte i Java):**

```
Termination (Java):           Resumption (inte Java):
try {                         try {
    line 1                        line 1
    line 2 -> EXCEPTION!          line 2 -> EXCEPTION!
    line 3 (SKIP)                 line 3 (FORTSÄTT HÄR)
}                             }
catch {                       catch {
    handle                        handle
}                             }
continue here                 
```

---

## DEL 3: KASTA OCH ÅTERKASTA EXCEPTIONS 🎯

### 🚀 **throw-Statement**

**Syntax:**

```java
throw new ExceptionType("Felmeddelande");
```

**När ska du kasta exceptions?**

1. Din metod upptäcker fel den inte kan hantera
2. Ogiltiga parametrar till metod
3. Constructor får ogiltig data (förhindrar skapande av ogiltigt objekt)
4. Precondition/Postcondition inte uppfylld

**Exempel:**

```java
public void setAge(int age) {
    if (age < 0 || age > 150) {
        throw new IllegalArgumentException(
            "Ålder måste vara 0-150, fick: " + age
        );
    }
    this.age = age;
}

public double calculatePercentage(int score, int total) {
    if (total == 0) {
        throw new ArithmeticException("Total kan inte vara noll");
    }
    return (double) score / total * 100;
}
```

**📌 Best practices:**

- Ge **tydliga felmeddelanden** som förklarar problemet
- Kasta **specifika exception-typer** (inte bara Exception)
- Kasta från **constructors** för att förhindra ogiltiga objekt

### 📢 **throws-Clause (Deklarera exceptions)**

**Syntax:**

```java
public void methodName() throws ExceptionType1, ExceptionType2 {
    // Metod som KAN kasta dessa exceptions
}
```

**🔑 Viktigt att förstå:**

- `throws` i metodsignatur = "denna metod KAN kasta dessa exceptions"
- **MÅSTE användas** för checked exceptions
- Frivilligt för unchecked exceptions (men bra dokumentation)
- Kan lista flera exceptions separerade med komma

**Exempel:**

```java
// Metod som läser från fil
public String readFile(String filename) throws IOException {
    FileReader file = new FileReader(filename);  // Kan kasta IOException
    // ...
}

// Metod som anropar readFile MÅSTE hantera eller deklarera
public void processData() throws IOException {  // Deklarera vidare
    String data = readFile("data.txt");
}

// ELLER fånga det:
public void processData() {
    try {
        String data = readFile("data.txt");
    } 
    catch (IOException e) {
        System.out.println("Kunde inte läsa fil");
    }
}
```

### 🔁 **Rethrowing Exceptions (Återkasta)**

**Varför återkasta?**

- Catch-block kan bara **delvis** hantera exception
- Vill logga/cleanup lokalt, men låta anropare hantera huvudproblemet
- Kedja exceptions (se senare)

**Syntax:**

```java
catch (ExceptionType e) {
    // Partial handling
    throw e;  // Återkasta SAMMA exception
}
```

**Komplett exempel:**

```java
public void outerMethod() {
    try {
        innerMethod();
    } 
    catch (Exception e) {
        System.err.println("Outer fångade: " + e);
    }
}

public void innerMethod() throws Exception {
    try {
        System.out.println("Försöker något farligt...");
        throw new Exception("Problem uppstod!");
    } 
    catch (Exception e) {
        System.err.println("Inner fångade först: " + e);
        throw e;  // Återkasta till outer!
    } 
    finally {
        System.out.println("Cleanup i inner");  // Körs före återkastning!
    }
}
```

**Output:**

```
Försöker något farligt...
Inner fångade först: java.lang.Exception: Problem uppstod!
Cleanup i inner
Outer fångade: java.lang.Exception: Problem uppstod!
```

**⚠️ VIKTIGT:** `finally` körs INNAN exception återkastas!

---

## DEL 4: STACK UNWINDING & DIAGNOSTIK 🔍

### 📚 **Stack Unwinding Process**

**Vad är stack unwinding?** När exception kastas men inte fångas i en metod, "rullas" method-call stacken av:

```
main()
  └─> methodA()
        └─> methodB()
              └─> methodC()  -> EXCEPTION! (inte fångad här)
              
Stack unwinding:
1. methodC terminerar (lokala variabler förstörs)
2. Försök fånga i methodB
3. Om inte fångad, methodB terminerar
4. Försök fånga i methodA
5. Om inte fångad, methodA terminerar
6. Försök fånga i main
7. Om inte fångad i main -> program terminerar
```

**Exempel:**

```java
public static void main(String[] args) {
    try {
        methodA();
    } 
    catch (Exception e) {
        System.out.println("Fångad i main!");
        e.printStackTrace();  // Visa hela stack trace
    }
}

public static void methodA() {
    methodB();  // Inte fångad här -> unwinding
}

public static void methodB() {
    methodC();  // Inte fångad här -> unwinding
}

public static void methodC() {
    throw new Exception("Fel i methodC!");  // Kastas här!
}
```

**Output:**

```
Fångad i main!
java.lang.Exception: Fel i methodC!
    at Example.methodC(Example.java:19)
    at Example.methodB(Example.java:15)
    at Example.methodA(Example.java:11)
    at Example.main(Example.java:5)
```

### 🔍 **printStackTrace() - Stack Trace**

**Stack trace** visar exakt vägen exception tog genom programmet:

```java
catch (Exception e) {
    e.printStackTrace();  // Skriver till System.err
}
```

**Vad visar en stack trace?**

```
Exception-typ: Meddelande
    at ClassName.methodName(FileName.java:lineNumber)
    at ClassName.methodName(FileName.java:lineNumber)
    ...
```

### 📊 **getStackTrace() - Programmatisk Access**

För mer kontroll, använd `getStackTrace()`:

```java
catch (Exception e) {
    StackTraceElement[] trace = e.getStackTrace();
    
    for (StackTraceElement element : trace) {
        System.out.printf("Klass: %s%n", element.getClassName());
        System.out.printf("Metod: %s%n", element.getMethodName());
        System.out.printf("Fil: %s%n", element.getFileName());
        System.out.printf("Rad: %d%n", element.getLineNumber());
        System.out.println("---");
    }
}
```

### 💬 **getMessage() - Exception Message**

Hämta det beskrivande meddelandet:

```java
catch (Exception e) {
    String message = e.getMessage();  // "Kan inte dela med noll"
    System.out.println("Fel: " + message);
}
```

**toString() vs getMessage():**

```java
Exception e = new Exception("Problem uppstod");

e.getMessage();  // "Problem uppstod"
e.toString();    // "java.lang.Exception: Problem uppstod"
```

---

## DEL 5: AVANCERADE KONCEPT 🚀

### 🔗 **Chained Exceptions**

**Problem:** Du vill kasta ny exception men behålla original-information.

**Lösning:** Chain exceptions!

**Syntax:**

```java
catch (OriginalException original) {
    throw new NewException("Nytt meddelande", original);  // Chain!
}
```

**Exempel:**

```java
public void loadUserData(int userId) throws DataException {
    try {
        // Försök läsa från databas
        connection.query("SELECT * FROM users WHERE id = " + userId);
    } 
    catch (SQLException original) {
        // Wrap SQL-exception i domain-specific exception
        throw new DataException(
            "Kunde inte ladda användare: " + userId, 
            original  // Bevara original exception!
        );
    }
}

// Anropare kan se BÅDE exceptions:
try {
    loadUserData(123);
} 
catch (DataException e) {
    System.out.println("Problem: " + e.getMessage());
    System.out.println("Orsak: " + e.getCause());  // Original SQLException!
}
```

**Varför är detta användbart?**

- Bevara **komplett** debug-information
- Presentera **användarvänligt** meddelande
- **Abstrahera** implementation details (SQL -> generisk DataException)

### 🎨 **Custom Exception Classes**

**När ska du skapa egna exceptions?**

- Din applikation har **specifika** feltyper
- Vill **gruppera** relaterade fel
- Behöver **extra data** i exception

**Best Practice - Skapa två versioner:**

```java
// Version 1: Endast meddelande
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

// Version 2: Med cause (för chaining)
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
    
    public InsufficientFundsException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

**Med extra data:**

```java
public class InsufficientFundsException extends Exception {
    private double balance;
    private double amount;
    
    public InsufficientFundsException(double balance, double amount) {
        super(String.format(
            "Insufficient funds: Balance=%.2f, Needed=%.2f", 
            balance, amount
        ));
        this.balance = balance;
        this.amount = amount;
    }
    
    public double getBalance() { return balance; }
    public double getAmount() { return amount; }
}
```

**Användning:**

```java
public void withdraw(double amount) throws InsufficientFundsException {
    if (amount > balance) {
        throw new InsufficientFundsException(balance, amount);
    }
    balance -= amount;
}

// Anropare kan använda extra data:
try {
    account.withdraw(1000);
} 
catch (InsufficientFundsException e) {
    System.out.println("Behöver: " + e.getAmount());
    System.out.println("Har: " + e.getBalance());
    System.out.println("Saknar: " + (e.getAmount() - e.getBalance()));
}
```

### ✅ **Preconditions & Postconditions**

**Definitioner:**

- **Precondition** = Villkor som MÅSTE vara sant när metod anropas
- **Postcondition** = Villkor som MÅSTE vara sant när metod returnerar

**Best Practice - Dokumentera i comments:**

```java
/**
 * Beräknar kvadratroten av ett tal.
 * 
 * @param number talet att beräkna roten för
 * @return kvadratroten av number
 * @throws IllegalArgumentException om number < 0
 * 
 * Precondition: number >= 0
 * Postcondition: result * result == number (med floating-point precision)
 */
public double sqrt(double number) {
    if (number < 0) {
        throw new IllegalArgumentException(
            "Precondition violation: number måste vara >= 0"
        );
    }
    
    double result = Math.sqrt(number);
    
    // Validera postcondition i debug-mode
    assert Math.abs(result * result - number) < 0.0001 
        : "Postcondition violation: felaktig beräkning";
    
    return result;
}
```

### 🐛 **Assertions**

**Syntax:**

```java
assert condition : "Meddelande om condition är false";
```

**⚠️ VIKTIGT:** Assertions är **avstängda** som standard!

**Aktivera assertions:**

```bash
java -ea MyProgram    # Enable assertions
java -ea:MyClass MyProgram    # Enable för specifik klass
```

**När ska assertions användas?**

- ✅ **Internal invariants** (tillstånd som alltid ska vara sant)
- ✅ **Control flow invariants** (kod som aldrig ska nås)
- ✅ **Preconditions/Postconditions** (under utveckling)
- ❌ INTE för validering av user input!
- ❌ INTE för validering av public method arguments!

**Exempel:**

```java
public void processPositiveNumber(int n) {
    assert n > 0 : "n måste vara positivt: " + n;
    
    // Internal logic
    int result = calculate(n);
    
    assert result != 0 : "result får inte vara noll";
    
    return result;
}

// Control flow assertion
switch (dayOfWeek) {
    case MONDAY:
    case TUESDAY:
    case WEDNESDAY:
    case THURSDAY:
    case FRIDAY:
        return "Weekday";
    case SATURDAY:
    case SUNDAY:
        return "Weekend";
    default:
        assert false : "Ogiltig dag!";  // Ska ALDRIG nås
        return null;
}
```

### 🔧 **try-with-resources**

**Problem:** Glömmer ofta att stänga resurser i finally-block

**Lösning:** try-with-resources gör det automatiskt!

**Syntax:**

```java
try (ResourceType resource = new ResourceType()) {
    // Använd resource
}  // resource.close() anropas AUTOMATISKT!
catch (Exception e) {
    // Hantera exceptions
}
```

**Krav:** Resource måste implementera `AutoCloseable` interface

**Exempel - Single resource:**

```java
// FÖRE try-with-resources (Java 6):
FileReader file = null;
try {
    file = new FileReader("data.txt");
    // Läs från fil
} 
catch (IOException e) {
    e.printStackTrace();
} 
finally {
    if (file != null) {
        try {
            file.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// MED try-with-resources (Java 7+):
try (FileReader file = new FileReader("data.txt")) {
    // Läs från fil
}  // close() anropas automatiskt!
catch (IOException e) {
    e.printStackTrace();
}
```

**Multiple resources:**

```java
try (
    FileReader input = new FileReader("input.txt");
    FileWriter output = new FileWriter("output.txt");
    BufferedReader buffer = new BufferedReader(input)
) {
    String line;
    while ((line = buffer.readLine()) != null) {
        output.write(line + "\n");
    }
}  // Alla tre close() anropas i omvänd ordning!
catch (IOException e) {
    e.printStackTrace();
}
```

**Fördelar:**

1. ✅ Kortare, renare kod
2. ✅ Kan INTE glömma close()
3. ✅ Hanterar suppressed exceptions korrekt
4. ✅ Resources stängs i omvänd ordning av öppning

---

## 📋 SAMMANFATTNING: BEST PRACTICES

### ✅ **DO (Gör):**

1. Fånga **specifika** exceptions först, generella sist
2. Använd **finally** för resource cleanup (eller try-with-resources)
3. Ge **tydliga** felmeddelanden i exceptions
4. Kasta exceptions från **constructors** vid ogiltig data
5. Dokumentera exceptions med **@throws** i Javadoc
6. Använd **chained exceptions** för att bevara context
7. Skapa **custom exceptions** för domain-specific errors
8. Använd **assertions** för internal validation (utveckling)

### ❌ **DON'T (Gör inte):**

1. Använd INTE exceptions för **normal kontrollflöde**
2. Fånga INTE `Error` (låt JVM hantera)
3. Fånga INTE bara `Exception` utan specifika typer
4. Kasta INTE nya exceptions från **finally** (förstör original!)
5. Använd INTE **empty catch blocks** (log åtminstone!)
6. Ignorera INTE exceptions (tysta dem inte)
7. Kasta INTE exceptions i **close()** methods om möjligt

### 🎯 **Exception Handling Pattern:**

```java
// KOMPLETT EXEMPEL med alla best practices:
public class DataProcessor {
    
    /**
     * Processar användardata från fil.
     * 
     * @param filename fil att läsa från
     * @throws DataProcessingException om något går fel
     */
    public void processFile(String filename) throws DataProcessingException {
        // Precondition
        if (filename == null || filename.isEmpty()) {
            throw new IllegalArgumentException("Filename får inte vara null eller tom");
        }
        
        // try-with-resources för automatic cleanup
        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            
            String line;
            while ((line = reader.readLine()) != null) {
                processLine(line);
            }
            
            // Postcondition check
            assert dataWasProcessed() : "Data processades inte korrekt";
            
        } 
        catch (FileNotFoundException e) {
            // Chain exception med context
            throw new DataProcessingException(
                "Kunde inte hitta fil: " + filename, 
                e
            );
        } 
        catch (IOException e) {
            throw new DataProcessingException(
                "IO-fel vid läsning av: " + filename, 
                e
            );
        }
        // Inget finally behövs - try-with-resources stänger automatiskt!
    }
    
    private void processLine(String line) throws DataProcessingException {
        try {
            // Process line
            if (line.trim().isEmpty()) {
                throw new IllegalArgumentException("Tom rad");
            }
            // ...
        } 
        catch (Exception e) {
            throw new DataProcessingException("Kunde inte processa rad: " + line, e);
        }
    }
    
    private boolean dataWasProcessed() {
        // Validation logic
        return true;
    }
}
```

---

## 🎓 NYCKELSKILLNADER: PYTHON VS JAVA

|Koncept|Python|Java|
|---|---|---|
|**Exception hierarchy**|Alla ärver från `BaseException`|Alla ärver från `Throwable`|
|**Checked exceptions**|Finns INTE|MÅSTE hanteras eller deklareras|
|**try-catch syntax**|`try/except`|`try/catch`|
|**finally**|`finally`|`finally` (samma!)|
|**Kasta exception**|`raise Exception()`|`throw new Exception()`|
|**Multiple exceptions**|`except (Type1, Type2):`|Multi-catch: `catch (Type1 \| Type2 e)`|
|**Resource management**|`with` statement|`try-with-resources`|
|**Assertions**|`assert condition`|`assert condition : message`|

---

**🎉 Du är nu redo att hantera exceptions som en proffs i Java! Kom ihåg: Exception handling handlar om att skriva robust, felsäker kod som kan hantera oväntade situationer på ett elegant sätt. 🚀**