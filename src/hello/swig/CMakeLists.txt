cmake_minimum_required(VERSION 3.10)

project(_example)

if (UNIX)
    add_compile_options(-Wall -Wextra -pedantic -Werror -Wno-unused-parameter)
endif(UNIX)

set(CMAKE_SHARED_LIBRARY_PREFIX_CXX "")
set(CMAKE_SHARED_LIBRARY_PREFIX_C "")

add_library(${PROJECT_NAME} SHARED example.c example_wrap.c)

if (UNIX)
      if (${CMAKE_MINOR_VERSION} GREATER 12)
        FIND_PACKAGE(Python3 COMPONENTS Development REQUIRED)
      else()
        FIND_PACKAGE(PythonLibs REQUIRED)
    endif()
endif()

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

