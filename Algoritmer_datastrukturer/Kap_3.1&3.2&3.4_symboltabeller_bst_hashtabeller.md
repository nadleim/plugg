# 📚 Symboltabeller: Grundläggande Implementationer, BST och Hashtabeller
## Komplett Sammanfattning av Avsnitt 3.1, 3.2 och 3.4

**Baserat på:** Sedgewick & Wayne: Algorithms 4th Edition

---

# 🌟 Översikt: Vad Är en Symboltabell?

## Definition och Syfte

> **Definition:** En **symboltabell** är en datastruktur för nyckel-värde-par som stödjer två fundamentala operationer: **insert** (sätt in ett nytt par) och **search** (sök efter värdet associerat med en given nyckel).

### Varför Är Symboltabeller Viktiga?

Symboltabeller är bland de **mest fundamentala datastrukturerna** inom datavetenskap:

```
┌─────────────────────────────────────────────────────────────────────┐
│  TYPISKA TILLÄMPNINGAR AV SYMBOLTABELLER                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Tillämpning         │  Nyckel          │  Värde                   │
│  ────────────────────┼──────────────────┼──────────────────────────│
│  Ordbok              │  Ord             │  Definition              │
│  Bokindex            │  Term            │  Lista av sidnummer      │
│  Fildelning          │  Låtnamn         │  Dator-ID                │
│  Bankhantering       │  Kontonummer     │  Transaktionsdetaljer    │
│  Webbsökning         │  Sökord          │  Lista av webbsidor      │
│  Kompilator          │  Variabelnamn    │  Typ och värde           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

# Del 1: Grundläggande Symboltabeller (Avsnitt 3.1)

---

## 📋 API:er för Symboltabeller

### Grundläggande (Oordnad) Symboltabell

```java
public class ST<Key, Value>
    ST()                          // Skapa en tom symboltabell
    void put(Key key, Value val)  // Sätt in nyckel-värde-par
                                  // (ta bort nyckel om val är null)
    Value get(Key key)            // Hämta värde för nyckeln
                                  // (null om nyckeln saknas)
    void delete(Key key)          // Ta bort nyckel (och värde)
    boolean contains(Key key)     // Finns ett värde för nyckeln?
    boolean isEmpty()             // Är tabellen tom?
    int size()                    // Antal nyckel-värde-par
    Iterable<Key> keys()          // Alla nycklar i tabellen
```

### Ordnad Symboltabell (Comparable-nycklar)

När nycklarna är `Comparable` kan vi stödja fler operationer:

```java
public class ST<Key extends Comparable<Key>, Value>
    // ... alla metoder ovan, plus:
    Key min()                     // Minsta nyckeln
    Key max()                     // Största nyckeln
    Key floor(Key key)            // Största nyckel ≤ key
    Key ceiling(Key key)          // Minsta nyckel ≥ key
    int rank(Key key)             // Antal nycklar < key
    Key select(int k)             // Nyckeln med rank k
    void deleteMin()              // Ta bort minsta nyckeln
    void deleteMax()              // Ta bort största nyckeln
    int size(Key lo, Key hi)      // Antal nycklar i [lo..hi]
    Iterable<Key> keys(Key lo, Key hi)  // Nycklar i [lo..hi]
```

---

## 🎯 Designval och Konventioner

### 1. Inga Dubbletter

- Endast **ett värde** associeras med varje nyckel
- Om man sätter in en nyckel som redan finns, **ersätts** det gamla värdet
- Detta kallas **associativ array** – tänk på nycklar som index

### 2. Null-konventioner

```
┌─────────────────────────────────────────────────────────────────────┐
│  NULL-KONVENTIONER                                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  • Nycklar får ALDRIG vara null (kastar exception)                 │
│                                                                     │
│  • Värden får ALDRIG vara null i tabellen                          │
│                                                                     │
│  • get(key) returnerar null om nyckeln inte finns                  │
│    → Gör det enkelt att testa: contains(key) = (get(key) != null)  │
│                                                                     │
│  • put(key, null) = delete(key) (lat borttagning)                  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 3. Lat vs Ivrig Borttagning

