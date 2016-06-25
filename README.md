# simpleDSTadjust

ESP8266 Daylight Saving Time (DST) adjust library.

This library implements missing DST functionality by implementing a wrapper API 
to the built-in time() function. It uses simple rules to define the start and 
end of DST. Rules work for most contries that observe DST.
 see https://en.wikipedia.org/wiki/Daylight_saving_time_by_country for details and exceptions
 see http://www.timeanddate.com/time/zones/ for standard abbreviations and additional information
Caution: DST rules may change in the future if you hardcode the rules.

Portions of code adapted from:
    - Arduino Timezone Library by Jack Christensen
    - Arduino Time Library by Paul Stoffregen

# Quick Start

## Installing

You can install by downloading the contents of the library as a .zip file and using the Arduino
Library Manager to add the .zip file.

## Usage

Include the library in your sketch

```cpp
#include <simpleDSTadjust.h>
```

Define the DST rules that you want

```cpp
//US Eastern Time Zone (New York, Boston)
struct dstRule StartRule = {"EDT", Second, Sun, Mar, 2, 3600};    // Daylight time = UTC/GMT -4 hours
struct dstRule EndRule = {"EST", First, Sun, Nov, 2, 0};          // Standard time = UTC/GMT -5 hour
```

Intialize a global instance of the library

```cpp
// Setup simpleDSTadjust Library rules
simpleDSTadjust dstAdjusted(StartRule, EndRule);
```

In your code replace your call to time() with the simpleDSTadjust class version of the function.

Minimal version:

```cpp
time_t t = dstAdjusted.time(null);
```

Version to get the DST adjusted timezone abbreviation

```cpp
char *dstAbbrev;
time_t t = dstAdjusted.time(&dstAbbrev);
```


# Documentation

## Methods

### simpleDSTadjust constructor
```
simpleDSTadjust(struct dstRule startRule, struct dstRule endRule )
```
> initializes the class with the dstRules.

### time
```
time_t time(char **abbrev)
```
> Returns the DST adjusted time in seconds since Jan 1 1970. This is the same functionality as the built-in time() function.
However the simpleDSTadjust class version of time() has a char pointer argument instead of a time_t pointer. This is intended
for obtaining the timezone abbreviation string. If the argument passed is NULL, there is no difference in functionality other
than the DST adjustment.
 
