cmake_minimum_required (VERSION 2.8)

Project(ssai NONE)
add_custom_target(update)
add_custom_target(backup  ${CMAKE_HOME_DIRECTORY}/script/backup-image.sh)
add_custom_target(restore ${CMAKE_HOME_DIRECTORY}/script/restore-image.sh)
add_custom_target(upload  ${CMAKE_HOME_DIRECTORY}/script/update-image.sh)

if (NOT DEFINED PLATFORM)
    set(PLATFORM "Xeon")
endif()

file(GLOB dirs "deployment" "*")
list(REMOVE_DUPLICATES dirs)
foreach (dir ${dirs})
    if(EXISTS ${dir}/CMakeLists.txt)
        add_subdirectory(${dir})
    endif()
endforeach()

# legal message
execute_process(COMMAND printf "\nThis script will build third party components licensed under various open source licenses into your container images. The terms under which those components may be used and distributed can be found with the license document that is provided with those components. Please familiarize yourself with those terms to ensure your distribution of those components complies with the terms of those licenses.\n\n")

execute_process(COMMAND printf "\n-- Setting: PLATFORM=${PLATFORM}\n")