- **Lat borttagning (lazy delete):** Sätt värdet till null, ta bort senare
- **Ivrig borttagning (eager delete):** Ta bort nyckeln direkt

---

## 🔗 Implementation 1: Sekventiell Sökning (Algoritm 3.1)

### Grundidé

Använd en **osorterad länkad lista** där varje nod innehåller en nyckel och ett värde.

### Implementation: SequentialSearchST

```java
public class SequentialSearchST<Key, Value> {
    private Node first;  // Första noden i länkade listan
    
    private class Node {
        Key key;
        Value val;
        Node next;
        
        public Node(Key key, Value val, Node next) {
            this.key = key;
            this.val = val;
            this.next = next;
        }
    }
    
    public Value get(Key key) {
        // Sök genom listan efter nyckeln
        for (Node x = first; x != null; x = x.next)
            if (key.equals(x.key))
                return x.val;    // Sökträff!
        return null;             // Sökmiss
    }
    
    public void put(Key key, Value val) {
        // Sök efter nyckeln, uppdatera om den finns
        for (Node x = first; x != null; x = x.next) {
            if (key.equals(x.key)) {
                x.val = val;     // Uppdatera värdet
                return;
            }
        }
        // Nyckeln finns inte – lägg till först i listan
        first = new Node(key, val, first);
    }
}
```

### Visualisering

```
┌─────────────────────────────────────────────────────────────────────┐
│  SEKVENTIELL SÖKNING I LÄNKAD LISTA                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  first                                                              │
│    │                                                                │
│    ▼                                                                │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐            │
│  │ L:11    │──▶│ P:10    │──▶│ M:9     │──▶│ A:8     │──▶ ...     │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘            │
│                                                                     │
│  get("M"): Skanna från first tills key.equals("M") → returnera 9   │
│                                                                     │
│  put("Z",99): Skanna hela listan (sökmiss)                         │
│               → Lägg till ny nod först: Z:99 → L:11 → P:10 → ...   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Proposition A: Prestanda

> **Proposition A:** Sökmissar och insättningar i en osorterad länkad lista med N nyckel-värde-par kräver N jämförelser. Sökträffar kräver N jämförelser i värsta fall.

**Korollarium:** Att sätta in N distinkta nycklar i en tom tabell kräver ~N²/2 jämförelser.

**Slutsats:** SequentialSearchST är **för långsam** för stora problem!

---

## 📊 Implementation 2: Binärsökning i Ordnad Array (Algoritm 3.2)

### Grundidé

Håll nycklarna i en **sorterad array** och använd **binärsökning** för att hitta nycklar snabbt.

### Implementation: BinarySearchST

```java
public class BinarySearchST<Key extends Comparable<Key>, Value> {
    private Key[] keys;
    private Value[] vals;
    private int N;
    
    public BinarySearchST(int capacity) {
        keys = (Key[]) new Comparable[capacity];
        vals = (Value[]) new Object[capacity];
    }
    
    public int size() { return N; }
    
    public Value get(Key key) {
        if (isEmpty()) return null;
        int i = rank(key);
        if (i < N && keys[i].compareTo(key) == 0)
            return vals[i];
        else
            return null;
    }
    
    // Binärsökning: antal nycklar mindre än key
    public int rank(Key key) {
        int lo = 0, hi = N - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            int cmp = key.compareTo(keys[mid]);
            if      (cmp < 0) hi = mid - 1;
            else if (cmp > 0) lo = mid + 1;
            else return mid;
        }
        return lo;
    }
    
    public void put(Key key, Value val) {
        int i = rank(key);
        // Om nyckeln redan finns, uppdatera värdet
        if (i < N && keys[i].compareTo(key) == 0) {
            vals[i] = val;
            return;
        }
        // Flytta större nycklar ett steg åt höger
        for (int j = N; j > i; j--) {
            keys[j] = keys[j-1];
            vals[j] = vals[j-1];
        }
        keys[i] = key;
        vals[i] = val;
        N++;
    }
}
```

### Visualisering av Binärsökning

```
┌─────────────────────────────────────────────────────────────────────┐
│  BINÄRSÖKNING: SÖK EFTER "P"                                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  keys[] = [A][C][E][H][L][M][P][R][S][X]                           │
│  index     0  1  2  3  4  5  6  7  8  9                            │
│                                                                     │
│  Steg 1: lo=0, hi=9, mid=4                                         │
│          keys[4]='L' < 'P' → lo = 5                                │
│                                                                     │
│  Steg 2: lo=5, hi=9, mid=7                                         │
│          keys[7]='R' > 'P' → hi = 6                                │
│                                                                     │
│  Steg 3: lo=5, hi=6, mid=5                                         │
│          keys[5]='M' < 'P' → lo = 6                                │
│                                                                     │
│  Steg 4: lo=6, hi=6, mid=6                                         │
│          keys[6]='P' = 'P' → SÖKTRÄFF! return 6                    │
│                                                                     │
│  Antal jämförelser: 4 = ⌊lg 10⌋ + 1                                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Proposition B: Prestanda

