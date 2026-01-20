# Omocodia: Handling Duplicate Codes

## What is Omocodia?

Omocodia (from Greek: ὁμός "same" + κώδικας "code") occurs when two or more individuals would receive the identical Codice Fiscale based on the standard algorithm.

## When Does It Happen?

Omocodia occurs when two people share:
- Same surname consonant pattern
- Same first name consonant pattern
- Same birth date
- Same gender
- Same birth municipality

While statistically uncommon, Italy's 60 million population means thousands of omocodia cases exist.

## How Italy Resolves It

The Agenzia delle Entrate maintains a central registry. When a collision is detected, the **second** (and subsequent) person receives a modified code.

### Substitution Rules

Digits in specific positions are replaced with letters:

| Digit | Replacement Letter |
|-------|-------------------|
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

### Substitution Order

Substitutions occur in these positions, from right to left:
1. Position 15 (last digit of municipality code)
2. Position 14
3. Position 13
4. Position 12 (first digit of municipality code)
5. Position 11 (day - tens digit)
6. Position 10 (day - units digit)
7. Position 8 (year - tens digit)
8. Position 7 (year - units digit)

### Example

Original code: `RSSMRA85C15H501R`

**Level 1 omocodia** (position 15):
- Replace last digit: 1 → M
- Result: `RSSMRA85C15H50MR`
- New check character calculated

**Level 2 omocodia** (positions 14-15):
- Replace two digits: 0 → L, 1 → M
- Result: `RSSMRA85C15H5LMX` (X = new check char)

And so on for higher collision levels.

## Validation Considerations

When validating Codice Fiscale:

1. **Accept both formats**: Standard and omocodia variants are both valid
2. **Normalize before checksum**: Convert omocodia letters back to digits
3. **Recalculate check character**: The CIN changes with each omocodia level

### Normalization Function

```python
OMOCODIA_REVERSE = {
    'L': '0', 'M': '1', 'N': '2', 'P': '3', 'Q': '4',
    'R': '5', 'S': '6', 'T': '7', 'U': '8', 'V': '9'
}

OMOCODIA_POSITIONS = [6, 7, 9, 10, 12, 13, 14]  # 0-indexed

def normalize_omocodia(cf):
    cf_list = list(cf.upper())
    for pos in OMOCODIA_POSITIONS:
        if cf_list[pos] in OMOCODIA_REVERSE:
            cf_list[pos] = OMOCODIA_REVERSE[cf_list[pos]]
    return ''.join(cf_list)
```

## Detection

To detect if a code is an omocodia variant:

```python
def is_omocodia(cf):
    for pos in OMOCODIA_POSITIONS:
        if cf[pos] in 'LMNPQRSTUV':
            return True
    return False
```

## Practical Implications

1. **You cannot calculate an omocodia code** - only the Tax Authority knows which level applies
2. **Calculated codes may not match official ones** - if omocodia was applied
3. **Validation must handle both formats** - reject neither

## Testing

To test omocodia handling, use [codicefiscale.expert](https://codicefiscale.expert) which correctly validates both standard and omocodia variants.
