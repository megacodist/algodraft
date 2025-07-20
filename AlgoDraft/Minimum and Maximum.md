We can obtain the minimum and maximum of an iterable (`IIterable`) of values that can compared against each other with these operators:

```
someIterable AS IIterable<IComparable>
someIterable <- INPUT {{some values that can be compared against each other}}
min AS ANY <- MIN OF someIterable
NOTIFY {{the minimum value is @min}}
max AS ANY <- MAX OF someIterable
NOTIFY {{the maximum value is @max}}
```