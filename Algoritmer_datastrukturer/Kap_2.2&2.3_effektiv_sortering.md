# 📚 Effektiv Sortering: Mergesort och Quicksort
## Komplett Sammanfattning av Avsnitt 2.2 och 2.3

**Baserat på:** Sedgewick & Wayne: Algorithms 4th Edition

---

# 🌟 Översikt: Från Kvadratisk till Linjäritmisk Tid

## Varför Är Detta Kapitel Så Viktigt?

I avsnitt 2.1 lärde vi oss elementära sorteringsalgoritmer (selection sort, insertion sort, shellsort) som har **kvadratisk** tidskomplexitet O(N²) i värsta fall. Det betyder att om vi fördubblar antalet element, tar det **fyra gånger** så lång tid!

Nu ska vi studera två algoritmer som är **fundamentalt snabbare**:

```
┌─────────────────────────────────────────────────────────────────────┐
│  JÄMFÖRELSE: KVADRATISK vs LINJÄRITMISK                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  N = 1 000 000 element                                             │
│                                                                     │
│  Insertion Sort:  ~N²/4 = 250 000 000 000 operationer              │
│                   (skulle ta TIMMAR)                               │
│                                                                     │
│  Mergesort:       ~N lg N = 20 000 000 operationer                 │
│                   (tar SEKUNDER)                                    │
│                                                                     │
│  SKILLNADEN: ~12 500 gånger snabbare!                              │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

Mergesort och quicksort är båda **linjäritmiska** – de tar tid proportionell mot N log N. Detta är en dramatisk förbättring som gör det möjligt att sortera **miljontals** element på rimlig tid.

---

# Del 1: Mergesort (Avsnitt 2.2)

---

## 🎯 Grundidén: Dela-och-Härska (Divide and Conquer)

### Vad Är Dela-och-Härska?

**Dela-och-härska** är ett kraftfullt algoritmparadigm där vi:

1. **Dela** problemet i mindre delproblem
2. **Härska** (lös) delproblemen rekursivt
3. **Kombinera** lösningarna till en lösning för hela problemet

### Mergesort i Tre Steg

```
┌─────────────────────────────────────────────────────────────────────┐
│  MERGESORT: GRUNDPRINCIPEN                                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. DELA: Dela arrayen i två halvor                                │
│                                                                     │
│     [M E R G E S O R T E X A M P L E]                              │
│              ↓              ↓                                       │
│     [M E R G E S O R]    [T E X A M P L E]                         │
│                                                                     │
│  2. SORTERA: Sortera varje halva rekursivt (!)                     │
│                                                                     │
│     [E E G M O R R S]    [A E E L M P T X]                         │
│                                                                     │
│  3. SLÅR SAMMAN (MERGE): Kombinera de sorterade halvorna           │
│                                                                     │
│     [A E E E E G L M M O P R R S T X]                              │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Varför Fungerar Detta?

**Nyckelinsikt:** Om vi kan slå samman (*merge*) två sorterade arrayer till en sorterad array effektivt, och vi kan dela problemet i hälften varje gång, får vi en algoritm som tar N log N tid!

---

## 🔧 Merge-operationen: Hjärtat i Algoritmen

### Problemet med In-place Merge

**Idealiskt** skulle vi vilja göra merge "på plats" utan extra minne. Men detta är **förvånansvärt svårt** att implementera effektivt!

**Lösning:** Vi använder en **hjälparray** (`aux[]`) för att temporärt lagra data under sammanslagningen.

### Abstract In-place Merge

```java
public static void merge(Comparable[] a, int lo, int mid, int hi) {
    // Slå samman a[lo..mid] med a[mid+1..hi]
    int i = lo, j = mid + 1;
    
    // Kopiera a[lo..hi] till aux[lo..hi]
    for (int k = lo; k <= hi; k++)
        aux[k] = a[k];
    
    // Slå samman tillbaka till a[lo..hi]
    for (int k = lo; k <= hi; k++) {
        if      (i > mid)              a[k] = aux[j++];  // Vänster uttömd
        else if (j > hi)               a[k] = aux[i++];  // Höger uttömd
        else if (less(aux[j], aux[i])) a[k] = aux[j++];  // Höger mindre
        else                           a[k] = aux[i++];  // Vänster mindre/lika
    }
}
```

