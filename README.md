## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```sh
$ open https://cmake.org/
```


## Homework

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

 ##Work report
 
 ### Задание 1
 
 Директория: formatter_lib

   1) Создал файл CMakeLists.txt в директории formatter_lib.

   cmake_minimum_required(VERSION 3.12)

project(formatter_lib)

set(SOURCES formatter.cpp formatter.h)

add_library(formatter STATIC ${SOURCES})

2) cmake -B _build
  
 ### Задание 2
 
 1) Создал файл CMakeLists.txt в директории formatter_ex.

cmake_minimum_required(VERSION 3.12)

project(formatter_ex)

set(SOURCES formatter_ex.cpp formatter_ex.h)

add_library(formatter_ex STATIC ${SOURCES})

target_link_libraries(formatter_ex PRIVATE formatter)

2) cmake -B _build

### Задание 3

1) Сначала создал CMakeLists.txt для приложения hello_world:

cmake_minimum_required(VERSION 3.12)
project(hello_world)

set(CMAKE_CXX_STANDARD 11)

# Подключение библиотеки formatter_ex_lib
add_subdirectory(formatter_ex_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

# Исходный файл hello_world.cpp
add_executable(hello_world hello_world.cpp)

# Линковка с библиотекой formatter_ex_lib
target_link_libraries(hello_world formatter_ex_lib)

2) Затем создал CMakeLists.txt для библиотеки solver_lib:

cmake_minimum_required(VERSION 3.12)
project(solver_lib)

set(CMAKE_CXX_STANDARD 11)

# Исходные файлы библиотеки solver_lib
add_library(solver_lib STATIC solver.cpp)

# Подключение библиотеки formatter_ex_lib
add_subdirectory(formatter_ex_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

# Линковка с библиотеками formatter_ex_lib и solver_lib
target_link_libraries(solver_lib formatter_ex_lib)

3) Затем создал CMakeLists.txt для приложения solver_application:

cmake_minimum_required(VERSION 3.12)
project(solver_application)

set(CMAKE_CXX_STANDARD 11)

# Подключение библиотек solver_lib и formatter_ex_lib
add_subdirectory(solver_lib)
add_subdirectory(formatter_ex_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

# Исходный файл solver_application.cpp
add_executable(solver_application solver_application.cpp)

# Линковка с библиотеками solver_lib и formatter_ex_lib
target_link_libraries(solver_application solver_lib formatter_ex_lib)

4) После этого выполнил сборку с помощью команды cmake . в каждой директории проекта (hello_world, solver_lib, solver_application).


## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2021 The ISC Authors
```
