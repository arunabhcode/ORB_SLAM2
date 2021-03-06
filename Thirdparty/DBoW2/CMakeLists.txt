cmake_minimum_required(VERSION 2.8)
project(DBoW2)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native -fPIC")
if(UNIX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
endif()

set(HDRS_DBOW2
  DBoW2/BowVector.h
  DBoW2/FORB.h
  DBoW2/FClass.h
  DBoW2/FeatureVector.h
  DBoW2/ScoringObject.h
  DBoW2/TemplatedVocabulary.h)
set(SRCS_DBOW2
  DBoW2/BowVector.cpp
  DBoW2/FORB.cpp
  DBoW2/FeatureVector.cpp
  DBoW2/ScoringObject.cpp)

set(HDRS_DUTILS
  DUtils/Random.h
  DUtils/Timestamp.h)
set(SRCS_DUTILS
  DUtils/Random.cpp
  DUtils/Timestamp.cpp)

find_package(OpenCV REQUIRED)
# if(NOT OpenCV_FOUND)
  # find_package(OpenCV 2.4.3 QUIET)
  # if(NOT OpenCV_FOUND)
      # message(FATAL_ERROR "OpenCV > 2.4.3 not found.")
  # endif()
# endif()

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# include_directories(${OpenCV_INCLUDE_DIRS})
add_library(DBoW2 STATIC ${SRCS_DBOW2} ${SRCS_DUTILS})
target_link_libraries(DBoW2 ${OpenCV_LIBS})
target_include_directories(DBoW2 PUBLIC
  ${PROJECT_BINARY_DIR}
  ${OpenCV_INCLUDE_DIRS}
)

include(GenerateExportHeader)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
set(CMAKE_VISIBILITY_INLINES_HIDDEN 1)
generate_export_header(DBoW2)

if(UNIX)
  file(COPY build/dbow2_export.h DESTINATION ${PROJECT_SOURCE_DIR}/DBoW2)
  file(COPY build/dbow2_export.h DESTINATION ${PROJECT_SOURCE_DIR}/DUtils)
endif()

install(TARGETS ${PROJECT_NAME}
  RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
  ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
  LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
install(FILES ${HDRS_DBOW2} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/ORB_SLAM2/Thirdparty/DBoW2/DBoW2)
install(FILES ${HDRS_DUTILS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/ORB_SLAM2/Thirdparty/DBoW2/DUtils)
install(FILES ${PROJECT_BINARY_DIR}/dbow2_export.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/ORB_SLAM2/Thirdparty/DBoW2/)