### Visualisering av Merge

```
┌─────────────────────────────────────────────────────────────────────┐
│  MERGE: SLÅR SAMMAN TVÅ SORTERADE HALVOR                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  a[] = [E E G M R | A C E R T]                                     │
│         ↑ i         ↑ j                                            │
│         vänster     höger                                          │
│         halva       halva                                          │
│                                                                     │
│  Steg 1: aux[j]='A' < aux[i]='E' → ta A, j++                       │
│  Steg 2: aux[i]='E' < aux[j]='C'? Nej! → ta C, j++                 │
│  Steg 3: aux[i]='E' ≤ aux[j]='E' → ta vänster E, i++               │
│  ...osv...                                                         │
│                                                                     │
│  Resultat: [A C E E E G M R R T]                                   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### De Fyra Fallen i Merge

| Fall | Villkor | Åtgärd |
|------|---------|--------|
| 1 | `i > mid` | Vänster halva uttömd → ta från höger |
| 2 | `j > hi` | Höger halva uttömd → ta från vänster |
| 3 | `aux[j] < aux[i]` | Element från höger är mindre → ta det |
| 4 | `aux[j] >= aux[i]` | Element från vänster är mindre/lika → ta det |

> 💡 **Stabilitet:** Genom att ta från vänster när elementen är lika, bevarar vi den ursprungliga ordningen för lika nycklar. Detta gör mergesort till en **stabil** sorteringsalgoritm!

---

## 📈 Top-Down Mergesort (Algoritm 2.4)

### Implementation

```java
public class Merge {
    private static Comparable[] aux;  // Hjälparray för merge
    
    public static void sort(Comparable[] a) {
        aux = new Comparable[a.length];  // Allokera utrymme EN GÅNG
        sort(a, 0, a.length - 1);
    }
    
    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo) return;           // Basfall: 0 eller 1 element
        int mid = lo + (hi - lo) / 2;   // Mittpunkt
        sort(a, lo, mid);               // Sortera vänster halva
        sort(a, mid + 1, hi);           // Sortera höger halva
        merge(a, lo, mid, hi);          // Slå samman resultaten
    }
}
```

### Rekursionsträdet: Visualisering

```
┌─────────────────────────────────────────────────────────────────────┐
│  REKURSIONSTRÄD FÖR MERGESORT (N = 16)                             │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Nivå 0:              a[0..15]                                     │
│                      /        \                                     │
│  Nivå 1:      a[0..7]          a[8..15]                            │
│              /      \          /      \                             │
│  Nivå 2:  a[0..3]  a[4..7]  a[8..11]  a[12..15]                    │
│           /    \   /    \   /    \    /     \                      │
│  Nivå 3: [0,1] [2,3] [4,5] [6,7] [8,9] [10,11] [12,13] [14,15]    │
│                                                                     │
│  Antal nivåer = lg N = lg 16 = 4                                   │
│  Arbete per nivå = N jämförelser                                   │
│  Totalt arbete = N × lg N                                          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Spårning av Rekursionen

```
sort(a, 0, 15)
    sort(a, 0, 7)
        sort(a, 0, 3)
            sort(a, 0, 1)
                merge(a, 0, 0, 1)      ← Första merge!
            sort(a, 2, 3)
                merge(a, 2, 2, 3)
            merge(a, 0, 1, 3)
        sort(a, 4, 7)
            ...
        merge(a, 0, 3, 7)
    sort(a, 8, 15)
        ...
    merge(a, 0, 7, 15)                 ← Sista merge!
```

---

## 📊 Prestandaanalys av Mergesort

### Proposition F: Antal Jämförelser

> **Proposition F:** Top-down mergesort använder mellan ½N lg N och N lg N jämförelser för att sortera en array med N element.

### Bevis (Förenklat)

**Steg 1:** Låt C(N) vara antalet jämförelser för att sortera N element.

