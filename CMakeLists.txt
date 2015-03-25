cmake_minimum_required(VERSION 2.8)

project(phoneme_classifier_project)


SET(START_DIR .)

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${START_DIR}/builds/win32 )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )

# Importing Infra
SET(INFRA_DIR ${START_DIR}/libs/infra/src)
add_library(infra STATIC ${INFRA_DIR}/infra_vv_funcs.cpp ${INFRA_DIR}/infra_vm_funcs.cpp ${INFRA_DIR}/infra_mm_funcs.cpp ${INFRA_DIR}/infra_exception.cpp ${INFRA_DIR}/infra_binary.cpp)
target_include_directories (infra PUBLIC ${INFRA_DIR})

# Importing cmd_line
SET(CMD_LINE_DIR ${START_DIR}/libs/cmd_line/src)
add_library(cmd_line STATIC ${CMD_LINE_DIR}/cmd_line.cpp)
target_include_directories (cmd_line PUBLIC ${CMD_LINE_DIR})

# Importing active_set
# SET(ACTIVE_SET_DIR ${START_DIR}/src)
# add_library(active_set STATIC ${ACTIVE_SET_DIR}/active_set.h)

# Importing phoneme_classifier
SET(PHONEME_CLASSIFIER_DIR ${START_DIR}/src)
add_library(phoneme_classifier STATIC ${PHONEME_CLASSIFIER_DIR}/Classifier.cpp
							${PHONEME_CLASSIFIER_DIR}/Dataset.cpp
							${PHONEME_CLASSIFIER_DIR}/HtkFile.cpp
							${PHONEME_CLASSIFIER_DIR}/KernelExpansion.cpp
							${PHONEME_CLASSIFIER_DIR}/ConcateFeatures.cpp)

target_link_libraries(phoneme_classifier LINK_PUBLIC infra)
target_include_directories(phoneme_classifier PUBLIC ${START_DIR}/include)

# Executeable 1: Decoder

add_executable(PhonemeFrameBasedDecode ${START_DIR}/src/PhonemeFrameBasedDecode.cpp)

target_link_libraries(PhonemeFrameBasedDecode LINK_PUBLIC cmd_line)
target_link_libraries(PhonemeFrameBasedDecode LINK_PUBLIC infra)
target_link_libraries(PhonemeFrameBasedDecode LINK_PUBLIC phoneme_classifier)


# Executeable 2: Trainer

add_executable(PhonemeFrameBasedTrain ${START_DIR}/src/PhonemeFrameBasedTrain.cpp)

target_link_libraries(PhonemeFrameBasedTrain LINK_PUBLIC cmd_line)
target_link_libraries(PhonemeFrameBasedTrain LINK_PUBLIC infra)
target_link_libraries(PhonemeFrameBasedTrain LINK_PUBLIC phoneme_classifier)

#installer

install(TARGETS PhonemeFrameBasedDecode DESTINATION bin)
install(TARGETS PhonemeFrameBasedTrain DESTINATION bin)