> **Proposition B:** Binärsökning i en ordnad array med N nycklar använder högst lg N + 1 jämförelser för en sökning (sökträff eller sökmiss).

**Men:** Insättning kräver fortfarande **linjär tid** O(N) för att flytta element!

### Sammanfattning av Elementära Implementationer

| Implementation | Sökning (värsta) | Insättning (värsta) | Sökning (genomsnitt) | Ordnade op? |
|----------------|------------------|---------------------|----------------------|-------------|
| SequentialSearchST | N | N | N/2 | Nej |
| BinarySearchST | lg N | **N** | lg N | Ja |

> ⚠️ **Problemet:** Vi vill ha snabb sökning OCH snabb insättning!

---

# Del 2: Binära Sökträd – BST (Avsnitt 3.2)

---

## 🌳 Vad Är ett Binärt Sökträd?

### Definition

> **Definition:** Ett **binärt sökträd (BST)** är ett binärt träd där varje nod har en nyckel och ett värde, och varje nods nyckel är:
> - **Större** än alla nycklar i dess **vänstra** subträd
> - **Mindre** än alla nycklar i dess **högra** subträd

### Visualisering

```
┌─────────────────────────────────────────────────────────────────────┐
│  BINÄRT SÖKTRÄD: STRUKTUR                                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                       ┌─────┐                                       │
│                       │  S  │  ← rot                                │
│                       └──┬──┘                                       │
│                    ┌─────┴─────┐                                    │
│                 ┌──┴──┐     ┌──┴──┐                                 │
│                 │  E  │     │  X  │                                 │
│                 └──┬──┘     └─────┘                                 │
│              ┌─────┴─────┐                                          │
│           ┌──┴──┐     ┌──┴──┐                                       │
│           │  A  │     │  R  │                                       │
│           └──┬──┘     └──┬──┘                                       │
│              │     ┌─────┴─────┐                                    │
│           ┌──┴──┐ ┌──┴──┐   ┌──┴──┐                                 │
│           │  C  │ │  H  │   │  null│                                │
│           └─────┘ └─────┘   └─────┘                                 │
│                                                                     │
│  Inorder-genomgång ger: A C E H R S X (sorterad ordning!)          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Nyckelobservation

Om vi projicerar alla nycklar i ett BST så att:
- Nycklar i vänster subträd hamnar till vänster om noden
- Nycklar i höger subträd hamnar till höger om noden

...får vi alltid nycklarna i **sorterad ordning**!

---

## 🔧 Implementation: BST (Algoritm 3.3)

### Node-klassen

```java
public class BST<Key extends Comparable<Key>, Value> {
    private Node root;  // Roten av BST
    
    private class Node {
        private Key key;           // Nyckeln
        private Value val;         // Associerat värde
        private Node left, right;  // Länkar till subträd
        private int N;             // Antal noder i subträdet
        
        public Node(Key key, Value val, int N) {
            this.key = key;
            this.val = val;
            this.N = N;
        }
    }
    
    public int size() { return size(root); }
    
    private int size(Node x) {
        if (x == null) return 0;
        else return x.N;
    }
}
```

### Sökning (get)

```java
public Value get(Key key) {
    return get(root, key);
}

