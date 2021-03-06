pipewire_daemon_sources = [
  'main.c',
]

pipewire_c_args = [
  '-DHAVE_CONFIG_H',
  '-D_GNU_SOURCE',
  '-DG_LOG_DOMAIN=g_log_domain_pipewire',
]

conf_config = configuration_data()
conf_config.set('VERSION', '"@0@"'.format(pipewire_version))
conf_config.set('media_session_path', join_paths(pipewire_bindir, 'pipewire-media-session'))
conf_config.set('pipewire_path', join_paths(pipewire_bindir, 'pipewire'))
conf_config.set('pipewire_pulse_path', join_paths(pipewire_bindir, 'pipewire-pulse'))
conf_install_dir = join_paths(get_option('sysconfdir'), 'pipewire')

conf_config_uninstalled = conf_config
conf_config_uninstalled.set('media_session_path',
  join_paths(meson.build_root(), 'src', 'examples', 'pipewire-media-session'))
conf_config_uninstalled.set('pipewire_path',
  join_paths(meson.build_root(), 'src', 'daemon', 'pipewire'))
conf_config_uninstalled.set('pipewire_pulse_path',
  join_paths(meson.build_root(), 'src', 'daemon', 'pipewire-pulse'))


conf_files = [
  [ 'pipewire.conf.in', 'pipewire.conf' ],
  [ 'client.conf.in', 'client.conf' ],
  [ 'client-rt.conf.in', 'client-rt.conf' ],
  [ 'jack.conf.in', 'jack.conf' ],
  [ 'pipewire-pulse.conf.in', 'pipewire-pulse.conf' ]
]

foreach c : conf_files
  configure_file(input : c.get(0),
                 output : c.get(1),
                 configuration : conf_config,
                 install_dir : conf_install_dir)
endforeach

configure_file(input : 'pipewire.conf.in',
               output : 'pipewire-uninstalled.conf',
               configuration : conf_config_uninstalled)

executable('pipewire',
  pipewire_daemon_sources,
  install: true,
  c_args : pipewire_c_args,
  include_directories : [configinc, spa_inc],
  dependencies : [pipewire_dep, ],
)

executable('pipewire-pulse',
  'pipewire-pulse.c',
  install: true,
  c_args : pipewire_c_args,
  include_directories : [configinc, spa_inc],
  dependencies : [pipewire_dep, ],
)

#desktop_file = i18n.merge_file(
#  input : 'pipewire.desktop.in',
#  output : 'pipewire.desktop',
#  po_dir : po_dir,
#  type : 'desktop',
#  install : true,
#  install_dir : join_paths(pipewire_sysconfdir, 'xdg', 'autostart')
#)
#
#desktop_utils = find_program('desktop-file-validate', required: false)
#if desktop_utils.found()
#  test('Validate desktop file', desktop_utils,
#    args: [ desktop_file ],
#  )
#endif

subdir('media-session.d')
if get_option('systemd') and systemd.found()
  subdir('systemd')
endif
