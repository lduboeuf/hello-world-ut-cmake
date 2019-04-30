cmake_minimum_required(VERSION 3.1)

project(hw LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt5 COMPONENTS Core Quick REQUIRED)

add_executable(${PROJECT_NAME} "main.cpp" "qml.qrc")
target_compile_definitions(${PROJECT_NAME} PRIVATE $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>)
target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Core Qt5::Quick)

#option (UBUNTU_TOUCH "Build for Ubuntu Touch." OFF)
if (UBUNTU_TOUCH)
    message("Building for Ubuntu Touch")

    set(UT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/packaging/ubuntu_touch)
    install(FILES ${UT_PATH}/manifest.json DESTINATION /)
    install(FILES ${UT_PATH}/${PROJECT_NAME}.apparmor DESTINATION /)
    install(FILES ${UT_PATH}/${PROJECT_NAME}.desktop DESTINATION /)
    install(FILES assets/logo.png DESTINATION /assets/)

    install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION /)


endif ()
