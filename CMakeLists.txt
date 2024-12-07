cmake_minimum_required(VERSION 3.16)

project(CurrencyConverter VERSION 0.1 LANGUAGES CXX)

# Ensure Qt auto-handling for UI files, moc, and rcc
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find the required Qt modules
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

# List your project's source files
set(PROJECT_SOURCES



    # Ensure this is correctly included
)

# If Qt version >= 6, use qt_add_executable, else use add_executable
if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(CurrencyConverter
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
else()
    add_executable(CurrencyConverter
        ${PROJECT_SOURCES}
    )
endif()

# Link necessary Qt libraries
target_link_libraries(CurrencyConverter PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

# Setting bundle and target properties for macOS and Windows
if(${QT_VERSION} VERSION_LESS 6.1.0)
  set(BUNDLE_ID_OPTION MACOSX_BUNDLE_GUI_IDENTIFIER com.example.CurrencyConverter)
endif()
set_target_properties(CurrencyConverter PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

# Install settings
include(GNUInstallDirs)
install(TARGETS CurrencyConverter
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

# Finalize the Qt executable setup for Qt 6
if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(CurrencyConverter)
endif()
