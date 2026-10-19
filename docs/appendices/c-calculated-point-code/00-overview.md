# Appendix: Calculated point code {#appendix-calculated-point-code .Appendix-H1}

The value of a calculated point is based on the values of one or more other points in the FD. The value is calculated using an expression that contains one or more operations. Operations are encoded using a code array defined in this section. An operation may have zero or more parameters. The number of parameters is fixed for each operation.

An expression and its constituent operations are encoded in Polish Notation (PN). Calculation of the expression may use a Last In First Out (LIFO) stack but all implementation details are specific to the vendor. The description specifies the coding of the expression and any rules significant to the calculation. An example of the encoding and a possible calculation scheme is given in Table 26.

The expression for the calculated point consists of a list of numeric codes and parameters representing the expression and inputs using PN/stack notation. The bytecodes are given in Table 25, showing the operation code, stack before, stack after, and a description of the operation. Each instruction has up to four values:

- The operation code.

- Zero to three parameters needed by that instruction.

Each instruction expects up to three values to be on the stack prior to execution. After execution they are replaced with a single result value on the stack.

## Evaluation of a calculated point value {#evaluation-of-a-calculated-point-value .Appendix-H2}

The calculation of a point value commences when determined by one of the execution strategies that apply to the calculated point.

After calculation, a single result value is present on the stack. An expression is not valid if this is not the case.

For binary inputs and outputs, the value 0 or 1 is used to represent the value.

Where logical operation results or single bit binary values are used, the 0 or 1 is encoded as an integer literal with value 0 or 1, respectively.

For double- or three-bit binary inputs, the values 0 to 7 are encoded as an integer literal.

Table 25 suggests a simple format for expression of the calculations in the Example Syntax column. However, the format of the calculation language may be specific to the configuration application creating the configuration message.

For example, the calculation may be specified using point names or may use a syntax created from the point type and point address. It is the calculation code which is common. This document defines the operation codes to be used irrespective of any language used to express the calculation to a user.
