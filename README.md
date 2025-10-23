
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
// fuer eine leere neue Liste zum returnrn
    dummy := &ListNode{}
    current := dummy
    return dummy.Next
```
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
| **Regex, Pasrsing Backtrack** |
walk backtrack trhoug strin
| **Backtracking – Permutationen** | Alle möglichen Anordnungen von Zahlen oder Buchstaben | 
how distiguish branches?

base cases?
how avoid duplicates?
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
| **JSON Parser** |
```go
package json_parser

// NewJSONParser erstellt einen neuen Parser für den gegebenen Input.
func NewJSONParser(input string) *JSONParser { return nil }

// tokenize zerlegt den Input in Tokens.
func (p *JSONParser) tokenize() {}

// current gibt das aktuelle Byte an der Leseposition zurück.
func (p *JSONParser) current() byte { return 0 }

// advance bewegt die Leseposition um ein Zeichen weiter.
func (p *JSONParser) advance() {}

// skipWhitespace überspringt Leerzeichen/Whitespace.
func (p *JSONParser) skipWhitespace() {}

// nextToken liest das nächste Token aus dem Input.
func (p *JSONParser) nextToken() Token { return Token{} }

// readString liest einen String (inkl. Escape-Handling).
func (p *JSONParser) readString() Token { return Token{} }

// readNumber liest eine Zahl (inkl. optionalem Minus und Dezimalteil).
func (p *JSONParser) readNumber() Token { return Token{} }

// readTrue liest das Literal true.
func (p *JSONParser) readTrue() Token { return Token{} }

// readFalse liest das Literal false.
func (p *JSONParser) readFalse() Token { return Token{} }

// readNull liest das Literal null.
func (p *JSONParser) readNull() Token { return Token{} }

// currentToken gibt das aktuelle Token im Token-Stream zurück.
func (p *JSONParser) currentToken() Token { return Token{} }

// consumeToken verbraucht das aktuelle Token und rückt weiter.
func (p *JSONParser) consumeToken() Token { return Token{} }

// expectToken erwartet ein bestimmtes Token und verbraucht es.
func (p *JSONParser) expectToken(expectedType TokenType) error { return nil }

// Parse startet den Parse-Vorgang (Recursive Descent) für den gesamten Input.
func (p *JSONParser) Parse() error { return nil }

// parseValue parst einen JSON-Wert (String, Number, Boolean, Null, Objekt, Array).
func (p *JSONParser) parseValue() error { return nil }

// parseObject parst ein JSON-Objekt { ... }.
func (p *JSONParser) parseObject() error { return nil }

// parseKeyValue parst ein Key-Value-Paar innerhalb eines Objekts.
func (p *JSONParser) parseKeyValue() error { return nil }

// parseArray parst ein JSON-Array [ ... ].
func (p *JSONParser) parseArray() error { return nil }

// IsValidJSON prüft, ob ein String gültiges JSON ist.
func IsValidJSON(jsonStr string) bool { p.advance() return false }

// ShowTokens gibt die erzeugten Tokens zu einem JSON-String aus.
func ShowTokens(jsonStr string) {}

// TestTokenizedParser führt einige Beispieltests gegen den Parser aus.
func TestTokenizedParser() {}
```
**Markdown**
```go
package markdown

// ---- Tokens ---------------------------------------------------------------

// TokenType definiert die Token-Arten für ein Markdown-Subset.
type TokenType int

const (
	// Blöcke
	TOK_HEADING TokenType = iota // '#'..'######' + Text (ATX)
	TOK_PARAGRAPH                // laufender Text
	TOK_CODE_FENCE               // ``` oder ~~~ (Beginn/Ende)
	TOK_BLOCKQUOTE               // '>'
	TOK_UL_ITEM                  // '-', '*', '+'
	TOK_OL_ITEM                  // '1.' '2.' ...
	TOK_HRULE                    // '---' '***' '___'

	// Inline
	TOK_TEXT         // normaler Text
	TOK_EMPH_OPEN    // '*', '_' (einfach)
	TOK_EMPH_CLOSE
	TOK_STRONG_OPEN  // '**', '__'
	TOK_STRONG_CLOSE
	TOK_CODE_SPAN    // `code`
	TOK_LINK_OPEN    // '['
	TOK_LINK_CLOSE   // ']'
	TOK_PAREN_OPEN   // '('
	TOK_PAREN_CLOSE  // ')'
	TOK_IMAGE_BANG   // '!'

	// Steuerung
	TOK_NEWLINE
	TOK_EOF
	TOK_INVALID
)

