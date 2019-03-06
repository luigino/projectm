dep_jack = dependency('jack', required: true)
#dep_qxt = dependency('qxt', required: false)

# TODO: build qprojectM-jack and projectM-jack as separate programs

projectM_jack_sources = [
   'qprojectM-jack.cpp',
   'ConfigFile.cpp'
]

# TODO: add remote control if we have qxt network lib
# it appears to be qt4-only
#### if QT_NETWORK_ENABLED
###if dep_qxt
###   projectM_jack_sources += [
###      'HTTPRemoteControl.cpp'
###   ]
###endif


###bin_PROGRAMS = projectM_jack

projectM_jack = executable(
   'projectM-jack',
   sources: projectM_jack_sources,
   include_directories: [
      include_directories('../libprojectM'),
      include_directories('../libprojectM/Renderer'),
      include_directories('../projectM-qt')
   ],
   dependencies: [
      dep_jack,
      dep_qt5
   ],
   link_with: [
      libprojectM_qt,
      libprojectM
   ],
   pie: true,
   install: true
)
### add sdl as link or dep, see how the logic is set

###projectM_jack_LDADD = \
###  -lSDL

install_man('projectM-jack.1')
