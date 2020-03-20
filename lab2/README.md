Synpsis

The goal of this exercise is to get familiar with the project structure and make the minimum changes so that you can hava a successful build on the master branch.

Clean

You first need to have clean repository, i.e. no changes. You have to commit everything you have worked on:
git add .
git commit -m "describe your changes"
Make sure you are on the master: git checkout master
Prepare

Get the code. To get updates from github, you just need to run git pull. You should now see lab2 folder.
Go to the folder (in the terminal, cd lab2)
Prepare the build:
For Linux/Mac, run: cmake .
For Windows, run: cmake . -G "MinGW Makefiles"
Project structure

The project is split into several classes organized in several files. The main.cpp is the entry point of the program. You should not make any changes to the main.cpp.

Classes

Each class is split into two files:

something.h - the header files (i.e. *.h) contains the declaration of the class, its member functions and variables. It is included via the #include directive in other .cpp files.
something.cpp - the CPP file contains the definition of the class member functions. It is what is actually compiled
There are 4 classes in the program:

Invoice - responsible for the state of the invoice being prepared, i.e. the products, quantities, totals, etc. It uses the Item class to keep track of product quantities. and is used by the TextPrinter class.
Item - responsible for tracking quantity and a subtotal of a given product, i.e. a line item of an invoice. It uses the Product class and is used by the Invoice class.
Product - responsible for the information about the product, i.e. name and price.
TextPrinter - responsible for printing invoice information to an output stream (e.g. cout). It uses the Invoice member functions to extract and present the information.
The main.cpp gives you the frame. In order to make it work and compile, you will need to make several changes to the class files.

Exercise

You should work in small iterations towards completing the exercise. I will illustrate the steps with a few examples, and then you should try on your own until you get to the final result.

The steps that we will be repeating are the following:

Build the program with make on Mac/Linux or mingw32-make on Windows.
Look at the first error.
Fix the first error so it dissapears (i.e. the code compiles, changes only in the .h files)
Go to #1 until no more errors during compilation.
Build the program with make on Mac/Linux or mingw32-make on Windows.
Linking fails - look at the first error and fix it by making changes in the .cpp files. Define empty member functions - don't try to implement them, our goal is to produce an executable.
Basically, when you repeat the process above several times, you will have an executable file lab1.exe on Windows or lab on Linux/Mac that doesn't do anything.

Example Steps

Build: make on Mac/Linux or mingw32-make on Windows

You will see errors. The first time, you should see something like:

[ 16%] Building CXX object CMakeFiles/lab1.dir/src/main.cpp.o
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:7:13: error: no matching constructor for initialization of 'Product'
    Product superMob("Super Mob", 12.90);
            ^        ~~~~~~~~~~~~~~~~~~
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit copy constructor) not viable: requires 1 argument, but 2 were provided
class Product {
      ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit move constructor) not viable: requires 1 argument, but 2 were provided
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit default constructor) not viable: requires 0 arguments, but 2 were provided
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:8:13: error: no matching constructor for initialization of 'Product'
    Product teaCup("Tea Cup", 5.30);
            ^      ~~~~~~~~~~~~~~~
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit copy constructor) not viable: requires 1 argument, but 2 were provided
class Product {
      ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit move constructor) not viable: requires 1 argument, but 2 were provided
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit default constructor) not viable: requires 0 arguments, but 2 were provided
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:9:13: error: no matching constructor for initialization of 'Product'
    Product redWineGlass("Red Wine Glass", 8.60);
            ^            ~~~~~~~~~~~~~~~~~~~~~~
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit copy constructor) not viable: requires 1 argument, but 2 were provided
class Product {
      ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit move constructor) not viable: requires 1 argument, but 2 were provided
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/product.h:8:7: note: candidate constructor (the implicit default constructor) not viable: requires 0 arguments, but 2 were provided
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:11:13: error: no member named 'add' in 'Invoice'
    invoice.add(superMob, 5);
    ~~~~~~~ ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:12:13: error: no member named 'add' in 'Invoice'
    invoice.add(teaCup, 12);
    ~~~~~~~ ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:13:13: error: no member named 'add' in 'Invoice'
    invoice.add(redWineGlass, 8);
    ~~~~~~~ ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:16:13: error: no member named 'print' in 'TextPrinter'
    printer.print(std::cout, invoice);
    ~~~~~~~ ^
7 errors generated.
make[2]: *** [CMakeFiles/lab1.dir/src/main.cpp.o] Error 1
make[1]: *** [CMakeFiles/lab1.dir/all] Error 2
make: *** [all] Error 2
Look at the first error:

[ 16%] Building CXX object CMakeFiles/lab1.dir/src/main.cpp.o
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:7:13: error: no matching constructor for initialization of 'Product'
    Product superMob("Super Mob", 12.90);
            ^        ~~~~~~~~~~~~~~~~~~
It indicates that there is no constructor for the Product class. Go to product.h and declare it:

class Product {
public:
    Product(string name, double price);
};
Notice that the constructor function ends with a semicolon - we will not provide a body at this point.

