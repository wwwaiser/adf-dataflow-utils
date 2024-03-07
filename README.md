# README for Azure Data Factory Dataflow Expression Function Collection

Welcome to this repository where we collect and share useful functions for Azure Data Factory (ADF) Dataflow expressions. This collection aims to assist developers and data engineers in creating more efficient and streamlined dataflows in Azure Data Factory by providing reusable expression functions that can be easily integrated into their projects.

## Function List

Currently, we have the following function available:

### 1. `capitalizePhrase`

This function capitalizes the initial letter of each word in a given phrase, allowing for exceptions and customizable delimiters.

#### Parameters

- `i1 (String)`: The phrase to capitalize.
- `i2 (String)`: Regular expression pattern to identify words to ignore and not capitalize. Default is to ignore "LLC" and "INC".
- `i3 (String)`: Delimiter used to split the phrase. Default is a space character.

#### Returns

- `String`: The capitalized phrase.

#### Expression

```dataflow
reduce(
    map(
        split(i1, coalesce(i3, ' ')),
        iif(
            regexMatch(#item, coalesce(i2, '^(LLC|INC)$')),
            #item,
            concat(left(upper(#item), 1), right(lower(#item), length(#item) - 1))
        )
    ),
    '',
    #acc + ' ' + #item,
    #result
)
```
