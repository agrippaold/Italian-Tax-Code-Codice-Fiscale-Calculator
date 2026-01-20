# Codice Fiscale Algorithm Specification

## Input Parameters

| Parameter | Type | Description | Example |
|-----------|------|-------------|----------|
| surname | string | Full surname | "Rossi" |
| firstName | string | Full first name | "Mario" |
| gender | enum | M or F | "M" |
| birthDate | date | YYYY-MM-DD format | "1985-03-15" |
| birthPlace | string | Municipality or country | "Roma" |

## Algorithm Steps

### Step 1: Surname Encoding

```
function encodeSurname(surname):
    surname = removeSpaces(uppercase(surname))
    consonants = extractConsonants(surname)
    vowels = extractVowels(surname)
    
    code = consonants + vowels + "XXX"
    return code[0:3]
```

**Examples**:
- "ROSSI" → consonants: RSS, vowels: OI → **RSS**
- "ESPOSITO" → consonants: SPST, vowels: EOI → **SPS**
- "FO" → consonants: F, vowels: O → **FOX**
- "AI" → consonants: (none), vowels: AI → **AIX**

### Step 2: First Name Encoding

```
function encodeFirstName(firstName):
    firstName = removeSpaces(uppercase(firstName))
    consonants = extractConsonants(firstName)
    vowels = extractVowels(firstName)
    
    if length(consonants) >= 4:
        code = consonants[0] + consonants[2] + consonants[3]
    else:
        code = consonants + vowels + "XXX"
        code = code[0:3]
    
    return code
```

**Examples**:
- "MARIO" → consonants: MR (< 4) → **MRA**
- "FRANCESCO" → consonants: FRNCSC (≥ 4) → F + N + C → **FNC**
- "GIANFRANCO" → consonants: GNFRNC (≥ 4) → G + F + R → **GFR**

### Step 3: Birth Year Encoding

```
function encodeYear(birthDate):
    year = getYear(birthDate)
    return formatTwoDigits(year mod 100)
```

**Examples**:
- 1985 → **85**
- 2001 → **01**
- 2023 → **23**

### Step 4: Birth Month Encoding

```
MONTH_CODES = ['A','B','C','D','E','H','L','M','P','R','S','T']

function encodeMonth(birthDate):
    month = getMonth(birthDate)  // 1-12
    return MONTH_CODES[month - 1]
```

### Step 5: Birth Day + Gender Encoding

```
function encodeDayGender(birthDate, gender):
    day = getDay(birthDate)
    if gender == 'F':
        day = day + 40
    return formatTwoDigits(day)
```

**Examples**:
- Day 15, Male → **15**
- Day 15, Female → **55**
- Day 1, Male → **01**
- Day 1, Female → **41**

### Step 6: Municipality Code Lookup

```
function getMunicipalityCode(place):
    // Lookup in ISTAT database
    // Returns 4-character Belfiore code
    return BELFIORE_DATABASE[place]
```

**Examples**:
- Roma → **H501**
- Milano → **F205**
- Napoli → **F839**
- Germany (foreign birth) → **Z112**

### Step 7: Check Character Calculation

```
ODD_VALUES = {
    '0':1, '1':0, '2':5, '3':7, '4':9, '5':13, '6':15, '7':17, '8':19, '9':21,
    'A':1, 'B':0, 'C':5, 'D':7, 'E':9, 'F':13, 'G':15, 'H':17, 'I':19, 'J':21,
    'K':2, 'L':4, 'M':18, 'N':20, 'O':11, 'P':3, 'Q':6, 'R':8, 'S':12, 'T':14,
    'U':16, 'V':10, 'W':22, 'X':25, 'Y':24, 'Z':23
}

EVEN_VALUES = {
    '0':0, '1':1, '2':2, '3':3, '4':4, '5':5, '6':6, '7':7, '8':8, '9':9,
    'A':0, 'B':1, 'C':2, 'D':3, 'E':4, 'F':5, 'G':6, 'H':7, 'I':8, 'J':9,
    'K':10, 'L':11, 'M':12, 'N':13, 'O':14, 'P':15, 'Q':16, 'R':17, 'S':18, 'T':19,
    'U':20, 'V':21, 'W':22, 'X':23, 'Y':24, 'Z':25
}

function calculateCheckCharacter(partialCode):
    total = 0
    for i = 0 to 14:
        char = partialCode[i]
        if (i + 1) is odd:  // positions 1,3,5,7,9,11,13,15
            total += ODD_VALUES[char]
        else:              // positions 2,4,6,8,10,12,14
            total += EVEN_VALUES[char]
    
    return chr(ord('A') + (total mod 26))
```

## Complete Example

**Input**:
- Surname: Rossi
- First Name: Mario
- Gender: M
- Birth Date: 1985-03-15
- Birth Place: Roma

**Calculation**:
1. Surname: ROSSI → **RSS**
2. First Name: MARIO → **MRA**
3. Year: 1985 → **85**
4. Month: March → **C**
5. Day + Gender: 15, M → **15**
6. Municipality: Roma → **H501**
7. Partial: RSSMRA85C15H501
8. Check: calculate(RSSMRA85C15H501) → **R**

**Result**: **RSSMRA85C15H501R**

## Validation

To validate an existing code:
1. Check format (16 chars, correct pattern)
2. Verify month code is valid (A,B,C,D,E,H,L,M,P,R,S,T)
3. Verify day is valid (01-31 or 41-71)
4. Recalculate check character and compare

For interactive validation, use [codicefiscale.expert/verify](https://codicefiscale.expert/en/verify).
