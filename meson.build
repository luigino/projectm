project('projectM', ['c', 'cpp'], default_options : ['cpp_std=c++11'], version: '3.1.1-rc1', license: 'LGPLv2.1+')

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
pkgdatadir=join_paths(get_option('prefix'), get_option('datadir'), meson.project_name())

have_emscripten = get_option('emscripten')
enable_gles = get_option('gles')
enable_jack = get_option('jack')
enable_qt = get_option('qt')
enable_sdl = get_option('sdl')
enable_threading = get_option('threading')

if get_variable('have_emscripten')
### check how they're used later
   set_variable('enable_threading', false)
# perhaps will have to search for gles here or create something under the GL check
   set_variable('enable_gles', true)
   set_variable('enable_sdl', true)
endif

if cxx.has_function('aligned_alloc', prefix: '#include <stdlib.h>')
   set_variable('have_aligned_alloc', true)
else
   error('stdlib does not support aligned_alloc')
endif
if cxx.has_function('posix_memalign', prefix: '#include <stdlib.h>')
   set_variable('have_posix_memalign', true)
else
   error('stdlib does not support posix_memalign')
endif

have_dlfcn_h = cxx.has_header('dlfcn.h')
have_inttypes_h = cxx.has_header('inttypes.h')
have_memory_h = cxx.has_header('memory.h')
### recheck this one
have_stdc_headers = cxx.has_header('stdcxx.h')
have_stdint_h = cxx.has_header('stdint.h')
have_stdlib_h = cxx.has_header('stdlib.h')
have_strings_h = cxx.has_header('strings.h')
have_string_h = cxx.has_header('string.h')
have_sys_stat_h = cxx.has_header('sys/stat.h')
have_sys_types_h = cxx.has_header('sys/types.h')
have_unistd_h = cxx.has_header('unistd.h')
have_windows_h = cxx.has_header('windows.h')

### since pthread is optional, will have to do this differently
if get_variable('enable_threading')
   dep_pthread = dependency('threads', required: true)
   have_pthread_prio_inherit = cxx.has_function('pthread_prio_inherit', prefix: '#include <pthread.h>')
   have_pthread_create_joinable = cxx.has_function('pthread_create_joinable', prefix: '#include <pthread.h>')
### add fflags and stuff, also autotools has this incompatbility
### check between pthreads and emscripten, see if it has to be seet here as well
endif

if host_os.startswith('linux')
   dep_main = cxx.find_library('dl', required: true)
   if not cxx.has_function('dlopen', dependencies: dep_main)
      error('libdl does not support dlopen')
   endif
endif

if host_os.startswith('darwin')
   dep_main = dependency('appleframeworks', modules : ['CoreFoundation'], required : true)
endif

#### need to find gles too as gl
if not get_variable('have_emscripten')
# still missing a few variables, check what config.h actually uses
   if cxx.has_header('GL/gl.h')
###   are we sure this adds compiler and linker flags?
      dep_gl = dependency('gl', required: true)
      set_variable('have_gl', true)
      set_variable('have_gl_gl_h', true)
   endif
   if cxx.has_header('OpenGL/gl.h')
###   doubt as for gl, also unsure about case
      dep_opengl = dependency('OpenGL', required: true)
      set_variable('have_gl', true)
      set_variable('have_opengl_gl_h', true)
   endif
endif

if not get_variable('have_emscripten')
   if not cxx.has_header('glm/glm.hpp')
      error('glm headers are required.')
   endif
endif

# create config.h
config_h = configuration_data()

# The original had a = between variable and value
config_h.set_quoted('DATADIR_PATH', pkgdatadir, description: 'Program data path')

if get_variable('have_emscripten')
   config_h.set10('EMSCRIPTEN', true, description: 'Define EMSCRIPTEN')
else
   config_h.set('EMSCRIPTEN', false, description: 'Define EMSCRIPTEN')
endif

if get_variable('have_aligned_alloc')
   config_h.set10('HAVE_ALIGNED_ALLOC', true, description: 'Define to 1 if you have the \`aligned_alloc\' function.')