Build: make on Mac/Linux or mingw32-make on Windows

Scanning dependencies of target lab1
[ 16%] Building CXX object CMakeFiles/lab1.dir/src/main.cpp.o
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:11:13: error: no member named 'add' in 'Invoice'
    invoice.add(superMob, 5);
    ~~~~~~~ ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:12:13: error: no member named 'add' in 'Invoice'
    invoice.add(teaCup, 12);
    ~~~~~~~ ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:13:13: error: no member named 'add' in 'Invoice'
    invoice.add(redWineGlass, 8);
    ~~~~~~~ ^
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:16:13: error: no member named 'print' in 'TextPrinter'
    printer.print(std::cout, invoice);
    ~~~~~~~ ^
4 errors generated.
make[2]: *** [CMakeFiles/lab1.dir/src/main.cpp.o] Error 1
make[1]: *** [CMakeFiles/lab1.dir/all] Error 2
make: *** [all] Error 2
Now there are fewer errors. Look at the next one:

/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:11:13: error: no member named 'add' in 'Invoice'
    invoice.add(superMob, 5);
    ~~~~~~~ ^
There is not add member function of the Invoice class. Go to invoice.h and declare it:

class Invoice {
public:
    void add(Product product, int quantity);
};
Build: make on Mac/Linux or mingw32-make on Windows

Scanning dependencies of target lab1
[ 16%] Building CXX object CMakeFiles/lab1.dir/src/main.cpp.o
/Users/kiril.vuchkov/uni/CITB205-2020/lab2/src/main.cpp:16:13: error: no member named 'print' in 'TextPrinter'
    printer.print(std::cout, invoice);
    ~~~~~~~ ^
1 error generated.
make[2]: *** [CMakeFiles/lab1.dir/src/main.cpp.o] Error 1
make[1]: *** [CMakeFiles/lab1.dir/all] Error 2
make: *** [all] Error 2
Only one error remains now - there is no print member function in the TextPrinter class - go to textprinter.h and declare it:

class TextPrinter {
public:
    void print(std::ostream &out, Invoice invoice);
};
Scanning dependencies of target lab1
[ 16%] Building CXX object CMakeFiles/lab1.dir/src/main.cpp.o
[ 33%] Building CXX object CMakeFiles/lab1.dir/src/product.cpp.o
[ 50%] Building CXX object CMakeFiles/lab1.dir/src/item.cpp.o
[ 66%] Building CXX object CMakeFiles/lab1.dir/src/invoice.cpp.o
[ 83%] Building CXX object CMakeFiles/lab1.dir/src/textprinter.cpp.o
[100%] Linking CXX executable lab1
Undefined symbols for architecture x86_64:
  "TextPrinter::print(std::__1::basic_ostream<char, std::__1::char_traits<char> >&, Invoice)", referenced from:
      _main in main.cpp.o
  "Invoice::add(Product, int)", referenced from:
      _main in main.cpp.o
  "Product::Product(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, double)", referenced from:
      _main in main.cpp.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make[2]: *** [lab1] Error 1
make[1]: *** [CMakeFiles/lab1.dir/all] Error 2
make: *** [all] Error 2
Voila! We have successfully compiled everything. However, the linking fails. Let's keep iterating.

It is actually one error, but with a few problems in it. The problem is that we have declared member functions, but we haven't defined their bodies anywhere. When the compiler tries to link the program, it expects to find the definitions of the declared functions in some of the .o files produced by the compilation phase.

Let's add the first body (TextPrinter::print). Go to textprinter.cpp and add it:

void TextPrinter::print(std::ostream &out, Invoice invoice) {}
Note that when we provide definitions (i.e. body) for member functions we prefix the name of function with the name of the class.

Build: make on Mac/Linux or mingw32-make on Windows:

Scanning dependencies of target lab1
[ 16%] Building CXX object CMakeFiles/lab1.dir/src/main.cpp.o
[ 33%] Building CXX object CMakeFiles/lab1.dir/src/textprinter.cpp.o
[ 50%] Linking CXX executable lab1
Undefined symbols for architecture x86_64:
  "Invoice::add(Product, int)", referenced from:
      _main in main.cpp.o
  "Product::Product(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >, double)", referenced from:
      _main in main.cpp.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make[2]: *** [lab1] Error 1
make[1]: *** [CMakeFiles/lab1.dir/all] Error 2
make: *** [all] Error 2
Now we need to go to invoice.cpp and product.cpp and add the empty function bodies:

void Invoice::add(Product product, int quantity) {}
Product::Product(string name, double price) {}
Let's build one last time - make on Mac/Linux or mingw32-make on Windows:

Scanning dependencies of target lab1
[ 16%] Building CXX object CMakeFiles/lab1.dir/src/product.cpp.o
[ 33%] Building CXX object CMakeFiles/lab1.dir/src/invoice.cpp.o
[ 50%] Linking CXX executable lab1
[100%] Built target lab1
The End

Voila! We have completed the exercise and have an executable file. Now you can run it (it will do nothing, for now):

Windows: lab1.exe
Mac/Linux: ./lab1
