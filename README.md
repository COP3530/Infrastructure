# Gradescope Autograder
### How it works
As an instructor, you create a new assignment on Gradescope, and upload your autograder zip file following their specifications here:https://gradescope-autograders.readthedocs.io/en/latest/specs/
 
 
 Your code produces output in the format they request. Students submit to Gradescope and have their work evaluated on demand. 
 
&nbsp;
### Configuration

In `config.yml`, define all files that must be submitted by the student.

Example `config.yml`:

```
limit_submissions: -1  # limit number of submissions accepted. Set to -1 for unlimited (default: -1)
required_files:
  - student_file.h
```

If any of the required files are missing, the autograder terminates with a message indicating a missing file.

If all required files are submitted, they are moved into the `/autograder/tests` directory.

### Writing Tests

Create a new directory under `/autograder/tests`. This is where you can include any test files. Create multiple test directories when you want to give partial credits.

Files:

* `run_test` (required):

    In order for the autograder to be able to run the test, you must include a `run_test` executable file that runs
    the test. This can be in any language, so make sure it includes a shebang line (`#!/usr/bin/env bash` for example).

* `test.yml` (optional but recommended)

    This is the configuration for the specific test.

    Example `test.yml` file:

    ```
    weight: 15.0  # weight/score of the test (default: 1)
    name: 'BST balancing'  # (default: '')
    message: 'insert() must maintain a balanced BST'  # (default: '')
    show_output: false  # whether to display stdout/stderr output on failure (default: true)
    timeout: 10  # test fails if it does not finish running in 10 seconds (default: null)
    visibility: hidden  # controls visibility of test case to students (default: visible)
    ```

* Any other files that `run_test` requires.
* Any header files that students require as long as students include your files. See more details on the example autograder zipfile.

### Example tests
---
#### Catch test
Catch2 is mainly a unit testing framework for C++

```
#include "student_file.h"
#include "catch.hpp"



TEST_CASE( "Test 01", "Example Project" )
{
  Node* head = nullptr;                     
  head = insertEnd(head, 4);
  //from student_file
  REQUIRE(interQuartile(head) == 3.0);    
}
```

#### IO test
File:
* `test.cpp`:

    A cpp file that writes your main function
* `input.txt`:

* `expected_output.txt`:

* `run_test`:
    
    an executable file that run diff command
    ```
    g++ -O0 -std=c++11 test.cpp -I ../ -o text.exe

    ./text.exe < input.txt > output.txt 
    diff expected_output.txt output.txt
    ```


&nbsp;

**Author:** `Xizhe Li`, 
**Date Created:** `18 July 2022`, 
**Last Modified:** `18 July 2022`