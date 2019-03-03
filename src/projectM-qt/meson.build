dep_pulse = dependency('libpulse', required: true)

projectM_qt_moc_headers = [
   'qprojectm_mainwindow.hpp',
   'qpresetfiledialog.hpp',
   'qplaylistfiledialog.hpp',
   'qplaylistmodel.hpp',
   'qprojectmconfigdialog.hpp',
   'qplaylisttableview.hpp',
   'qpresettextedit.hpp',
   'qpreseteditordialog.hpp',
   'qprojectm.hpp',
   'qprojectmwidget.hpp'
]

projectM_qt_uic_headers = [
   'qprojectm_mainwindow.ui',
   'qpreseteditordialog.ui',
   'qprojectmconfigdialog.ui'
]

projectM_qt_qrc_headers = 'application.qrc'

libprojectM_qt_sources = [
   'qprojectm_mainwindow.cpp',
   'configfile.hpp',
   'configfile.cpp',
   'qplaylistfiledialog.cpp',
   'qplaylistmodel.cpp',
   'qprojectmconfigdialog.cpp',
   'qpresettextedit.cpp',
   'qpreseteditordialog.cpp',
   'qxmlplaylisthandler.hpp',
   'nullable.hpp'
]

qt5 = import('qt5')
dep_qt5 = dependency('qt5', modules: ['Core', 'Gui', 'Widgets', 'OpenGL'])

libprojectM_qt_files = qt5.preprocess(
   moc_headers: projectM_qt_moc_headers,
   ui_files: projectM_qt_uic_headers,
   qresources: projectM_qt_qrc_headers,
   dependencies: dep_qt5
)

libprojectM_qt = static_library(
   'projectM-qt',
   sources: [
      libprojectM_qt_sources,
      libprojectM_qt_files
  ], 
  c_args: [
      '-fvisibility=hidden',
      '-ffunction-sections',
      '-fdata-sections'
   ],
   include_directories: [
      include_directories('../libprojectM'),
      include_directories('../libprojectM/Renderer'),
   ],
   dependencies: [
      dep_gl,
      dep_qt5
   ],
   link_with: libprojectM,
   pic: true
)
