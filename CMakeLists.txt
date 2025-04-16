project(ImGui)
add_library(ImGui STATIC)

target_include_directories(ImGui 
PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
PRIVATE
    ${CMAKE_SOURCE_DIR}/AntiLight/vendor/SDL/include
)

set(IMGUI_SOURCES
    imconfig.h
    imgui.h
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_tables.cpp
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    backends/imgui_impl_sdl3.h
    backends/imgui_impl_sdl3.cpp
    backends/imgui_impl_opengl3.h
    backends/imgui_impl_opengl3.cpp
)


if(WIN32)
    list(APPEND IMGUI_SOURCES
        backends/imgui_impl_dx11.h
        backends/imgui_impl_dx11.cpp
    )
    target_compile_definitions(ImGui PRIVATE "IMGUI_IMPL_API=__declspec(dllexport)")
    target_compile_definitions(ImGui PRIVATE _CRT_SECURE_NO_WARNINGS)
elseif(UNIX AND NOT WIN32)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
endif()

target_sources(ImGui PRIVATE ${IMGUI_SOURCES})

target_compile_definitions(ImGui PRIVATE 
    $<$<CONFIG:Debug>:IMGUI_DEBUG>
    $<$<CONFIG:Release>:IMGUI_RELEASE>
    $<$<CONFIG:Dist>:IMGUI_DIST>
)

target_compile_options(ImGui PRIVATE 
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O2>
    $<$<CONFIG:Dist>:-O2 -s>
)