private Value get(Node x, Key key) {
    // Returnera värdet för key i subträdet rotat vid x
    // Returnera null om key inte finns
    if (x == null) return null;
    
    int cmp = key.compareTo(x.key);
    
    if      (cmp < 0) return get(x.left, key);   // Gå vänster
    else if (cmp > 0) return get(x.right, key);  // Gå höger
    else              return x.val;               // Sökträff!
}
```

### Visualisering av Sökning

```
┌─────────────────────────────────────────────────────────────────────┐
│  BST SÖKNING: SÖK EFTER "R"                                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│           S                                                        │
│          / \                                                       │
│         E   X      1. "R" < "S" → gå VÄNSTER                       │
│        / \                                                         │
│       A   R        2. "R" > "E" → gå HÖGER                         │
│        \  |                                                        │
│         C ▼        3. "R" = "R" → SÖKTRÄFF! Returnera värdet       │
│           H                                                        │
│                                                                     │
│  Antal jämförelser: 3 (= djupet av noden + 1)                      │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Insättning (put)

```java
public void put(Key key, Value val) {
    root = put(root, key, val);
}

private Node put(Node x, Key key, Value val) {
    // Uppdatera värde om nyckeln finns, annars lägg till ny nod
    if (x == null) 
        return new Node(key, val, 1);
    
    int cmp = key.compareTo(x.key);
    
    if      (cmp < 0) x.left  = put(x.left, key, val);
    else if (cmp > 0) x.right = put(x.right, key, val);
    else              x.val = val;  // Uppdatera värdet
    
    x.N = size(x.left) + size(x.right) + 1;
    return x;
}
```

### Visualisering av Insättning

```
┌─────────────────────────────────────────────────────────────────────┐
│  BST INSÄTTNING: SÄTT IN "L"                                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  FÖRE:                           EFTER:                            │
│                                                                     │
│           S                             S                          │
│          / \                           / \                         │
│         E   X                         E   X                        │
│        / \                           / \                           │
│       A   R                         A   R                          │
│        \                             \  /                          │
│         C                             C H                          │
│          \                             \ \                         │
│           H                             (L) ← NY NOD!              │
│                                                                     │
│  Sökvväg: S→E→R→H→null                                             │
│  Ny nod kopplas till null-länken                                   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📊 Prestandaanalys av BST

### Dualitet med Quicksort

> **Nyckelinsikt:** BST är **duala till quicksort**!

- Roten motsvarar det första pivot-elementet
- Vänster subträd innehåller element som är mindre (som vänster partition)
- Höger subträd innehåller element som är större (som höger partition)

Denna observation låter oss återanvända analysen från quicksort!

### Proposition C: Genomsnittlig Sökning

> **Proposition C:** Sökträffar i ett BST byggt från N slumpmässiga nycklar kräver ~2 ln N (ungefär 1.39 lg N) jämförelser i genomsnitt.

**Bevis (skiss):**
- Antalet jämförelser för en sökträff = 1 + nodens djup
- Den genomsnittliga interna väglängden följer samma rekurrens som quicksort
- Lösningen är ~2N ln N, så genomsnittet är ~2 ln N per sökning

### Proposition D: Sökmissar och Insättningar

> **Proposition D:** Sökmissar och insättningar i ett BST byggt från N slumpmässiga nycklar kräver ~2 ln N (ungefär 1.39 lg N) jämförelser i genomsnitt.

### Proposition E: Värsta Fall

> **Proposition E:** I ett BST tar alla operationer tid proportionell mot trädets **höjd** i värsta fall.

**Värsta fallet:** Om nycklarna sätts in i sorterad ordning blir trädet en **länkad lista** med höjd N!

```
┌─────────────────────────────────────────────────────────────────────┐
│  VÄRSTA FALL: NYCKLAR I SORTERAD ORDNING                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Insättningsordning: A, C, E, H, R, S, X                           │
│                                                                     │
│  A                                                                 │
│   \                                                                │
│    C                                                               │
│     \                                                              │
│      E                                                             │
│       \                                                            │
│        H                                                           │
│         \                                                          │
│          R         Höjd = N - 1 = 6                                │
│           \        Sökning tar O(N) tid!                           │
│            S                                                       │
│             \                                                      │
│              X                                                     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Sammanfattning av BST-prestanda