else
   config_h.set('HAVE_ALIGNED_ALLOC', false, description: 'Define to 1 if you have the \`aligned_alloc\' function.')
endif

if get_variable('have_dlfcn_h')
   config_h.set10('HAVE_DLFCN_H', true, description: 'Define to 1 if you have the <dlfcn.h> header file.')
else
   config_h.set('HAVE_DLFCN_H', false, description: 'Define to 1 if you have the <dlfcn.h> header file.')
endif

if get_variable('have_gl', false)
   config_h.set10('HAVE_GL', true, description: 'Defined if a valid OpenGL implementation is found.')
else
   config_h.set('HAVE_GL', false, description: 'Defined if a valid OpenGL implementation is found.')
endif

if get_variable('have_gl_gl_h', false)
   config_h.set10('HAVE_GL_GL_H', true, description: 'Define to 1 if you have the <GL/gl.h> header file.')
else
   config_h.set('HAVE_GL_GL_H', false, description: 'Define to 1 if you have the <GL/gl.h> header file.')
endif

if get_variable('have_inttypes_h')
   config_h.set10('HAVE_INTTYPES_H', true, description: 'Define to 1 if you have the <inttypes.h> header file.')
else
   config_h.set('HAVE_INTTYPES_H', false, description: 'Define to 1 if you have the <inttypes.h> header file.')
endif

if get_variable('have_memory_h')
   config_h.set10('HAVE_MEMORY_H', true, description: 'Define to 1 if you have the <memory.h> header file.')
else
   config_h.set('HAVE_MEMORY_H', false, description: 'Define to 1 if you have the <memory.h> header file.')
endif

if get_variable('have_opengl_gl_h', false)
   config_h.set10('HAVE_OPENGL_GL_H', true, description: 'Define to 1 if you have the <OpenGL/gl.h> header file.')
else
   config_h.set('HAVE_OPENGL_GL_H', false, description: 'Define to 1 if you have the <OpenGL/gl.h> header file.')
endif

if get_variable('have_posix_memalign')
   config_h.set10('HAVE_POSIX_MEMALIGN', true, description: 'Define to 1 if you have the \`posix_memalign\' function.')
else
   config_h.set('HAVE_POSIX_MEMALIGN', false, description: 'Define to 1 if you have the \`posix_memalign\' function.')
endif

if get_variable('have_pthread_prio_inherit')
   config_h.set10('PTHREAD_PRIO_INHERIT', true, description: 'Have PTHREAD_PRIO_INHERIT.')
else
   config_h.set('PTHREAD_PRIO_INHERIT', false, description: 'Have PTHREAD_PRIO_INHERIT.')
endif

if get_variable('have_stdint_h')
   config_h.set10('HAVE_STDINT_H', true, description: 'Define to 1 if you have the <stdint.h> header file.')
else
   config_h.set('HAVE_STDINT_H', false, description: 'Define to 1 if you have the <stdint.h> header file.')
endif

if get_variable('have_stdlib_h')
   config_h.set10('HAVE_STDLIB_H', true, description: 'Define to 1 if you have the <stdlib.h> header file.')
else
   config_h.set('HAVE_STDLIB_H', false, description: 'Define to 1 if you have the <stdlib.h> header file.')
endif

if get_variable('have_strings_h')
   config_h.set10('HAVE_STRINGS_H', true, description: 'Define to 1 if you have the <strings.h> header file.')
else
   config_h.set('HAVE_STRINGS_H', false, description: 'Define to 1 if you have the <strings.h> header file.')
endif

if get_variable('have_string_h')
   config_h.set10('HAVE_STRING_H', true, description: 'Define to 1 if you have the <string.h> header file.')
else
   config_h.set('HAVE_STRING_H', false, description: 'Define to 1 if you have the <string.h> header file.')
endif

if get_variable('have_sys_stat_h')
   config_h.set10('HAVE_SYS_STAT_H', true, description: 'Define to 1 if you have the <sys/stat.h> header file.')
else
   config_h.set('HAVE_SYS_STAT_H', false, description: 'Define to 1 if you have the <sys/stat.h> header file.')
endif

