# Italian Municipality Codes (Codici Belfiore)

## Overview

Every Italian municipality (comune) has a unique 4-character code assigned by ISTAT (Italian National Institute of Statistics). This code, known as the "Belfiore code," is used in positions 12-15 of the Codice Fiscale.

## Code Format

```
X 0 0 0
│ └─┬─┘
│   └── 3-digit numeric identifier
└────── Letter (A-M for Italy, Z for foreign countries)
```

### Italian Municipalities
- First character: A-M (varies by province)
- Characters 2-4: Numeric identifier

### Foreign Countries
- First character: Z
- Characters 2-4: Country identifier

## Examples

### Major Italian Cities

| City | Province | Belfiore Code |
|------|----------|---------------|
| Roma | RM | H501 |
| Milano | MI | F205 |
| Napoli | NA | F839 |
| Torino | TO | L219 |
| Palermo | PA | G273 |
| Genova | GE | D969 |
| Bologna | BO | A944 |
| Firenze | FI | D612 |
| Bari | BA | A662 |
| Venezia | VE | L736 |

### Foreign Countries (Examples)

| Country | Code |
|---------|------|
| Germany | Z112 |
| France | Z110 |
| United Kingdom | Z114 |
| United States | Z404 |
| China | Z210 |
| Brazil | Z602 |
| Argentina | Z600 |

## Historical Codes

Municipalities that have been:
- **Merged**: Old codes remain valid for births before the merge
- **Abolished**: Codes preserved in historical records
- **Renamed**: Code typically unchanged

## Database Size

- **Active municipalities**: ~7,900
- **Historical municipalities**: ~2,000+
- **Foreign countries**: ~250

## Lookup Tools

For municipality code lookup:
- [ISTAT Official Database](https://www.istat.it/)
- [codicefiscale.expert/municipalities](https://codicefiscale.expert) - Interactive search

## Usage Notes

1. **Always use official codes**: Don't guess or construct codes
2. **Handle historical data**: Old codes must be accepted for validation
3. **Foreign births**: Use Z-codes, not Italian municipality codes
4. **Case insensitive**: H501 = h501