**Rekurrensrelation:**
```
C(N) = C(⌊N/2⌋) + C(⌈N/2⌉) + kostnaden för merge
```

- Första termen: sortera vänster halva
- Andra termen: sortera höger halva  
- Tredje termen: merge (mellan N/2 och N jämförelser)

**Steg 2:** För N = 2^n får vi:
```
C(2^n) = 2·C(2^(n-1)) + 2^n
```

Dividera båda sidor med 2^n:
```
C(2^n)/2^n = C(2^(n-1))/2^(n-1) + 1
```

**Steg 3:** Upprepa n-1 gånger (teleskopering):
```
C(2^n)/2^n = C(2^0)/2^0 + n = 0 + n = n
```

**Steg 4:** Multiplicera med 2^n:
```
C(N) = C(2^n) = n·2^n = N lg N
```

### Proposition G: Antal Arrayåtkomster

> **Proposition G:** Top-down mergesort använder högst 6N lg N arrayåtkomster för att sortera en array med N element.

**Bevis:** Varje merge använder högst:
- 2N för kopiering till aux[]
- 2N för kopiering tillbaka till a[]
- 2N för jämförelser

Totalt: 6N per nivå × lg N nivåer = 6N lg N

### Sammanfattning av Komplexitet

| Aspekt | Komplexitet |
|--------|-------------|
| Tid (värsta fall) | O(N log N) |
| Tid (bästa fall) | O(N log N) |
| Tid (genomsnitt) | O(N log N) |
| Extra minne | O(N) |
| Stabil? | Ja ✓ |

> ⚠️ **Nackdel:** Mergesort kräver **extra minne** proportionellt mot N för hjälparrayen. Detta kan vara problematiskt för mycket stora arrayer.

---

## 🔧 Förbättringar av Mergesort

### 1. Cutoff till Insertion Sort för Små Subarrayer

**Observation:** För små arrayer är insertion sort snabbare än mergesort på grund av lägre overhead.

```java
private static final int CUTOFF = 15;  // Typiskt 5-15

private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo + CUTOFF) {
        Insertion.sort(a, lo, hi);  // Byt till insertion sort!
        return;
    }
    int mid = lo + (hi - lo) / 2;
    sort(a, lo, mid);
    sort(a, mid + 1, hi);
    merge(a, lo, mid, hi);
}
```

**Resultat:** 10-15% snabbare i praktiken!

### 2. Testa Om Redan Sorterat

**Observation:** Om `a[mid] <= a[mid+1]` är halvorna redan i rätt ordning – ingen merge behövs!

```java
private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo) return;
    int mid = lo + (hi - lo) / 2;
    sort(a, lo, mid);
    sort(a, mid + 1, hi);
    if (!less(a[mid+1], a[mid])) return;  // Redan sorterat!
    merge(a, lo, mid, hi);
}
```

**Resultat:** Linjär tid för redan sorterade arrayer!

### 3. Eliminera Kopiering till Hjälparray

**Avancerad teknik:** Växla roller mellan `a[]` och `aux[]` på varje rekursionsnivå för att undvika kopiering.

---

## 📉 Bottom-Up Mergesort

### Alternativ Approach: Bygg Underifrån

Istället för att dela uppifrån (rekursivt) kan vi bygga underifrån (iterativt):

1. Betrakta varje element som en sorterad subarray av storlek 1
2. Slå samman par av subarrayer till storlek 2
3. Slå samman par av subarrayer till storlek 4
4. ...fortsätt tills hela arrayen är sorterad

### Implementation

```java
public class MergeBU {
    private static Comparable[] aux;
    
    public static void sort(Comparable[] a) {
        int N = a.length;
        aux = new Comparable[N];
        
        // sz: storlek på subarrayer som slås samman
        for (int sz = 1; sz < N; sz = sz + sz) {
            // lo: startindex för vänster subarray
            for (int lo = 0; lo < N - sz; lo += sz + sz) {
                merge(a, lo, lo + sz - 1, Math.min(lo + sz + sz - 1, N - 1));
            }
        }
    }
}
```

