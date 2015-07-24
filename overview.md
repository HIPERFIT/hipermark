#################################################
####### Overview of Hipermark Architecture ######
#################################################

Hipermark works with this hierarchy:

1. Benchmark: A specific mathematical problem or function (in the mathematical sense) which can be solved by a computer. The specific solutions to this problem can be written in many different languages and can use parallel or sequential logic.

2. Implementation: A specific solution to a problem. An implementation contains the source code of the solution to a specific problem. Each problem can have many different implementations which can use openMP or openCL and can be written in C, C++, futhark, Haskell etc.

3. Case: An implementation plus input and output data plus any configurations. A case run can be run since both the source code and the input data is present. A case validates if it produces the output data which is expected and with which the function call is made. So a case is called with both input data and the expected output data.


#################################################
####### API and Functionality of Hipermark ######
#################################################

Hipermark is written in Python and is object-oriented.

The implementation and benchmark concepts are implemented as classes in Hipermark, the problem concept is not.

#################################################
############### Design Standards ################
#################################################

Documentation in this program follows the Docstring convention found in PEP 0257:
https://www.python.org/dev/peps/pep-0257/#what-is-a-docstring
