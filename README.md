# test
this is a test project
1. config:
        cd build/;
        cmake ..
2. compile:
        cd build/;
        make
3. install:
        cd build/;
        make install
4. release:
        cd build/;
        cpack --config CPackConfig.cmake;
        cpack --config CPackSourceConfig.cmake
5. clear:
        cd build/;
        make clean;
        rm -rf *