### Visualisering

```
┌─────────────────────────────────────────────────────────────────────┐
│  BOTTOM-UP MERGESORT                                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Input: [M][E][R][G][E][S][O][R][T][E][X][A][M][P][L][E]           │
│                                                                     │
│  sz=1:  [E M][G R][E S][O R][E T][A X][M P][E L]                   │
│          ↑     ↑     ↑     ↑     ↑     ↑     ↑     ↑               │
│          merge 1-by-1                                              │
│                                                                     │
│  sz=2:  [E G M R][E O R S][A E T X][E L M P]                       │
│              ↑        ↑        ↑        ↑                          │
│              merge 2-by-2                                          │
│                                                                     │
│  sz=4:  [E E G M O R R S][A E E L M P T X]                         │
│                  ↑                ↑                                 │
│                  merge 4-by-4                                      │
│                                                                     │
│  sz=8:  [A E E E E G L M M O P R R S T X]                          │
│                         ↑                                          │
│                         merge 8-by-8                               │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Proposition H

> **Proposition H:** Bottom-up mergesort använder mellan ½N lg N och N lg N jämförelser samt högst 6N lg N arrayåtkomster för att sortera en array med N element.

### Top-Down vs Bottom-Up

| Aspekt | Top-Down | Bottom-Up |
|--------|----------|-----------|
| Approach | Rekursiv | Iterativ |
| Kod-komplexitet | Mer intuitiv | Lite kortare |
| Stackanvändning | O(log N) | O(1) |
| Prestanda | Identisk | Identisk |
| Bäst för | Arrayer | Länkade listor! |

> 💡 **Praktiskt tips:** Bottom-up mergesort är utmärkt för att sortera **länkade listor** eftersom den inte kräver indexerad åtkomst!

---

# Del 2: Quicksort (Avsnitt 2.3)

---

## 🎯 Introduktion till Quicksort

### Varför Quicksort?

> **Quicksort är förmodligen den mest använda sorteringsalgoritmen.**

Quicksort är populär eftersom den:
- Är relativt **enkel att implementera**
- Fungerar bra för **olika typer av data**
- Är **väsentligt snabbare** än andra metoder i typiska tillämpningar
- Sorterar **in-place** (kräver bara O(log N) extra minne för rekursion)
- Har en **kort inre loop** (extremt snabb i praktiken)

### Quicksort vs Mergesort: Olika Filosofier

```
┌─────────────────────────────────────────────────────────────────────┐
│  TVÅ OLIKA DELA-OCH-HÄRSKA STRATEGIER                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  MERGESORT:                                                        │
│  1. Dela (enkelt: mitt i arrayen)                                  │
│  2. Sortera halvor rekursivt                                       │
│  3. Kombinera (svårt: merge-operation)                             │
│                                                                     │
│  Arbete EFTER rekursion (merge)                                    │
│                                                                     │
│  ─────────────────────────────────────────────────                 │
│                                                                     │
│  QUICKSORT:                                                        │
│  1. Partitionera (svårt: omordna element)                          │
│  2. Sortera delar rekursivt                                        │
│  3. Kombinera (enkelt: ingenting att göra!)                        │
│                                                                     │
│  Arbete FÖRE rekursion (partition)                                 │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🔧 Grundalgoritmen

### Översikt

