project('mp3chop', 'cpp', version: '2.2')

cc = meson.get_compiler('cpp')
cdata = configuration_data()
check_headers = [
  ['HAVE_FCNTL_H', 'fcntl.h'],
  ['HAVE_UNISTD_H', 'unistd.h'],
]

foreach h : check_headers
  if cc.has_header(h.get(1))
    cdata.set(h.get(0), 1)
  endif
endforeach

check_functions = [
  ['HAVE_GETOPT_LONG', 'getopt_long', '#include<getopt.h>'],
  ['HAVE_GETOPT', 'getopt', '#include<getopt.h>'],
]

foreach f : check_functions
  if cc.has_function(f.get(1), prefix : f.get(2))
    cdata.set(f.get(0), 1)
  endif
endforeach

cdata.set('MP3CHOP_VERSION', meson.project_version())

configure_file(input : 'config.h.meson',
               output : 'config.h',
               configuration : cdata)

sources = [
  'header.cpp',
  'mp3_timecode.cpp',
  'buffer.cpp',
  'file_data_source.cpp',
  'file_data_sink.cpp',
  'filter.cpp',
  'mp3_processor.cpp',
  'chop.cpp',
  'xing_frame.cpp'
]

libchop = static_library('chop', sources)

mp3chop_exe = executable('mp3chop', 'main.cpp',
                         link_with: libchop,
                         install: true)

catch2_with_main_dep = dependency('catch2-with-main', fallback : [ 'catch2', 'catch2_with_main_dep' ], required : false)
if catch2_with_main_dep.found()
  input_buffer_test = executable('input_buffer_test',
                                 'input_buffer_test.cpp',
                                 dependencies: catch2_with_main_dep,
                                 link_with: [ libchop ])
  test('input_buffer_test', input_buffer_test)

  output_buffer_test = executable('output_buffer_test',
                                  'output_buffer_test.cpp',
                                  dependencies: catch2_with_main_dep,
                                  link_with: [ libchop ])
  test('output_buffer_test', output_buffer_test)
endif

subdir('approval-tests')

install_man('mp3chop.1')
