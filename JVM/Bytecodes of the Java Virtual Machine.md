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
- 3 ways: constant value is either implicit in the opcode itself (1-15), follows the opcode in the bytecode stream as operand (16, 17) or is taken from the constant pool(18-20).



Sr. No. | Opcode | Operand(s) | Description
--- | --- | --- | ---
1 | `iconst_m1` | none | pushes -1i
2 | `iconst_0` | none | pushes 0i
3 | `iconst_1` | none | pushes 1i
4 | `iconst_2` | none | pushes 2i
5 | `iconst_3` | none | pushes 3i
6 | `iconst_4` | none | pushes 4i
7 | `iconst_5` | none | pushes 5i
8 | `fconst_0` | none | pushes 0f
9 | `fconst_1` | none | pushes 1f
10 | `fconst_2` | none | pushes 2f
11 | `lconst_0` | none | pushes 0l
12 | `lconst_1` | none | pushes 1l
13 | `dconst_0` | none | pushes 0d
14 | `dconst_1` | none | pushes 1d
15 | `aconst_null` | none | pushes null object reference
16 | `bipush` | byte1 | expands byte1 (a byte type) to an int and pushes
17 | `sipush` | byte1, byte2 | expands byte1, byte2 (a short type) to an int and pushes
18 | `ldc1` | indexbyte1 | pushes 32-bit constant_pool entry specified by indexbyte1
19 | `ldc2` | indexbyte1, indexbyte2 | pushes 32 bit constant_pool entry specified by indexbyte1 and indexbyte2
20 | `ldc2w` | indexbyte1, indexbyte2 | pushes 64-bit constant_pool entry specified by indexbyte1 and indexbyte2

- `int` and `float` are 32 bit and each slot on stack is also 32 bits. So, when pushed, occupies 1 slot.
- `long` and `double` are 64 bits and occupy 2 consecutive slots.
- `aconst_null` pushes a null object reference and will refer to a java object on heap. `null` reference indicates that the object reference does not refer to any valid object.
- `bipush` and `sipush`: used to push integer constants that are within the valid range for byte or short type. byte or short that follows the opcode is expanded to an int before pushing. Operations on bytes and shorts that have been pushed onto stack are actually done on their int equivalents.
- `ldc1`, `ldc2` and `ldc2w` push constants from constant pool. All constants associated with a class are stored in the class's constant_pool. These have operands which tell the index of constant pool. JVM looks up the index in constant pool, determines constant's type and pushes.
- Differnce between `ldc1` and `ldc2` is that `ldc1` can refer only constant pool locations 1-255 because its index is 1 byte. Constant pool 0 is unused. `ldc2` has 2 byte index  so it can refer to any constant pool location.
- `ldc2w` has also 2 byte index and it is used to push long or double which occupy 64-bits.

## Pushing local variables onto the stack