```
┌─────────────────────────────────────────────────────────────────────┐
│  QUICKSORT: GRUNDPRINCIPEN                                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Input:    [Q U I C K S O R T E X A M P L E]                       │
│                                                                     │
│  Shuffle:  [K R A T E L E P U I M Q C X O S]  (slumpmässig ordning)│
│                                                                     │
│  Partition (pivot = K):                                            │
│            [E C A I E | K | L P U T M Q R X O S]                   │
│             < K        = K    > K                                  │
│                                                                     │
│  K är nu på sin SLUTGILTIGA position!                              │
│                                                                     │
│  Sortera vänster del rekursivt                                     │
│  Sortera höger del rekursivt                                       │
│  KLART! (ingen merge behövs)                                       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Implementation (Algoritm 2.5)

```java
public class Quick {
    public static void sort(Comparable[] a) {
        StdRandom.shuffle(a);         // VIKTIGT: Slumpa först!
        sort(a, 0, a.length - 1);
    }
    
    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo) return;
        int j = partition(a, lo, hi); // Partitionera
        sort(a, lo, j - 1);           // Sortera vänster del
        sort(a, j + 1, hi);           // Sortera höger del
    }
}
```

> ⚠️ **Kritiskt:** Slumpmässig blandning (`shuffle`) i början är **avgörande** för att garantera bra prestanda!

---

## 🎲 Partitionering: Nyckeln till Quicksort

### Vad Ska Partitionering Åstadkomma?

Efter `partition(a, lo, hi)` ska följande gälla:

1. **Pivotelementet** `a[j]` är på sin **slutgiltiga position**
2. **Alla element till vänster** om `a[j]` är **≤ a[j]**
3. **Alla element till höger** om `a[j]` är **≥ a[j]**

### Strategi för Partitionering

```
┌─────────────────────────────────────────────────────────────────────┐
│  PARTITIONERINGSSTRATEGI                                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  1. Välj a[lo] som pivotelement (v)                                │
│                                                                     │
│  2. Skanna från VÄNSTER: hitta element ≥ v                         │
│     Skanna från HÖGER: hitta element ≤ v                           │
│                                                                     │
│  3. BYT dessa element                                              │
│                                                                     │
│  4. Upprepa tills pekarna möts                                     │
│                                                                     │
│  5. Byt pivoten till sin rätta position                            │
│                                                                     │
│  FÖRE:  [v |  ?  ?  ?  ?  ?  ?  ?  ?  |]                          │
│          lo                          hi                            │
│                                                                     │
│  UNDER: [v | ≤v  ≤v | ? ? ? | ≥v  ≥v |]                           │
│          lo       i         j        hi                            │
│                                                                     │
│  EFTER: [≤v  ≤v | v | ≥v  ≥v  ≥v  ≥v ]                            │
│          lo      j                  hi                             │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Implementation av Partition

```java
private static int partition(Comparable[] a, int lo, int hi) {
    int i = lo, j = hi + 1;      // Skanningspekare
    Comparable v = a[lo];         // Pivotelement
    
    while (true) {
        // Skanna från vänster, hitta element ≥ v
        while (less(a[++i], v)) 
            if (i == hi) break;
        
        // Skanna från höger, hitta element ≤ v
        while (less(v, a[--j])) 
            if (j == lo) break;
        
        // Kontrollera om pekarna har mötts
        if (i >= j) break;
        
        // Byt element som är på fel sida
        exch(a, i, j);
    }
    
    // Sätt pivoten på sin slutgiltiga position
    exch(a, lo, j);
    return j;
}
```

### Detaljerad Spårning av Partition

