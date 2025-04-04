---
title: "Levenshtein Distance"
description: "An introduction to Levenshtein distance with an implementation in Go"
date: 2022-12-26T00:04:51+10:30
tags: ["programming", "code", "golang"]
featured_image: "/images/golang.png"
---
Levenshtein distance is a measure of the similarity between two strings, calculated as the minimum number of single-character edits (insertions, deletions, or substitutions) required to transform one string into the other. Here is an example of how you can calculate the Levenshtein distance between two strings in Go:

```Go
package main

import (
	"fmt"
	"math"
)

func main() {
	s1 := "kitten"
	s2 := "sitting"

	distance := levenshteinDistance(s1, s2)
	fmt.Printf("The Levenshtein distance between %s and %s is %d\n", s1, s2, distance)
}

func levenshteinDistance(s1, s2 string) int {
	// Convert strings to rune slices for Unicode support
	r1 := []rune(s1)
	r2 := []rune(s2)

	// Initialize a two-dimensional matrix with all values set to zero
	matrix := make([][]int, len(r1)+1)
	for i := range matrix {
		matrix[i] = make([]int, len(r2)+1)
	}

	// Set the initial values of the first row and column of the matrix
	for i := 1; i <= len(r1); i++ {
		matrix[i][0] = i
	}
	for j := 1; j <= len(r2); j++ {
		matrix[0][j] = j
	}

	// Calculate the Levenshtein distance using the dynamic programming algorithm
	for i := 1; i <= len(r1); i++ {
		for j := 1; j <= len(r2); j++ {
			cost := 0
			if r1[i-1] != r2[j-1] {
				cost = 1
			}
			matrix[i][j] = min(matrix[i-1][j]+1, matrix[i][j-1]+1, matrix[i-1][j-1]+cost)
		}
	}

	// Return the final value in the bottom-right corner of the matrix
	return matrix[len(r1)][len(r2)]
}

func min(a, b, c int) int {
	// Return the minimum of three integers
	return int(math.Min(float64(a), math.Min(float64(b), float64(c))))
}
```
This code defines a function `levenshteinDistance` that takes in two strings as input and returns an integer representing the Levenshtein distance between them. The function first converts the input strings to slices of runes (Unicode characters) to support Unicode characters. It then initializes a two-dimensional matrix with all values set to zero, and sets the initial values of the first row and column of the matrix to the lengths of the input strings.

The function then uses a dynamic programming algorithm to calculate the Levenshtein distance. It iterates over the elements of the input strings and compares them, adding a cost of 1 if they are different and 0 if they are the same. It then calculates the minimum of the three values in the matrix: the value above, the value to the left, and the value to the upper-left (diagonal) of the current position. This value is then added to the current position in the matrix.

Finally, the function returns the final value in the bottom-right corner of the matrix, which represents the Levenshtein distance between the input strings.

Here is an example of how you can use this function:
```go
s1 := "kitten"
s2 := "sitting"
distance := levenshteinDistance(s1, s2)
fmt.Printf("The Levenshtein distance between %s and %s is %d\n", s1, s2, distance)
```

This will output the following:
```bash
The Levenshtein distance between kitten and sitting is 3
```


