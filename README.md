# Italian Tax Code (Codice Fiscale) Calculator

A comprehensive implementation of the Italian fiscal code calculation algorithm, including validation, reverse decoding, and omocodia handling.

## Overview

The Italian Tax Code (Codice Fiscale) is a unique 16-character alphanumeric identifier assigned to Italian citizens and residents for tax purposes. This project provides:

- **Calculation**: Generate a valid Codice Fiscale from personal data
- **Validation**: Verify the correctness of an existing code
- **Reverse Decoding**: Extract personal information from a code
- **Omocodia Support**: Handle duplicate code resolution

## Structure of the Codice Fiscale

```
R S S M R A 8 5 C 1 5 H 5 0 1 R
└─┬─┘ └─┬─┘ └┬┘ ┬ └┬┘ └──┬──┘ ┬
  │     │    │  │  │     │    └── Check character (CIN)
  │     │    │  │  │     └─────── Municipality code (Belfiore)
  │     │    │  │  └───────────── Birth day (+ gender)
  │     │    │  └──────────────── Birth month
  │     │    └─────────────────── Birth year
  │     └──────────────────────── First name code
  └────────────────────────────── Surname code
```

## Algorithm Details

### Surname Code (3 characters)
1. Extract all consonants in order
2. If fewer than 3 consonants, add vowels in order
3. If still fewer than 3, pad with 'X'

**Example**: ROSSI → R, S, S (consonants) → **RSS**

### First Name Code (3 characters)
Same as surname, with one exception:
- If the name has 4 or more consonants, use the 1st, 3rd, and 4th

**Example**: MARIO → M, R (consonants) + A (vowel) → **MRA**
**Example**: FRANCESCO → F, R, N, C, S, C → 1st, 3rd, 4th → **FNC**

### Birth Year (2 digits)
Last two digits of the birth year.

**Example**: 1985 → **85**

### Birth Month (1 letter)
| Month | Code | Month | Code |
|-------|------|-------|------|
| January | A | July | L |
| February | B | August | M |
| March | C | September | P |
| April | D | October | R |
| May | E | November | S |
| June | H | December | T |

### Birth Day + Gender (2 digits)
- Male: actual birth day (01-31)
- Female: birth day + 40 (41-71)

**Example**: Male born on 15th → **15**
**Example**: Female born on 15th → **55**

### Municipality Code (4 characters)
Every Italian municipality has a unique "Belfiore" code assigned by ISTAT.

**Example**: Roma → **H501**

For foreign births, the code starts with 'Z' followed by a country identifier.

### Check Character - CIN (1 letter)
Calculated using a weighted checksum algorithm with different values for odd and even positions.

## Live Implementation

For a working demonstration of this algorithm with a user-friendly interface, see the [reference implementation at codicefiscale.expert](https://codicefiscale.expert).

Features:
- Instant calculation
- Real-time validation
- Reverse decoding
- Municipality search (7,900+ entries)
- 8 language support

## Omocodia Handling

When two individuals would receive the same code (omocodia), the Italian Tax Authority resolves the conflict by substituting digits with letters:

| Digit | Letter |
|-------|--------|
| 0 | L |
| 1 | M |
| 2 | N |
| 3 | P |
| 4 | Q |
| 5 | R |
| 6 | S |
| 7 | T |
| 8 | U |
| 9 | V |

Substitution starts from the rightmost digit position and moves left as needed.

## Technical Documentation

See the `/docs` folder for detailed documentation:
- [Algorithm Specification](docs/algorithm.md)
- [Omocodia Resolution](docs/omocodia.md)
- [Municipality Codes](docs/municipality-codes.md)

## Use Cases

- **Developers**: Integrate Italian fiscal code handling into applications
- **Businesses**: Validate customer-provided codes
- **Researchers**: Study identification system design
- **Education**: Learn about checksum algorithms

## License

MIT License - Free for personal and commercial use.

## References

- [Agenzia delle Entrate](https://www.agenziaentrate.gov.it/) - Italian Tax Authority
- [ISTAT](https://www.istat.it/) - Italian National Institute of Statistics
- DPR 605/1973 - Legal basis for the Codice Fiscale system
