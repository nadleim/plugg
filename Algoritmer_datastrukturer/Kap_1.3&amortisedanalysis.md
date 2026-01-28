# 📚 Elementära Datastrukturer och Amorterad Analys
## Komplett Sammanfattning

**Baserat på:**
- Sedgewick & Wayne: Algorithms 4th Ed., Avsnitt 1.3 och 1.4
- Thore Husfeldt: "Amortised Analysis" (supplement till kursboken)

---

# Del 1: Elementära Datastrukturer (Avsnitt 1.3)

---

## 🎯 Introduktion

### Varför Studera Dessa Datastrukturer?

Bags, queues och stacks är **fundamentala abstrakta datatyper (ADT:er)** som är viktiga av tre skäl:

1. **Byggstenar** – Används för att konstruera mer komplexa datastrukturer
2. **Illustration** – Visar samspelet mellan datastrukturer och algoritmer
3. **Utgångspunkter** – Grunden för att utveckla kraftfullare ADT:er

### Två Huvudmål

1. **Operationer oberoende av storlek** – Varje operation ska ta konstant tid
2. **Minnesutrymme proportionellt mot storlek** – Aldrig slösa för mycket minne

---

## 📋 API:er för Grundläggande Samlingar

### Stack (LIFO – Last In, First Out)

```java
public class Stack<Item> implements Iterable<Item>
    Stack()                // Skapa en tom stack
    void push(Item item)   // Lägg till element överst
    Item pop()             // Ta bort och returnera översta elementet
    boolean isEmpty()      // Är stacken tom?
    int size()             // Antal element i stacken
```

### Queue (FIFO – First In, First Out)

```java
public class Queue<Item> implements Iterable<Item>
    Queue()                 // Skapa en tom kö
    void enqueue(Item item) // Lägg till element sist
    Item dequeue()          // Ta bort och returnera första elementet
    boolean isEmpty()       // Är kön tom?
    int size()              // Antal element i kön
```

### Bag (Påse)

```java
public class Bag<Item> implements Iterable<Item>
    Bag()                  // Skapa en tom bag
    void add(Item item)    // Lägg till ett element
    boolean isEmpty()      // Är bag:en tom?
    int size()             // Antal element i bag:en
```

---

## 📏 Array-baserad Implementation

### FixedCapacityStack – Fast Storlek

```java
public class FixedCapacityStack<Item> {
    private Item[] a;    // stack-element
    private int N;       // antal element
    
    public FixedCapacityStack(int cap) {
        a = (Item[]) new Object[cap];
    }
    
    public boolean isEmpty() { return N == 0; }
    public int size()        { return N; }
    
    public void push(Item item) { a[N++] = item; }
    public Item pop()           { return a[--N]; }
}
```

### Problem med Fast Kapacitet

```
┌─────────────────────────────────────────────────────────────────────┐
│  PROBLEM MED FIXEDCAPACITYSTACK                                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ✗ Klienten måste gissa maxstorlek i förväg                        │
│                                                                     │
│  ✗ Slösar minne om samlingen oftast är liten                       │
│                                                                     │
│  ✗ Overflow om samlingen växer för stor                            │
│                                                                     │
│  Behöver: DYNAMISK storleksändring!                                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📈 Resizing Array – Dynamisk Storleksändring

### Grundidé

- **Dubbla** arrayens storlek när den blir full
- **Halvera** arrayens storlek när den är 25% full

### Implementation (Algoritm 1.1)

```java
public class ResizingArrayStack<Item> implements Iterable<Item> {
    private Item[] a = (Item[]) new Object[1];  // stack items
    private int N = 0;                           // antal element
    
    public boolean isEmpty() { return N == 0; }
    public int size()        { return N; }
    
    private void resize(int max) {
        // Flytta stacken till en ny array av storlek max
        Item[] temp = (Item[]) new Object[max];
        for (int i = 0; i < N; i++)
            temp[i] = a[i];
        a = temp;
    }
    