| Operation | Värsta fall | Genomsnitt (slumpmässiga nycklar) |
|-----------|-------------|-----------------------------------|
| Sökning | N | 1.39 lg N |
| Insättning | N | 1.39 lg N |
| Min/Max | N | 1.39 lg N |
| Floor/Ceiling | N | 1.39 lg N |
| Rank/Select | N | 1.39 lg N |

---

## 🔧 Ordnade Operationer i BST

### Min och Max

```java
public Key min() {
    return min(root).key;
}

private Node min(Node x) {
    if (x.left == null) return x;
    return min(x.left);
}

public Key max() {
    return max(root).key;
}

private Node max(Node x) {
    if (x.right == null) return x;
    return max(x.right);
}
```

### Floor och Ceiling

**floor(key):** Största nyckel ≤ key

```java
public Key floor(Key key) {
    Node x = floor(root, key);
    if (x == null) return null;
    return x.key;
}

private Node floor(Node x, Key key) {
    if (x == null) return null;
    int cmp = key.compareTo(x.key);
    
    if (cmp == 0) return x;
    if (cmp < 0)  return floor(x.left, key);
    
    // cmp > 0: floor kan vara i höger subträd
    Node t = floor(x.right, key);
    if (t != null) return t;
    else           return x;
}
```

### Rank och Select

**rank(key):** Antal nycklar < key

```java
public int rank(Key key) {
    return rank(key, root);
}

private int rank(Key key, Node x) {
    if (x == null) return 0;
    int cmp = key.compareTo(x.key);
    
    if      (cmp < 0) return rank(key, x.left);
    else if (cmp > 0) return 1 + size(x.left) + rank(key, x.right);
    else              return size(x.left);
}
```

**select(k):** Nyckeln med rank k

```java
public Key select(int k) {
    return select(root, k).key;
}

private Node select(Node x, int k) {
    if (x == null) return null;
    int t = size(x.left);
    
    if      (t > k) return select(x.left, k);
    else if (t < k) return select(x.right, k - t - 1);
    else            return x;
}
```

---

# Del 3: Hashtabeller (Avsnitt 3.4)

---

## 🎯 Grundidén med Hashing

### Tid-Rum-avvägning

Hashing är ett klassiskt exempel på **tid-rum-avvägning**:

- Om vi hade **obegränsat minne** kunde vi använda nyckeln direkt som index
- Om vi hade **obegränsad tid** kunde vi använda sekventiell sökning
- **Hashing** ger en balans mellan dessa extremer

### Två Delar av Hashing

```
┌─────────────────────────────────────────────────────────────────────┐
│  HASHING: TVÅ KOMPONENTER                                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. HASHFUNKTION                                                   │
│     Omvandlar nyckeln till ett array-index                         │
│     key → hash(key) → index i [0, M-1]                             │
│                                                                     │
│  2. KOLLISIONSHANTERING                                            │
│     Hantera fallet när två olika nycklar hashar till samma index   │
│     hash(key1) = hash(key2) men key1 ≠ key2                        │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🔢 Hashfunktioner

### Krav på en Bra Hashfunktion

1. **Lätt att beräkna** (konstant tid)
2. **Jämnt fördelad** – varje index ska vara lika sannolikt
3. **Deterministisk** – samma nyckel ger alltid samma hash

### Modulär Hashing för Heltal

```java
private int hash(Key key) {
    return (key.hashCode() & 0x7fffffff) % M;
}
```

**Förklaring:**
- `key.hashCode()` returnerar ett 32-bitars heltal (kan vara negativt)
- `& 0x7fffffff` tar bort teckenbiten (gör talet icke-negativt)
- `% M` ger ett index i intervallet [0, M-1]

### hashCode() i Java

```java
// Exempel: hashCode för String
public int hashCode() {
    int hash = 0;
    for (int i = 0; i < length(); i++)
        hash = (hash * 31) + charAt(i);
    return hash;
}
```

### Antagande J: Likformig Hashing

> **Antagande J (Uniform Hashing Assumption):** Hashfunktionerna vi använder fördelar nycklar likformigt och oberoende bland heltalen mellan 0 och M-1.

Detta är ett idealiserande antagande som guidar vår analys.

---

## ⛓️ Kollisionshantering 1: Separate Chaining (Algoritm 3.5)

### Grundidé

Bygg en **länkad lista** för varje array-index. Nycklar som hashar till samma index läggs i samma lista.

### Visualisering

```
┌─────────────────────────────────────────────────────────────────────┐
│  SEPARATE CHAINING: M = 5                                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  st[0] → [S:0] → [X:7] → null                                      │
│  st[1] → [E:12] → null                                             │
│  st[2] → [A:8] → null                                              │
│  st[3] → [L:11] → [P:10] → null                                    │
│  st[4] → [M:9] → [H:5] → [C:4] → [R:3] → null                     │
│                                                                     │
│  get("H"): hash("H") = 4 → skanna lista st[4] → hitta H → return 5 │
│                                                                     │
│  put("Z",99): hash("Z") = 2 → lägg till först i st[2]              │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Implementation

