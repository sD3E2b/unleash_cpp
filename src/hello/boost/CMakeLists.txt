cmake_minimum_required(VERSION 3.10)

project(simple)

if (UNIX)
    add_compile_options(-Wall -Wextra -pedantic -Werror -Wno-unused-parameter)
endif(UNIX)

set(CMAKE_SHARED_LIBRARY_PREFIX_CXX "")


add_library(${PROJECT_NAME} SHARED simple.cpp)

if (UNIX)
      if (${CMAKE_MINOR_VERSION} GREATER 12)
        FIND_PACKAGE(Python3 COMPONENTS Development REQUIRED)
        FIND_PACKAGE(Boost COMPONENTS python${Python3_VERSION_MAJOR}${Python3_VERSION_MINOR} REQUIRED)
      else()
        FIND_PACKAGE(Boost COMPONENTS python3 REQUIRED)
        FIND_PACKAGE(PythonLibs REQUIRED)
    endif()
endif()

if (${CMAKE_MINOR_VERSION} GREATER 12)
   target_include_directories(${PROJECT_NAME}
       PUBLIC ${Boost_INCLUDE_DIRS}
       PUBLIC ${Python3_INCLUDE_DIRS}
   )

  target_link_libraries(${PROJECT_NAME}
      PUBLIC Boost::python${Python3_VERSION_MAJOR}${Python3_VERSION_MINOR}
      PUBLIC ${Python3_LIBRARIES}
  )
else()
  target_include_directories(${PROJECT_NAME}
      PUBLIC ${Boost_INCLUDE_DIRS}
      PUBLIC ${PYTHON_INCLUDE_DIRS}
  )

  target_link_libraries(${PROJECT_NAME}
      PUBLIC ${Boost_LIBRARIES}
      PUBLIC ${PYTHON_LIBRARIES}
  )
endif()

