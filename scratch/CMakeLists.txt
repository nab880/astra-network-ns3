set(astra-sim-ns3-libs-list
  "libapplications"
  "libcore"
  "libcsma"
  "libinternet"
  "libpoint-to-point"
  "libnetwork"
)
list(JOIN astra-sim-ns3-libs-list " " astra-sim-ns3-libs)
set(astra-sim-dir "${PROJECT_SOURCE_DIR}/../../../")
include_directories("../scratch")
# Disable fmt consteval format-string checking for AstraSimNetwork.cc and the
# AstraSim subdir: the bundled fmt's FMT_COMPILE_STRING consteval path fails
# under ns-3's C++20 + modern libc++ (see docs/BUILD.md fix #2).
add_compile_definitions(FMT_USE_CONSTEVAL=0)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../../../../ AstraSim)

build_exec(
  EXECNAME AstraSimNetwork
  SOURCE_FILES ${astra-sim-dir}/astra-sim/network_frontend/ns3/AstraSimNetwork.cc
  DIRECTORY_PATHS ${astra-sim-dir}/extern/helper
  LIBRARIES_TO_LINK "${astra-sim-ns3-libs-list}" "AstraSim"
  EXECUTABLE_DIRECTORY_PATH "${PROJECT_SOURCE_DIR}/build/scratch/"
)
