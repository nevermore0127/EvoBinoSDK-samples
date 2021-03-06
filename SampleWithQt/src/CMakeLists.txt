cmake_minimum_required(VERSION 3.1)

set(TARGET_NAME SampleWithQt)

if(DEFINED PROJECT_NAME)
	set(insideSDK true)
endif(DEFINED PROJECT_NAME)

if(insideSDK)
	# inside sdk project
	SET(EvoBinoSDK_LIBRARIES evo_depthcamera)
else()
	# external build
	PROJECT( ${TARGET_NAME} )
	FIND_PACKAGE(EvoBinoSDK REQUIRED)
endif()

#For QT
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
find_package(Qt5Widgets REQUIRED)
find_package(Qt5OpenGL REQUIRED)

find_package(CUDA REQUIRED)
include_directories(${EvoBinoSDK_INCLUDE_DIRS} ${CUDA_INCLUDE_DIRS})

if(insideSDK)
	ADD_EXE_MODULE(${TARGET_NAME} ${FOLDERNAME})
	set_target_properties (${TARGET_NAME} PROPERTIES FOLDER ${FOLDERNAME})
else()
	file(GLOB DIR_HEADERS "*.h")
	file(GLOB DIR_CPP_SRCS "*.cpp")

	add_executable(${TARGET_NAME} ${DIR_HEADERS} ${DIR_CPP_SRCS})
endif()

target_link_libraries(${TARGET_NAME} ${EvoBinoSDK_LIBRARIES} ${CUDA_LIBRARIES} Qt5::Widgets Qt5::OpenGL) 

