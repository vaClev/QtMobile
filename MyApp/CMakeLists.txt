cmake_minimum_required(VERSION 3.16)

project(MyApp VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Включаем AUTOMOC, чтобы Qt автоматически генерировал мета-код для сигналов/слотов.
set(CMAKE_AUTOMOC ON)

find_package(Qt6 REQUIRED COMPONENTS Quick)

# Устанавливаем политику Qt: новый формат префикса ресурсов для QML.
qt_policy(SET QTP0001 NEW)
# Устанавливаем политику Qt: поддержка qmldir-файлов в подпапках.
qt_policy(SET QTP0004 NEW)

#qt_standard_project_setup(REQUIRES 6.8) 26 macOs

# ---- Настройки, специфичные для платформы iOS ----
if(IOS)
    # Указываем, что сборка будет производиться для iPhoneOS (не симулятор).
    set(CMAKE_OSX_SYSROOT iphoneos)
    # Настройки подписи кода для iOS (будут сконфигурированы в Xcode).
    set(CMAKE_XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY "iPhone Developer")
    set(CMAKE_XCODE_ATTRIBUTE_DEVELOPMENT_TEAM "")
    set(CMAKE_XCODE_ATTRIBUTE_PROVISIONING_PROFILE_SPECIFIER "")
endif()

# Уникальный идентификатор приложения для macOS и iOS.
set(MACOSX_BUNDLE_GUI_IDENTIFIER "com.yourcompany.myapp")
set(CMAKE_XCODE_ATTRIBUTE_PRODUCT_BUNDLE_IDENTIFIER "com.yourcompany.myapp")


#########################################
qt_add_executable(MyApp
    Source/main.cpp
)

qt_add_qml_module(MyApp
    URI MyApp
    VERSION 1.0
    QML_FILES
      qml/Main.qml
    RESOURCES
      qml/qmldir
)

########################################
set_target_properties(MyApp PROPERTIES
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

target_link_libraries(MyApp
    PRIVATE Qt6::Quick
)

include(GNUInstallDirs)
install(TARGETS MyApp
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
