# Bytecodes of the Java Virtual Machine

- When JVM loads a class file, it gets one stream of bytecodes for each method in the class.
- the bytecodes stream are stored in the method area of JVM.
- bytecodes for a method are executed when that method is invoked during the course of running the program by interpretation, JIT or any other technique that was chosen in JVM design.
- bytecodes are sequence of instructions each consisting of a one-byte `opcode` followed by 0 or more `operands`.
```
// Bytecode stream: 03 3b 84 00 01 1a 05 68 3b a7 ff f9


// Disassembly:
iconst_0      // 03
istore_0      // 3b
iinc 0, 1     // 84 00 01
iload_0       // 1a
iconst_2      // 05
imul          // 68
istore_0      // 3b
goto -7       // a7 ff f9
```

- all computation in the JVm centers on the stack. Since JVM does not have registers for storing arbitrary values, everything must be pushed to stack before it can be used in a calculation.
- in above example, bytecodes sequence a local variable is multiplied by two by first pushing the local variable on to the stack with `iload_0` instruction, then pushing two onto the stack with `iconst_2`. After both the integers have been pushed to the stack, the `imul` instruction effectively pops the two integers off the stack, multiplies them, and pushes the result back onto the stack. The result is popped off the top of the stack and stored back to the local variable by the `istore_0` instruction.

## Primitive Types

Type | Definition
--- | --- | ---
`byte` | 1 byte
`short` | 2 byte
`int` | 4 byte
`long` | 8 byte
`float` | 4 byte
`double` | 8 byte
`char` | 2 byte


All primitive types that occupy more than 1 byte are stored in big-endian order in the bytecode stream.
- JVM has several opecodes that push a local variable onto the stack. `iload`, `lload`, `fload`,`dload` push int, long, float and double respectively.

## Pushing constants
- 3 ways: constant value is either implicit in the opcode itself, follows the opcode in the bytecode stream as operand or is taken from the constant pool.



Opcode | Operand(s) | Description
--- | --- | ---
`iconst_m1` | none | pushes -1i
`iconst_0` | none | pushes 0i
`iconst_1` | none | pushes 1i
`iconst_2` | none | pushes 2i
`iconst_3` | none | pushes 3i
`iconst_4` | none | pushes 4i
`iconst_5` | none | pushes 5i
`fconst_0` | none | pushes 0f
`fconst_1` | none | pushes 1f
`fconst_2` | none | pushes 2f
`lconst_0` | none | pushes 0l
`lconst_1` | none | pushes 1l
`dconst_0` | none | pushes 0d
`dconst_1` | none | pushes 1d
`aconst_null` | none | pushes null object reference
`bipush` | byte1 | expands byte1 (a byte type) to an int and pushes
`sipush` | byte1, byte2 | expands byte1, byte2 (a short type) to an int and pushes

- `int` and `float` are 32 bit and each slot on stack is also 32 bits. So, when pushed, occupies 1 slot.
- `long` and `double` are 64 bits and occupy 2 consecutive slots.
- `aconst_null` pushes a null object reference and will refer to a java object on heap. `null` reference indicates that the object reference does not refer to any valid object.
