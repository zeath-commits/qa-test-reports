# Python import repair: a grouped amount stopped the whole batch

A personal demonstration using four fictional records.

## The failure

The import's row-processing step used `float(value)` for each amount. It stopped at the first grouped value, `"1,250.50"`, with a `ValueError`. None of the later rows were checked.

## The fix

Validate the number format before removing thousands separators, then parse the amount with Python's `Decimal`. Record rejected rows by input position and continue processing the batch. Blank or malformed amounts stay out of the total.

| Row | Input amount | Result |
| --- | --- | --- |
| 1 | `"1,250.50"` | Accepted as `1250.50` |
| 2 | `9.5` | Accepted as `9.5` |
| 3 | Empty string | Rejected |
| 4 | `"oops"` | Rejected |

**Result:** 2 accepted, 2 rejected, total `1260.00`.

## Checks

Eight unit tests passed. They cover grouped values, integers, signs and whitespace, malformed separators, blanks, non-finite values, booleans, and accounting for every input row.

This sample starts with rows already parsed into Python dictionaries. The format uses a period for decimals and optional comma grouping for thousands.

[Fixed-price Python or CSV/JSON import repair on LaborX](https://laborx.com/gigs/fix-one-python-script-or-csv-json-import-bug-124300)
