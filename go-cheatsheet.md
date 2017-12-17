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
	// firstFlight - new variable of type string.
	var firstFlight string = "First Class to Perth"
	fmt.Println("firstFlight:", firstFlight)

	// secondFlight - Go can guess that it's a string without us specifying it.
	secondFlight := "Economy Class to Lisbon"
	fmt.Println("secondFlight:", secondFlight)
	
	// secondflight - We assign a new value to it.
	// Because Go is a static type language, the type of a variable can never change afterwards.
	secondFlight = "First Class to Tokyo"
	fmt.Println("secondFlight:", secondFlight)

	// This would throw an error, because it's not a new variable
	// secondFlight := "Business Class to Oslo"

	// This would throw an error because the variable type changed when re-assigning it.
	// secondFlight = 41
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

// string - When defining a new function, specify what it will return.
// If not, it expects nothing and throws error: too many arguments to return have (string) want ()
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

	// arrayFlightsExplicit - Initiate new variable that is an array of n values of type string.
	var arrayFlightsExplicit [3]string
	
	arrayFlightsExplicit[0] = "First Class to Lisbon"
	arrayFlightsExplicit[1] = "Economy Class to Tokyo"
	arrayFlightsExplicit[2] = "Economy Class to Florence"
	fmt.Println("arrayFlightsExplicit:", arrayFlightsExplicit)

	// arrayFlightsImplicit - Defining the content of the array directly inline.
	// An array can also contain a func if that func returns a value of same type.
	arrayFlightsImplicit := [3]string{"First Class to Lisbon", "Economy Class to Tokyo", oneNewFlight()}
	fmt.Println("arrayFlightsImplicit:", arrayFlightsImplicit)

	// An array has a fixed number of values. If we define more, it will throw an error:
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
	arrayFlights := [3]string{"First Class to Florence", "First Class to Lisbon", "Economy Class to Perth"}

	// A slice is an array that can grow or shrink
	sliceFlightsFromArray := arrayFlights[0:2]
	fmt.Println("sliceFlightsFromArray:", sliceFlightsFromArray)

	// sliceFlights - Initiating a slice and defining its values.
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

- In Object Oriented programming, we use classes to extend functionality.
- Then initiate new instances of those classes.
- Finally, we use methods to create functions that belong to the instance.

- In Go, we achieve this by "extending" a base type (like string, []string or integer etc.).

```go
// featuredFlights - is an extension of the type []string
type featuredFlights []string

// By making an extra type, it gives us the ability to create a method (a function with a receiver).
// That function will only work with that specific type.
// f - is the actual copy ("instance" in OO) of the featuredFlights we're working on.
func (f featuredFlights) printIt() {
	for i, flight := range f {
		fmt.Println("sliceFlight:", i, flight)
	}
}

func main() {
	// sliceFlights := []string{"Economy Class to Lisbon", "Business Class to Perth"}
	sliceFlights := featuredFlights{"Economy Class to Lisbon", "Business Class to Perth"}

	// Because we extended type featuredFlights with a printIt() functionnality,
	// any variable of type featuredFlights can call this function printIt() on itself.
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

// f - We are going to split this slice of type ftFlights, (1) pickedFlights (2) remainingFlights
// ftNumber - is the number of featured flights.
func pickFlights(f ftFlights, ftNumber int) (ftFlights, ftFlights) {

	// We can return multiple values simply with a comma.
	// Here we return (1) pickedFlights (2) remainingFlights.
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
- We will convert all flights combinations to a string, in which each value is separated by a comma. 
- Then we'll convert this string to a slice of bytes in order to inject it in ***ioutil.WriteFile()*** .

- For doing this we will use what we call a **type conversion**. `[]byte("Hello")` converts hello to a byte slice.
- See http://asciitable.com to find out decimal values for each letter of a string.
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

// f - is the string slice of type ftFlights that we convert to a single string, which is converted to a byte slice.
// error - ioutil.WriteFile returns an error, so we just return ioutil.WriteFile as that's also what we want to return.
func (f ftFlights) saveToFile(fileName string) error {
	return ioutil.WriteFile(fileName, []byte(f.toString()), 0666)
}

// f - is the string slice that we will convert to a single string with the default Go package strings.Join().
// Each value is separated by a comma in the single string.
func (f ftFlights) toString() string {
	return strings.Join([]string(f), ",")
}

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