// Token repräsentiert ein Lexem aus der Eingabe.
type Token struct {
	Type     TokenType
	Value    string // z.B. Textinhalt, Fence-Sequenz, Heading-Hashes
	Position int    // Byte-Offset
}

// ---- AST ------------------------------------------------------------------

// NodeType für die AST-Knoten.
type NodeType int

const (
	N_DOCUMENT NodeType = iota
	N_HEADING
	N_PARAGRAPH
	N_TEXT
	N_EMPH
	N_STRONG
	N_CODE_SPAN
	N_CODE_BLOCK
	N_LINK
	N_IMAGE
	N_BLOCKQUOTE
	N_LIST_UNORDERED
	N_LIST_ORDERED
	N_LIST_ITEM
	N_HORIZONTAL_RULE
)

// Node ist ein AST-Knoten.
type Node struct {
	Type     NodeType
	Literal  string            // z.B. Text, Code, URL, Title
	Level    int               // z.B. Heading-Level, Listen-Start
	Attrs    map[string]string // z.B. href/title/alt/lang
	Children []*Node
}

// ---- Lexer ----------------------------------------------------------------

// Lexer zerlegt Eingabetext in Tokens (zeilen- und inline-basiert).
type Lexer struct {
	input string
	pos   int
	line  int
	col   int
}

// NewLexer erstellt einen neuen Lexer.
func NewLexer(input string) *Lexer { return nil }

// NextToken liefert das nächste Token (kombiniert Block- und Inline-Erkennung).
func (l *Lexer) NextToken() Token { return Token{} }

// peek/advance-Helfer: Zeichennavigation.
func (l *Lexer) current() byte { return 0 }
func (l *Lexer) advance()      {}
func (l *Lexer) startsWith(s string) bool { return false }

// skipWhitespace am Zeilenanfang (Indentation, Spaces).
func (l *Lexer) skipWhitespace() {}

// scanLine liest bis zum Zeilenende.
func (l *Lexer) scanLine() string { return "" }

// scanBlock erkennt Blockkonstrukte (Heading, Lists, Quote, Fence, HR).
func (l *Lexer) scanBlock() Token { return Token{} }

// scanInline zerlegt eine Zeile in Inline-Tokens (emph/strong/code/link).
func (l *Lexer) scanInline(line string) []Token { return nil }

// ---- Parser ---------------------------------------------------------------

// Parser wandelt Token-Stream in einen AST um.
type Parser struct {
	lexer       *Lexer
	buffer      []Token // Lookahead-Puffer (für Inline/Block-Wechsel)
	currentTok  Token
	peeked      bool
}

// NewParser erstellt einen neuen Parser.
func NewParser(input string) *Parser { return nil }

// Parse parst das gesamte Dokument und gibt die Wurzel zurück.
func (p *Parser) Parse() (*Node, error) { return nil, nil }

// consume/peek-Helfer für Token-Fluss.
func (p *Parser) next() Token { return Token{} }
func (p *Parser) peek() Token { return Token{} }
func (p *Parser) expect(tt TokenType) (Token, error) { return Token{}, nil }

// parseDocument parst fortlaufend Blöcke bis EOF.
func (p *Parser) parseDocument() (*Node, error) { return nil, nil }

// ---- Block-Parser ----

// parseHeading parst ATX-Überschriften.
func (p *Parser) parseHeading() (*Node, error) { return nil, nil }

// parseParagraph parst einen Absatz (inkl. inline).
func (p *Parser) parseParagraph() (*Node, error) { return nil, nil }

// parseBlockquote parst verschachtelte Blockquotes.
func (p *Parser) parseBlockquote() (*Node, error) { return nil, nil }

// parseList parst geordnete/ungeordnete Listen inkl. Verschachtelung.
func (p *Parser) parseList(ordered bool) (*Node, error) { return nil, nil }

// parseListItem parst ein einzelnes Listenelement (inkl. Folgeblöcke).
func (p *Parser) parseListItem() (*Node, error) { return nil, nil }

// parseCodeFence parst fenced code blocks (```lang ... ```).
func (p *Parser) parseCodeFence() (*Node, error) { return nil, nil }

