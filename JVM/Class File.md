# Class File

## Magic and Version numbers

- The first 4 bytes of every class file are always 0xCAFEBABE. This magic number makes Java class files easier to identify, because the odds are slim that non-class files would start with the same initial four bytes. The number is called magic because it can be pulled out of a hat by the file format designers. The only requirement is that it is not already being used by another file format that may be encountered in the real world.
- The second four bytes of the class file contain the major and minor version numbers. These numbers identify the version of the class file format to which a particular class file adheres and allow JVMs to verify that the class file is loadable. Every JVM has a maximum version it can load, and JVMs will reject class files with later versions.

## Constant Pool
