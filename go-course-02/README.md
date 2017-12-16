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
	// not a new variable, it's just an assigned
	// a new value.
	// secondCard := "Three of Spades"
}
```
