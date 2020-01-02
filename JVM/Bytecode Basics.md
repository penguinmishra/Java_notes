# Bytecode Basics

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
--- | ---
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

- Local variables are stored on stack frame (portion of stack being used by the currently executing method).
- Each stack frame has 3 sections- local variables, execution environment and operand stack.
- Pushing local variable onto the stack means moving a value from local variables section of the stack frame to the operand section.
- Operand section of currently executing method is always on the top of the stack, so pushing a value onto the operand section of the current stack frame is the same as pushing a value onto the top of the stack.
- Each local variable of a method has a unique index. The local variable section of a method's stack frame can be thought of as an array of 32-bit slots, each one addressable by the array index. Local variables of type long or double, which occupy two slots, are referred to by the lower of the two slot indexes. For example, a double that occupies slots two and three would be referred to by an index of two.
- some opcodes have the index of variables in them itself such as `iload_0`. Some opcodes such as `iload` do not have index so they take local variable index(8 bit index) following the opcode.
- This 8 bit index variable limits the number of local variables to 256. A separate instruction `wide` can extend an 8 bit index by another 8 index which raises the local variable limit to 64 kilobytes. `wide` opcode is followed by an 8 bit operand. The `wide` opcode can precede an instruction such as `iload` that takes an 8 bit unsigned local variable index.
- 32-bit object reference variables from stack frame's local variable section is pushed with opcodes 21-25 in below list. 
<br><br>


Sr. No. | Opcode | Operand(s) | Description
--- | --- | --- | ---
1 | `iload` | `vindex` | pushes int variable from local variable position vindex
2 | `iload_0` | none | pushes int variable from local variable 0
3 | `iload_1` | none | pushes int variable from local variable position 1
4 | `iload_2` | none | pushes int variable from local variable position 2
5 | `iload_3` | none | pushes int variable from local variable position 3
6 | `fload` | `vindex` | pushes float variable from local variable position vindex
7 | `fload_0` | none | pushes float variable from local variable position 0
8 | `fload_1` | none | pushes float variable from local variable position 1
9 | `fload_2` | none | pushes float variable from local variable position 2
10 | `fload_3` | none | pushes float variable from local variable position 3
11 | `lload` | `vindex` | pushes long from local variable positions vindex and (vindex + 1)
12 | `lload_0` | none | pushes long from local variable positions zero and one
13 | `lload_1` | none | pushes long from local variable positions one and two
14 | `lload_2` | none | pushes long from local variable positions two and three
15 | `lload_3` | none | pushes long from local variable positions three and four
16 | `dload` | `vindex` | pushes double from local variable positions vindex and (vindex + 1)
17 | `dload_0` | none | pushes double from local variable positions zero and one
18 | `dload_1` | none | pushes double from local variable positions one and two
19 | `dload_2` | none | pushes double from local variable positions two and three
20 | `dload_3` | none | 	pushes double from local variable positions three and four
21 | `aload` | `vindex` | pushes object reference from local variable position vindex
22 | `aload_0` | none | pushes object reference from local variable position zero
23 | `aload_1` | none | pushes object reference from local variable position one
24 | `aload_2` | none | pushes object reference from local variable position two
25 | `aload_3` | none | pushes object reference from local variable position three

## Popping back to local variables

For each push opcode, there exists a pop opcode which pops the top of the stack into the local variables. These can be formed by replacing `load` in push opcodes by `store`:

Opcode | Operand(s) | Description
--- | --- | ---
`istore` | vindex | pops int to local variable position vindex
`istore_0` | none | pops int to local variable position zero
`istore_1` | none |	pops int to local variable position 1
`istore_2` | none |	pops int to local variable position 2
`istore_3` | none |	pops int to local variable position 3
`fstore` | vindex | pops float to local variable position vindex
`fstore_0` | none | pops float to local variable position zero
`fstore_1` | none | pops float to local variable position one
`fstore_2` | none | pops float to local variable position two
`fstore_3` | none | pops float to local variable position three
`lstore` | vindex | pops long to local variable positions vindex and (vindex + 1)
`lstore_0` | none | pops long to local variable positions zero and one
`lstore_1` | none | pops long to local variable positions one and two
`lstore_2` | none | pops long to local variable positions two and three
`lstore_3` | none | pops long to local variable positions three and four
`dstore` | vindex | pops double to local variable positions vindex and (vindex + 1)
`dstore_0` | none | pops double to local variable positions zero and one
`dstore_1` | none | pops double to local variable positions one and two
`dstore_2` | none | pops double to local variable positions two and three
`dstore_3` | none | pops double to local variable positions three and four
`astore` | vindex | pops object reference to local variable position vindex
`astore_0` | none | pops object reference to local variable position zero
`astore_1` | none | pops object reference to local variable position one
`astore_2` | none | pops object reference to local variable position two
`astore_3` | none | pops object reference to local variable position three


## Type Conversion

- The JVM has many opcodes for converting one primitive to another. No opcode follows the conversion opcode in bytecode stream. The value to convert is taken from the top of the stack. JVM pops the value from the top of the stack and pushes the result back on the top. Also the opcodes do not take any values! So no operands.
- Opcodes that convert from an int to a type smaller than int are shown in the following table. No opcodes exist that convert directly from a long, float, or double to the types smaller than int. Therefore converting from a float to a byte, for example, would require two steps. First the float must be converted to an int with f2i, then the resulting int can be converted to a byte with int2byte.
Opcode | Description
`i2l` | int to long
`i2f` | int to float
`i2d` | int to double
`l2i` | long to int
`l2f` | long to float
`l2d` | long to double
`f2i` | float to int
`f2l` | float to long
`f2d` | float to double
`d2i` | double to int
`d2l` | double to long
`d2f` | double to float
`int2byte` | int to byte
`int2char` | int to char
`int2short` | int tp short

- Although opcodes exist that convert an int to primitive types smaller than int (byte, short, and char), no opcodes exist that convert in the opposite direction. This is because any bytes, shorts, or chars are effectively converted to int before being pushed onto the stack. Arithmetic operations upon bytes, shorts, and chars are done by first converting the values to int, performing the arithmetic operations on the ints, and being happy with an int result. This means that if you add 2 bytes you get an int, and if you want a byte result you must explicitly convert the int result back to a byte. For example, the following code won't compile:
```
class BadArithmetic {

        byte addOneAndOne() {
                byte a = 1;
                byte b = 1;
                byte c = a + b;
                return c;
        }
}
```
When presented with the above code, javac objects with the following remark:
```
BadArithmetic.java(7): Incompatible type for declaration. Explicit cast needed to convert int to byte.
                byte c = a + b;
		^
```
To remedy the situation, the Java programmer must explicitly convert the int result of the addition of a + b back to a byte, as in the following code:
```
class GoodArithmetic {

        byte addOneAndOne() {
                byte a = 1;
                byte b = 1;
                byte c = (byte) (a + b);
                return c;
        }
}
```
Now the code compiles and the instructions will be as follows:
```
iconst_1 // Push int constant 1.
istore_1 // Pop into local variable 1, which is a: byte a = 1;
iconst_1 // Push int constant 1 again.
istore_2 // Pop into local variable 2, which is b: byte b = 1;
iload_1 // Push a (a is already stored as an int in local variable 1).
iload_2 // Push b (b is already stored as an int in local variable 2).
iadd // Perform addition. Top of stack is now (a + b), an int.
int2byte // Convert int result to byte (result still occupies 32 bits).
istore_3 // Pop into local variable 3, which is byte c: byte c = (byte) (a + b);
iload_3 // Push the value of c so it can be returned.
ireturn // Proudly return the result of the addition: return c;
```