if get_variable('have_sys_types_h')
   config_h.set10('HAVE_SYS_TYPES_H', true, description: 'Define to 1 if you have the <sys/types.h> header file.')
else
   config_h.set('HAVE_SYS_TYPES_H', false, description: 'Define to 1 if you have the <sys/types.h> header file.')
endif

if get_variable('have_unistd_h')
   config_h.set10('HAVE_UNISTD_H', true, description: 'Define to 1 if you have the <unistd.h> header file.')
else
   config_h.set('HAVE_UNISTD_H', false, description: 'Define to 1 if you have the <unistd.h> header file.')
endif

if get_variable('have_windows_h')
   config_h.set10('HAVE_WINDOWS_H', true, description: 'Define to 1 if you have the <windows.h> header file.')
else
   config_h.set('HAVE_WINDOWS_H', false, description: 'Define to 1 if you have the <windows.h> header file.')
endif

# This doesn't appear to be used at all
config_h.set_quoted('LT_OBJDIR', '.libs', description: 'Define to the sub-directory where libtool stores uninstalled libraries.')

config_h.set_quoted('PACKAGE', meson.project_name(), description: 'Name of package')
config_h.set_quoted('PACKAGE_BUGREPORT', 'mischa@mvstg.biz', description: 'Define to the address where bug reports for this package should be sent.')
config_h.set_quoted('PACKAGE_NAME', meson.project_name(), description: 'Define to the full name of this package.')
config_h.set_quoted('PACKAGE_STRING', meson.project_name()+' '+ meson.project_version(), description: 'Define to the full name and version of this package.')
config_h.set_quoted('PACKAGE_TARNAME', meson.project_name(), description: 'Define to the one symbol short name of this package.')
config_h.set_quoted('PACKAGE_URL', 'https://github.com/revmischa/projectm', description: 'Define to the home page for this package.')
config_h.set_quoted('PACKAGE_VERSION', meson.project_version(), description: 'Define to the version of this package.')

if get_variable('have_pthread_create_joinable')
   config_h.set10('PTHREAD_CREATE_JOINABLE', true, description: 'Define to necessary symbol if this constant uses a non-standard name on\n   your system')
else
   config_h.set('PTHREAD_CREATE_JOINABLE', false, description: 'Define to necessary symbol if this constant uses a non-standard name on\n   your system')
endif

# The original had a = between variable and value
config_h.set_quoted('PROJECTM_PREFIX', get_option('prefix'), description: 'Program prefix')

# The original had a = between variable and value
config_h.set_quoted('RESOURCE_PREFIX', join_paths(get_option('datadir'), meson.project_name()), description: 'Resource prefix')

# the test is perhaps weak, see if you can do anything about it
if get_variable('have_stdc_headers')
   config_h.set10('STDC_HEADERS', true, description: 'Define to 1 if you have the ANSI C header files.')
else
   config_h.set('STDC_HEADERS', false, description: 'Define to 1 if you have the ANSI C header files.')
endif

# The original had a = between variable and value
config_h.set_quoted('SYSCONFDIR', get_option('sysconfdir'), description: 'Program config path')

### see above how to add variables and stuff
# is this supposed to be set to 1?
if get_variable('enable_gles')
   config_h.set10('USE_GLES', true, description: 'Define USE_GLES')
else
   config_h.set('USE_GLES', false, description: 'Define USE_GLES')
endif

### see above how to add variables and stuff
# is this supposed to be set to 1?
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
'cflags:                 ${CFLAGS}\n'+
'ldflags:                '+'${LDFLAGS}\n\n'+
'- - -\n\n'+
'Applications:\n'+
'=====\n\n'+
'libprojectM:            yes\n'+
'Threading:              '+get_variable('enable_threading').to_string('yes', 'no')+'\n'+
'SDL:                    '+get_variable('enable_sdl').to_string('yes', 'no')+'\n'+
'Qt & Pulseaudio:        '+get_variable('enable_qt').to_string('yes', 'no')+'\n'+
'OpenGLES:               '+get_variable('enable_gles').to_string('yes', 'no')+'\n'+
'Emscripten:             '+get_variable('have_emscripten').to_string('yes', 'no')+'\n'
)
