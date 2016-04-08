# A Go package for calculating the Levenshtein distance between two strings

This package implements distance and similarity metrics for strings, based on the Levenshtein measure, in [Go](http://golang.org).

## Maturity

[![Build Status](https://travis-ci.org/agext/lev.svg?branch=master)](https://travis-ci.org/agext/lev)

v1.1 Stable: Guaranteed no breaking changes to the API in future v1.x releases. No known bugs or performance issues. Probably safe to use in production, though provided on "AS IS" basis.

## Overview

[![GoDoc](https://godoc.org/github.com/agext/lev?status.png)](https://godoc.org/github.com/agext/lev)

The Levenshtein `Distance` between two strings is the minimum total cost of edits that would convert the first string into the second. The allowed edit operations are insertions, deletions, and substitutions, all at character (one UTF-8 code point) level. Each operation has a default cost of 1, but each can be assigned its own cost equal to or greater than 0.

A `Distance` of 0 means the two strings are identical, and the higher the value the more different the strings. Since in practice we are interested in finding if the two strings are "close enough", it often does not make sense to continue the calculation once the result is mathematically guaranteed to exceed a desired threshold. Providing this value to the `Distance` function allows it to take a shortcut and return a lower bound instead of an exact cost when the threshold is exceeded.

The `Similarity` function calculates the distance, then converts it into a normalized metric within the range 0..1, with 1 meaning the strings are identical, and 0 that they have nothing in common. A minimum similarity threshold can be provided to speed up the calculation of the metric for strings that are far too dissimilar for the purpose at hand. All values under this threshold are rounded down to 0.

The `Match` function provides a similarity metric, with the same range and meaning as `Similarity`, but with a bonus for string pairs that share a common prefix and have a similarity above a "bonus threshold". It uses the same method as proposed by Winkler for the Jaro distance, and the reasoning behind it is that these string pairs are very likely spelling variations or errors, and they are more closely linked than the edit distance alone would suggest.

The underlying `Calculate` function is also exported, to allow the building of other derivative metrics, if needed.

## Installation

```
go get github.com/agext/lev
```

## License

Package lev is released under the Apache 2.0 license. See the [LICENSE](LICENSE) file for details.
