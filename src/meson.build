subdir('libprojectM')

if get_variable('enable_jack')
   subdir('projectM-jack')
endif

if get_variable('enable_sdl')
   subdir('projectM-sdl')
   subdir('projectM-test')
endif

if get_variable('enable_qt')
   subdir('projectM-qt')
   subdir('projectM-pulseaudio')
endif

if get_variable('have_emscripten')
   subdir('projectM-emscripten')
endif

