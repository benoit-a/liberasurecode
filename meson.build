project('erasurecode', 'c', default_options : ['buildtype=debugoptimized', 'c_std=c17', 'optimization=2', 'werror=true'])

ec_inc = include_directories('include/erasurecode', 'include/isa_l', 'include/rs_vand', 'include/xor_codes')
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

subdir('src')

zlib_dep = c_compiler.find_library('z')
m_dep = c_compiler.find_library('m')
dl_dep = c_compiler.find_library('dl')

ec_deps = [zlib_dep, m_dep, dl_dep]

c_args = ['-D_GNU_SOURCE=1', '-pedantic']
c_args += optim_flags

ec_lib = library('erasurecode', ec_files, include_directories: ec_inc, dependencies: ec_deps, c_args: c_args)

ec_dep = declare_dependency(link_with : ec_lib, dependencies: ec_deps,
  include_directories : ec_inc)

liberasurecode_test = executable('liberasurecode_test', files('test/liberasurecode_test.c'), dependencies: [ec_dep])

test('liberasurecode_test', liberasurecode_test)
