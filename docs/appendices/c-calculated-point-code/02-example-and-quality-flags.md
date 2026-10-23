## Example {#example .Appendix-H2}

This section presents an example of a calculation, how it is encoded and how it could be calculated.

| 🛈 | The method of calculation is vendor dependent and need not follow the example shown here. |
|--------------|-------------------------------------------------|

The example calculation expression takes the average of two analogue points (AI1 and AI2) using the calculation below:

> Calculated Point Value = (AI1 + AI2) / 2

This expression is encoded as follows:

| Instruction | Bytecode | Syntax | Type | Parameter 1 | Parameter 2 |
|---------------|-----------|--------|-------------------------------|------------------|------------------|
| 1 | 33 | / | Divide |  |  |
| 2 | 30 | \+ | Add |  |  |
| 3 | 10 | AI1 | Source Point Value (SPV) | 0 (Analogue In) | 1 (Point number) |
| 4 | 10 | AI2 | Source Point Value (SPV) | 0 (Analogue In) | 2 (Point number) |
| 5 | 0 | 2 | Integer Literal (IL) | 2 |  |

: Example of a calculation

This appears in the configuration as an array of the numbers below (spacing for clarity only):

33, 30, 10, 0, 1, 10, 0, 2, 0,2

## Quality flags {#quality-flags .Appendix-H2}

All calculated points have quality flags as any other Lucid point. The rules for calculation of those flags are given below:

- If a calculated point is Offline or its Restart flag is set, then no calculation need be performed for that point.

- Offline for a calculated point is the logical OR of all Offline flags for every point in the calculation together with the inverted On Scan setting of the calculated point. Therefore, if any of the contributing points are Offline/Not On Scan then the calculated point is Offline, also, if the calculated point is set to Off Scan, then it is Offline.

- Restart for a calculated point is the logical OR of all Restart flags for every point in the calculation. If any point in the calculation has not had Restart reset, then Restart is set for the calculated point and hence the value should be disregarded.

- Comms Lost, Remote Forced and Local Forced for a calculated point is the logical OR of the same flags for every point in the calculation. It is not possible to set these for any other reason for the calculated point, they are dependent entirely on the source points.

- Calculated points are either a binary or analogue point, hence the following rules are used.

<!-- -->

- For a binary calculated point Chatter is the logical OR of the same flag for all binary points used in the calculation.

- For an analogue calculated point Overrange is the logical OR of the same flag in all analogue points used in the calculation, OR'd with the overrange indication resulting from the calculation. So, for instance, if a calculation multiplies two source numbers together and gets an answer out of range then the Overrange flag would be set irrespective of the state of the source points.

- For an analogue calculated point Reference Error is set to the logical OR of the same flag for all analogue points used in the calculation.

- Rollover and Discontinuity are never used.

<!-- -->

- The State flag for a calculated binary point is the calculated value of the point.

- At start-up and before any calculation is performed, the default value of all flags is 0, apart from Restart which is set to 1.

- Where a calculation uses source points which have a different point type to the calculated point, the flag of the calculated point is determined only from those flags in the source points which match those in the calculated point. Where appropriate, it is recommended that calculations separately deal with non-matching object flags.
