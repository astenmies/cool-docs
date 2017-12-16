# Go CheatSheet
- [Variables](#variables)
- [For Loops](#for-loops)

## <a name="variables"/>Variables

```go
package main

import "fmt"

func main() {
	// Create a new variable firstCard
	// of type string.
	var firstCard string = "Ace of Clubs"
	fmt.Println("firstCard:", firstCard)

	// Same as firstcard, but let Go understand
	// that this is of type string.
	secondCard := "Ace of Spades"
	fmt.Println("secondCard:", secondCard)

	// Because Go is a static type language
	// secondCard can never be assigned another
	// type than string afterwards.
	secondCard = "Two of Spades"
	fmt.Println("secondCard:", secondCard)

	// This would throw an error, because it's
	// not a new variable, it's just being assigned
	// a new value.
	// secondCard := "Three of Spades"
}
```
## Functions
```go
func main() {
	oneCard := oneNewCard()
	fmt.Println(oneCard)
}

// When define a function, we need to be very explicit
// on what data type will be returned (string here).
// If we don't, Go expects us to return nothing
// And will throw error:
// too many arguments to return have (string) want ().
func oneNewCard() string {
	return "Four of Spades"
}
```

## Arrays and Slices
Arrays, and slices can only contain one type of data. We cannot mix strings and integers for instance.

### Arrays
An array is made of a fixed length list of data.
```go
func main() {
	// Create a new variable which is an
	// array of n values of type string.
	var arrayCardsExplicit [3]string
	arrayCardsExplicit[0] = "Ace of Clubs"
	arrayCardsExplicit[1] = "Two of Clubs"
	arrayCardsExplicit[2] = "Three of Clubs"
	fmt.Println("arrayCardsExplicit:", arrayCardsExplicit)

	// A more implicit way of defining an array of types string.
	// It can also contain a function if it returns a value of same type.
	arrayCardsImplicit := [3]string{"Four of Clubs", "Five of Clubs", oneNewCard()}
	fmt.Println("arrayCardsImplicit:", arrayCardsImplicit)

	// If we assign more than the fixed number of values
	// to the array, it will throw an error:
	// array index n out of bounds [0:n]
	// arrayCardsImplicit := [2]string{"Four of Clubs", "Five of Clubs", "Exceeding Card"}
}

func oneNewCard() string {
	return "Four of Spades"
}
```

### Slices
A slice is an array that can grow or shrink. [More Doc](https://blog.golang.org/go-slices-usage-and-internals)

```go
func main() {
	// Create a new variable which is an
	// array of type string.
	arrayCardsImplicit := [3]string{"Four of Clubs", "Five of Clubs", "Six of Clubs"}

	// A slice is an array that can grow or shrink
	sliceCardsFromArray := arrayCardsImplicit[0:2]
	fmt.Println("sliceCardsFromArray:", sliceCardsFromArray)

	// A more implicit way of defining each value of the
	// slice of type string. It can also contain a function
	// if that function returns a value of the same type.
	sliceCardsImplicit := []string{"Seven of Clubs", "Eight of Clubs", oneNewCard()}
	fmt.Println("sliceCardsImplicit:", sliceCardsImplicit)

	// We can append data of type string to a slice.
	sliceCardsImplicit = append(sliceCardsImplicit, "Five of Spades")
	fmt.Println("sliceCardsImplicit:", sliceCardsImplicit)
}

func oneNewCard() string {
	return "Four of Spades"
}
```

### <a name="for-loops"/>For loops
```go
func main() {
	sliceCards := []string{"Seven of Clubs", "Eight of Clubs"}

	// A simple for loop.
	// Range instructs to take slice Cards and loop over it.
	// We use := because in for loops, every single time that
	// we step through this list of cards we really throw away
	// the two variables (i and card). So we re-declare them each time.
	for i, card := range sliceCards {
		fmt.Println("sliceCard:", i, card)
	}

	// If we don't care about the index, we use underscore
	for _, card := range sliceCards {
		fmt.Println("sliceCard:", card)
	}
}
```