```
┌─────────────────────────────────────────────────────────────────────┐
│  PARTITIONERINGSSPÅRNING                                           │
│  Pivot v = K                                                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Initial:    i  j   [K][R][A][T][E][L][E][P][U][I][M][Q][C][X][O][S]│
│              0  16                                                  │
│                                                                     │
│  Skanna:     i=1 (R≥K), j=12 (C≤K)                                 │
│  Byt R,C:        [K][C][A][T][E][L][E][P][U][I][M][Q][R][X][O][S]  │
│                                                                     │
│  Skanna:     i=3 (T≥K), j=9 (I≤K)                                  │
│  Byt T,I:        [K][C][A][I][E][L][E][P][U][T][M][Q][R][X][O][S]  │
│                                                                     │
│  Skanna:     i=5 (L≥K), j=6 (E≤K)                                  │
│  Byt L,E:        [K][C][A][I][E][E][L][P][U][T][M][Q][R][X][O][S]  │
│                                                                     │
│  Skanna:     i=6, j=5 (pekarna har korsats!)                       │
│  Byt K med a[j]:                                                   │
│  Resultat:       [E][C][A][I][E][K][L][P][U][T][M][Q][R][X][O][S]  │
│                   ≤K      ≤K  =K  ≥K          ≥K                   │
│                                                                     │
│  Return j = 5                                                      │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## ⚠️ Kritiska Implementationsdetaljer

### 1. Varför Slumpa Först?

**Problem:** Om arrayen redan är sorterad (eller nästan sorterad) och vi alltid väljer första elementet som pivot, får vi **kvadratisk tid**!

```
Sorterad array: [1, 2, 3, 4, 5, 6, 7, 8]
Pivot = 1 → Partitioner: [1 | 2, 3, 4, 5, 6, 7, 8]
Pivot = 2 → Partitioner: [2 | 3, 4, 5, 6, 7, 8]
...
= N partitioneringar med N element vardera = O(N²)!
```

**Lösning:** Slumpmässig blandning garanterar att vi med **extremt hög sannolikhet** får bra partitioner.

### 2. Hantering av Lika Nycklar

**Viktigt:** Vi stannar skanningen för element som är **lika med** pivoten (inte bara större/mindre). Detta undviker kvadratisk tid för arrayer med många dubbletter!

### 3. Undvik Oändlig Rekursion

**Problem:** Om partitioneringen inte garanterar att **minst ett element** hamnar på rätt plats, kan vi hamna i oändlig loop.

**Lösning:** Vår implementation garanterar alltid att pivoten placeras korrekt.

---

## 📊 Prestandaanalys av Quicksort

### Proposition K: Genomsnittlig Tid

> **Proposition K:** Quicksort använder ~2N ln N jämförelser (och en sjättedel så många byten) i genomsnitt för att sortera en array med N distinkta nycklar.

### Intuition för Beviset

**Bästa fall:** Perfekt balanserad partitionering (arrayen delas exakt på mitten varje gång)
- Ger samma rekurrens som mergesort: C(N) = 2C(N/2) + N
- Lösning: C(N) ~ N lg N

**Genomsnittligt fall:** Partitioneringen är "ganska bra" i genomsnitt
- Matematisk analys visar: C(N) ~ 2N ln N ≈ 1.39N lg N
- Endast ~39% fler jämförelser än bästa möjliga!

### Proposition L: Värsta Fall

> **Proposition L:** Quicksort använder ~N²/2 jämförelser i värsta fall, men slumpmässig blandning skyddar mot detta.

**Värsta fallet:** Varje partition ger en tom subarray och en med N-1 element.

```
Antal jämförelser: N + (N-1) + (N-2) + ... + 1 = N(N-1)/2 ~ N²/2
```

**Men:** Sannolikheten för detta är **astronomiskt liten** efter slumpmässig blandning!

> 💡 **Praktisk konsekvens:** Sannolikheten att quicksort använder lika många jämförelser som insertion sort är mindre än sannolikheten att din dator träffas av blixten under sorteringen!

### Sammanfattning av Komplexitet

| Aspekt | Komplexitet |
|--------|-------------|
| Tid (värsta fall) | O(N²) |
| Tid (bästa fall) | O(N log N) |
| Tid (genomsnitt) | O(N log N) |
| Extra minne | O(log N) för rekursion |
| Stabil? | Nej ✗ |

---

## 🔧 Förbättringar av Quicksort

### 1. Cutoff till Insertion Sort

Samma idé som för mergesort – för små subarrayer är insertion sort snabbare.

```java
private static final int CUTOFF = 10;  // Typiskt 5-15

private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo + CUTOFF) {
        Insertion.sort(a, lo, hi);
        return;
    }
    // ... resten av quicksort
}
```

**Resultat:** 15-20% snabbare i praktiken!

### 2. Median-of-Three Partitionering

**Idé:** Istället för att alltid välja första elementet som pivot, välj **medianen av tre element** (första, mittersta, sista).

**Fördelar:**
- Bättre partitionering i genomsnitt
- Eliminerar behov av gränskontroller i inre loop
- Cirka 10% snabbare

### 3. Tre-vägs Partitionering (Dijkstras Dutch National Flag)

**Problem:** För arrayer med många **duplicerade nycklar** kan standard-quicksort vara ineffektiv.

**Lösning:** Partitionera i **tre delar**: mindre än, lika med, och större än pivoten.

```
┌─────────────────────────────────────────────────────────────────────┐
│  TRE-VÄGS PARTITIONERING                                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  FÖRE:  [  ?  ?  ?  ?  ?  ?  ?  ?  ?  ]                           │
│                                                                     │
│  EFTER: [ < v | = v = v = v | > v > v ]                           │
│          lt              gt                                        │
│                                                                     │
│  Alla element = v behöver INTE sorteras rekursivt!                 │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Implementation av Tre-vägs Quicksort

