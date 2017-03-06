def merge_dicts(x, y): 
  z = x.copy()
  z.update(y)
  return z

genrule(
  name = 'configure', 
  out = 'out',
  srcs = glob([
    'README',
    'install-sh',
    'config.guess',
    'config.sub',
    'missing',
    'Makefile.in',
    'ltmain.sh',
    'libglog.pc.in',
    'src/config.h.in',
    'src/glog/logging.h.in',
    'src/glog/raw_logging.h.in',
    'src/glog/stl_logging.h.in',
    'src/glog/vlog_is_on.h.in',
    'configure',
  ]),
  cmd = 'cp -r $SRCDIR $OUT && cd $OUT && ./configure',
)

genrule(
  name = 'config.h', 
  out = 'config.h',
  cmd = 'cp $(location :configure)/src/config.h $OUT',
)

genrule(
  name = 'logging.h', 
  out = 'logging.h',
  cmd = 'cp $(location :configure)/src/glog/logging.h $OUT',
)

genrule(
  name = 'vlog_is_on.h', 
  out = 'vlog_is_on.h',
  cmd = 'cp $(location :configure)/src/glog/vlog_is_on.h $OUT',
)

genrule(
  name = 'raw_logging.h', 
  out = 'raw_logging.h',
  cmd = 'cp $(location :configure)/src/glog/raw_logging.h $OUT',
)

cxx_library(
  name = 'glog',
  header_namespace = '',
  exported_headers = merge_dicts({
    'glog/logging.h': ':logging.h',
    'glog/vlog_is_on.h': ':vlog_is_on.h',
    'glog/raw_logging.h': ':raw_logging.h',
  },
  subdir_glob([
    ('src', 'glog/**/*.h'),
  ])),
  headers = {
    'config.h': ':config.h',
  },
  srcs = [
    'src/demangle.cc',
    'src/raw_logging.cc',
    'src/signalhandler.cc',
    'src/symbolize.cc',
    'src/logging.cc',
    'src/utilities.cc',
    'src/vlog_is_on.cc',
  ],
  visibility = [
    'PUBLIC',
  ],
)
