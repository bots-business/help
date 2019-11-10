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



| Mask | Description |
| :--- | :--- |
| `d` | Day of the month as digits; no leading zero for single-digit days. |
| `dd` | Day of the month as digits; leading zero for single-digit days. |
| `ddd` | Day of the week as a three-letter abbreviation. |
| `dddd` | Day of the week as its full name. |
| `m` | Month as digits; no leading zero for single-digit months. |
| `mm` | Month as digits; leading zero for single-digit months. |
| `mmm` | Month as a three-letter abbreviation. |
| `mmmm` | Month as its full name. |
| `yy` | Year as last two digits; leading zero for years less than 10. |
| `yyyy` | Year represented by four digits. |
| `h` | Hours; no leading zero for single-digit hours \(12-hour clock\). |
| `hh` | Hours; leading zero for single-digit hours \(12-hour clock\). |
| `H` | Hours; no leading zero for single-digit hours \(24-hour clock\). |
| `HH` | Hours; leading zero for single-digit hours \(24-hour clock\). |
| `M` | Minutes; no leading zero for single-digit minutes. Uppercase M unlike CF `timeFormat`'s m to avoid conflict with months. |
| `MM` | Minutes; leading zero for single-digit minutes. Uppercase MM unlike CF `timeFormat`'s mm to avoid conflict with months. |
| `s` | Seconds; no leading zero for single-digit seconds. |
| `ss` | Seconds; leading zero for single-digit seconds. |
| `l` _or_ `L` | Milliseconds. `l` gives 3 digits. `L` gives 2 digits. |
| `t` | Lowercase, single-character time marker string: _a_ or _p_. No equivalent in CF. |
| `tt` | Lowercase, two-character time marker string: _am_ or _pm_. No equivalent in CF. |
| `T` | Uppercase, single-character time marker string: _A_ or _P_. Uppercase T unlike CF's t to allow for user-specified casing. |
| `TT` | Uppercase, two-character time marker string: _AM_ or _PM_. Uppercase TT unlike CF's tt to allow for user-specified casing. |
| `Z` | US timezone abbreviation, e.g. _EST_ or _MDT_. With non-US timezones or in the Opera browser, the GMT/UTC offset is returned, e.g. _GMT-0500_ No equivalent in CF. |
| `o` | GMT/UTC timezone offset, e.g. _-0500_ or _+0230_. No equivalent in CF. |
| `S` | The date's ordinal suffix \(_st_, _nd_, _rd_, or _th_\). Works well with `d`. No equivalent in CF. |
| `'…'`_or_`"…"` | Literal character sequence. Surrounding quotes are removed. No equivalent in CF. |
| `UTC:` | Must be the first four characters of the mask. Converts the date from local time to UTC/GMT/Zulu time before applying the mask. The "UTC:" prefix is removed. No equivalent in CF. |



