cmake_minimum_required(VERSION 3.14)
project(txt211json VERSION 1.0.0 LANGUAGES CXX)

include(FetchContent)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_executable(${PROJECT_NAME} src/main.cpp src/utf.cpp src/normalize.cpp src/json.cpp)
option(BUILD_SHARED_LIBS "Build using shared libraries" OFF)

fetchcontent_declare(
    l2encdec
    GIT_REPOSITORY https://github.com/ritsuwastaken/open-l2encdec.git
    GIT_TAG 1.3.5
)
fetchcontent_declare(
    json
    GIT_REPOSITORY https://github.com/nlohmann/json.git
    GIT_TAG v3.12.0
)
# not the intended way to install utfcpp, but it works for this version
fetchcontent_declare(
    utfcpp
    GIT_REPOSITORY https://github.com/nemtrif/utfcpp.git
    GIT_TAG v4.0.9
)
set(UTF8_TESTS OFF CACHE BOOL "" FORCE)
set(UTF8_SAMPLES OFF CACHE BOOL "" FORCE)
fetchcontent_makeavailable(l2encdec json utfcpp)

target_link_libraries(${PROJECT_NAME} PRIVATE l2encdec nlohmann_json::nlohmann_json)
target_include_directories(${PROJECT_NAME} PRIVATE ${utfcpp_SOURCE_DIR}/source)
