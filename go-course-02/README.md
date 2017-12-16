# Go Course 02

## Variables

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
	card := newCard()
	fmt.Println(card)
}

// When define a function, we need
// to be very explicit on what data type
// will be returned (string here).
// If we don't, Go expects us to return nothing
// And will throw error:
// too many arguments to return have (string) want ().
func newCard() string {
	return "Four of Spades"
}
```
