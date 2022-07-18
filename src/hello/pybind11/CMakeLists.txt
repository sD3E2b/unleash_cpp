cmake_minimum_required(VERSION 3.10)

project(simple)

if (UNIX)
    add_compile_options(-Wall -Wextra -pedantic -Werror -Wno-unused-parameter)
endif(UNIX)

set(CMAKE_SHARED_LIBRARY_PREFIX_CXX "")


add_library(${PROJECT_NAME} SHARED simple.cpp)

if (UNIX)
      if (${CMAKE_MINOR_VERSION} GREATER 12)
          FIND_PACKAGE(Python3 COMPONENTS Development Interpreter REQUIRED)
      else()
        FIND_PACKAGE(PythonLibs REQUIRED)
    endif()
endif()
find_package(pybind11 REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE pybind11::pybind11)

if (${CMAKE_MINOR_VERSION} GREATER 12)
   target_include_directories(${PROJECT_NAME}
       PUBLIC ${Python3_INCLUDE_DIRS}
   )

  target_link_libraries(${PROJECT_NAME}
      PUBLIC ${Python3_LIBRARIES}
  )
else()
  target_include_directories(${PROJECT_NAME}
      PUBLIC ${PYTHON_INCLUDE_DIRS}
  )

  target_link_libraries(${PROJECT_NAME}
      PUBLIC ${PYTHON_LIBRARIES}
  )
endif()

