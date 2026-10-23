## Operation codes {#operation-codes .Appendix-H2}

Calculations refer to other points by an enumeration of the point type: (ai=0, bi=1, ao=2, bo=3, c=4, s=5) and their point number.

| Operation Code | Example Syntax | Type | Parameter 1 | Parameter 2 | Parameter 3 | 3<sup>rd</sup> down on stack | 2<sup>nd</sup> down on stack | Top of stack | Result on stack | Description |
|-------|------------|-----------|-------|-------|-------|-------|------|------|------|------------------------|
| 0 | -1337 | Integer Literal | Integer value |  |  |  |  |  | Number | Integer literal left on top of the stack |
| 1 | 3.14 | Floating point literal | Floating point value |  |  |  |  |  | Number | Floating point literal left on top of the stack |
| 10 | "AI1" | Source point value | Point type number | Point Number |  |  |  |  | Number | Point value is read and placed on top of the stack. If the point has never been read, 0 is used |
| 11 | Q("AI1",3) | Source point quality flag | Point type number | Point Number | Bit number within quality flags |  |  |  | Number | Point quality flag value is read and placed on top of the stack as 0 or 1. |
| 12 | T("AI1") | Source point timestamp (seconds) | Point type number | Point Number |  |  |  |  | Number | Epoch time (**seconds** since 1/1/1970) of last point update is placed on top of the stack. If never processed, 0 is used |
| 12 | TMillis("AI1") | Source point timestamp (milliseconds) | Point type number | Point Number |  |  |  |  | Number | Epoch time (**milliseconds** since 1/1/1970) of last point update is placed on top of the stack. If never processed, 0 is used |
| 20 | \- | Unary minus |  |  |  |  |  | Number | -Number | Negate number on top of stack |
| 30 | \+ | Add |  |  |  |  | Number1 | Number2 | Number1 + Number2 | Add two numbers and place on top of stack |
| 31 | \- | Subtract |  |  |  |  | Number1 | Number2 | Number1 - Number2 |  |
| 32 | \* | Multiply |  |  |  |  | Number1 | Number2 | Number1 \* Number2 |  |
| 33 | / | Divide |  |  |  |  | Number1 | Number2 | Number1 / Number2 |  |
| 34 | \*\* | Power |  |  |  |  | Number1 | Number2 | Number1 \*\* Number2 |  |
| 35 | MOD | Modulo |  |  |  |  | Number1 | Number2 | Number1 MOD Number2 | Remainder after division |
| 40 | NOT | Logical NOT |  |  |  |  |  | Number | Not Number | All logical operators treat zero as false and non-zero as true. The NOT value of a number is 0 or 1 |
| 50 | OR | Logical |  |  |  |  | Number1 | Number2 | Number1 OR Number2 |  |
| 51 | AND | Logical |  |  |  |  | Number1 | Number2 | Number1 AND Number2 |  |
| 52 | XOR | Logical |  |  |  |  | Number1 | Number2 | Number1 XOR Number2 |  |
| 53 | XNOR | Logical |  |  |  |  | Number1 | Number2 | Number1 XNOR Number2 |  |
| 60 | IIF | Immediate If |  |  |  | Condition | Number1 | Number2 | Number | If Condition is true, result is Number1, else Number2 |
| 70 | ABS | Absolute Value |  |  |  |  |  | Number1 | Number | If Number1 is negative, return -Number1 |
| 71 | INT | Integer |  |  |  |  |  | Number1 | Number | The integer version of Number (truncation) |
| 80 | MAX | Return largest of two numbers |  |  |  |  | Number1 | Number2 | Number | The result is the largest of the two parameters |
| 81 | MIN | Return smallest of two numbers |  |  |  |  | Number1 | Number2 | Number | The result is the smallest of two parameters |
| 90 | = | Equal to |  |  |  |  | Number1 | Number2 | Number | Result is Boolean evaluation of Number1 = Number2 (false is zero, true is one) |
| 91 | != | Not equal to |  |  |  |  | Number1 | Number2 | Number | Result is Boolean evaluation of Number1 != Number2 (false is zero, true is one) |
| 92 | \> | Greater than |  |  |  |  | Number1 | Number2 | Number | Result is Boolean evaluation of Number1 \> Number2 (false is zero, true is one) |
| 93 | \>= | Greater than or equal to |  |  |  |  | Number1 | Number2 | Number | Result is Boolean evaluation of Number1 \>= Number2 (false is zero, true is one) |
| 94 | \< | Less than |  |  |  |  | Number1 | Number2 | Number | Result is Boolean evaluation of Number1 \< Number2 (false is zero, true is one) |
| 95 | \<= | Less than or equal to |  |  |  |  | Number1 | Number2 | Number | Result is Boolean evaluation of Number1 \<= Number2 (false is zero, true is one) |
| 100 | TimeWeekDay(z) | Day of week |  |  |  |  |  | Number1 (epoch seconds) | Number (day of week) | Get the day of week from the Epoch 1-7 (Mon-Sun) |
| 101 | TimeHour(z) |  |  |  |  |  |  | Number1 (epoch seconds) | Number | 0-23 |
| 102 | TimeMin(z), |  |  |  |  |  |  | Number1 (epoch seconds) | Number | 0-59 |
| 103 | TimeSec(z), |  |  |  |  |  |  | Number1 (epoch seconds) | Number | 0-59 |
| 104 | TimeDay(z) |  |  |  |  |  |  | Number1 (epoch seconds) | Number | 1-31 |
| 105 | TimeMonth(z) |  |  |  |  |  |  | Number1 (epoch seconds) | Number | 1-12 |
| 106 | TimeYear(z) |  |  |  |  |  |  | Number1 (epoch Seconds) | Number | Year e.g. 2022 |
| 107 | TimeEpoch |  |  |  |  |  |  |  | Number | Count of seconds since Jan 01 1970 UTC |
| 108 | TimeEpochMillis |  |  |  |  |  |  |  | Number | Count of milliseconds since Jan 01 1970 UTC |
| 120 | Previous() |  |  |  |  |  |  |  | Number | Get the value of this calculated point which is about to be overwritten |

: Operation codes


| 🛈   | All unused bytecodes are reserved for future Lucid use. |
|-----|---------------------------------------------------------|

The operation codes do not support any bitwise operators.

The operation codes do not support parentheses as the Polish Notation takes care of this.

There is no support for local time. All times are based on UTC as defined in Lucid.