```java
public class SeparateChainingHashST<Key, Value> {
    private int N;                              // Antal nyckel-värde-par
    private int M;                              // Tabellstorlek
    private SequentialSearchST<Key, Value>[] st; // Array av listor
    
    public SeparateChainingHashST() {
        this(997);  // Standardstorlek
    }
    
    public SeparateChainingHashST(int M) {
        this.M = M;
        st = (SequentialSearchST<Key, Value>[]) new SequentialSearchST[M];
        for (int i = 0; i < M; i++)
            st[i] = new SequentialSearchST<Key, Value>();
    }
    
    private int hash(Key key) {
        return (key.hashCode() & 0x7fffffff) % M;
    }
    
    public Value get(Key key) {
        return st[hash(key)].get(key);
    }
    
    public void put(Key key, Value val) {
        st[hash(key)].put(key, val);
    }
}
```

### Proposition K: Prestanda för Separate Chaining

> **Proposition K:** I en hashtabell med separate chaining och M listor och N nycklar är sannolikheten (under Antagande J) att antalet nycklar i en lista är inom en liten konstant faktor av N/M extremt nära 1.

**Praktiskt:** Med M ~ N/5 är genomsnittlig listlängd ~5, vilket ger **konstant tid** för sökning och insättning!

### Fördelar med Separate Chaining

- Enkel att implementera
- Snabb sökning och insättning (genomsnitt)
- Listlängden växer sakta även med dåliga hashfunktioner
- Borttagning är enkelt

---

## 📍 Kollisionshantering 2: Linear Probing (Algoritm 3.6)

### Grundidé: Open Addressing

Istället för länkade listor, använd **tomma platser** i tabellen för att markera slut på sökningar.

**Linear probing:**
1. Hasha nyckeln till index i
2. Om platsen är tom: sökmiss (eller lägg till här)
3. Om platsen har rätt nyckel: sökträff
4. Annars: prova nästa plats (i+1, wraparound vid M)

### Visualisering

```
┌─────────────────────────────────────────────────────────────────────┐
│  LINEAR PROBING: M = 16                                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Index:  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14 15│
│  keys:  [P] [M] [ ] [ ] [A] [C] [S] [H] [L] [E] [R] [ ] [ ] [ ] [ ][X]│
│  vals:  [10][ 9][ ] [ ] [8] [5] [0] [5] [11][12][3] [ ] [ ] [ ] [ ][7]│
│                                                                     │
│  get("H"):                                                         │
│    hash("H") = 4                                                   │
│    keys[4] = 'A' ≠ 'H' → prova 5                                   │
│    keys[5] = 'C' ≠ 'H' → prova 6                                   │
│    keys[6] = 'S' ≠ 'H' → prova 7                                   │
│    keys[7] = 'H' = 'H' → SÖKTRÄFF! return vals[7] = 5              │
│                                                                     │
│  get("Z"):                                                         │
│    hash("Z") = 11                                                  │
│    keys[11] = null → SÖKMISS! return null                          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Implementation

```java
public class LinearProbingHashST<Key, Value> {
    private int N;          // Antal nyckel-värde-par
    private int M = 16;     // Tabellstorlek
    private Key[] keys;     // Nycklarna
    private Value[] vals;   // Värdena
    