```java
private static void sort(Comparable[] a, int lo, int hi) {
    if (hi <= lo) return;
    
    int lt = lo, i = lo + 1, gt = hi;
    Comparable v = a[lo];
    
    while (i <= gt) {
        int cmp = a[i].compareTo(v);
        if      (cmp < 0) exch(a, lt++, i++);  // Mindre: flytta till vänster
        else if (cmp > 0) exch(a, i, gt--);    // Större: flytta till höger
        else              i++;                  // Lika: låt vara
    }
    
    // Nu: a[lo..lt-1] < v = a[lt..gt] < a[gt+1..hi]
    sort(a, lo, lt - 1);
    sort(a, gt + 1, hi);
}
```

### Visualisering av Tre-vägs Partitionering

```
┌─────────────────────────────────────────────────────────────────────┐
│  TRE-VÄGS PARTITIONERING: EXEMPEL                                  │
│  Array: [B A B A B A B A C A D A B R A]                            │
│  Pivot v = B                                                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Start:  lt=0, i=1, gt=14                                          │
│          [B][A][B][A][B][A][B][A][C][A][D][A][B][R][A]             │
│           lt i                                               gt    │
│                                                                     │
│  a[1]='A' < 'B': exch(a,lt,i), lt++, i++                          │
│          [A][B][B][A][B][A][B][A][C][A][D][A][B][R][A]             │
│              lt i                                            gt    │
│                                                                     │
│  ... efter alla steg ...                                           │
│                                                                     │
│  Resultat: [A][A][A][A][A][A][B][B][B][B][C][D][R]                 │
│             <B  <B  <B  <B  <B  =B =B =B =B  >B >B >B              │
│                                lt       gt                         │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Proposition N: Entropi-optimal Sortering

> **Proposition N:** Quicksort med tre-vägs partitionering använder ~(2 ln 2)NH jämförelser för att sortera N element, där H är Shannon-entropin definierad från frekvenserna av nyckelvärden.

**Praktisk betydelse:**
- För arrayer med **många dubbletter** kan tre-vägs quicksort vara **linjär**!
- Detta gör tre-vägs quicksort till det självklara valet för **bibliotekssortering**

---

## 📊 Jämförelse: Mergesort vs Quicksort

```
┌─────────────────────────────────────────────────────────────────────┐
│  MERGESORT vs QUICKSORT: FULLSTÄNDIG JÄMFÖRELSE                    │
├──────────────────────┬──────────────────┬──────────────────────────┤
│  Aspekt              │  Mergesort       │  Quicksort               │
├──────────────────────┼──────────────────┼──────────────────────────┤
│  Tid (genomsnitt)    │  O(N log N)      │  O(N log N)              │
│  Tid (värsta fall)   │  O(N log N) ✓    │  O(N²) *                 │
│  Extra minne         │  O(N) ✗          │  O(log N) ✓              │
│  Stabil?             │  Ja ✓            │  Nej ✗                   │
│  In-place?           │  Nej ✗           │  Ja ✓                    │
│  Adaptiv?            │  Nej             │  Ja (med 3-way)          │
│  Cache-effektiv?     │  Måttlig         │  Utmärkt ✓               │
│  Konstant faktor     │  Högre           │  Lägre ✓                 │
├──────────────────────┼──────────────────┼──────────────────────────┤
│  Bäst för            │  Länkade listor  │  Generell sortering      │
│                      │  Extern sortering│  In-memory arrayer       │
│                      │  Stabilitet krävs│  Prestanda kritisk       │
└──────────────────────┴──────────────────┴──────────────────────────┘

