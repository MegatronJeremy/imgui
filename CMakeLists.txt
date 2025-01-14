# ImGui CMake Configuration
add_library(ImGui STATIC)

# Set C++ as the programming language for this target
set_target_properties(ImGui PROPERTIES
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

# Add source files
target_sources(ImGui PRIVATE
    imconfig.h
    imgui.h
    imgui.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_tables.cpp
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    imgui_demo.cpp
)

# Output directories
set_target_properties(ImGui PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/Binaries/$<CONFIG>/ImGui
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/Binaries/$<CONFIG>/ImGui
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/Binaries/$<CONFIG>/ImGui
)

# Platform-specific settings
if(WIN32)
    target_compile_definitions(ImGui PRIVATE WINDOWS)
    target_compile_options(ImGui PRIVATE /EHsc)
elseif(UNIX)
    target_compile_options(ImGui PRIVATE -fPIC)
endif()

# Configuration-specific settings
target_compile_options(ImGui PRIVATE
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O2>
    $<$<CONFIG:Dist>:-O2>
)

target_compile_definitions(ImGui PRIVATE
    $<$<CONFIG:Debug>:DEBUG>
    $<$<CONFIG:Release>:NDEBUG>
    $<$<CONFIG:Dist>:NDEBUG>
)