    public LinearProbingHashST() {
        keys = (Key[])   new Object[M];
        vals = (Value[]) new Object[M];
    }
    
    private int hash(Key key) {
        return (key.hashCode() & 0x7fffffff) % M;
    }
    
    public Value get(Key key) {
        for (int i = hash(key); keys[i] != null; i = (i + 1) % M)
            if (keys[i].equals(key))
                return vals[i];
        return null;
    }
    
    public void put(Key key, Value val) {
        if (N >= M/2) resize(2*M);  // Dubbla storlek om halvfull
        
        int i;
        for (i = hash(key); keys[i] != null; i = (i + 1) % M)
            if (keys[i].equals(key)) {
                vals[i] = val;
                return;
            }
        keys[i] = key;
        vals[i] = val;
        N++;
    }
}
```

### Clustering: Problemet med Linear Probing

```
┌─────────────────────────────────────────────────────────────────────┐
│  CLUSTERING                                                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  [A][C][S][H][L][E][R][ ][ ][ ][ ][ ][ ][ ][ ][ ]                  │
│   └──────────────────┘                                             │
│         KLUSTER                                                    │
│                                                                     │
│  Problem: Långa kluster bildas över tid                            │
│  • Nya nycklar som hashar till klustret förlänger det              │
│  • Långa kluster → långsam sökning                                 │
│  • Ännu fler kollisioner → ännu längre kluster                     │
│                                                                     │
│  Lösning: Håll load factor α = N/M låg (typiskt < 0.5)             │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Load Factor (α)

> **Definition:** **Load factor** α = N/M = (antal nycklar) / (tabellstorlek)

- För separate chaining: α är genomsnittlig listlängd (kan vara > 1)
- För linear probing: α är andel upptagna platser (måste vara < 1)

### Proposition M: Prestanda för Linear Probing

> **Proposition M:** I en hashtabell med linear probing med M platser och N = αM nycklar är det förväntade antalet probes (under Antagande J):
>
> - **Sökträff:** ~½(1 + 1/(1-α))
> - **Sökmiss:** ~½(1 + 1/(1-α)²)

**Exempel för α = 0.5:**
- Sökträff: ~1.5 probes
- Sökmiss: ~2.5 probes

**Exempel för α = 0.75:**
- Sökträff: ~2.5 probes
- Sökmiss: ~8.5 probes

### Array Resizing

För att hålla α kontrollerad, använd **dynamisk storleksändring**:

```java
private void resize(int cap) {
    LinearProbingHashST<Key, Value> t;
    t = new LinearProbingHashST<Key, Value>(cap);
    
    for (int i = 0; i < M; i++)
        if (keys[i] != null)
            t.put(keys[i], vals[i]);
    
    keys = t.keys;
    vals = t.vals;
    M = t.M;
}
```

### Borttagning i Linear Probing

**Problem:** Om vi sätter en plats till null kan det avbryta sökning för nycklar längre fram!

**Lösning:** Efter borttagning, sätt in alla nycklar i klustret igen:

```java
public void delete(Key key) {
    if (!contains(key)) return;
    
    int i = hash(key);
    while (!key.equals(keys[i]))
        i = (i + 1) % M;
    
    // Ta bort nyckeln
    keys[i] = null;
    vals[i] = null;
    
    // Sätt in alla efterföljande nycklar i klustret igen
    i = (i + 1) % M;
    while (keys[i] != null) {
        Key   keyToRedo = keys[i];
        Value valToRedo = vals[i];
        keys[i] = null;
        vals[i] = null;
        N--;
        put(keyToRedo, valToRedo);
        i = (i + 1) % M;
    }
    N--;
    
    if (N > 0 && N == M/8) resize(M/2);
}
```

---

## 📊 Jämförelse av Symboltabellimplementationer

