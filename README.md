# Code Protection
> Guides, tips and essentials showing how to protect your source code. [+ some basic Computer Science tips &amp; definitions :)]

---

The best way to protect your source code is to convert it into machine code (also called "native code" or "native executable"). Machine code is the only programming language understood by CPUs.

> Machine code, also known as machine language, is the elemental language of computers. It is read by the computer's central processing unit (CPU), is composed of digital binary numbers and looks like a very long sequence of zeros and ones. Ultimately, the source code of every human-readable programming language must be translated to machine language by a compiler or an interpreter, because binary code is the only language that computer hardware can understand.
> 
> *source: https://www.techtarget.com/whatis/definition/machine-code-machine-language*

Such compilation process is called "Ahead of time compilation (AOT)" which involves converting a normal readable source code into its machine code directly. Examples of AOT compilers are GCC, clang (and most of the other C/C++ compilers) and the compiler of Golang (when we use the `go build` command to build a Go project)

For the other programming languages, you'll either find a direct source code to machine code compiler, **__or__** a source code to C/C++ compiler then from C/C++ to machine code

- For example, for Python there's Nuitka and Cython, both of them convert your Python code to C then compile the C to machine code using an AOT C compiler. (see https://github.com/Nuitka/Nuitka/issues/392#issuecomment-493660479, https://www.reddit.com/r/Python/comments/8w8us1/comment/e1tob8f/?utm_source=share&utm_medium=web2x&context=3 and https://github.com/Nuitka/Nuitka/issues/723#issuecomment-633019263)
- For Java there's GraalVM's native image: https://www.graalvm.org/22.2/reference-manual/java/compiler/#ahead-of-time-compilation . Also see https://www.baeldung.com/ahead-of-time-compilation
- For Kotlin there's Kotlin Native: https://kotlinlang.org/docs/native-overview.html
- For .NET (C#, VB.NET, etc) there's .NET Native (https://stackoverflow.com/a/35467422/8524395) or Mono's AOT compiler (https://www.mono-project.com/docs/advanced/aot/#full-aot)
- For PHP there's KPHP: https://vkcom.github.io/kphp/, or https://www.peachpie.io/ which compiles PHP into .NET (check the above line to compile the generated .NET into machine code)
- Golang (Go) is already a compiled programming language that naturally compiles to native code and you won't need to take any additional steps there :)

*for more, try Googling*:
- `compile [programming language name] to machine code`
- `compile [programming language name] to native code`
- `aot compile [programming language name]` (this one isn't recommended since AOT is always relatively defined. i.e. compiling Java source code to a JAR file containing JVM bytecode is considered AOT compilation [compiled/built before execution])
- `compile [programming language name] bytecode to machine code`
- `compile [programming language name] to c` (this one isn't the best since there can be compilers to native code without the need to use a C compiler).

In the end, remember that the main goal is to compile into machine (native) code

---

## Difference between JIT and AOT compilations

JIT compilers are like the Java VM or the .NET (C#/VB.NET/F#) CIL compiler.
JIT compilers generate an executable file which contains bytecode. On execution, the bytecode is converted by the installed VM (Java VM or .NET runtime for example) into machine code. Most of the JIT-compiled programming languages (as well as the interpreted languages) will require you to install an additional software (the JIT compiler/interpreter) in order to run the programs programmed in them, examples of such software are the Python interpreter, JVM runtime, .NET core/.NET framework, etc.

On the other hand, AOT compilers would generate the machine code in a direct way and you'll be able to execute it without the need for any JIT bytecode interpreters.

For more information check:
- https://en.m.wikipedia.org/wiki/Compiler
- https://en.m.wikipedia.org/wiki/List_of_programming_languages_by_type#Compiled_languages
- https://en.m.wikipedia.org/wiki/List_of_programming_languages_by_type#Interpreted_languages
- https://softwareengineering.stackexchange.com/a/155134/419528
- https://www.quora.com/Does-C%2B%2B-use-AOT-or-JIT-compilation-1/answer/Rolf-van-Kleef?ch=15&oid=52839449&share=3bfe68ea&srid=9WRvE&target_type=answer
- https://www.geeksforgeeks.org/difference-between-byte-code-and-machine-code/
- https://www.cesarsotovalero.net/blog/aot-vs-jit-compilation-in-java.html
- https://masuday.github.io/fortran_tutorial/compiler.html

## Difference between a compiler and a packer

Some tools like py2exe, cx_Freeze or PyInstaller are seen as compilers by some, However this is fatally incorrect. Such tools act like packers that just pack the Python runtime along with your Python source code (think of it as a ZIP archive). So unlike a compiler, packers don't generate any native (machine) code and your full Python source code will be easily accessible (see https://stackoverflow.com/a/12056778/8524395 and http://web.archive.org/web/20220824101030/https://yeah366.com/2022/03/Pythons-Packer-Nuitka/).
You can always check whether an EXE file is compiled or packed, by something like "Detect it Easy" (see https://superuser.com/a/1689825/936854)

DiE will tell you the compiler used for an EXE file/assembly.

Examples:
- For files packed using PyInstaller, DiE will report the "Packer" field as "PyInstaller"
- For C# JIT compiled files, DiE will not have a "Compiler" field, and will have the "Library" field as ".NET"

## Difference between the two terms "build" and "compile"

Actually, there's no difference between the two terms at all. In most of the projects we see on GitHub, we often see the instructions of **building** the project.
We can also say that such instructions show us how to "compile" the project. Simply, we can use either "build" or "compile" :)

## How does the Clang C compiler work? Does it produce machine code like GCC (is it considered an AOT compiler)?

Short answer, yes.

A little longer answer: Clang is built on the top of LLVM. LLVM is used by a lot of compilers and is often used to make programming languages.
LLVM translates its own bytecode (called LLVM IR) to machine code.

So what does Clang do? Clang takes your C/C++ source code, converts it into LLVM IR, then passes the LLVM IR to LLVM in order to get it compiled into machine (native) code.

See https://stackoverflow.com/a/3509258/8524395, https://stackoverflow.com/questions/9148890/how-to-make-clang-compile-to-llvm-ir and https://stackoverflow.com/a/3744093/8524395 for examples and more explanation :)

## Are programming languages like C# and Java really compiled or rather interpreted?
This can be confusing actually. Programming languages like C# and Java depend on their JIT compilers (the installed .NET runtime/JVM runtime).

When you compile your C# application or Java project, you don't actually get an executable containing native code, but you get an executable containing the bytecode understood by the corresponding JIT compiler (.NET runtime/JVM runtime).
Such JIT compilers then will translate the needed bytecode bits into native code once needed (during execution).

I personally disagree with calling such programming languages compiled, they probably have their own definition of "compilation" but what I'd really consider a compiled program is those that are directly understood by the computer processor.

For more information see:
- https://stackoverflow.com/a/8837367/8524395
- https://stackoverflow.com/a/1326084/8524395

## Can C/C++ be interpreted/JIT compiled?

Even though that's not recommended, yes. There are a few C/C++ JIT compilers/interpreters available there, Cling (https://root.cern/cling/) is one of the popular and most used ones. See https://stackoverflow.com/questions/69539/have-you-used-any-of-the-c-interpreters-not-compilers

## What's the difference between an interpreter and a JIT compiler?

a JIT compiler translates bytecode into native code once a specific bytecode is about to get executed. An interpreter reads the source code on execution, converts it into bytecode then converts the bytecode into native code. See https://stackoverflow.com/questions/3718024/jit-vs-interpreters, https://www.reddit.com/r/dartlang/comments/malf1u/comment/grsv8xg/?utm_source=share&utm_medium=web2x&context=3 and https://pediaa.com/what-is-the-difference-between-interpreter-and-jit-compiler/

## What's assembly language and how does it differ from machine code?
Assembly language (usually ends with `.asm` file extension) is a low-level programming language that requires a software called an assembler to convert it into machine code. In the end, we can consider it a programming language but it's not so easy to decompile assembly code (this gets even harder after compiling the assembly code to machine code, which is the max difficulty we can reach)

Assembly language is only a little bit easier to understand than machine code, and actually this advantage of assembly language made the first ever compiler be written in assembly code (instead of machine code directly, which would have been a much harder task to accomplish)

**Q:** But what uses assembly language? **A:** LLVM's `llc` command is an example. It compiles the LLVM IR (LLVM bytecode) into assembly language which then can be compiled later into machine code using an assembler.

See:
- https://llvm.org/docs/CommandGuide/llc.html
- https://stackoverflow.com/a/13492330/8524395
- https://pediaa.com/what-is-the-difference-between-machine-code-and-assembly-language/
- https://byjus.com/free-ias-prep/difference-between-compiler-and-assembler/
- https://en.wikipedia.org/wiki/Assembly_language
- https://en.wikipedia.org/wiki/Machine_code#Assembly_languages

Most of the C, C++ (and machine code executables in general) reverse engineering tools (like IDA Pro, Ghidra, RetDec, Snowman, Boomerang, etc) are actually just disassemblers (https://en.wikipedia.org/wiki/Disassembler) which convert the machine code of a native executable into a little easier-to-understand form (assembly language). Some of the tools mentioned (RetDec, Snowman and Boomerang) would attempt to convert the disassembled output into its original C source code, However it still would be very hard to understand.

## What proves that decompiling machine code is hard?
- https://stackoverflow.com/questions/1921656/compiling-c-sharp-to-native/30847025#30847025
- https://stackoverflow.com/a/52818963/8524395
- https://stackoverflow.com/a/7347168/8524395
- https://stackoverflow.com/a/45994104/8524395
- https://stackoverflow.com/a/68214794/8524395
- https://changsin.medium.com/how-to-protect-python-source-code-5892a153d0ac#6f78
- https://blogs.cisco.com/developer/securingpythoncodewithcython01
- https://medium.com/@xpl/protecting-python-sources-using-cython-dcd940bb188e
- https://qr.ae/pvbsA7
- https://qr.ae/pvbs2S
- https://sinister.ly/Thread-Which-programming-language-is-hard-to-reverse-engineering?pid=929264#pid929264
- https://stackoverflow.com/questions/205059/is-there-a-c-decompiler
- https://softwareengineering.stackexchange.com/questions/229761/why-cant-native-machine-code-be-easily-decompiled
- https://github.com/Nuitka/Nuitka/issues/392
- https://softwareengineering.stackexchange.com/a/155134/419528
- https://stackoverflow.com/q/65531208/8524395
- https://stackoverflow.com/questions/14308610/how-hard-is-it-really-to-decompile-assembly-code
- https://reverseengineering.stackexchange.com/a/312/41794
- https://stackoverflow.com/a/8601420/8524395
- and most importantly: https://en.wikipedia.org/wiki/Machine_code#Readability_by_humans

enough right? :)

## Why does the output machine-code-compiled file contain my method names and strings?
This is the normal behaviour of machine code executables. If you want to protect/hide such metadata from the output file, you'll have to use an obfuscator before compiling.

Some compilers also have a "Debug" mode instead of the "Production Release" mode. Debug modes usually keep a lot of debug information and metadata that can help reverse engineering your application. So make sure your compilers aren't putting such debug information in its calculations.

See:
- https://softwareengineering.stackexchange.com/questions/155131/is-it-important-to-obfuscate-c-application-code#comment295359_155131
- https://www.quora.com/Why-my-c-exe-string-variable-values-are-visible-when-I-decompiled-the-release-build-exe-with-cutter-How-do-I-make-sure-my-string-variable-values-are-not-visible-in-the-cutter-or-any-other-decompiler/answer/Thomas-Maierhofer-1?ch=15&oid=206511938&share=0175d9a1&srid=9WRvE&target_type=answer
- https://stackoverflow.com/questions/31492934/why-are-there-text-function-names-inside-exe-file
- https://stackoverflow.com/q/11291895/8524395
- https://stackoverflow.com/questions/17732833/remove-classes-string-name-from-compiled-release-exe
- https://curatedgo.com/r/when-you-compile-unixpicklegobfuscate/index.html
- https://www.reddit.com/r/golang/comments/k0ttu0/blackrota_a_heavily_obfuscated_backdoor_written/
- https://stackoverflow.com/questions/37101223/how-to-obfuscate-string-of-variable-function-and-package-names-in-golang-binary
- https://binary.ninja/2020/12/02/deobfuscation-of-gobfuscate-golang-binaries.html

Examples of obfuscators:
- https://github.com/burrowers/garble for Golang
- https://gist.github.com/oddmario/f8f313b01634c906f579c3df132767e2 for Python
- Use Google for more :)

## Finally,
More resources explaining the steps of computer program execution, compilations, and more definitions for machine code can be found here:

- https://en.wikipedia.org/wiki/Execution_(computing) (look at the box on the right side of this article)
- https://en.wikipedia.org/wiki/Machine_code
- https://en.wikipedia.org/wiki/Low-level_programming_language#Machine_code
- https://en.wikipedia.org/wiki/Programming_language_generations
- https://en.wikipedia.org/wiki/Timeline_of_programming_languages
- https://en.wikipedia.org/wiki/Lists_of_programming_languages
- https://stackoverflow.com/questions/266253/is-there-a-programming-language-below-assembly
- https://cs.stackexchange.com/questions/131943/is-there-code-below-microcode/131951#131951
- https://www.google.com/search?q=what+languages+are+understood+by+a+cpu&oq=what+languages+are+understood+by+a+cpu
- https://en.wikipedia.org/wiki/Computer_program
- https://superuser.com/questions/134483/how-was-the-first-compiler-compiled
- https://stackoverflow.com/questions/1653649/how-was-the-first-compiler-written
- https://stackoverflow.com/questions/1629513/when-someone-writes-a-new-programming-language-what-do-they-write-it-in
- https://stackoverflow.com/questions/845355/do-programming-language-compilers-first-translate-to-assembly-or-directly-to-mac
- https://cs.stackexchange.com/questions/14749/why-do-compilers-produce-assembly-code *(summary: most of the compilers compile into assembly first then from assembly into machine code)*
- https://stackoverflow.com/questions/14630852/how-is-machine-code-stored-in-the-exe-file
- https://stackoverflow.com/questions/1495638/whats-in-a-exe-file
- https://stackoverflow.com/questions/24973973/is-exe-made-of-pure-machine-code-only
- https://en.wikipedia.org/wiki/Executable
- https://en.wikipedia.org/wiki/Portable_Executable
- https://stackoverflow.com/questions/466790/assembly-code-vs-machine-code-vs-object-code
- https://medium.com/@bdov_/what-happens-when-you-type-gcc-main-c-a4454564e96d
- https://www.scaler.com/topics/c/compilation-process-in-c/
- https://icarus.cs.weber.edu/~dab/cs1410/textbook/1.Basics/machine.html
- https://www.geeksforgeeks.org/why-executable-files-are-os-dependent/
