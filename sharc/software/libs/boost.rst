.. _boost_sharc:

Boost C++ Library
=================

.. sidebar:: Boost C++ Library

   :Latest version: 1.64.0
   :URL: www.boost.org

Boost provides free, peer-reviewed and portable C++ source libraries.

Usage
-----

To activate the library run: ::

        module load libs/boost/1.64.0/gcc-4.9.4

Note that this **also activates a particular version of the GCC compiler** (as Boost depends on the C++ standard library, which is provided by the compiler).  You must use this version of the compiler to build your code if you want to use this build of the Boost library.  If you want to use a different compiler / compiler version then you need to request that a new build of Boost be compiled or compile a new build yourself.

Boost has been built *without* Python support: use :ref:`conda <sharc-python-conda>` and install the ``boost`` conda package if you want to use Boost with Python.

Build a simple program using Boost
----------------------------------

Many Boost libraries are header-only which makes them particularly simple to compile. The following program reads a sequence of integers from standard input, uses ``Boost.Lambda`` to multiply each number by three, and writes them to standard output (taken from http://www.boost.org/doc/libs/1_64_0/more/getting_started/unix-variants.html):

.. code-block:: c++

        #include <boost/lambda/lambda.hpp>
        #include <iostream>
        #include <iterator>
        #include <algorithm>

        int main()
        {
            using namespace boost::lambda;
            typedef std::istream_iterator<int> in;

            std::for_each(
                in(std::cin), in(), std::cout << (_1 * 3) << " " );
        }

Copy this into a file called ``example1.cpp`` and compile with: ::

        g++ example1.cpp -o example

Provided you loaded the correct modules given above, the program should compile without error.

Linking to a Boost library
--------------------------
The following program is taken from the official Boost documentation http://www.boost.org/doc/libs/1_64_0/more/getting_started/unix-variants.html

.. code-block:: c++

        #include <boost/regex.hpp>
        #include <iostream>
        #include <string>

        int main()
        {
            std::string line;
            boost::regex pat( "^Subject: (Re: |Aw: )*(.*)" );

            while (std::cin)
            {
                std::getline(std::cin, line);
                boost::smatch matches;
                if (boost::regex_match(line, matches, pat))
                    std::cout << matches[2] << std::endl;
            }
        }


This program makes use of the ``Boost.Regex`` library, which has a separately-compiled binary component we need to link to.
Assuming that the above program is called ``example2.cpp,`` compile with the following command: ::

        g++ example2.cpp -o example2 -lboost_regex

If you get an error message that looks like this: ::

        example2.cpp:1:27: error: boost/regex.hpp: No such file or directory

the most likely cause is that you forgot to load the correct modules as detailed above.

Installation Notes
------------------

This section is primarily for administrators of the system.

Boost in turn was installed as a dependency of Caffe.  It in turn depends on the ``libunistring`` and ``icu`` libraries.

Version 1.64.0
^^^^^^^^^^^^^^

Built using:

- gcc 4.9.4
- icu 58.2
- libunistring 0.9.7

Has run-time dependencies on those three modules (gcc is required for the C++ standard library).

Built without Python support.

#. Download, configure, build, test and install using :download:`this script </sharc/software/install_scripts/libs/boost/1.64.0/gcc-4.9.4/install.sh>` (installation log: :download:`install.log </sharc/software/install_scripts/libs/boost/1.64.0/gcc-4.9.4/install.log>`).
#. Install :download:`this modulefile </sharc/software/modulefiles/libs/boost/1.64.0/gcc-4.9.4>` as ``/usr/local//modulefiles/libs/boost/1.64.0/gcc-4.9.4``
#. Test by compiling and running the two programs shown above.