// parseHorizontalRule parst HR.
func (p *Parser) parseHorizontalRule() (*Node, error) { return nil, nil }

// ---- Inline-Parser ----

// parseInline wandelt Inline-Tokens in eine Node-Liste (Text, Emph, Strong, Link, CodeSpan).
func (p *Parser) parseInline(inlineTokens []Token) ([]*Node, error) { return nil, nil }

// parseEmphasis parst *emph* und **strong** (inkl. verschachtelt).
func (p *Parser) parseEmphasis(tokens []Token, i int) (node *Node, next int, err error) {
	return nil, i, nil
}

// parseCodeSpan parst `code`.
func (p *Parser) parseCodeSpan(tokens []Token, i int) (node *Node, next int, err error) {
	return nil, i, nil
}

// parseLinkOrImage parst [text](url "title") bzw. ![alt](src "title").
func (p *Parser) parseLinkOrImage(tokens []Token, i int) (node *Node, next int, err error) {
	return nil, i, nil
}

// ---- Öffentliche API / Utils ---------------------------------------------

// ParseMarkdown parst Markdown-Text in einen AST.
func ParseMarkdown(input string) (*Node, error) { return nil, nil }

// RenderText rendert den AST zurück in Klartext (für Tests).
func RenderText(n *Node) string { return "" }

// IsValidMarkdown prüft minimal, ob Parsen ohne Fehler möglich ist.
func IsValidMarkdown(input string) bool { return false }

// ShowTokens gibt die Tokenfolge aus (Debug).
func ShowTokens(input string) {}

// Walk ruft fn für jeden Knoten im AST (Preorder) auf.
func Walk(n *Node, fn func(*Node)) {}

// FindAll sucht alle Knoten eines Typs im AST.
func FindAll(n *Node, t NodeType) []*Node { return nil }
```
**Binary Search**
```go
func search(nums []int, target int) int {
    left := 0
    right := len(nums) - 1

    for left <= right {
        middle := left + (right - left) / 2
        if target == nums[middle] {
            return middle
        }
        if target < nums[middle] {
            right = middle - 1
        }
        if target > nums[middle] {
            left = middle + 1
        }
    }
    return -1
}
```
**JSON to NOde**
```go
package json_parser

// --- Tokens / Lexer ---

// TokenType beschreibt die möglichen JSON-Tokens.
type TokenType int

const (
	STRING TokenType = iota
	NUMBER
	BOOLEAN_TRUE
	BOOLEAN_FALSE
	NULL
	LBRACE    // {
	RBRACE    // }
	LBRACKET  // [
	RBRACKET  // ]
	COLON     // :
	COMMA     // ,
	EOF
	INVALID
)

// Token repräsentiert ein Token aus dem Lexer.
type Token struct {
	Type     TokenType
	Value    string
	Position int
}

// JSONParser hält Input, Tokenliste und Parsing-Position.
type JSONParser struct {
	input             string
	pos               int
	tokens            []Token
	currentTokenIndex int
	debug             bool
	lastErr           error
}

// NewJSONParser erstellt einen Parser für den gegebenen Input.
func NewJSONParser(input string) *JSONParser { return &JSONParser{input: input} }

// tokenize zerlegt den Input in Tokens.
func (p *JSONParser) tokenize()                           {}
func (p *JSONParser) current() byte                       { return 0 }
func (p *JSONParser) advance()                            {}
func (p *JSONParser) skipWhitespace()                     {}
func (p *JSONParser) nextToken() Token                    { return Token{} }
func (p *JSONParser) readString() Token                   { return Token{} }
func (p *JSONParser) readNumber() Token                   { return Token{} }
func (p *JSONParser) readTrue() Token                     { return Token{} }
func (p *JSONParser) readFalse() Token                    { return Token{} }
func (p *JSONParser) readNull() Token                     { return Token{} }
func (p *JSONParser) currentToken() Token                 { return Token{Type: EOF} }
func (p *JSONParser) consumeToken() Token                 { return Token{Type: EOF} }
func (p *JSONParser) expectToken(t TokenType) error       { return nil }
func (p *JSONParser) EnableDebug(enable bool)             { p.debug = enable }
func (p *JSONParser) LastError() error                    { return p.lastErr }

// --- AST (Nodes) ---

// NodeKind beschreibt die Knotenarten des JSON-AST.
type NodeKind int

