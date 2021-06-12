# Cross-compile Qt5 application from linux to windows
This approach uses statically linked qt, no `.dll`s are required

## Steps
1) Compile qt and additional libraries with [mxe](https://github.com/mxe/mxe).\
You can use prepared docker image:
```
docker build -t cross_compile -f Dockerfile.<version> .
```
If `docker build` reports errors when compiling packages, manually add problematic ones in dockerfile.

2) To simplify integration qml modules, use provided cmake file from [OlivierLDff's](https://github.com/OlivierLDff/QtStaticCMake) project.\
Add his file and update your `CMakeLists.txt`
```
find_package(Qt5 QUIET COMPONENTS QmlWorkerScript)


get_target_property(QT_TARGET_TYPE Qt5::Core TYPE)
if(${QT_TARGET_TYPE} STREQUAL "STATIC_LIBRARY")
    include(<path to cmake with macro>)
    qt_generate_plugin_import(<target> VERBOSE)

    qt_generate_qml_plugin_import(<target>
    QML_SRC <path to qml resource>
    EXTRA_PLUGIN
        <extra plugins>
        # QtQuickVirtualKeyboardPlugin
        # QtQuickVirtualKeyboardSettingsPlugin
        # QtQuickVirtualKeyboardStylesPlugin
        # QmlFolderListModelPlugin
        # QQuickLayoutsPlugin
    VERBOSE
    )

endif()

if(TARGET Qt5::QmlWorkerScript)
    target_link_libraries(<target> PRIVATE Qt5::QmlWorkerScript)
endif()
```
