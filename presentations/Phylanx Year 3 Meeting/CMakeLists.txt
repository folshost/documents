# Require a recent version of cmake
cmake_minimum_required(VERSION 2.8.4 FATAL_ERROR)

project(slides)

if(NOT EXISTS ${CMAKE_BINARY_DIR}/fonts)
    file(COPY ${CMAKE_SOURCE_DIR}/fonts DESTINATION ${CMAKE_BINARY_DIR})
endif()

if(NOT EXISTS ${CMAKE_BINARY_DIR}/figures)
    file(COPY ${CMAKE_SOURCE_DIR}/figures DESTINATION ${CMAKE_BINARY_DIR})
    file(GLOB figure_list ${CMAKE_SOURCE_DIR}/figures/source/*.tex)
    foreach(elt ${figure_list})
        execute_process(COMMAND xelatex -output-directory=${CMAKE_BINARY_DIR}/figures ${elt})
    endforeach(elt)
endif()

add_custom_target(
    slides
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMAND cp ${CMAKE_SOURCE_DIR}/*.sty  ${CMAKE_BINARY_DIR}
    COMMAND cp ${CMAKE_SOURCE_DIR}/*.tex  ${CMAKE_BINARY_DIR}
    COMMAND xelatex main.tex && xelatex main.tex && xelatex main.tex
)
