projectM_pulseaudio_moc_headers = [
   'QPulseAudioDeviceChooser.hpp',
   'QPulseAudioDeviceModel.hpp',
   'QPulseAudioThread.hpp'
]

projectM_pulseaudio_uic_headers = [
   'PulseDeviceChooserDialog.ui'
]

projectM_pulseaudio_sources = [
   'qprojectM-pulseaudio.cpp',
   'QPulseAudioDeviceChooser.cpp',
   'QPulseAudioDeviceModel.cpp',
   'QPulseAudioThread.cpp'
]

projectM_pulseaudio_files = qt5.preprocess(
   moc_headers: projectM_pulseaudio_moc_headers,
   ui_files: projectM_pulseaudio_uic_headers,
   dependencies: dep_qt5
)

projectM_pulseaudio = executable(
   'projectM-pulseaudio',
   sources: [
      projectM_pulseaudio_sources,
      projectM_pulseaudio_files
   ],
   c_args: [
   '-fvisibility=hidden',
   '-ffunction-sections',
   'fdata-sections'
   ],
   include_directories: [
      include_directories('../libprojectM'),
      include_directories('../libprojectM/Renderer'),
      include_directories('../projectM-qt'),
      include_directories('../..')
   ],
   dependencies: [
      dep_pulse,
      dep_qt5
   ],
   link_with: [
      libprojectM_qt,
      libprojectM
   ],
   pie: true,
   install: true
)

install_data('projectM-pulseaudio.desktop', install_dir: join_paths(get_option('datadir'), 'applications'))
install_data('../projectM-qt/images/icons/prjm16-transparent.svg', install_dir: join_paths(get_option('datadir'), 'pixmaps'))
install_man('projectM-pulseaudio.1')
