project('homeworld',
        'c',
        version: '0.1',
        default_options: ['warning_level=3', 'c_std=c17'])

dep_x11 = dependency('x11')
dep_gl = dependency('gl')
dep_sdl = dependency('sdl2')
dep_glew = dependency('glew')
dep_math = meson.get_compiler('c').find_library('m', required: false)

c_base_args = ['-DHW_GAME_HOMEWORLD', '-DHAVE_CONFIG_H', '-D_REENTRANT', '-DGENERIC_ETGCALLFUNCTION']
#c_base_args += '-Wno-unused-variable'
#c_base_args += '-Wno-unused-parameter'
# build is noisy due to having so many damn warnings, lets just hide them for now...
c_base_args += '-w'

if build_machine.cpu_family() == 'x86_64'
	c_base_args += ['-D_X86_64', '-D_X86_64_FIX_ME']
elif build_machine.cpu_family() == 'x86'
	c_base_args += ['-malign-double', '-D_X86', '-msse']
elif build_machine.cpu_family() in ['arm', 'aarch64']
	c_base_args += ['-DARM']
endif

if build_machine.system() == 'linux'
	c_base_args += ['-D_LINUX_FIX_ME', '-D_GNU_SOURCE']
elif build_machine.system() == 'darwin'
	c_base_args += ['-D__APPLE___FIX_ANIM', '-D__APPLE___FIX_LAN', '-D__APPLE___FIX_MISC']

	if build_machine.cpu_family() == 'x86_64'
		c_base_args += ['-D__APPLE___64', '-D__APPLE___FIX_64']
	elif build_machine.cpu_family() == 'x86'
		c_base_args += ['-D__APPLE___86', '-D__APPLE___FIX_86']
	endif
elif build_machine.system() == 'windows'

endif

if get_option('debug')
	c_args = c_base_args + '-DHW_BUILD_FOR_DEBUGGING'
else
	c_args = c_base_args + '-DHW_BUILD_FOR_DISTRIBUTION'
endif

subdir('tools')
subdir('src')
subdir('tools/biggie')
subdir('tools/monochrome-btg')
subdir('assets')

if build_machine.cpu_family() == 'x86_64'
	subdir('tools/x86_64')
endif

alias_target('tools', exe_tools)
alias_target('missions', missions)
alias_target('assets', assets)

executable(meson.project_name(),
           src, game_wrapped_functions,
           include_directories: inc_src,
           link_with: [lib_jpg, lib_sdl],
           dependencies: [dep_x11, dep_gl, dep_sdl, dep_math, dep_glew],
           c_args: c_args,
           install: true)
