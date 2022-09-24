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
        list(APPEND INTERFACE_LIBS glfw)
        if (IMGUI_GLFW_INCLUDE_PATH)
            target_include_directories(imgui PUBLIC ${IMGUI_GLFW_INCLUDE_PATH})
        else ()
            message(FATAL_ERROR "IMGUI_GLFW_INCLUDE_PATH not specified")
        endif ()
        target_sources(imgui
                PRIVATE
                backends/imgui_impl_opengl3.cpp
                backends/imgui_impl_glfw.cpp
                )
    else ()
        message("NO BACKEND SPECIFIED")
    endif ()
endif ()

target_include_directories(imgui
        PUBLIC .
        PUBLIC backends
        )

target_link_libraries(imgui INTERFACE ${INTERFACE_LIBS})
