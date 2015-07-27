#################################################
# Overview of hipermark Architecture #
#################################################

Hipermark works with this hierarchy:

1. Benchmark: A specific mathematical problem or function (in the mathematical sense) which can be solved by a computer. The specific solutions to this problem can be written in many different languages and can use parallel or sequential logic. It is called a benchmark because it provides a basis for which to compare different implementations of solutions to this problem.

2. Implementation: A specific solution to a problem. An implementation contains the source code of the solution to a specific problem. Each problem can have many different implementations which can use openMP or openCL and can be written in C, C++, futhark, Haskell etc.

3. Case: An implementation plus input and output data plus any configurations. A case run can be run on a computer since both the source code and the input data is present. A case validates if it produces the output data which is expected from the input data with which the function call is made. So a case is called with both input data and the expected output data. 

#################################################
## API and Functionality of hipermark ##
#################################################

Hipermark is written in Python and is object-oriented.

The program runs only one benchmark at a time. This is specified when the program is executed.

The implementations are compiled one time for each case meaning that m implementations and n datasets will lead to m*n compilations in total and n compilations for each implementation. The source code should only be compiled once for every implementation. See "To Do".

The implementation and benchmark concepts are implemented as classes in Hipermark, the case concept is not.

#################################################
### Functionality of hipermark ###
#################################################

1. When the program is executed, it creates an overview of all benchmarks, implementations, and datasets and checks that the necessary files and folders are present.

2. For each implementation and for each dataset it then runs the function implementation.instantiate. This function makes the target directory for the running of a case and sets some variables found in the "instantiate" file which is located in implementation folder. It then creates a new process which runs the instantiate file and switches the folder of the child process to the target directory. There exists one instantiation file and one make file per implementation. So the same file is run once for each input.

3. The instantiate script copies the needed library and configuration files to the target directory (in /instantiations/<instantiation>). It creates a run file where the number of cores (and othr run-time parameters?) are set. When the run file is executed, it runs the binary file with the input found in input.data. The instantiate then calls these procedures:
  1. creates a input.data which formats the input.json data in a implementation/benchmark-dependant way.
  2. generates a platform.mk file from /lib/generate\_platform\_mk.py with the argument /config/platform_example.json.
  3. Sets NCORES with /lib/json_get.py
  4. Runs the make file that has been copied to the target folder.


#################################################
### Design Standards ###
#################################################

Documentation in this program follows the Docstring convention found in PEP 0257:
https://www.python.org/dev/peps/pep-0257/#what-is-a-docstring


#################################################
## To Do ##
#################################################

* The source code should only be compiled once for every implementation.
There only exists one instantiate file and one make file for each implementation, so the same instantiate file is run several times, and only the HIPERMARK_INPUT differ. This means that the HIPERMARK input should be changed somehow. It could include all possible inputs or something else could be done. The run file could then contain an array of all the possible inputs that could be run with this implementation.

The input data options probably need to be set when instantiate is run. The instantiate method needs to be changed to accept an array of input data.

An idea is to set the global variable $DATASET and then call instantiate/make from hipermark with the argument "data". The script should then create the input.data files for each available dataset. Hipermark then also needs to remember the names of the "compiled" datasets so that it can run one time for each dataset.

Evt. vha. argumenter. Ellers vha. miljøvariable -- kan disse klare arrays?
Man kan også blot angive stien til dataset-mappen.

Det skal være nemt at tilføje nye dataset uden at ændre implementeringer.

Instantiate og/eller Makefilerne bør ikke være for komplicerede. Det gælder også "run"-filerne. 

#################################################
###### Data Structure ######
#################################################

The data should be structured as follows:

* Root folder
  * benchmarks
    * benchmark1
      * datasets
        * dataset1
          * input.json
          * output.json
        * dataset2 ...
      * implementations
        * implementation1
          * instantiate
          * Makefile
          * source file(s)
        * implementation2 ...
      * lib
        * include
          * various library files for this specific benchmark (benchmark1)
    * benchmark2 ...
  * config
    * platform_example.json -- what is this?
  * DOCS
  * instantiations (created by running program)
    * instance1 (benchmark + implementation + dataset)
      * Makefile
      * needed library file(s)
      * source code file(s)
      * setup.mk
      * input.data
      * output.data
      * run
      * stderr.log
      * stdout.log
      * binary file for this impl.
    * instance2 ...
  * lib
    * include
      * ParserC.h
      * SDK_stub.h
      * Util.h
      * Utilities.cl
    * generate\_platform\_mf.py
    * json_get.py
    * linearise_data.py
    * platform.macbookpro.intel.mk
    * platform.macbookpro.mk
    * setup.mk