* Med slumpmässig blandning är detta extremt osannolikt
```

### Property T: Quicksort Är Snabbast

> **Property T:** Quicksort är den snabbaste generella sorteringsalgoritmen.

**Evidens:**
- Kortast inre loop av alla jämförelsebaserade sorteringar
- Utmärkt cache-prestanda (sekventiell minnesåtkomst)
- ~1.39N lg N jämförelser med liten konstant
- Med tre-vägs partitionering: linjär för många praktiska fall

---

## 🧠 Konceptuell Sammanfattning

### Mergesort: Styrkor och Svagheter

**Styrkor:**
- **Garanterad** O(N log N) tid
- **Stabil** (bevarar ordning för lika nycklar)
- Utmärkt för **extern sortering** och **länkade listor**
- Förutsägbar prestanda

**Svagheter:**
- Kräver **O(N) extra minne**
- Högre konstant faktor än quicksort
- Inte adaptiv (kan inte utnyttja befintlig ordning)

### Quicksort: Styrkor och Svagheter

**Styrkor:**
- **Snabbast i praktiken** för de flesta tillämpningar
- **In-place** (endast O(log N) extra för rekursionsstack)
- Utmärkt **cache-prestanda**
- **Adaptiv** med tre-vägs partitionering

**Svagheter:**
- **Inte stabil**
- O(N²) värsta fall (men extremt osannolikt)
- Kräver slumpmässig blandning för garantier

### När Använda Vilken?

| Situation | Rekommendation |
|-----------|----------------|
| Allmän sortering | Quicksort (vanligtvis) |
| Stabilitet krävs | Mergesort |
| Länkade listor | Bottom-up mergesort |
| Extern sortering (stora filer) | Mergesort |
| Många dubbletter | Tre-vägs quicksort |
| Worst-case garanti kritisk | Mergesort |
| Minnesbegränsning | Quicksort |

---

## 🔑 Propositioner att Komma Ihåg

### Mergesort

| Proposition | Påstående |
|-------------|-----------|
| **F** | Top-down mergesort: ½N lg N till N lg N jämförelser |
| **G** | Top-down mergesort: högst 6N lg N arrayåtkomster |
| **H** | Bottom-up mergesort: samma som F och G |

### Quicksort

| Proposition | Påstående |
|-------------|-----------|
| **K** | ~2N ln N ≈ 1.39N lg N jämförelser i genomsnitt |
| **L** | ~N²/2 jämförelser i värsta fall (men osannolikt) |
| **N** | Tre-vägs quicksort är entropi-optimal |

---

## 📝 Praktiska Övningar

### Mergesort

**2.2.2** Visa hur top-down mergesort sorterar E A S Y Q U E S T I O N.

**2.2.3** Visa hur bottom-up mergesort sorterar samma array.

**2.2.8** Visa att med testet `a[mid] <= a[mid+1]` blir mergesort linjär för sorterade arrayer.

### Quicksort

**2.3.1** Visa hur partition() partitionerar E A S Y Q U E S T I O N.

**2.3.2** Visa hur quicksort sorterar samma array (ignorera shuffle).

**2.3.5** Skriv kod för att sortera en array med bara två distinkta nycklar.

**2.3.12** Visa hur tre-vägs partitionering fungerar på B A B A B A B A C A D A B R A.

---

## 🎯 Minnesregler

### MERGE - För Mergesort

```
M - Mitten (dela alltid på mitten)
E - Extra minne krävs (O(N))
R - Rekursiv (eller iterativ bottom-up)
G - Garanterad N log N
E - Effektiv merge-operation
```

### QUICK - För Quicksort

```
Q - Quickest i praktiken
U - Unstable (ej stabil)
I - In-place (bara O(log N) extra)
C - Choose pivot wisely (shuffle!)
K - Killer för duplicates (3-way)
```

---

*Detta material är baserat på "Algorithms, 4th Edition" av Robert Sedgewick och Kevin Wayne.*
