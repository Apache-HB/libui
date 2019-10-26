# 17 march 2019
project('libui', ['c', 'cpp'],
	meson_version : '>=0.48.0',
	default_options : [
		'c_std=c99',					# strict C99
		'cpp_std=c++11',				# strict C++11
		'cpp_eh=none'					# shut the compiler up in some cases
	],
	license : 'MIT'
)

args = []
links = []

if host_machine.system() == 'darwin'
	add_languages('objc', required : true)
	args += '-mmacosx-version-min=10.8'
	links += '-mmacosx-version-min=10.8'
endif

if meson.get_compiler('c').get_id() == 'msvc'
	args += [
		'/wd4100',
		'/bigobj'
	]

	links += [
		'/LARGEADDRESSAWARE',
		'/INCREMENTAL:NO',
		'/MANIFEST:NO'
	]
endif

args += '-Dlibui_EXPORTS'

sources = []
deps = []

subdir('common')
if host_machine.system()== 'windows'
	subdir('windows')
elif host_machine.system() == 'darwin'
	subdir('darwin')
else
	subdir('unix')
endif

incdir = include_directories('.')

libui = static_library('ui', sources,
	dependencies : deps,
	c_args : args,
	cpp_args : args,
	objc_args : args,
	link_args : links,
	install : true
)

libui_dep = declare_dependency(
	link_with : libui,
	include_directories : incdir
)

pkg_mod = import('pkgconfig')

pkg_mod.generate(libui,
	version : '0.0.1',
	name : 'libui',
	filebase : 'ui',
	description : 'C crossplatform ui'
)

subdir('test')
subdir('examples')