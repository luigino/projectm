projectM_unittest_sources = [
   'projectM-unittest.cpp',
   'getConfigFilename.cpp',
   'ConfigFile.h',
   'getConfigFilename.h'
]

projectM_unittest = executable(
   'projectM-unittest',
   sources: projectM_unittest_sources,
   include_directories: [
      include_directories('../..'),
      include_directories('../libprojectM'),
      include_directories('../libprojectM/Renderer')
   ],
   dependencies: dep_sdl,
   link_with: libprojectM
)

#### TODO investigate these old tests programms projectM_test projectM_test-memleak projectM_test-texture
###bin_PROGRAMS = projectM-unittest
###

# unsure
###projectM_unittest_LDFLAGS = -static
###projectM_unittest_PROGRAM = projectM-unittest

