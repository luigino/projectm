# default
sdl_version = '2.0.5'
### testing env variable, dirty hack
###if check_travis.returncode() == 0 and check_travis.stdout().strip() != null #wtf
# this check passes even if $TRAVIS=0
check_travis = run_command('sh', '-c', 'test $TRAVIS')
if check_travis.returncode() == 0
   message('We are on Travis CI')
   sdl_version = '2.0.2' # travis has old SDL, we don't care
endif
if enable_emscripten
   sdl_version = '2.0.0' # emscripten has old SDL, we don't care
endif

dep_sdl = dependency('sdl2', version: '>='+sdl_version, required: true)

projectM_sdl_sources = [
   'pmSDL.cpp',
   'projectM_SDL_main.cpp',
   'pmSDL.hpp'
]

projectM_sdl = executable(
   'projectMSDL',
   sources: projectM_sdl_sources,
   include_directories: include_directories('../libprojectM'),
   dependencies: [dep_gl, dep_sdl],
   link_with: libprojectM
)
