cmake_minimum_required(VERSION 3.10)

get_filename_component(PRJDIR ${CMAKE_CURRENT_SOURCE_DIR} NAME)
project(isx VERSION 1.0.0 LANGUAGES C
   DESCRIPTION "ISx is an InstallShield installer extractor.")
string(TOUPPER ${PRJDIR} PRJ_UC)

if(MSVC)
   add_compile_options(/W3 /experimental:preprocessor)
else() #←assume GCC or Clang
   add_compile_options(-Wall -Wextra -Wpedantic)
endif()
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
set(CMAKE_VISIBILITY_INLINES_HIDDEN YES)

set(EXE ${PROJECT_NAME})
file(GLOB SRC CONFIGURE_DEPENDS *.c *.h)

add_executable(${EXE} ${SRC})
target_compile_features(${EXE} PRIVATE c_std_99)
target_compile_definitions(${EXE} PRIVATE PRJ_DESC="${PROJECT_DESCRIPTION}")
target_compile_definitions(${EXE} PRIVATE PRJ_NAME="${PROJECT_NAME}")
target_compile_definitions(${EXE} PRIVATE PRJ_UNAME="${PRJ_UC}")
target_compile_definitions(${EXE} PRIVATE PRJ_VERSION="${PROJECT_VERSION}")

find_package(Iconv REQUIRED)
target_link_libraries(${EXE} PRIVATE Iconv::Iconv)

# set(BIN "${PROJECT_SOURCE_DIR}/bin")
# set_target_properties(${EXE} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${BIN})
# if(NOT GENERATOR_IS_MULTI_CONFIG)
#    set_target_properties(${EXE} PROPERTIES
#       RUNTIME_OUTPUT_DIRECTORY "${BIN}/$<CONFIG>")
# endif()

install(
   TARGETS ${PROJECT_NAME}
   DESTINATION .)
