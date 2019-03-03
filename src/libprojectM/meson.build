libprojectM_version = '3.1.1'
subdir('Renderer')
subdir('NativePresetFactory')
subdir('MilkdropPresetFactory')

libprojectM_sources = [
   'ConfigFile.cpp',
   'Preset.cpp',
   'PresetLoader.cpp',
   'timer.cpp',
   'KeyHandler.cpp',
   'PresetChooser.cpp',
   'TimeKeeper.cpp',
   'PCM.cpp',
   'PresetFactory.cpp',
   'fftsg.cpp',
   'wipemalloc.cpp',
   'PipelineMerger.cpp',
   'PresetFactoryManager.cpp',
   'projectM.cpp',
   'TestRunner.cpp',
   'TestRunner.hpp',
   'Common.hpp',
   'PipelineMerger.hpp',
   'PresetLoader.hpp',
   'HungarianMethod.hpp',
   'Preset.hpp',
   'RandomNumberGenerators.hpp',
   'IdleTextures.hpp',
   'PresetChooser.hpp',
   'TimeKeeper.hpp',
   'KeyHandler.hpp',
   'PresetFactory.hpp',
   'projectM.hpp',
   'BackgroundWorker.h',
   'PCM.hpp',
   'PresetFactoryManager.hpp',
   'projectM.hpp',
   'projectM-opengl.h',
   'ConfigFile.h',
   'carbontoprojectM.h',
   'cocoatoprojectM.h',
   'lvtoprojectM.h',
   'dlldefs.h',
   'resource.h',
   'event.h',
   'sdltoprojectM.h',
   'fatal.h',
   'timer.h',
   'fftsg.h',
   'win32-dirent.h',
   'glError.h',
   'wipemalloc.h'
]
### so far these don't appear to be used
###   'omptl/omptl_numeric_extentions_ser.hpp',
###   'omptl/omptl_algorithm_par.hpp',
###   'omptl/omptl_numeric_par.hpp',
###   'omptl/omptl_algorithm_ser.hpp',
###   'omptl/omptl_numeric_ser.hpp',
###   'omptl/omptl_numeric_extentions.hpp',
###   'omptl/omptl_tools.hpp',
###   'omptl/omptl_numeric_extentions_par.hpp',
###   'omptl/omptl.cpp',
###   'omptl/omptl_numeric.cpp',
###   'omptl/omptl_algorithm.cpp'
###]

# omitting vendor because it doesn't seem to do any good (perhaps used by Windows?)
libprojectM_inc = [
  include_directories('Renderer'),
  include_directories('.')
]

# using library() instead of shared_library() so that each platform can decide what's best
libprojectM = library(
   'projectM',
   version: libprojectM_version,
   sources: libprojectM_sources,
   include_directories: libprojectM_inc,
   link_with: [
      libMilkdropPresetFactory,
      libNativePresetFactory,
      libRenderer
   ],
   dependencies: [
      dep_main,
      dep_gl,
      dep_pthread
   ],
   install: true
)

install_headers(
   'Common.hpp',
   'PCM.hpp',
   'dlldefs.h',
   'event.h',
   'fatal.h',
   'projectM.hpp',
   subdir: 'libprojectM'
)

### dep_gl is to be seen, perhaps it has to be global
### find a way to get dep_opengl as well, see how to overcome it being undefined

# if you want to pass it via the command line, either do  
# add_project_arguments('-DDATADIR_PATH="@0@"'.format(pkgdatadir), language: 'c')
# or  c_args:  ['-DDATADIR_PATH="@0@"'.format(pkgdatadir)]

# create config.inp
config_inp = configuration_data()

config_inp.set('DATADIR', join_paths(get_option('prefix'), get_option('datadir')))
config_inp.set('PACKAGE', meson.project_name())

configure_file(input: 'config.inp.meson',
               output: 'config.inp',
               configuration: config_inp,
               install_dir: get_option('datadir')
               )

# create libprojectM.pc
config_pkgconfig = configuration_data()
config_pkgconfig.set('prefix', get_option('prefix'))
config_pkgconfig.set('exec_prefix', get_option('prefix'))
config_pkgconfig.set('libdir', join_paths(get_option('prefix'), get_option('libdir')))
config_pkgconfig.set('includedir', join_paths(get_option('prefix'), get_option('includedir')))
config_pkgconfig.set('datarootdir', join_paths(get_option('prefix'), get_option('datadir')))
config_pkgconfig.set('PACKAGE_NAME', meson.project_name())
config_pkgconfig.set('PACKAGE_VERSION', libprojectM_version)

configure_file(input: 'libprojectM.pc.in',
               output: 'libprojectM.pc',
               configuration: config_pkgconfig,
               install_dir: join_paths(get_option('libdir'), 'pkgconfig')
               )
