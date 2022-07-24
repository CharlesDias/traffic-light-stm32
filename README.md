# Traffic Light STM32
Development of a simple traffic light using the STM32F4 microcontroller programmed in C++.

# Build the source project

1. Clone the project for your local machine.
```console
git clone https://github.com/CharlesDias/traffic-light-stm32.git
```

2. Access the project folder **traffic-light-stm32**
```console
cd  traffic-light-stm32
```

3. Pull the latest docker image used to build the project.
```console
docker pull charlesdias/gcc-arm-cmake
```

4. If you want, check the GCC version.
```console
docker run --rm charlesdias/gcc-arm-cmake arm-none-eabi-gcc -v
```

5. And, the CMake version.
```console
docker run --rm charlesdias/gcc-arm-cmake cmake --version
```

6. Run the image docker.
```console
docker run --rm -it -v $(pwd):/home/project -w /home/project charlesdias/gcc-arm-cmake
```

7. Execute the build CMake commands. This command will create the build folder in the root project path and build the project.
```console
cmake -DCMAKE_TOOLCHAIN_FILE=../source/arm-none-eabi-gcc.cmake -DCMAKE_BUILD_TYPE=Debug -S ./source -B ./build
cmake --build ./build -- -j 4
```
Or,

```console
cmake -G "Unix Makefiles" -DCMAKE_TOOLCHAIN_FILE=../source/arm-none-eabi-gcc.cmake -DCMAKE_BUILD_TYPE=Debug -S ./source -B ./build
cmake --build ./build -- -j 4
```

If the compilation process finished successfully, you will see a similar output console message:

```console
[100%] Linking CXX executable traffic-light-stm32f4.out
Memory region         Used Size  Region Size  %age Used
             RAM:        2680 B       128 KB      2.04%
           FLASH:       12780 B       512 KB      2.44%
   text    data     bss     dec     hex filename
  12760      20    2668   15448    3c58 traffic-light-stm32f4.out
[100%] Built target traffic-light-stm32f4.out
```

8. If you want to clean the output files, run the command bellow.
```console
cmake --build ./build --target clean
```

# Requirements

* Git Conventional Commits. Link: https://github.com/qoomon/git-conventional-commits
