cmake_minimum_required(VERSION 3.18)

# Name of project
set(PROJ_NAME FinalProj)
project(${PROJ_NAME})

# Specify project files: header files and source files
set(HDRS
    file_utils.h
    game.h
    game_object.h
    player_game_object.h
	
    enemy_game_object.h
    stationary_enemy.h
    moving_enemy.h
    kamakaze_enemy.h
    boss_middle.h
    boss_edge.h
	
    temp_game_object.h
    collectible_game_object.h
    background_game_object.h
    bullet_game_object.h
    standard_shot.h
    triple_shot.h
    flavour_game_object.h
    obstacle_game_object.h
    shader.h
    geometry.h
    sprite.h
    particles.h
    particle_system.h
	
    text_game_object.h

    player_particles.h
    bullet_particles.h
    cone_particle_system.h
)
 
set(SRCS
    file_utils.cpp
    game.cpp
    game_object.cpp
    main.cpp
    player_game_object.cpp
	
    enemy_game_object.cpp
    stationary_enemy.cpp
    moving_enemy.cpp
    kamakaze_enemy.cpp
    boss_middle.cpp
    boss_edge.cpp
	
    temp_game_object.cpp
    collectible_game_object.cpp
    background_game_object.cpp
    bullet_game_object.cpp
    standard_shot.cpp
    triple_shot.cpp
    flavour_game_object.cpp
    obstacle_game_object.cpp
    shader.cpp
    sprite.cpp
    sprite_vertex_shader.glsl
    sprite_fragment_shader.glsl
	
    player_particles.cpp
    bullet_particles.cpp
    cone_particle_system.cpp
    sprite_vertex_shader.glsl
    sprite_fragment_shader.glsl
    player_particle_vertex_shader.glsl
    player_particle_fragment_shader.glsl
    bullet_particle_fragment_shader.glsl

    text_game_object.cpp
    text_fragment_shader.glsl

    particles.cpp
    particle_system.cpp
    particle_vertex_shader.glsl
    particle_fragment_shader.glsl
)

# Add path name to configuration file
configure_file(path_config.h.in path_config.h)

# Add executable based on the source files
add_executable(${PROJ_NAME} ${HDRS} ${SRCS})

# Directories to include for header files, so that the compiler can find
# path_config.h
target_include_directories(${PROJ_NAME} PUBLIC ${CMAKE_CURRENT_BINARY_DIR})

# Require OpenGL library
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIR})
target_link_libraries(${PROJ_NAME} ${OPENGL_gl_LIBRARY})

# Other libraries needed
set(LIBRARY_PATH "" CACHE PATH "Folder with GLEW, GLFW, GLM, and SOIL libraries")
include_directories(${LIBRARY_PATH}/include)
if(NOT WIN32)
    find_library(GLEW_LIBRARY GLEW)
    find_library(GLFW_LIBRARY glfw)
    find_library(SOIL_LIBRARY SOIL)
elseif(WIN32)
    find_library(GLEW_LIBRARY glew32s HINTS ${LIBRARY_PATH}/lib)
    find_library(GLFW_LIBRARY glfw3 HINTS ${LIBRARY_PATH}/lib)
    find_library(SOIL_LIBRARY SOIL HINTS ${LIBRARY_PATH}/lib)
endif(NOT WIN32)
target_link_libraries(${PROJ_NAME} ${GLEW_LIBRARY})
target_link_libraries(${PROJ_NAME} ${GLFW_LIBRARY})
target_link_libraries(${PROJ_NAME} ${SOIL_LIBRARY})

# The rules here are specific to Windows Systems
if(WIN32)
    # Avoid ZERO_CHECK target in Visual Studio
    set(CMAKE_SUPPRESS_REGENERATION TRUE)
 
    # This will use the proper libraries in debug mode in Visual Studio
    set_target_properties(${PROJ_NAME} PROPERTIES DEBUG_POSTFIX _d)
endif(WIN32)
