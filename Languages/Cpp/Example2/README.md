###  Purpose

C++ code example on the FASRC cluster. `sum2.cpp` is a variation of [`sum.cpp`](../Example1/sum.cpp). It computes integer sum from 1 to N (N is read from command line).

Since this code reads an input from the command line, it cannot be run as a batch job and only as an interactive job.

### Contents

* `sum2.cpp`: c++ source code 

### C++ code

```cpp
/*
  Program: sum2.cpp

	   Computes sum of integers from 1 to N
	   ( N is read from command line )
 */
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

#define XTAB '\t'
#define YTAB '\v'

// Main program.............................................
int main(){
  int i;
  int n;
  int k;
  string mystr;
  cout << YTAB;
  cout << "*********************************************\n";
  cout << "* This program computes the sum of integers *\n";
  cout << "* from 1 to N, where N is an integer of our *\n";
  cout << "* choice.                                   *\n";
  cout << "*********************************************\n";
  cout << YTAB;
  cout << "Please, enter an integer: ";
  getline(cin,mystr);
  stringstream(mystr) >> n;
  k = 0; // Initialize
  for ( i = 1; i <= n; i++ ){
    k = k + i;
  }
  /* Write out results */
  cout << YTAB;
  cout << "You have entered " << n << '.' << endl;
  cout << "Sum of integers from 1 to " << n << " is " <<
    k << ".\n";
  cout << YTAB;
  return 0;
}
```

### Compile

We recommend compiling on a compute node. Request an interactive job to use a compute node, e.g.,

```bash
salloc --partition test --time 00:30:00 -c 2 --mem-per-cpu 2G
```

* Intel compilers, e.g.,

```bash
module load intel
icpc -O2 -o sum2.x sum2.cpp  # for intel version < 23.2, use `icpc`
icpx -O2 -o sum2.x sum2.cpp  # for intel version >= 23.2, use `icpx`.
```

* GNU compilers, e.g.,

```bash
module load gcc
g++ -O2 -o sum2.x sum2.cpp
```

### Run interactive code

On a compute node

```bash
./sum2.x
```

Example output

```bash

*********************************************
* This program computes the sum of integers *
* from 1 to N, where N is an integer of our *
* choice.                                   *
*********************************************

Please, enter an integer: 9

You have entered 9.
Sum of integers from 1 to 9 is 45.

```
