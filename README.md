
# Coding Interview Spickzettel (einfach erklärt)

| Thema | Typische Problemstellungen | Vorgehen – Schritt für Schritt (Einsteigerfreundlich erklärt) |
|---|---|---|
| **Sliding Window – feste Größe** | Durchschnitt oder Maximum eines Fensters der Länge K; Anzahl gültiger Fenster; Summe in jedem festen Abschnitt | 1️⃣ Stell dir ein Fenster vor, das über das Array gleitet. <br>2️⃣ Beginne links (l=0) und erweitere das Fenster nach rechts (r). <br>3️⃣ Addiere neue Elemente in eine laufende Summe. <br>4️⃣ Wenn das Fenster zu groß ist (`r - l + 1 > K`): Ziehe das linke Element wieder ab und verschiebe `l`. <br>5️⃣ Wenn das Fenster genau K groß ist: speichere das Ergebnis (z. B. Durchschnitt, Maximum, Summe). <br>➡️ **Erkennung:** Fenstergröße ist **fest** und ändert sich nie. |
```go
// Codebeispiel Sliding Window – feste Größe
```
| **Sliding Window – variable Größe** | Längster Substring mit Summe ≤ S; Kürzester Subarray ≥ S; längste Sequenz mit Bedingung | 1️⃣ Fenster starten (l=0). <br>2️⃣ Füge Elemente hinzu (r++) und aktualisiere Summe oder Zähler. <br>3️⃣ Wenn Bedingung verletzt ist (z. B. Summe > S): verschiebe `l` nach rechts, um das Fenster kleiner zu machen. <br>4️⃣ Sobald die Bedingung wieder passt: prüfe, ob das aktuelle Fenster das beste Ergebnis liefert (z. B. längstes oder kürzestes). <br>➡️ **Erkennung:** Fenster wächst und schrumpft je nach Regel, **nicht fest**. |
```go
func twoSum(nums []int, target int) []int {
    var res []int
    start := 0
    for start < len(nums) {
        end := len(nums) - 1
        for end > start {
            if nums[start] + nums[end] == target {
                res = append(res,start)
                res = append(res,end)
                return res
            }
            end--
        }
        start++
    }
    return res
}
```
| **Sliding Window – variable Größe mit Duplikaten** | Längster Substring ohne Wiederholungen; Anzahl Substrings mit ≤ K verschiedenen Buchstaben | 1️⃣ Verwende ein Wörterbuch/Map, um zu zählen, welche Zeichen im Fenster sind. <br>2️⃣ Füge neue Zeichen hinzu (`r++`). <br>3️⃣ Wenn ein Zeichen doppelt vorkommt oder zu viele verschiedene Zeichen enthalten sind, schiebe `l` vor, bis Regel wieder gilt. <br>4️⃣ Notiere jedes Mal die Fensterlänge, wenn sie die bisher größte ist. <br>➡️ **Erkennung:** Zeichen oder Zahlen dürfen **nicht doppelt** vorkommen → du brauchst eine **Map oder ein Set**. |
```go
func fourSum(nums []int, target int) [][]int {
    var res [][]int
    if len(nums) < 4 {
        return res
    }
    slices.Sort(nums)
    for i := 0 ; i < len(nums) ; i++ {
        if i > 0 && nums[i - 1] == nums[i] {
            continue
        }
        for j := i + 1 ; j < len(nums) ; j++ {
            if j > i + 1 && nums[j - 1] == nums[j] {
                continue
            }
            start := j + 1
            end := len(nums) - 1
            for start < end {
                if nums[start] + nums[end] + nums[i] + nums[j] < target {
                    start++
                } else if nums[start] + nums[end] + nums[i] + nums[j] > target {
                    end--
                } else {
                    var respart []int
                    respart = append(respart,nums[i])
                    respart = append(respart,nums[j])
                    respart = append(respart,nums[start])
                    respart = append(respart,nums[end])
                    res = append(res, respart)
                    start++
                    end--
                    for start < len(nums) - 1 && nums[start] == nums[start - 1] {
                        start++
                    }
                    for end > start && nums[end] == nums[end + 1] {
                        end--
                    }
                }
            }
        }
    }
    return res
}
```
| **HashMaps für String-Analysen** | Anagramme prüfen, Wortfrequenzen zählen, erstes einzigartiges Zeichen, Top-K Wörter | 1️⃣ Erstelle eine leere Map (z. B. `map[char]int`). <br>2️⃣ Laufe durch den String und zähle, wie oft jedes Zeichen vorkommt. <br>3️⃣ Danach entscheide anhand der Map (z. B. welches Zeichen nur einmal vorkommt, oder welche Counts gleich sind). <br>4️⃣ Bei mehreren Strings (z. B. Anagramme): erstelle Count-Maps für beide und vergleiche sie. <br>➡️ **Erkennung:** Du musst **Zählen oder Vergleichen** → Map ist fast immer die richtige Wahl. |
```go
func lengthOfLongestSubstring(s string) int {
    var runes [256]int
    for i := range runes { 
        runes[i] = -1
    }
    left := 0
    maxLen := 0

    for right :=0 ; right < len(s) ; right++ {
        char := s[right]
        if runes[char] >= left {
            left = runes[char] + 1
        }
        runes[char] = right

        currentLen:= right - left + 1
        if currentLen > maxLen {
            maxLen = currentLen
        }
    }
    return maxLen
}
```
| **Recursive Descent Parsing** | Mathematische Ausdrücke (+,-,*,/), verschachtelte Klammern, boolesche Logik | 1️⃣ Zerlege den Ausdruck in Tokens (Zahlen, Operatoren, Klammern). <br>2️⃣ Definiere eine Regel: z. B. „Ein Ausdruck besteht aus Termen, getrennt durch + oder -“. <br>3️⃣ Implementiere für jede Regel eine eigene Funktion. <br>4️⃣ Die Funktionen rufen sich gegenseitig auf (rekursiv). <br>5️⃣ Halte die Reihenfolge der Operatoren korrekt (Multiplikation vor Addition). <br>➡️ **Erkennung:** Wenn du **verschachtelte** oder **hierarchische Strukturen** (Klammern, Regeln) erkennst → Parsing. |
```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "strings"
    "unicode"
)

const (
	D = "\033[0m"
	R = "\033[31m"
	G = "\033[32m"
	Y = "\033[33m"
	B = "\033[34m"
	P = "\033[35m"
	C = "\033[36m"
	W = "\033[37m"
)

func skipWhitespace(input string, pos int) int {
    for pos < len(input) && unicode.IsSpace(rune(input[pos])) {
        pos++
    }
    return pos
}

// Parse eine Zahl (eine oder mehrere Ziffern)
func parseNumber(input string, pos int) (int, int, error) {
    num := 0
    for pos < len(input) && input[pos] >= '0' && input[pos] <= '9' {
        num = (num * 10) + int(input[pos]-'0')
        pos++
    }
    return num, pos, nil
}

// Parse einen Factor: Number oder (Expression)
func parseFactor(input string, pos int) (int, int, error) {
    pos = skipWhitespace(input, pos)

    if pos >= len(input) {
        return 0, pos, fmt.Errorf("unexpected end at position %d", pos)
    }

    if input[pos] >= '0' && input[pos] <= '9' {
        return parseNumber(input, pos)
    } else if input[pos] == '(' {
        pos++
        res, newPos, err := parseExpression(input, pos)
        if err != nil {
            return 0, newPos, err
        }
        pos = newPos

        pos = skipWhitespace(input, pos)
        if pos >= len(input) || input[pos] != ')' {
            return 0, pos, fmt.Errorf("expected ')' at position %d", pos)
        }
        pos++
        return res, pos, nil
    }
    return 0, pos, fmt.Errorf("expected number or '(' at position %d", pos)
}

// Parse einen Term: Factor (* oder / Factor)*
func parseTerm(input string, pos int) (int, int, error) {
	result, newPos, err := parseFactor(input, pos)
	if err != nil {
    	return 0, pos, err
	}
	pos = newPos
	pos = skipWhitespace(input, pos)
	for pos < len(input) && (input[pos] == '*' || input[pos] == '/') {
		operator := input[pos]
		pos++
		facTwo, newerPos, err := parseFactor(input, pos)
		if err != nil {
    		return 0, pos, err
		}
		pos = newerPos
		if operator == '*' {
			result = result * facTwo
		} else {
			if facTwo == 0 {
    			return 0, pos, fmt.Errorf("division by zero")
			}
			result = result / facTwo
		}
		pos = skipWhitespace(input, pos)
	}
    // return: (result, neue Position, error)
    return result, pos, nil
}

// Parse eine Expression: Term (+ oder - Term)*
func parseExpression(input string, pos int) (int, int, error) {
	pos = skipWhitespace(input, pos)
	result, newPos, err := parseTerm(input, pos)
	if err != nil {
    	return 0, pos, err
	}
	pos = newPos
	pos = skipWhitespace(input, pos)
	for pos < len(input) && (input[pos] == '+' || input[pos] == '-') {
		operator := input[pos]
		pos++
		facTwo, newerPos, err := parseTerm(input, pos)
		if err != nil {
    		return 0, pos, err
		}
		pos = newerPos
		if operator == '+' {
			result = result + facTwo
		} else {
			result = result - facTwo
		}
		pos = skipWhitespace(input, pos)
	}
    // return: (result, neue Position, error)
    return result, pos, nil
}

func calculate(input string) (int, error) {
    result, pos, err := parseExpression(input, 0)
    if err != nil {
        return 0, err
    }

    pos = skipWhitespace(input, pos)
    if pos < len(input) {
        return 0, fmt.Errorf("unexpected character at position %d", pos)
    }

    return result, nil
}

func main() {
    fmt.Println(C + "\n████████████████████████████████████████" + D)
    fmt.Println(C + "██                                    ██" + D)
    fmt.Println(C + "██    Welcome to the Go Calculator    ██" + D)
    fmt.Println(C + "██                                    ██" + D)
    fmt.Println(C + "████████████████████████████████████████" + D)

    scanner := bufio.NewScanner(os.Stdin)

    for {
        fmt.Print(B + ": " + D)

        if !scanner.Scan() {
            break
        }

        prompt := strings.TrimSpace(scanner.Text())

        if prompt == "EXIT" {
            break
        }

        if prompt == "" {
            continue
        }

        res, err := calculate(prompt)
        if err != nil {
            fmt.Println(R + err.Error() + D)
        } else {
            fmt.Println(G + fmt.Sprintf("%d", res) + D)
        }
    }
}
```
| **Integer-/Math-Aufgaben (Go)** | Größter gemeinsamer Teiler (GCD), kleinster gemeinsamer Vielfaches (LCM), Max/Min finden, Summen, Differenzen | 1️⃣ Überlege: muss ich Werte vergleichen, Summen bilden oder aufteilen? <br>2️⃣ Schreibe Hilfsfunktionen (`min`, `max`, `abs`). <br>3️⃣ Für GCD: wiederhole, bis Rest 0 ist (`a, b = b, a % b`). <br>4️⃣ Für LCM: `a / gcd(a,b) * b`. <br>5️⃣ Teste immer mit negativen Zahlen oder Overflow-Grenzen. <br>➡️ **Erkennung:** Wenn es um **Zahlenverhältnisse** oder **Optimierung (größer/kleiner)** geht. |
```go
func reverse(x int) int {
    ascii := strconv.Itoa(x)
    asciir := ""
    i := 0
    ri := len(ascii) - 1
    if ascii[i] == '-' {
        i++
        asciir += string('-')
    }
    for ri >= i {
        asciir += string(ascii[ri])
        ri--
    }

    val, err := strconv.Atoi(asciir)
    if err == nil {
        fmt.Printf("%T \n %v", val, val)
    }
    if val < math.MinInt32 || val > math.MaxInt32 {
        return 0
    }
    return val
}
```
| **Linked Lists – Head vs. Dummy Node** | Entferne das N-te Element von hinten, füge Knoten ein, merge sortiere Listen | 1️⃣ Wenn sich der Kopf der Liste ändern kann → **Dummy Node** verwenden. <br>2️⃣ Dummy zeigt auf den echten Kopf. So verlierst du ihn nie. <br>3️⃣ Arbeite mit Zeigern (`prev`, `curr`, `next`) und sichere `next` bevor du etwas veränderst. <br>4️⃣ Wenn du fertig bist, gib `dummy.Next` zurück. <br>➡️ **Erkennung:** Wenn du etwas am **Anfang** der Liste änderst → Dummy benutzen! |
```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{Next: head}
    tmp := dummy
    lens := head
    len := 0
    for lens != nil {
        fmt.Println(lens.Val)
        lens = lens.Next
        len++
    }
    rm := len - n
    for rm > 0 && tmp != nil {
        tmp = tmp.Next
        rm--
    }
    tmp.Next = tmp.Next.Next

    return dummy.Next
}
```
| **Go Maps – Beispiel: Römische Ziffern** | Roman → Integer, Integer → Roman | 1️⃣ Erstelle eine Map mit Buchstaben und ihren Werten (`I=1, V=5, X=10` ...). <br>2️⃣ Laufe durch den String. <br>3️⃣ Wenn der aktuelle Wert kleiner ist als der nächste → **subtrahiere** ihn. <br>4️⃣ Sonst addiere ihn. <br>5️⃣ Addiere alles, fertig. <br>➡️ **Erkennung:** Wenn feste Symbol-Tabelle bekannt ist → **Map Lookup** verwenden. |
```go
func intToRoman(num int) string {
    values := []int{1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1}
    symbols := []string{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"}
    
    result := ""
    
    for i := 0; i < len(values); i++ {
        for num >= values[i] {
            result += symbols[i]
            num -= values[i]
        }
    }
    
    return result
}
```
```go
func romanToInt(s string) int {
    values := []int{1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1}
    symbols := []string{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"}
    i := 0
    var matched int
    num := 0
    for i < len(s) {
        index := -1
        if (i + 1) < len(s) {
            checker := s[i:(i + 2)]
            fmt.Println("c> " + checker)
            for p, symbol := range symbols {
                if symbol == checker {
                    index = p
                    break
                }
            }
            matched = 2
        }
        if index < 0 {
            fmt.Println("n> " + string(s[i]))
            for p, symbol := range symbols {
                if symbol == string(s[i]) {
                    index = p
                    break
                }
            }
            matched = 1
        }
        if index >= 0 {
            num += values[index]
        }
        i = i + matched
    }
    return num
}
```
| **Backtracking – Permutationen** | Alle möglichen Anordnungen von Zahlen oder Buchstaben | 1️⃣ Du willst **alle Kombinationen**, also probiere alles aus. <br>2️⃣ Nutze eine Liste für das aktuelle Ergebnis (`path`). <br>3️⃣ Wenn das Ergebnis komplett ist (alle Elemente benutzt): speichere es. <br>4️⃣ Danach gehe einen Schritt zurück (**Backtrack**) und probiere den nächsten Weg. <br>➡️ **Erkennung:** Wenn du **„alle Möglichkeiten“** oder **„alle Reihenfolgen“** suchst. |
```go
func backtrack(res *[]string, current string, opened int, closed int, n int) {
    if opened == n && closed == n {
        *res = append(*res, current)
    }
    if opened < n {
        backtrack(res, current + "(", opened + 1, closed, n)
    }
    if opened > closed {
        backtrack(res, current + ")", opened, closed + 1, n)
    }
}

func generateParenthesis(n int) []string {
    var res []string
    backtrack(&res, "", 0, 0, n)
    return res
}
```
| **Backtracking – Kartesisches Produkt** | Alle Kombinationen aus mehreren Listen (z. B. `[a,b] × [1,2] → a1,a2,b1,b2`) | 1️⃣ Gehe Ebene für Ebene durch (z. B. erste Liste → zweite → dritte). <br>2️⃣ In jedem Schritt wähle ein Element und geh tiefer. <br>3️⃣ Wenn du am Ende bist (alle Listen durch): speichere Ergebnis. <br>➡️ **Erkennung:** Du willst **alle Kombinationen**, aber Reihenfolge innerhalb von Gruppen ist egal. |
```go
func letterCombinations(digits string) []string {
    if digits == "" {
        return []string{}
    }
    res := []string{""}
    var phoneMap = map[rune][]rune{
        '2': {'a', 'b', 'c'},
        '3': {'d', 'e', 'f'},
        '4': {'g', 'h', 'i'},
        '5': {'j', 'k', 'l'},
        '6': {'m', 'n', 'o'},
        '7': {'p', 'q', 'r', 's'},
        '8': {'t', 'u', 'v'},
        '9': {'w', 'x', 'y', 'z'},
    }
    for i := 0 ; i < len(digits) ; i++ {
        temp := []string{} 
        for _, resval := range res {
            for _, value := range phoneMap[rune(digits[i])] {
                temp = append(temp, resval + string(value))
            }
        }
        res = temp
    }
    return res
}
```
| **Stack-Probleme** | Klammerprüfung, nächstgrößeres Element, Histogrammfläche, Syntaxvalidierung | 1️⃣ Stack ist wie ein Tellerstapel: Du siehst immer den obersten. <br>2️⃣ Bei öffnenden Symbolen: auf den Stack legen. <br>3️⃣ Bei schließenden Symbolen: obersten prüfen, ob er passt, sonst Fehler. <br>4️⃣ Für Zahlenprobleme (z. B. nächstgrößeres Element): Stack enthält Indizes, die auf nächste größere Zahl warten. <br>➡️ **Erkennung:** Wenn du **letzte offene Elemente**, **Verschachtelungen** oder **„nächstes Größeres/Kleineres“** brauchst. |
```go
func isValid(s string) bool {
    var stack []rune
    for i := 0 ; i < len(s) ; i++ {
        if rune(s[i]) == '(' || rune(s[i]) == '{' || rune(s[i]) == '[' {
            stack = append(stack, rune(s[i]))
        } else if len(stack) > 0 {
            top := rune(stack[len(stack) - 1])
            if top == '(' && s[i] == ')' || top == '{' && s[i] == '}' || top == '[' && s[i] == ']' {
                stack = stack[:(len(stack) - 1)]
            } else {
                return false
            }
        } else {
            return false
        }
    }
    return len(stack) == 0
}
```