const (
	NodeObject NodeKind = iota
	NodeArray
	NodeString
	NodeNumber
	NodeBoolean
	NodeNull
)

// Member ist ein Key-Value-Paar eines JSON-Objekts.
type Member struct {
	Key   string
	Value *Node
}

// Node ist ein AST-Knoten für JSON.
type Node struct {
	Kind     NodeKind
	Str      string    // für String-Literale
	Num      float64   // für Zahlen
	Bool     bool      // für true/false
	Members  []Member  // für Objekte
	Elements []*Node   // für Arrays
	// Optional: Position/Span für bessere Fehlermeldungen
	Start int
	End   int
}

// --- Parser (Tokens -> AST) ---

// Parse baut den AST für das komplette Dokument.
func (p *JSONParser) Parse() (*Node, error) { return nil, nil }

// parseValue parst einen beliebigen JSON-Wert.
func (p *JSONParser) parseValue() (*Node, error) { return nil, nil }

// parseObject parst ein Objekt { ... } und liefert NodeObject.
func (p *JSONParser) parseObject() (*Node, error) { return nil, nil }

// parseArray parst ein Array [ ... ] und liefert NodeArray.
func (p *JSONParser) parseArray() (*Node, error) { return nil, nil }

// --- Node-Helfer ---

// NewStringNode erzeugt einen String-Knoten.
func NewStringNode(s string) *Node { return &Node{Kind: NodeString, Str: s} }

// NewNumberNode erzeugt einen Number-Knoten.
func NewNumberNode(f float64) *Node { return &Node{Kind: NodeNumber, Num: f} }

// NewBooleanNode erzeugt einen Boolean-Knoten.
func NewBooleanNode(b bool) *Node { return &Node{Kind: NodeBoolean, Bool: b} }

// NewNullNode erzeugt einen Null-Knoten.
func NewNullNode() *Node { return &Node{Kind: NodeNull} }

// NewObjectNode erzeugt einen leeren Objekt-Knoten.
func NewObjectNode() *Node { return &Node{Kind: NodeObject, Members: []Member{}} }

// NewArrayNode erzeugt einen leeren Array-Knoten.
func NewArrayNode() *Node { return &Node{Kind: NodeArray, Elements: []*Node{}} }

// AddMember hängt ein Key-Value-Paar an ein Objekt.
func (n *Node) AddMember(key string, val *Node) {}

// AddElement hängt ein Element an ein Array.
func (n *Node) AddElement(val *Node) {}

// --- Öffentliche API ---

// ParseJSONToAST: High-Level-Einstieg: Input -> AST.
func ParseJSONToAST(jsonStr string) (*Node, error) {
	p := NewJSONParser(jsonStr)
	p.tokenize()
	return p.Parse()
}

// IsValidJSON prüft ausschließlich auf Gültigkeit (ohne AST-Nutzung).
func IsValidJSON(jsonStr string) bool {
	ast, err := ParseJSONToAST(jsonStr)
	_ = ast
	return err == nil
}

// Walk traversiert den AST (Preorder) mit Tiefenangabe.
func Walk(n *Node, depth int, visit func(*Node, int)) {
	if n == nil || visit == nil {
		return
	}
	visit(n, depth)
	switch n.Kind {
	case NodeObject:
		for _, m := range n.Members {
			Walk(m.Value, depth+1, visit)
		}
	case NodeArray:
		for _, e := range n.Elements {
			Walk(e, depth+1, visit)
		}
	}
}

// ToCompactJSON rendert den AST als kompaktes JSON (nützlich für Tests).
func ToCompactJSON(n *Node) string { return "" }

// ToPrettyJSON rendert den AST als formatiertes JSON (Indent).
func ToPrettyJSON(n *Node, indent string) string { return "" }

// ShowTokens gibt die Tokens (Debug).
func ShowTokens(jsonStr string) {}

// TestASTParser führt Beispieltests durch (Debug).
func TestASTParser() {}
```
*Bitshifting*
```go
func divide(dividend int, divisor int) int {
    signed := 1
    count := 0
    if dividend < 0 || divisor < 0 {
        signed = -1
    }
    if dividend < 0 && divisor < 0 {
        signed = 1
    }
    dividend = int(math.Abs(float64(dividend)))
    divisor = int(math.Abs(float64(divisor)))
    for dividend >= divisor {
        divisorTrack := divisor
        multiple := 1
        for dividend >= (divisorTrack << 1) {
            divisorTrack = divisorTrack << 1
            multiple = multiple << 1
        }
        
        dividend -= divisorTrack
        count += multiple
    }
    if count * signed > math.MaxInt32 {
        return math.MaxInt32
    }
    if count * signed < math.MinInt32 {
        return math.MinInt32
    }
    return count * signed
}
```


|**Sceleton Skelleton**|
```go
package skeleton

