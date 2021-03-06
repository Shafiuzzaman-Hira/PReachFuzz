
```
docker pull aflplusplus/aflplusplus
wget https://ftp.gnu.org/gnu/binutils/binutils-2.28.tar.gz
tar -xzvf binutils-2.28.tar.gz
```

Build binary for afl-cov support
```
mkdir gcov-project
cp -r binutils-2.28 gcov-project/binutils-2.28
cd  gcov-project/binutils-2.28
./configure --prefix="/gcov-project/install/" CC="gcc" CXX="g++" CFLAGS="-fprofile-arcs -ftest-coverage" --disable-shared
make
make install
```
argument fuzzing:
https://liveoverflow.com/why-pick-sudo-research-target-part-1/#from-command-line-arguments-to-fuzzing
https://github.com/mirrorer/afl/blob/master/experimental/argv_fuzzing/argv-fuzz-inl.h

```
cd binutils-2.28
CC=afl-cc  ./configure  --prefix="/install/"
make
make install
```

```
afl-fuzz -V 10800 -i in -o out install/bin/strings
```
```
cd gcov-project/binutils-2.28
afl-cov -d ../../out --coverage-cmd "cat AFL_FILE | ../install/bin/strings" --code-dir . --enable-branch-coverage --overwrite
```
