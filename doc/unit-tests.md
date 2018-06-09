Compiling/running unit tests
------------------------------------

Unit tests will be automatically compiled if dependencies were met in `./configure`
and tests weren't explicitly disabled.

After configuring, they can be run with `make check`.

To run the pepcashd tests manually, launch `src/test/test_pepcash`.

To add more pepcashd tests, add `BOOST_AUTO_TEST_CASE` functions to the existing
.cpp files in the `test/` directory or add new .cpp files that
implement new BOOST_AUTO_TEST_SUITE sections.

To run the pepcash-qt tests manually, launch `src/qt/test/test_pepcash-qt`

To add more pepcash-qt tests, add them to the `src/qt/test/` directory and
the `src/qt/test/test_main.cpp` file.
