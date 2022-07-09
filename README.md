# Traffic Light STM32
Development of a simple traffic light using the STM32F4 microcontroller programmed in C++.

# Build the source project

1. Access the project folder **traffic-light-stm32**

2. Pull the latest docker image used to build the project.
```console
docker pull charlesdias/gcc-arm-cmake
```

2. If you want, check the GCC version.
```console
$ docker run --rm charlesdias/gcc-arm-cmake arm-none-eabi-gcc -v
```

3. And, the CMake version
```console
docker run --rm charlesdias/gcc-arm-cmake cmake --version
```

4. Run the image docker
```console
docker run --rm -it -v $(pwd):/home/project -w /home/project charlesdias/gcc-arm-cmake
```

5. Inside the docker image, create a subdirectory called build (this name **build** is just a sugestion).
```console
mkdir -p build
```

6. Build CMake commands:
```console
cmake -DCMAKE_TOOLCHAIN_FILE=../source/arm-none-eabi-gcc.cmake -DCMAKE_BUILD_TYPE=Debug -S ./source -B ./build
cmake --build ./build -- -j 4
```
Or,

```console
cmake -G "Unix Makefiles" -DCMAKE_TOOLCHAIN_FILE=../source/arm-none-eabi-gcc.cmake -DCMAKE_BUILD_TYPE=Debug -S ./source -B ./build
cmake --build ./build -- -j 4
```

7. If you want to clean the output files run the command bellow:
```console
cmake --build ./build --target clean
```

# Requirements

* Git Conventional Commits. Link: https://github.com/qoomon/git-conventional-commits
