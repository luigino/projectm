add_languages(['objc', 'objcpp'])

dep_platform += dependency('appleframeworks', modules: ['CoreGraphics', 'AppKit', 'Cocoa', 'AVFoundation'], required: true)

iProjectM_sources = [
   'getConfigFilename.cpp',
   'getConfigFilename.h',
   'iprojectM_mac.mm',
   'iprojectM.hpp',
   'iprojectM.mm',
   'macos/iTunesAPI.cpp',
   'macos/iTunesAPI.h',
   'macos/iTunesVisualAPI.h',
   'projectM Visualizer/AppDelegate.h',
   'projectM Visualizer/AppDelegate.m',
   'projectM Visualizer/main.m',
   'projectM Visualizer/Preferences.h',
   'projectM Visualizer/Preferences.m',
   'projectM Visualizer/AudioInputDeviceList.h',
   'projectM Visualizer/AudioInputDeviceList.m'
]

iProjectM = executable(
   'iProjectM',
   sources: iProjectM_sources,
   include_directories: [
      include_directories('macos'),
      include_directories('..'),
   ],
   dependencies: [
      dep_gl,
      dep_platform
   ],
   link_with: libprojectM
)
