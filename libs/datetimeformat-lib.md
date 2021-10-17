# DateTimeFormat Lib

Convert time to time string by mask.

Implemented from [http://blog.stevenlevithan.com/archives/date-time-format](http://blog.stevenlevithan.com/archives/date-time-format)

```javascript
Libs.DateTimeFormat.format(time, mask);
```

Example:

```javascript
var now = new Date();
Libs.DateTimeFormat.format(now, "m/dd/yy");

Libs.DateTimeFormat.format(now, "ddd mmm dd yyyy HH:MM:ss");

Libs.DateTimeFormat.format(now, "h:MM TT");

```



| Mask           | Description                                                                                                                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `d`            | Day of the month as digits; no leading zero for single-digit days.                                                                                                                                |
| `dd`           | Day of the month as digits; leading zero for single-digit days.                                                                                                                                   |
| `ddd`          | Day of the week as a three-letter abbreviation.                                                                                                                                                   |
| `dddd`         | Day of the week as its full name.                                                                                                                                                                 |
| `m`            | Month as digits; no leading zero for single-digit months.                                                                                                                                         |
| `mm`           | Month as digits; leading zero for single-digit months.                                                                                                                                            |
| `mmm`          | Month as a three-letter abbreviation.                                                                                                                                                             |
| `mmmm`         | Month as its full name.                                                                                                                                                                           |
| `yy`           | Year as last two digits; leading zero for years less than 10.                                                                                                                                     |
| `yyyy`         | Year represented by four digits.                                                                                                                                                                  |
| `h`            | Hours; no leading zero for single-digit hours (12-hour clock).                                                                                                                                    |
| `hh`           | Hours; leading zero for single-digit hours (12-hour clock).                                                                                                                                       |
| `H`            | Hours; no leading zero for single-digit hours (24-hour clock).                                                                                                                                    |
| `HH`           | Hours; leading zero for single-digit hours (24-hour clock).                                                                                                                                       |
| `M`            | <p>Minutes; no leading zero for single-digit minutes.<br>Uppercase M unlike CF <code>timeFormat</code>'s m to avoid conflict with months.</p>                                                     |
| `MM`           | <p>Minutes; leading zero for single-digit minutes.<br>Uppercase MM unlike CF <code>timeFormat</code>'s mm to avoid conflict with months.</p>                                                      |
| `s`            | Seconds; no leading zero for single-digit seconds.                                                                                                                                                |
| `ss`           | Seconds; leading zero for single-digit seconds.                                                                                                                                                   |
| `l` _or_ `L`   | Milliseconds. `l` gives 3 digits. `L` gives 2 digits.                                                                                                                                             |
| `t`            | <p>Lowercase, single-character time marker string: <em>a</em> or <em>p</em>.<br>No equivalent in CF.</p>                                                                                          |
| `tt`           | <p>Lowercase, two-character time marker string: <em>am</em> or <em>pm</em>.<br>No equivalent in CF.</p>                                                                                           |
| `T`            | <p>Uppercase, single-character time marker string: <em>A</em> or <em>P</em>.<br>Uppercase T unlike CF's t to allow for user-specified casing.</p>                                                 |
| `TT`           | <p>Uppercase, two-character time marker string: <em>AM</em> or <em>PM</em>.<br>Uppercase TT unlike CF's tt to allow for user-specified casing.</p>                                                |
| `Z`            | <p>US timezone abbreviation, e.g. <em>EST</em> or <em>MDT</em>. With non-US timezones or in the Opera browser, the GMT/UTC offset is returned, e.g. <em>GMT-0500</em><br>No equivalent in CF.</p> |
| `o`            | <p>GMT/UTC timezone offset, e.g. <em>-0500</em> or <em>+0230</em>.<br>No equivalent in CF.</p>                                                                                                    |
| `S`            | <p>The date's ordinal suffix (<em>st</em>, <em>nd</em>, <em>rd</em>, or <em>th</em>). Works well with <code>d</code>.<br>No equivalent in CF.</p>                                                 |
| `'…'`_or_`"…"` | <p>Literal character sequence. Surrounding quotes are removed.<br>No equivalent in CF.</p>                                                                                                        |
| `UTC:`         | <p>Must be the first four characters of the mask. Converts the date from local time to UTC/GMT/Zulu time before applying the mask. The "UTC:" prefix is removed.<br>No equivalent in CF.</p>      |

