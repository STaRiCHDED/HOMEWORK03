
## Homework03

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

**Удачной стажировки!**
### Tutorial
### Part 1
```sh
1.
$ mkdir HOMEWORK03
$ cd HOMEWORK03
$ git clone https://github.com/tp-labs/lab03 tmp
$ cd formatter_lib
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(formatter)
EOF
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
$cat >> CMakeLists.txt <<EOF
add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
EOF
$cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
$ cmake -H. -B_build
$ cmake --build _build
$ ls _build/libformatter.a
```

### Part 2
```sh
$ cd ..
$ cd formatter_ex_lib
$ cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.10)
> project(formatter_ex)
> EOF
$ cat > CMakeLists.txt <<EOF
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> set(CMAKE_CURRENT_SOURCE_DIR /home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp/formatter_ex_lib)
> EOF
$ cat >> CMakeLists.txt <<EOF
> add_library(formatter_ex STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
> EOF
$ cat >> CMakeLists.txt <<EOF
> include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
> EOF
$ cat >> CMakeLists.txt <<EOF
> include_directories(/home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp/formatter_lib)
> target_link_libraries(formatter_ex formatter)
> EOF
$ cmake -H. -B_build
$ cmake --build _build
$ cd ..
$ git remote remove origin
$ git remote add origin https://github.com/STaRiCHDED/HOMEWORK03.git
$ git add .
$ git commit -m"Homework03"
$ git push origin master
```
### Part 3
```sh
1.
$ cd hello_world_application
$ cat >CMakeLists.txt<<EOF
> cmake_minimum_required(VERSION 3.10)
> project(hello_world)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> set(CMAKE_CURRENT_SOURCE_DIR /home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp)
> 
> add_executable(hello_world \${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)
> 
> include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
> 
> add_library(hello STATIC /home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp/formatter_ex_lib/formatter_ex.cpp /home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp/formatter_lib/formatter.cpp)
> 
> target_link_libraries(hello_world hello)
> EOF
$ cmake -H. -B_build
$ cmake --build _build
$ _build/hello_world
-------------------------
hello, world!
-------------------------
2.
$ cd ..
$ cd solver_application
$ cat >CMakeLists.txt<<EOF
cmake_minimum_required(VERSION 3.10)
project(Solver)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp)
add_executable(Solver ${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
add_library(solver STATIC /home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp/solver_lib/solver.cpp 
/home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp/formatter_ex_lib/formatter_ex.cpp 
/home/nikitaklimov/STaRiCHDED/workspace/projects/HOMEWORK03/tmp/formatter_lib/formatter.cpp)
target_link_libraries(Solver solver)
$ cmake -H. -B_build
$ cmake --build _build
$ _build/Solver
1
2
3
-------------------------
error: discriminant < 0
-------------------------

$ _build/Solver
1  
2
1
-------------------------
x1 = -1.000000
-------------------------
-------------------------
x2 = -1.000000
-------------------------
$ cd ..
$ git add .
$ git commit -a -m"HelloandSolve"
$ git push origin master
```

## Report

```sh
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2020 The ISC Authors
```