import (
	"fmt"
	"time"
)

// -------------------------
// Type definitions & enums
// -------------------------

// ItemType represents different types of items
type ItemType int

const (
	TypeText ItemType = iota
	TypeNumber
	TypeBool
	TypeList
	TypeMap
	TypeUnknown
)

// String method for enum - implements Stringer interface
func (t ItemType) String() string {
	return [...]string{
		"TEXT", "NUMBER", "BOOL", "LIST", "MAP", "UNKNOWN",
	}[t]
}

// Status represents processing states
type Status int

const (
	StatusPending Status = iota
	StatusActive
	StatusComplete
	StatusError
)

func (s Status) String() string {
	return [...]string{
		"PENDING", "ACTIVE", "COMPLETE", "ERROR",
	}[s]
}

// -------------------------
// Core interfaces
// -------------------------

// Processor defines methods for processable items
type Processor interface {
	Process() error
	Validate() bool
}

// -------------------------
// Basic item implementation
// -------------------------

// Item represents a basic data item
type Item struct {
	ID        string
	Name      string
	Type      ItemType
	Status    Status
	CreatedAt time.Time
	Data      interface{}
}

// NewItem creates a new item with the given parameters
func NewItem(name string, itemType ItemType, data interface{}) *Item {
	return &Item{
		ID:        fmt.Sprintf("item-%d", time.Now().UnixNano()),
		Name:      name,
		Type:      itemType,
		Status:    StatusPending,
		CreatedAt: time.Now(),
		Data:      data,
	}
}

// Process implements the Processor interface
func (i *Item) Process() error {
	// Simple processing example
	i.Status = StatusActive
	
	// Processing logic would go here
	// ...
	
	i.Status = StatusComplete
	return nil
}

// Validate implements the Processor interface
func (i *Item) Validate() bool {
	// Basic validation
	return i.Name != "" && i.Data != nil
}

// GetValue returns the item's data with proper type assertion
func (i *Item) GetValue() (interface{}, error) {
	switch i.Type {
	case TypeText:
		if val, ok := i.Data.(string); ok {
			return val, nil
		}
	case TypeNumber:
		if val, ok := i.Data.(float64); ok {
			return val, nil
		}
		if val, ok := i.Data.(int); ok {
			return val, nil
		}
	case TypeBool:
		if val, ok := i.Data.(bool); ok {
			return val, nil
		}
	case TypeList:
		if val, ok := i.Data.([]interface{}); ok {
			return val, nil
		}
	case TypeMap:
		if val, ok := i.Data.(map[string]interface{}); ok {
			return val, nil
		}
	}
	return nil, fmt.Errorf("type mismatch for %s", i.Type)
}

// -------------------------
// Item Manager
// -------------------------

// ItemManager handles collections of items
type ItemManager struct {
	items map[string]*Item
}

// NewItemManager creates a new item manager
func NewItemManager() *ItemManager {
	return &ItemManager{
		items: make(map[string]*Item),
	}
}

// Add adds an item to the manager
func (m *ItemManager) Add(item *Item) error {
	if !item.Validate() {
		return fmt.Errorf("invalid item: %s", item.Name)
	}
	
	m.items[item.ID] = item
	return nil
}

// Get retrieves an item by ID
func (m *ItemManager) Get(id string) (*Item, bool) {
	item, exists := m.items[id]
	return item, exists
}

// ProcessAll processes all items
func (m *ItemManager) ProcessAll() map[string]error {
	results := make(map[string]error)
	
	for id, item := range m.items {
		results[id] = item.Process()
	}
	
	return results
}

