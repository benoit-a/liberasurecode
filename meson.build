project('erasurecode', 'c', default_options : ['buildtype=debugoptimized', 'c_std=c11', 'optimization=2', 'werror=true'])

ec_inc = [include_directories('include/erasurecode', 'include/isa_l', 'include/rs_vand', 'include/xor_codes')]
# ec_files = [files('get_flags_from_cpuid.c')]
ec_files = []

c_compiler = meson.get_compiler('c')

flags = [['-mmmx','-DINTEL_MMX'],
         ['-msse', '-DINTEL_SSE'],
         ['-msse2','-DINTEL_SSE2'],['-msse3','-DINTEL_SSE3'],['-mssse3','-DINTEL_SSSE3'],['-msse4.1','-DINTEL_SSE41'],
         ['-msse4.2','-DINTEL_SSE42'],['-mavx','-DINTEL_AVX']]

optim_flags = []

foreach f : flags
  if c_compiler.has_argument(f[0])
    optim_flags += f
  endif
endforeach

conf_data = configuration_data()
if c_compiler.has_function('calloc')
conf_data.set('HAVE_CALLOC', 1)
endif

if c_compiler.has_header('ctype.h')
  conf_data.set('HAVE_CTYPE_H', '1')
endif

conf_data.set('HAVE_DLFCN_H', '1')
conf_data.set('HAVE_ERRNO_H', '1')
conf_data.set('HAVE_FREE', '1')
conf_data.set('HAVE_ICONV_H', '1')
conf_data.set('HAVE_INTTYPES_H', '1')
conf_data.set('HAVE_LIMITS_H', '1')
conf_data.set('HAVE_MALLOC', '1')
conf_data.set('HAVE_MEMORY_H', '1')
conf_data.set('HAVE_OPENLOG', '1')
conf_data.set('HAVE_PTHREAD_H', '1')
conf_data.set('HAVE_SIGNAL_H', '1')
conf_data.set('HAVE_STDARG_H', '1')
conf_data.set('HAVE_STDDEF_H', '1')
conf_data.set('HAVE_STDINT_H', '1')
conf_data.set('HAVE_STDIO_H', '1')
conf_data.set('HAVE_REALLOC', '1')
conf_data.set('HAVE_STRINGS_H', '1')
conf_data.set('HAVE_STRING_H', '1')
conf_data.set('HAVE_UNISTD_H', '1')
conf_data.set('HAVE_STDLIB_H', '1')
conf_data.set('HAVE_SYSLOG_H', '1')
conf_data.set('HAVE_SYS_STAT_H', '1')
conf_data.set('HAVE_SYS_TYPES_H', '1')
conf_data.set('STDC_HEADERS', '1')
conf_data.set('_ALL_SOURCE', '1')
conf_data.set('_POSIX_PTHREAD_SEMANTICS', '1')
conf_data.set('_TANDEM_SOURCE', '1')
conf_data.set('__EXTENSIONS__', '1')
conf_data.set_quoted('PACKAGE', 'liberasurecode')
conf_data.set_quoted('PACKAGE_BUGREPORT', 'tusharsg AT gmail DOT com, kmgreen2 AT gmail DOT com')
conf_data.set_quoted('PACKAGE_NAME', 'liberasurecode')
conf_data.set_quoted('PACKAGE_STRING', 'liberasurecode -')
conf_data.set_quoted('PACKAGE_TARNAME', 'liberasurecode')
conf_data.set_quoted('PACKAGE_URL', 'https://github.com/openstack/liberasurecode')
conf_data.set('SIZEOF_LONG', '8')
conf_data.set('_GNU_SOURCE', '1')
conf_data.set_quoted('VERSION', '-')
conf_data.set_quoted('PACKAGE_VERSION', '-')

inc = configure_file(input : 'include/config_liberasurecode.h.in',
               output : 'config_liberasurecode.h',
               configuration : conf_data)

ec_inc += [include_directories('.')]

subdir('src')

zlib_dep = c_compiler.find_library('z')
m_dep = c_compiler.find_library('m')
dl_dep = c_compiler.find_library('dl')
threads_dep = dependency('threads')
ec_deps = [zlib_dep, m_dep, dl_dep, threads_dep]

c_args = ['-D_GNU_SOURCE=1', '-pedantic']
c_args += optim_flags

ec_lib = library('erasurecode', ec_files + [inc], include_directories: ec_inc, dependencies: ec_deps, c_args: c_args)

ec_dep = declare_dependency(link_with : ec_lib, dependencies: ec_deps,
  include_directories : ec_inc)

liberasurecode_test = executable('liberasurecode_test', files('test/liberasurecode_test.c'), dependencies: [ec_dep])

test('liberasurecode_test', liberasurecode_test)