```
┌────────────────────────────────────────────────────────────────────────────────┐
│  JÄMFÖRELSE AV SYMBOLTABELLIMPLEMENTATIONER                                   │
├─────────────────────────┬─────────────┬─────────────┬─────────────┬───────────┤
│                         │  Värsta fall│ Genomsnitt  │ Genomsnitt  │  Ordnade  │
│  Implementation         │  sökning    │  sökning    │ insättning  │  op?      │
├─────────────────────────┼─────────────┼─────────────┼─────────────┼───────────┤
│  SequentialSearchST     │     N       │    N/2      │     N       │    Nej    │
│  (osorterad länkad lista)                                                     │
├─────────────────────────┼─────────────┼─────────────┼─────────────┼───────────┤
│  BinarySearchST         │   lg N      │   lg N      │     N       │    Ja     │
│  (ordnad array)                                                               │
├─────────────────────────┼─────────────┼─────────────┼─────────────┼───────────┤
│  BST                    │     N       │ 1.39 lg N   │ 1.39 lg N   │    Ja     │
│  (binärt sökträd)                                                             │
├─────────────────────────┼─────────────┼─────────────┼─────────────┼───────────┤
│  SeparateChainingHashST │    N        │   N/(2M)    │   N/M       │    Nej    │
│  (separate chaining)    │ (alla i en) │  ≈ O(1)     │  ≈ O(1)     │           │
├─────────────────────────┼─────────────┼─────────────┼─────────────┼───────────┤
│  LinearProbingHashST    │    N        │   ≈ 1.5     │   ≈ 2.5     │    Nej    │
│  (linear probing, α=0.5)│             │  ≈ O(1)     │  ≈ O(1)     │           │
└─────────────────────────┴─────────────┴─────────────┴─────────────┴───────────┘
```

---

## 🎯 När Använda Vad?

### Använd Hashtabell Om:

- Du **inte** behöver ordnade operationer (min, max, floor, ceiling, rank, select)
- Du vill ha **snabbast möjliga** sökning och insättning
- Dina nycklar har en bra hashfunktion

### Använd BST Om:

- Du behöver **ordnade operationer**
- Du vill ha garanterad logaritmisk prestanda (med balanserat träd)
- Du behöver **range queries** (hitta alla nycklar i ett intervall)

### Använd Ordnad Array Om:

- Du har **få insättningar** men **många sökningar**
- Du behöver ordnade operationer
- Minne är begränsat

---

## 🔑 Sammanfattning

### Nyckelbegrepp

| Begrepp | Förklaring |
|---------|------------|
| **Symboltabell** | Datastruktur för nyckel-värde-par |
| **Binärsökning** | O(lg N) sökning i sorterad array |
| **BST** | Binärt träd där vänster < rot < höger |
| **Hashfunktion** | Omvandlar nyckel till array-index |
| **Kollision** | Två nycklar hashar till samma index |
| **Separate Chaining** | Lista vid varje index |
| **Linear Probing** | Sök nästa tomma plats |
| **Load Factor α** | N/M, styr prestanda |

### Propositioner

| Proposition | Påstående |
|-------------|-----------|
| **A** | SequentialSearchST: N jämförelser per operation |
| **B** | BinarySearchST: lg N + 1 jämförelser för sökning |
| **C** | BST sökning: ~2 ln N jämförelser i genomsnitt |
| **D** | BST insättning: ~2 ln N jämförelser i genomsnitt |
| **E** | BST: alla operationer O(höjd) i värsta fall |
| **K** | Separate chaining: listlängd nära N/M |
| **M** | Linear probing: ~1.5 probes vid α = 0.5 |

---

## 📝 Minnesregler

### HASH - För Hashtabeller

```
H - Hashfunktion omvandlar nyckel till index
A - Array (eller arrayer) lagrar data
S - Separate chaining eller linear probing
H - Håll load factor låg för bra prestanda
```

### BST - Egenskaper

```
B - Binärt träd med två barn per nod
S - Sorterat: vänster < rot < höger
T - Tid: O(höjd) för alla operationer
```

---

*Detta material är baserat på "Algorithms, 4th Edition" av Robert Sedgewick och Kevin Wayne.*
