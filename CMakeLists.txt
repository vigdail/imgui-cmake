cmake_minimum_required(VERSION 3.0)
project(ImGui)

if (IMGUI_SHARED)
    set(LIB_TYPE SHARED)
else ()
    set(LIB_TYPE STATIC)
endif ()

add_library(imgui ${LIB_TYPE})

target_sources(imgui
        PRIVATE
        imgui_demo.cpp
        imgui_draw.cpp
        imgui_tables.cpp
        imgui_widgets.cpp
        imgui.cpp
        )

set(INTERFACE_LIBS)
if (IMGUI_WITH_BACKEND)
    if (IMGUI_BACKEND_GLFW)
        message("BUILDING IMGUI WITH GLFW BACKEND")
        find_package(OpenGL REQUIRED)
        list(APPEND INTERFACE_LIBS ${OPENGL_LIBRARIES} glfw)
        target_sources(imgui
                PRIVATE
                backends/imgui_impl_opengl3.cpp
                backends/imgui_impl_glfw.cpp
                )
    endif ()
else()
    message("NO BACKEND SPECIFIED')
endif ()

set(OpenGL_GL_PREFERENCE GLVND)
target_include_directories(imgui
        PUBLIC .
        PUBLIC backends
        )

target_link_libraries(imgui INTERFACE ${INTERFACE_LIBS})
