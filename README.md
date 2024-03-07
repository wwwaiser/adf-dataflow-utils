# Azure Data Factory Dataflow Expression Function Collection

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

### Example of usage

Let's demonstrate how to use the `capitalizePhrase` function in a dataflow scenario:

Suppose you have a dataset with a column `company_names` that contains company names in lowercase, and you want to capitalize each name while ignoring certain acronyms like 'LLC' and 'INC'. Here's how you could apply the `capitalizePhrase` function:

1. Add a Derived Column transformation in your dataflow.
2. Use the following expression to capitalize the company names:

```dataflow
capitalizePhrase('GOOGLE LLC', '^(LLC|INC)$', ' ') -> Google LLC
```

### Usage

To use a function from this collection in your ADF dataflow, copy the expression provided for the function and integrate it into your dataflow script. Ensure that you replace the parameter placeholders (i1, i2, i3) with your actual data or variables.

You can copy and paste these functions and replace parameters with your own values, or you can add them to your dataflow library. For more information on how to incorporate these functions into your library, check the documentation [here](https://learn.microsoft.com/en-us/azure/data-factory/concepts-data-flow-udf)

### Contribution

Contributions are welcome! If you have a useful ADF Dataflow expression function that you'd like to share, please feel free to create a pull request or open an issue to discuss your ideas.

### License

This repository is available under [insert your chosen license here], which allows for flexible use and distribution of the provided functions.

Enjoy enhancing your Azure Data Factory dataflows with this collection of expressions!
