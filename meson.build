project('projectM', ['c', 'cpp'], default_options : ['cpp_std=c++11'], version: '3.1.1-rc3', license: 'LGPLv2.1+')

cc = meson.get_compiler('c')
cxx = meson.get_compiler('cpp')

if cxx.has_argument('-stdlib=libc++')
   add_project_arguments('-stdlib=libc++', language: 'cpp')
endif

add_project_arguments([
   '-Wall',
   '-Wchar-subscripts',
   '-Wformat-security',
   '-Wpointer-arith',
   '-Wshadow',
   '-Wsign-compare',
   '-Wtype-limits'
   ],
   language: ['c', 'cpp']
)

add_project_arguments('-include'+join_paths(meson.build_root(), 'config.h'), language: 'cpp')

host_os = host_machine.system()

enable_emscripten = get_option('emscripten')
enable_gles = get_option('gles')
enable_jack = get_option('jack')
enable_llvm = get_option('llvm')
enable_qt = get_option('qt')
enable_sdl = get_option('sdl')
enable_threading = get_option('threading')

have_aligned_alloc = cxx.has_function('aligned_alloc', prefix: '#include <stdlib.h>')
have_posix_memalign = cxx.has_function('posix_memalign', prefix: '#include <stdlib.h>')

dep_platform = []
dep_pthread = []
# dep_platform is not going to be available on Windows, see what to do
if host_os.startswith('linux')
   dep_platform = cxx.find_library('dl', required: true)
   if not cxx.has_function('dlopen', dependencies: dep_platform)
      error('libdl does not support dlopen')
   endif
endif

if host_os.startswith('darwin')
   dep_platform = dependency('appleframeworks', modules : ['CoreFoundation'], required : true)
endif

if enable_llvm
   dep_llvm = dependency('llvm', required: true)
endif

if enable_emscripten
### check how they're used later
   enable_threading = false
   enable_gles = true
   enable_sdl = true
endif

### since pthread is optional, will have to do this differently
if enable_threading
   dep_pthread = dependency('threads', required: true)
endif

### try to understand which library gles uses, if I don't use plain gl as dep, linking fails
if enable_gles and cxx.has_header('GLES3/gl3.h')
###
   dep_gl = dependency('gl', required: true)
###   dep_gl = cxx.find_library('GLESv2', required: true)
###elif not enable_emscripten
elif cxx.has_header('GL/gl.h') or cxx.has_header('OpenGL/gl.h')
   dep_gl = dependency('gl', required: true)
   enable_gles = false
endif
###endif

###   if cxx.has_header('OpenGL/gl.h')
###   unsure about this one, test on OSX
###      dep_gl = dependency('OpenGL', required: true)
###   endif
###endif

###if not enable_emscripten
###   if not cxx.has_header('glm/glm.hpp')
###      error('glm headers are required.')
###   endif
###endif

# this might be excessive, we only need the glm/glm.hpp header
if not enable_emscripten
   dependency('glm', required: true)
endif


# create config.h
config_h = configuration_data()

# The original had a = between variable and value
# also actually used
config_h.set_quoted('DATADIR_PATH', join_paths(get_option('prefix'), get_option('datadir'), meson.project_name()), description: 'Program data path')

# actually used
if enable_emscripten
   config_h.set10('EMSCRIPTEN', true, description: 'Define EMSCRIPTEN')
else
   config_h.set('EMSCRIPTEN', false, description: 'Define EMSCRIPTEN')
endif

# actually used
if have_aligned_alloc
   config_h.set10('HAVE_ALIGNED_ALLOC', true, description: 'Define to 1 if you have the \`aligned_alloc\' function.')
else
   config_h.set('HAVE_ALIGNED_ALLOC', false, description: 'Define to 1 if you have the \`aligned_alloc\' function.')
endif

# actually used in the sources
if have_posix_memalign
   config_h.set10('HAVE_POSIX_MEMALIGN', true, description: 'Define to 1 if you have the \`posix_memalign\' function.')
else
   config_h.set('HAVE_POSIX_MEMALIGN', false, description: 'Define to 1 if you have the \`posix_memalign\' function.')
endif

# The original had a = between variable and value
# also actually used in the sources
config_h.set_quoted('PROJECTM_PREFIX', get_option('prefix'), description: 'Program prefix')

# The original had a = between variable and value
# actually used in the sources
### check it's supposed to be just share/projectM
config_h.set_quoted('RESOURCE_PREFIX', join_paths(get_option('datadir'), meson.project_name()), description: 'Resource prefix')

# The original had a = between variable and value
### not actually used outside of Makefile.am
config_h.set_quoted('SYSCONFDIR', get_option('sysconfdir'), description: 'Program config path')

### see above how to add variables and stuff
# is this supposed to be set to 1?
# also actually used
if enable_gles
   config_h.set10('USE_GLES', true, description: 'Define USE_GLES')
else
   config_h.set('USE_GLES', false, description: 'Define USE_GLES')
endif

### see above how to add variables and stuff
# is this supposed to be set to 1?
# also actually used
if get_variable('enable_threading')
   config_h.set10('USE_THREADS', true, description: 'Define USE_THREADS')
else
   config_h.set('USE_THREADS', false, description: 'Define USE_THREADS')
endif

config_h.set_quoted('VERSION', meson.project_version(), description: 'Version number of package.')

configure_file(output: 'config.h', configuration: config_h)

### configure Makefile.am in the top dir and subdirs/subprojects
###AC_CONFIG_FILES([
###  src/projectM-emscripten/Makefile
###  ])

subdir('src')

message(
'\n\nprojectM v'+meson.project_version()+
'\n\n=====\n\n'+
'prefix:                 '+get_option('prefix')+'\n'+
'sysconfdir:             '+get_option('sysconfdir')+'\n'+
'libdir:                 '+join_paths(get_option('prefix'), get_option('libdir'))+'\n'+
'includedir:             '+join_paths(get_option('prefix'), get_option('includedir'))+'\n\n'+
'compiler:               '+cxx.get_id()+'\n'+
'cflags:                 not supported by meson\n'+
'cxxflags:               not supported by meson\n'+
'libs:                   not supported by meson\n'+
'ldflags:                not supported by meson\n\n'+
'- - -\n\n'+
'Applications:\n'+
'=====\n\n'+
'libprojectM:            yes\n'+
'Threading:              '+enable_threading.to_string('yes', 'no')+'\n'+
'SDL:                    '+enable_sdl.to_string('yes', 'no')+'\n'+
'Qt & Pulseaudio:        '+enable_qt.to_string('yes', 'no')+'\n'+
'OpenGLES:               '+enable_gles.to_string('yes', 'no')+'\n'+
'Emscripten:             '+enable_emscripten.to_string('yes', 'no')+'\n'+
'llvm:                   '+enable_llvm.to_string('yes', 'no')+'\n'
)