    public void push(Item item) {
        // Lägg till element överst på stacken
        if (N == a.length) resize(2 * a.length);  // DUBBLA!
        a[N++] = item;
    }
    
    public Item pop() {
        // Ta bort element från toppen av stacken
        Item item = a[--N];
        a[N] = null;  // Undvik loitering
        if (N > 0 && N == a.length/4) resize(a.length/2);  // HALVERA!
        return item;
    }
}
```

### Varför Halvera vid 25% (och inte 50%)?

```
┌─────────────────────────────────────────────────────────────────────┐
│  VARFÖR HALVERA VID 25%?                                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  OM VI HALVERAR VID 50% (Algoritm 1.1'):                           │
│  ─────────────────────────────────────────                         │
│  push → full → resize(2N) → pop → resize(N/2) → push → resize(2N) │
│                                                                     │
│  = THRASHING! Varje operation tar O(N) tid!                        │
│                                                                     │
│  OM VI HALVERAR VID 25% (Algoritm 1.1):                            │
│  ─────────────────────────────────────────                         │
│  Arrayen är alltid mellan 25% och 100% full                        │
│  Efter dubblering: 50% full                                        │
│  Efter halvering: 50% full                                         │
│                                                                     │
│  = Amorterad KONSTANT tid per operation!                           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

> ⚠️ **Viktigt:** Detta är exakt skillnaden mellan Algoritm 1.1 (bra) och Algoritm 1.1' (dålig) som Husfeldt beskriver!

### Loitering (Minnesläcka)

**Problem:** När vi poppar, finns referensen kvar i arrayen.

```java
// FEL - orsakar loitering
public Item pop() {
    return a[--N];  // a[N] pekar fortfarande på objektet!
}

// RÄTT - förhindrar loitering
public Item pop() {
    Item item = a[--N];
    a[N] = null;    // Släpp referensen för garbage collection
    return item;
}
```

---

## 🔗 Länkade Listor

### Definition

> **Definition:** En **länkad lista** är en rekursiv datastruktur som antingen är tom (null) eller en referens till en nod som innehåller ett generiskt element och en referens till en länkad lista.

### Node-klassen

```java
private class Node {
    Item item;   // Datat
    Node next;   // Referens till nästa nod
}
```

### Visualisering

```
┌─────────────────────────────────────────────────────────────────────┐
│  first                                                              │
│    │                                                                │
│    ▼                                                                │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐                         │
│  │  "to"   │───▶│  "be"   │───▶│  "or"   │───▶ null                │
│  └─────────┘    └─────────┘    └─────────┘                         │
└─────────────────────────────────────────────────────────────────────┘
```

### Stack med Länkad Lista (Algoritm 1.2)

```java
public class Stack<Item> implements Iterable<Item> {
    private Node first;  // Toppen av stacken
    private int N;       // Antal element
    
    private class Node {
        Item item;
        Node next;
    }
    
    public boolean isEmpty() { return first == null; }
    public int size()        { return N; }
    
    public void push(Item item) {
        Node oldfirst = first;
        first = new Node();
        first.item = item;
        first.next = oldfirst;
        N++;
    }
    
    public Item pop() {
        Item item = first.item;
        first = first.next;
        N--;
        return item;
    }
}
```

### Queue med Länkad Lista (Algoritm 1.3)

```java
public class Queue<Item> implements Iterable<Item> {
    private Node first;  // Äldsta elementet
    private Node last;   // Nyaste elementet
    private int N;
    
    private class Node {
        Item item;
        Node next;
    }
    
    public void enqueue(Item item) {
        Node oldlast = last;
        last = new Node();
        last.item = item;
        last.next = null;
        if (isEmpty()) first = last;
        else           oldlast.next = last;
        N++;
    }
    
    public Item dequeue() {
        Item item = first.item;
        first = first.next;
        if (isEmpty()) last = null;
        N--;
        return item;
    }
}
```

### Proposition D – Länkade Listor

> **Proposition D:** I länkade list-implementationerna av Bag (Algoritm 1.4), Stack (Algoritm 1.2) och Queue (Algoritm 1.3) tar alla operationer **konstant tid i värsta fall**.

**Bevis:** Direkt från koden – antalet instruktioner för varje operation är begränsat av en liten konstant. (Förutsätter att Java skapar nya Node-objekt i konstant tid.)

---

## 📊 Jämförelse: Resizing Array vs Länkad Lista

```
┌────────────────────────────────────────────────────────────────────┐
│         RESIZING ARRAY vs LÄNKAD LISTA                            │
├─────────────────────┬──────────────────────┬──────────────────────┤
│  Aspekt             │  Resizing Array      │  Länkad Lista        │
├─────────────────────┼──────────────────────┼──────────────────────┤
│  Värsta fall/op     │  O(N) vid resize     │  O(1) konstant ✓     │
├─────────────────────┼──────────────────────┼──────────────────────┤
│  Amorterat/op       │  O(1) konstant ✓     │  O(1) konstant ✓     │
├─────────────────────┼──────────────────────┼──────────────────────┤
│  Indexerad åtkomst  │  O(1) ✓              │  O(N) ✗              │
├─────────────────────┼──────────────────────┼──────────────────────┤
│  Minne per element  │  ~8 bytes (referens) │  ~40 bytes (nod)     │
├─────────────────────┼──────────────────────┼──────────────────────┤
│  Cache-prestanda    │  Utmärkt ✓           │  Dålig ✗             │
└─────────────────────┴──────────────────────┴──────────────────────┘
```

---

# Del 2: Amorterad Analys

---

## 🎯 Vad Är Amorterad Analys?

### Terminologi från Finansvärlden

Ordet "amortize" kommer från latin *ad mortis* ("till döden") och används i finans för att "skriva av" kostnader över tid.

> **Idé:** Även om en enskild operation kan vara dyr, kan vi "skriva av" den dyra kostnaden över många billiga operationer.

### Formell Definition

> **Definition:** Låt T(N) vara den totala körtiden för en sekvens av N operationer. **Den amorterade tiden per operation** är T(N)/N.

### Varför Inte Bara "Genomsnitt"?

```
┌─────────────────────────────────────────────────────────────────────┐
│  "GENOMSNITT" KAN BETYDA MÅNGA SAKER:                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. Genomsnitt över alla möjliga INDATASEKVENSER                   │
│     (t.ex. push/pop med lika sannolikhet)                          │
│                                                                     │
│  2. Genomsnitt över alla möjliga INDATA                            │
│     (kallas "average case complexity")                              │
│                                                                     │
│  3. Förväntat värde för RANDOMISERADE algoritmer                   │
│     (kallas "expected running time")                                │
│                                                                     │
│  4. Total tid / antal operationer i VÄRSTA FALL                    │
│     ← Detta är AMORTERAD TID!                                      │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Amorterad tid** är ett specifikt, precist värsta-fall-genomsnitt över antalet operationer.

### Viktigt: Betalning i Förväg!

I bank är det OK att låna pengar först och betala av senare. I algoritmanalys måste vi **alltid betala i förväg** – varje dyr operation måste redan vara betald av tidigare billiga operationer.

> 💡 **Analogi:** "Spargrisanalys" (*piggy bank analysis*) är en bättre term!

---

## 📝 Proposition A1 (Endast Push)

> **Proposition A1:** I resizing array-implementationen av Stack (Algoritm 1.1) är det genomsnittliga antalet arrayåtkomster per operation konstant i värsta fall, för **godtycklig sekvens av push()-operationer** som startar från en tom datastruktur.

### Varför "från tom datastruktur"?

**Motexempel:** Börja med en stack som är resultatet av 2^k - 1 push-operationer.
- N = 2^k - 1
- a.length = 2^k

En enda push() till kommer orsaka resize() med linjärt antal arrayåtkomster!

### Varför "genomsnitt"?

**Motexempel:** Samma exempel – en enskild push() tar linjär tid.

---

## 🐷 Proposition E – Fullständigt Bevis

> **Proposition E:** I resizing array-implementationen av Stack (Algoritm 1.1) är det amorterade antalet arrayåtkomster per operation konstant i värsta fall, för godtycklig sekvens av operationer som startar från en tom datastruktur.

### Bevismetod: Spargrisanalys (Piggy Bank)

**Idé:** Varje operation sätter in ett antal mynt i spargrisen. De dyra resize()-operationerna betalas sedan från spargrisen.

### Steg 1: Bestäm Insättningsbeloppen

```
┌─────────────────────────────────────────────────────────────────────┐
│  INSÄTTNINGAR I SPARGRISEN                                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  push() sätter in 8 mynt                                           │
│  pop()  sätter in 4 mynt                                           │
│                                                                     │
│  (Mer exakt: push() kostar 9 mynt totalt, varav 1 för själva       │
│   arrayåtkomsten och 8 i spargrisen. pop() kostar 6 mynt totalt,   │
│   varav 2 för arrayåtkomsterna och 4 i spargrisen.)                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Steg 2: Analysera resize()

**Kostnad för resize(max):** max + 2N arrayåtkomster

**Observation:** När resize() anropas gäller max = 2N:
- Från push(): max = 2 * a.length = 2N
- Från pop(): max = a.length/2 = 2 * (a.length/4) = 2N

**Således kostar varje resize():** 4N mynt

### Steg 3: Tillstånd Efter resize()

```
┌─────────────────────────────────────────────────────────────────────┐
│  TILLSTÅND DIREKT EFTER resize()                                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  a[] = [■][■][■]...[■][ ][ ][ ]...[ ]                              │
│         ├────N────┤├────N────┤                                     │
│         upptagna   lediga                                          │
│                                                                     │
│  Arrayen är EXAKT 50% full!                                        │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Steg 4: Visa att Spargrisen Räcker

**Fall 1: resize() från push() (dubblering vid overflow)**

```
┌─────────────────────────────────────────────────────────────────────┐
│  FÖRE push() som orsakar resize():                                 │
│                                                                     │
│  a[] = [■][■][■]...[■]                                             │
│         ├────N────┤                                                │
│         arrayen är FULL                                            │
│                                                                     │
│  Sedan senaste resize():                                           │
│  • Arrayen var 50% full (N/2 element)                              │
│  • Nu är den 100% full (N element)                                 │
│  • Alltså: minst N/2 push()-operationer har gjorts                 │
│  • Insatt: N/2 × 8 = 4N mynt ✓                                     │
│                                                                     │
│  Kostnad för resize(): 4N mynt ✓                                   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Fall 2: resize() från pop() (halvering vid underflow)**

```
┌─────────────────────────────────────────────────────────────────────┐
│  FÖRE pop() som orsakar resize():                                  │
│                                                                     │
│  a[] = [■][■]...[■][ ][ ][ ]...[ ][ ][ ][ ]...[ ]                  │
│         ├───N───┤├──────────3N─────────────┤                       │
│         25% full    75% tomt                                       │
│                                                                     │
│  Sedan senaste resize():                                           │
│  • Arrayen var 50% full (2N element i array av storlek 4N)         │
│  • Nu är den 25% full (N element)                                  │
│  • Alltså: minst N pop()-operationer har gjorts                    │
│  • Insatt: N × 4 = 4N mynt ✓                                       │
│                                                                     │
│  Kostnad för resize(): 4N mynt ✓                                   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Slutsats

Varje resize() är redan betald när den inträffar. Således är den amorterade kostnaden per operation **konstant**. ∎

---

## 📍 Lokaliserad Spargrisanalys

### Alternativ Metod

Istället för en global spargris kan vi placera mynten **i själva datastrukturen**.

**push():** Placerar 8 mynt i cellen den just fyllde

**pop():** Placerar 4 mynt i cellen den just tömde

### Visualisering

```
a[] = [ to ][ be ][ or ][ not ][null][null][null][null]
      N=4, a.length=8

pop(); → returnerar "not", placerar 4 mynt i cell 3
pop(); → returnerar "or", placerar 4 mynt i cell 2

a[] = [ to ][ be ][$$$][$$$][null][null][null][null]
      N=2, a.length=8
      ($$$ = 4 mynt vardera)

Nästa pop() → N blir 1 = 8/8 → resize()!
Kostnad: 4×2 = 8 mynt
Vi har: 4 + 4 = 8 mynt ✓
```

### Intuition för Lokala Mynt

När dubblering sker:
- Mynten i cellerna N/2 till N-1 betalar för att allokera nya celler
- Och för att kopiera sina egna värden

När halvering sker:
- Mynten i de tomma cellerna betalar för att skapa den nya arrayen
- Och för att kopiera de kvarvarande elementen

---

## ❌ Algoritm 1.1' – Varför Det Går Fel

### Modifierad Version (Halvering vid 50%)

```java
// ALGORITM 1.1' (FEL!)
public Item pop() {
    Item item = a[--N];
    a[N] = null;
    if (N > 0 && N == a.length/2) resize(a.length/2);  // ← Ändring!
    return item;
}
```

### Övning 1 (Husfeldt): Visa Kvadratisk Tid

**Sekvens som ger kvadratiskt antal arrayåtkomster:**

```
Starta med N = 2^k element (array är full)

Gör upprepade gånger:
  pop()  → N = 2^k - 1, array halveras till 2^k, kostar 2^k
  push() → N = 2^k, array full, dubbleras till 2^(k+1), kostar 2^(k+1)
  pop()  → N = 2^k - 1, array halveras...

Varje par av operationer kostar ~3 × 2^k arrayåtkomster!
```

**Resultat:** M operationer kan kosta ~M × 2^k = O(M × N) arrayåtkomster.

Detta är **linjärt per operation** i värsta fall, inte konstant!

---

## 🔢 Mekanisk Räknare (Bonus-exempel)

### Problem

En mekanisk räknare har siffror 0-9. När en siffra går från 9 till 0, roteras även nästa siffra.

**Fråga:** Vad är den amorterade kostnaden per inkrement?

### Proposition C

> **Proposition C:** Med start från 0 kräver en mekanisk räknare ett konstant amorterat antal operationer per inkrement.

### Bevis (Aggregeringsmetoden)

Låt N vara totalt antal inkrement. Räknaren har k = ⌊log₁₀ N⌋ + 1 siffror.

- Siffra 0 (enor) roteras N gånger
- Siffra 1 (tior) roteras ⌊N/10⌋ gånger
- Siffra r roteras ⌊N/10^r⌋ gånger

**Totalt antal rotationer:**

```
⌊N/10⁰⌋ + ⌊N/10¹⌋ + ... + ⌊N/10^(k-1)⌋

≤ N/1 + N/10 + N/100 + ...

= N × (1 + 1/10 + 1/100 + ...)

= N × (10/9)

< 2N
```

**Amorterad kostnad:** < 2N / N = 2 = O(1) konstant! ∎

---

## 📋 Övningar från Husfeldt

### Övning 2: Tivoli-biljett

> En 10-åkningsbiljett på Tivoli kostar 200 DKK.
> - Värsta fallet för en enskild åktur?
> - Amorterad kostnad per åktur?

**Svar:**
- Värsta fall: 200 DKK (om du bara åker en gång)
- Amorterad: 200/10 = 20 DKK per åktur

### Övning 3: Bob löparen

> Bob springer 6 km varje dag utom söndag.
> - Amorterad km per dag?
> - Värsta fall km per dag?

**Svar:**
- Amorterad: (6 × 6) / 7 = 36/7 ≈ 5.14 km/dag
- Värsta fall: 6 km (om vi räknar lata dagar som dåliga: 0 km)

### Övning 4: Ran med tärning

> Ran kastar en tärning varje morgon och springer så många km.
> - Genomsnittligt antal km per dag?
> - Värsta fall?
> - Amorterat?

**Svar:**
- Genomsnitt (förväntningsvärde): (1+2+3+4+5+6)/6 = 3.5 km
- Värsta fall: 6 km
- Amorterat: Samma som genomsnitt, 3.5 km (randomiserad process)

### Övning 5: Kan vi lägga allt på push()?

> Om pop() inte betalar något, hur mycket måste push() betala?

**Svar:** push() måste betala för:
- Sin egen arrayåtkomst: 1 mynt
- Potential dubblering: 4 mynt per element
- Potential halvering (orsakad av framtida pops): 4 mynt per element

**Totalt:** push() måste betala **12 mynt**.

### Övning 6: Kan vi lägga allt på pop()?

> Om push() inte betalar något, hur mycket måste pop() betala?

**Svar:** Detta fungerar **inte**! Om vi gör N push-operationer utan pop, och sedan en resize sker, har vi 0 mynt i spargrisen men behöver 4N mynt.

---

## 🎯 Sammanfattning: Amorterad Analys

### Nyckelinsikter

```
┌─────────────────────────────────────────────────────────────────────┐
│  AMORTERAD ANALYS – HUVUDPUNKTER                                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. Definition: T(N)/N där T(N) är total tid för N operationer     │
│                                                                     │
│  2. Skillnad från "genomsnitt": Vi tar värsta fall för sekvensen,  │
│     inte genomsnitt över slumpmässiga indata                       │
│                                                                     │
│  3. Spargrismetoden: Varje operation "betalar in" till spargrisen  │
│     Dyra operationer "tar ut" från spargrisen                      │
│                                                                     │
│  4. Viktig princip: Dyra operationer måste vara förbetalda!        │
│                                                                     │
│  5. Algoritm 1.1 fungerar (halvering vid 25%)                      │
│     Algoritm 1.1' fungerar INTE (halvering vid 50%)                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Propositioner att Komma Ihåg

| Proposition | Påstående |
|-------------|-----------|
| **D** | Länkade listor: O(1) värsta fall |
| **E** | Resizing array: O(1) amorterat |
| **A1** | Endast push: O(1) amorterat |
| **C** | Mekanisk räknare: O(1) amorterat |

---

## 📊 Visualisering av Amorterad Kostnad

```
┌─────────────────────────────────────────────────────────────────────┐
│  KOSTNAD PER OPERATION ÖVER TID                                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Kostnad │                                                          │
│     N    │            █ (resize)                                   │
│          │                                                          │
│    N/2   │        █                                                 │
│          │                                                          │
│    N/4   │    █                                                     │
│          │                                                          │
│     8    │  █                                                       │
│     4    │ █                                                        │
│     1    │█─█─█─█─█─█─█─█─█─█─█─█─█─█─█──── amorterad ~konstant    │
│          └──────────────────────────────────────────────→          │
│            1  2  4  8  16        N/2       N    Operationer        │
│                                                                     │
│  De flesta operationer kostar 1                                    │
│  Få operationer kostar mycket (resize)                             │
│  GENOMSNITTET är konstant!                                         │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🔑 Minnesregler

### SPAR – Spargrisanalys

```
S - Sätt in mynt vid varje operation
P - Pengar måste räcka till dyra operationer
A - Amorterad kostnad = total / antal
R - Resize betalas av sparade mynt
```

### DAHE – Dubblering och Halvering

```
D - Dubbla när arrayen blir FULL
A - Arrayen är 50% full efter resize
H - Halvera när arrayen är 25% full
E - Ej halvera vid 50% (→ thrashing!)
```

---

## 📖 Referenser

- **[SW]** Sedgewick & Wayne: *Algorithms*, 4th Edition
  - Avsnitt 1.3: Bags, Queues, and Stacks
  - Avsnitt 1.4: Analysis of Algorithms (Proposition E)
  
- **Husfeldt:** *Amortised Analysis* (kurskomplement)
  - Fullständigt bevis av Proposition E
  - Övning 1.4.32 i boken

---

*Detta material är en sammanställning baserad på Sedgewick & Wayne samt Thore Husfeldts kompletterande material.*
