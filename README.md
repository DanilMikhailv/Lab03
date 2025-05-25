# Lab03
Лабораторная работа по ТиМП03

**Часть 1**

1. Я клонировал репозиторий и перешел в директорию formatter_lib

```
┌──(danil㉿danil)-[~]
└─$ git --version
git version 2.45.2
┌──(danil㉿danil)-[~]
└─$ cd lab03                                                                                           
┌──(danil㉿danil)-[~/lab03]
└─$ cd formatter_lib

```

2. Я создал файл CMakeLists.txt и записал туда следующий текст:

```
┌──(danil㉿danil)-[~/lab03/formatter_lib]
└─$ nano CmakeLists.txt
```

CMakeLists.txt

```
cmake_minimum_required(VERSION 3.10)
project(formatter_lib)

add_library(formatter STATIC
    formatter.cpp
)

target_include_directories(formatter PUBLIC
    ${PROJECT_SOURCE_DIR}
)

install(TARGETS formatter ARCHIVE DESTINATION lib)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/ DESTINATION include/formatter)
```

Потом я уже заметил что у меня два файла, один C**m**ake... второй C**M**ake...
Файл с маленькой буквой я удалил и продублировал его содержимое в CMake...

3. Создал папку сборки и перешел в нее:

```
┌──(danil㉿danil)-[~/lab03/formatter_lib]
└─$ mkdir build
```

4. Запуск cmake

```
┌──(danil㉿danil)-[~/lab03/formatter_lib]
└─$ cmake -B build
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.8s)
-- Generating done (0.0s)
-- Build files have been written to: /home/danil/lab03/formatter_lib/build

```

5. В директории build запустил make

```
┌──(danil㉿danil)-[~/lab03/formatter_lib/build]
└─$ make                   
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```

6. Проверка результата

```
┌──(danil㉿danil)-[~/lab03/formatter_lib/build]
└─$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  libformatter.a  Makefile

```

**Часть 2**

1. Переходим в директорию formatter_ex_lib

```
┌──(danil㉿danil)-[~/lab03]
└─$ cd formatter_ex_lib
```

2. Создаем файл CMake.txt

```
┌──(danil㉿danil)-[~/lab03/formatter_ex_lib]
└─$ nano CMakeLists.txt
```
CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.10)
project(formatter_ex_lib)

# показываем где искать файлы
include_directories(${PROJECT_SOURCE_DIR}/../formatter_lib)

# создаем библеотеку
add_library(formatter_ex STATIC
    formatter_ex.cpp
)

# показываем что formatter_ex зависит от formatter
target_link_libraries(formatter_ex PRIVATE formatter)

# путь к заголовочным файлам этой библиотеки
target_include_directories(formatter_ex PUBLIC
    ${PROJECT_SOURCE_DIR}
)
```

3. Используем cmake:

```
┌──(danil㉿danil)-[~/lab03/formatter_ex_lib/build]
└─$ cmake ..      
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/danil/lab03/formatter_ex_lib/build
                                                                                   
┌──(danil㉿danil)-[~/lab03/formatter_ex_lib/build]
└─$ make       
[ 50%] Building CXX object CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
                                                                                   
┌──(danil㉿danil)-[~/lab03/formatter_ex_lib/build]
└─$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  libformatter_ex.a  Makefile
```

**Часть 3**

1. В директории hello_world_application создаем CMakeLists.txt со следующим содержимым:

```
# hello_world_application/CMakeLists.txt

cmake_minimum_required(VERSION 3.10)
project(hello_world_application)

# Подключаем библиотеки
add_subdirectory(../formatter_lib formatter_lib_build)
add_subdirectory(../formatter_ex_lib formatter_ex_lib_build)

# Добавляем наше приложение
add_executable(hello_world hello_world.cpp)

# Линкуемся с библиотеками по имени
target_link_libraries(hello_world PRIVATE
    formatter_ex
    formatter
)
```
2. Создаем директорию build и запускаем cmake и make

```
mkdir build %% cd build

┌──(danil㉿danil)-[~/lab03/hello_world_application/build]
└─$ cmake ..
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.4s)
-- Generating done (0.0s)
-- Build files have been written to: /home/danil/lab03/hello_world_application/build

┌──(danil㉿danil)-[~/lab03/hello_world_application/build]
└─$ make
[ 16%] Building CXX object formatter_lib_build/CMakeFiles/formatter.dir/formatter.cpp.o                                                                            
[ 33%] Linking CXX static library libformatter.a
[ 33%] Built target formatter
[ 50%] Building CXX object formatter_ex_lib_build/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o                                                                      
[ 66%] Linking CXX static library libformatter_ex.a
[ 66%] Built target formatter_ex
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world

┌──(danil㉿danil)-[~/lab03/hello_world_application/build]
└─$ ls
CMakeCache.txt  cmake_install.cmake     formatter_lib_build  Makefile
CMakeFiles      formatter_ex_lib_build  hello_world
```
3. В директории  создаем CMakeLists.txt со следующим содержимым:

```
# hello_world_application/CMakeLists.txt

cmake_minimum_required(VERSION 3.10)
project(solver_app)

# Указываем где искать заголовочные файлы
include_directories(
    ${PROJECT_SOURCE_DIR}/../formatter_ex_lib
    ${PROJECT_SOURCE_DIR}/../formatter_lib
    ${PROJECT_SOURCE_DIR}/../solver_lib
)

# Указываем где искать .a файлы
link_directories(
    ${PROJECT_SOURCE_DIR}/../formatter_ex_lib/build
    ${PROJECT_SOURCE_DIR}/../formatter_lib/build
    ${PROJECT_SOURCE_DIR}/../solver_lib/build
)

# Собираем исполняемый файл
add_executable(solver equation.cpp)

# Линкуемся с библиотеками по имени
target_link_libraries(solver PRIVATE
    formatter_ex
    formatter
    solver_lib
)
```
4. Создаем директорию build и вызываем cmake и make

```
┌──(danil㉿danil)-[~/lab03/solver_application/build]
└─$ cmake ..
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.8s)
-- Generating done (0.0s)
-- Build files have been written to: /home/danil/lab03/solver_application/build
                                                                                   
┌──(danil㉿danil)-[~/lab03/solver_application/build]
└─$ make
[ 50%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
                                                                                   
┌──(danil㉿danil)-[~/lab03/solver_application/build]
└─$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  Makefile  solver
```
Выгрузил проект в данный репозиторий и изменил README.md
