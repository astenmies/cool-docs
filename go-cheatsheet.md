# <a name="top"/>Go Airlines CheatSheet
This cheatsheet illustrates the case of an airline company to explain various concepts that we may encounter while programming.

## First coding mission
- At Go Airlines we offer three classes of flight; First, Business and Economy. 
- Since last year, we cover five destinations;  Florence, Lisbon, Oslo, Perth and Tokyo. 
- On our web app, we want to display five featured flights at the time in a specific section.
- The featured flights need to be randomly picked.
- A featured flight cannot be displayed twice before each possibility has been displayed (so we will need to keep track of remaining flights when picking featured flights).

## Similar cases
Flights | Classes | Destinations
--- | --- | ---
Jackets | Classes | Colors
Accomodations | Types | Locations
Cars | Brands | Colors
Whatever | Whatever | Whatever

## Table of Contents

- [Variables](#variables) - Initiate, define their type and value.
- [Functions](#functions) - Specify what they return.
- [Arrays & Slices](#arrays-slices)
	- [Arrays](#arrays) - Fixed list of n values.
	- [Slices](#slices) - Arrays that can grow or shrink.
- [For Loops](#for-loops)
	- [Simple Iteration](#simple-iteration) - Iterate over a range.
	- [Combinations](#combinations) - List all possible combinations between two slices.
- [Custom Types](#custom-types) - Create a copy ("instance") of a certain type.
- [Methods / Receiver Functions](#methods) - Extend functionnality for a custom type.
- [Function Returning Multiple Values](#function-returning-multiple-values) - Pick n in a slice, return (1) the pick and (2) the remaining values.
- [Byte Slices / Write File](#byte-slices-write-file) - Write a file with a slice of bytes (ASCII characters).

## <a name="variables"/>Variables

```go
package main

import "fmt"

func main() {
	// Create a new variable firstFlight
	// of type string.
	var firstFlight string = "First Class to Perth"
	fmt.Println("firstFlight:", firstFlight)

	// Same as firstFlight, but let Go understand
	// that this is of type string.
	secondFlight := "Economy Class to Lisbon"
	fmt.Println("secondFlight:", secondFlight)

	// Because Go is a static type language
	// secondFlight can never be assigned another
	// type than string afterwards.
	secondFlight = "First Class to Tokyo"
	fmt.Println("secondFlight:", secondFlight)

	// This would throw an error, because it's
	// not a new variable, it's just being assigned
	// a new value.
	// secondFlight := "Business Class to Oslo"
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

---

## <a name="functions"/>Functions
```go
func main() {
	oneFlight := oneNewFlight()
	fmt.Println(oneFlight)
}

// When define a function, we need to be very explicit
// on what data type will be returned (string here).
// If we don't, Go expects us to return nothing
// And will throw error:
// too many arguments to return have (string) want ().
func oneNewFlight() string {
	return "Business Class to Tokyo"
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

---

## <a name="arrays-slices"/>Arrays and Slices
Arrays, and slices can only contain one type of data. We cannot mix strings and integers for instance.

### <a name="arrays"/>Arrays
An array is made of a fixed length list of data.

- `arrayName[startIndexIncluding:upToNotIncluding]`
- `flights[0:2]` or `flights[:2]` will return flight 0, flight 1
- `flights[2:]`will return from flight 2 included, to the end of the array

```go
func main() {
	// Create a new variable which is an
	// array of n values of type string.
	var arrayFlightsExplicit [3]string
	arrayFlightsExplicit[0] = "First Class to Lisbon"
	arrayFlightsExplicit[1] = "Economy Class to Tokyo"
	arrayFlightsExplicit[2] = "Economy Class to Florence"
	fmt.Println("arrayFlightsExplicit:", arrayFlightsExplicit)

	// A more implicit way of defining an array of types string.
	// It can also contain a function if it returns a value of same type.
	arrayFlightsImplicit := [3]string{"First Class to Lisbon", "Economy Class to Tokyo", oneNewFlight()}
	fmt.Println("arrayFlightsImplicit:", arrayFlightsImplicit)

	// If we assign more than the fixed number of values
	// to the array, it will throw an error:
	// array index n out of bounds [startIndexIncluding:upToNotIncluding] or [0:n]
	// arrayFlightsImplicit := [2]string{"First Class to Lisbon", "Economy Class to Tokyo", "Exceeding Flight"}
}

func oneNewFlight() string {
	return "Economy Class to Florence"
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

### <a name="slices"/>Slices
A slice is an array that can grow or shrink. [More Doc](https://blog.golang.org/go-slices-usage-and-internals)

```go
func main() {
	// Create a new variable which is an
	// array of type string.
	arrayFlights := [3]string{"First Class to Florence", "First Class to Lisbon", "Economy Class to Perth"}

	// A slice is an array that can grow or shrink
	sliceFlightsFromArray := arrayFlights[0:2]
	fmt.Println("sliceFlightsFromArray:", sliceFlightsFromArray)

	// A more implicit way of defining each value of the
	// slice of type string. It can also contain a function
	// if that function returns a value of the same type.
	sliceFlights := []string{"First Class to Florence", "First Class to Lisbon", oneNewFlight()}
	fmt.Println("sliceFlights:", sliceFlights)

	// We can append data of type string to a slice.
	sliceFlights = append(sliceFlights, "Business Class to Oslo")
	fmt.Println("sliceFlights:", sliceFlights)
}

func oneNewFlight() string {
	return "Economy Class to Perth"
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

---

## <a name="for-loops"/>For Loops
### <a name="simple-iteration"/>Simple Iteration
```go
func main() {
	sliceFlights := []string{"Economy Class to Perth", "Economy Class to Tokyo"}

	// A simple for loop.
	// Range instructs to take slice Flights and loop over it.
	// We use := because in for loops, every single time that
	// we step through this list of flights we really throw away
	// the two variables (i and flight). So we re-declare them each time.
	for i, flight := range sliceFlights {
		fmt.Println("sliceFlight:", i, flight)
	}

	// If we don't care about the index, we use underscore
	for _, flight := range sliceFlights {
		fmt.Println("sliceFlight:", flight)
	}
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

### <a name="combinations"/>Combinations
A basic loop inside a loop, notice how similar to vanilla this looks. All the basic programming stuff that you already know, applies to Go in a similar fashion.
```go
func main() {
	flights := []string{}

	flightClasses := []string{"First Class", "Business Class", "Economy Class"}
	flightDestinations := []string{"Florence", "Lisbon", "Oslo", "Perth", "Tokyo"}

	for _, class := range flightClasses {
		for _, destination := range flightDestinations {
			flights = append(flights, class+" to "+destination)
			fmt.Println("flight:", class+" to "+destination)
		}
	}
	fmt.Println("flights:", flights)
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

---

## <a name="custom-types"/>Custom Types

A custom type is a copy ("instance") of an existing type.

```go
// We specify that any variable of type featuredFlights is an "extension" of type slice of string.
type featuredFlights []string

func main() {
	sliceFlights := featuredFlights{"First Class to Tokyo", "Business Class to Oslo"}
	fmt.Println("sliceFlights:", sliceFlights)
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

---

## <a name="methods"/>Methods / Receiver Functions

A method is a function with a special receiver argument. By convention, we name the receiver the first letter of its type (here d for featuredFlights). We can think of a receiver as "this", or "self" in OO, but in Go we never use those words!

```go
// In Object Oriented programming, we use classes to extend
// functionnality, and then initiate new instances of those classes.
// So in OO we use methods to create functions that belong to the instance.

// In Go, we achieve this by "extending" a base type (like string or integer ...).
type featuredFlights []string

// By making an extra type, it gives us the ability to create a method (a function with a receiver).
// That function will only work with that specific type.
// So that method belongs to the extended type ("instance" in OO)
// Variable d is the actual copy ("instance" in OO) of the featuredFlights we're working on.
// Now any variable of type featuredFlights can call this function printIt() on itself.
func (f featuredFlights) printIt() {
	for i, flight := range f {
		fmt.Println("sliceFlight:", i, flight)
	}
}

func main() {
	// sliceFlights := []string{"Economy Class to Lisbon", "Business Class to Perth"}
	sliceFlights := featuredFlights{"Economy Class to Lisbon", "Business Class to Perth"}

	// Because we extended type featuredFlights with a printIt() functionnality,
	// we can now use it on sliceFlights variable.
	sliceFlights.printIt()
}
```
<div align="right">▲<a href="#top">Back to Top</a></div>

## <a name="function-returning-multiple-values"/>Function returning multiple values
The goal here is to split our slice of flights in two. On one side, the determined number of flights we need to pick (four in this case), on the other side we return the remaining flights to keep track of what we have already displayed.

```go
func main() {
	// First get all possible flights
	flights := newFtFlights()

	// Then pick four of them
	pickedFlights, remainingFlights := pickFlights(flights, 4)

	pickedFlights.printIt("picked")
	remainingFlights.printIt("remaining")
}

// Create a slice of all possible flights.
func newFtFlights() ftFlights {
	flights := []string{}

	flightClasses := []string{"First Class", "Business Class", "Economy Class"}
	flightDestinations := []string{"Florence", "Lisbon", "Oslo", "Perth", "Tokyo"}

	// Combine all possibilities of both slices.
	for _, class := range flightClasses {
		for _, destination := range flightDestinations {
			flights = append(flights, class+" to "+destination)
		}
	}
	return flights
}

// We are going to split our ftFlights.
// So our function receives one ftFlights type variable
// and returns two different ftFlights type variables.
// ftNumber is the number of featured flights.
func pickFlights(f ftFlights, ftNumber int) (ftFlights, ftFlights) {

	// We can return multiple values simply with a comma.
	// Here we return the ftNumber of flights, and the remaining flights.
	return f[:ftNumber], f[ftNumber:]
}

// Let's print it out.
func (f ftFlights) printIt(log string) {
	for i, flight := range f {
		fmt.Println(log, i, flight)
	}
}
```

<div align="right">▲<a href="#top">Back to Top</a></div>

## <a name="byte-slices-write-file"/>Byte Slices / Write File
See http://asciitable.com to find out decimal values for each letter of a string. This is required because go package ioutil requires a byte slice to **write a file**. The main challenge here is to convert our ***ftFlights*** type (which is the extension of a string slice), to a byte slice.

For doing this we will use what we call a **type conversion**. `[]byte("Hello")` converts hello to a byte slice.

```go
type ftFlights []string

func main() {
	// First get all possible flights
	flights := newFtFlights()

	// Then pick four of them
	pickedFlights, remainingFlights := pickFlights(flights, 4)

	pickedFlights.printIt("picked")
	remainingFlights.printIt("remaining")

	flights.saveToFile("all_flights")
}
```

- f is the slice of bytes that we require in ioutil.
- The only thing this function returns is error, for the rest it's just a file being written so no other return required.
- We convert the string to a byte slice.
- ioutil.WriteFile returns an error, so we just return ioutil.WriteFile.
```go
func (f ftFlights) saveToFile(fileName string) error {
	return ioutil.WriteFile(fileName, []byte(f.toString()), 0666)
}
```

- Let's first convert ftFlights to a string.
- Example, we will turn ["red", "green", "blue"] slice of string
- To "red,green,blue" string. Using commas in the string seems appropriate.
- Go string package can do this! Join concatenates a slice of string to create a single string. We just need to define the separator we want (comma here).
```go
func (f ftFlights) toString() string {
	
	return strings.Join([]string(f), ",")
}
```
```go
////////////////////////
// PREVIOUS SECTIONS //
//////////////////////

// Create a slice of all possible flights.
func newFtFlights() ftFlights {
	flights := []string{}

	flightClasses := []string{"First Class", "Business Class", "Economy Class"}
	flightDestinations := []string{"Florence", "Lisbon", "Oslo", "Perth", "Tokyo"}

	// Combine all possibilities of both slices.
	for _, class := range flightClasses {
		for _, destination := range flightDestinations {
			flights = append(flights, class+" to "+destination)
		}
	}
	return flights
}

// We are going to split our ftFlights to (1) picked, (2) remaining.
func pickFlights(f ftFlights, ftNumber int) (ftFlights, ftFlights) {
	return f[:ftNumber], f[ftNumber:]
}

// Let's print it out.
func (f ftFlights) printIt(log string) {
	for i, flight := range f {
		fmt.Println(log, i, flight)
	}
}
```

<div align="right">▲<a href="#top">Back to Top</a></div>