// Count returns the number of items by type
func (m *ItemManager) Count(itemType ItemType) int {
	count := 0
	for _, item := range m.items {
		if item.Type == itemType {
			count++
		}
	}
	return count
}
```
# JSON Parser - Grundlegendes Schema

## Tokenize-Prozess

1. **Initialisierung**
   - Leere Token-Liste erstellen
   - Position im Input auf 0 setzen

2. **Hauptschleife**
   - Whitespace überspringen
   - Aktuelles Zeichen prüfen und entsprechendes Token erstellen
   - Token zur Liste hinzufügen
   - Position im Input vorwärts bewegen
   - Bis EOF oder Ende des Inputs erreicht ist

3. **Token-Typen erkennen**
   - Strukturzeichen direkt erkennen: `{`, `}`, `[`, `]`, `:`, `,`
   - Strings mit `"` erkennen und Escape-Sequenzen verarbeiten
   - Zahlen mit optionalem Vorzeichen, Dezimalteil und Exponent erkennen
   - Literale erkennen: `true`, `false`, `null`
   - Ungültige Zeichen als `INVALID` markieren

4. **Abschluss**
   - Sicherstellen, dass ein EOF-Token am Ende der Liste steht

## Parse-Prozess

1. **Initialisierung**
   - Falls nötig, tokenize() aufrufen
   - Token-Index auf 0 setzen

2. **Rekursiver Abstieg**
   - Haupteinstieg ist parseValue()
   - Basierend auf Token-Typ wird der entsprechende Parser aufgerufen:
     - Strings, Zahlen, Booleans, null werden direkt in Knoten umgewandelt
     - Für Objekte wird parseObject() aufgerufen
     - Für Arrays wird parseArray() aufgerufen

3. **Objekt-Parsing**
   - `{` konsumieren
   - Leeres Objekt erkennen oder Schlüssel-Wert-Paare parsen
   - Jedes Paar besteht aus String-Key, `:` und einem beliebigen Wert
   - Weitere Paare folgen nach einem `,`
   - `}` am Ende konsumieren

4. **Array-Parsing**
   - `[` konsumieren
   - Leeres Array erkennen oder Werte parsen
   - Jedes Element ist ein beliebiger JSON-Wert
   - Weitere Elemente folgen nach einem `,`
   - `]` am Ende konsumieren

## AST (Abstract Syntax Tree) Aufbau

1. **Knotentypen**
   - `NodeObject`: Repräsentiert ein JSON-Objekt mit Schlüssel-Wert-Paaren
   - `NodeArray`: Repräsentiert ein JSON-Array mit geordneten Elementen
   - `NodeString`: Enthält einen String-Wert
   - `NodeNumber`: Enthält einen numerischen Wert (als float64)
   - `NodeBoolean`: Enthält einen Boolean-Wert (true/false)
   - `NodeNull`: Repräsentiert den JSON-Wert null

2. **Struktur der Knoten**
   - Jeder Knoten hat eine `Kind`-Eigenschaft (NodeKind)
   - Typ-spezifische Felder:
     - `Str` für Strings
     - `Num` für Zahlen
     - `Bool` für Booleans
     - `Members` für Objekte (Liste von Key-Value-Paaren)
     - `Elements` für Arrays (Liste von Knoten)
   - Optional: Position im Quelldokument (Start/End)

3. **Objektknoten-Aufbau**
   - `NewObjectNode()` erstellt leeres Objekt mit `Kind: NodeObject`
   - `AddMember()` fügt Key-Value-Paar zur Member-Liste hinzu
   - Schlüssel sind immer Strings
   - Werte können beliebige Knoten sein (rekursive Struktur)

4. **Array-Knoten-Aufbau**
   - `NewArrayNode()` erstellt leeres Array mit `Kind: NodeArray`
   - `AddElement()` fügt Element zur Element-Liste hinzu
   - Elemente können beliebige Knoten sein (rekursive Struktur)

5. **Primitive Knoten**
   - `NewStringNode(s)`: Erstellt String-Knoten mit Wert `s`
   - `NewNumberNode(f)`: Erstellt Number-Knoten mit Wert `f`
   - `NewBooleanNode(b)`: Erstellt Boolean-Knoten mit Wert `b`
   - `NewNullNode()`: Erstellt Null-Knoten

6. **Baumstruktur**
   - Wurzel ist ein beliebiger JSON-Wert (Objekt, Array, primitiv)
   - Verschachtelung durch rekursive Referenzen:
     - Objekt-Members verweisen auf weitere Knoten
     - Array-Elements verweisen auf weitere Knoten
   - Die Struktur spiegelt exakt die Hierarchie des JSON-Dokuments wider

