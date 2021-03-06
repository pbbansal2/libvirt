node_device_driver_sources = [
  'node_device_driver.c',
]

stateful_driver_source_files += files(node_device_driver_sources)

if conf.has('WITH_HAL')
  node_device_driver_sources += 'node_device_hal.c'
endif

if conf.has('WITH_UDEV')
  node_device_driver_sources += 'node_device_udev.c'
endif

driver_source_files += files(node_device_driver_sources)

if conf.has('WITH_NODE_DEVICES')
  node_device_driver_impl = static_library(
    'virt_driver_nodedev_impl',
    [
      node_device_driver_sources,
    ],
    dependencies: [
      access_dep,
      hal_dep,
      libnl_dep,
      pciaccess_dep,
      src_dep,
      udev_dep,
    ],
    include_directories: [
      conf_inc_dir,
    ],
  )

  virt_modules += {
    'name': 'virt_driver_nodedev',
    'link_whole': [
      node_device_driver_impl,
    ],
    'link_args': [
      libvirt_no_undefined,
    ],
  }

  virt_daemons += {
    'name': 'virtnodedevd',
    'c_args': [
      '-DDAEMON_NAME="virtnodedevd"',
      '-DMODULE_NAME="nodedev"',
    ],
  }

  virt_daemon_confs += {
    'name': 'virtnodedevd',
  }

  virt_daemon_units += {
    'service': 'virtnodedevd',
    'service_in': files('virtnodedevd.service.in'),
    'name': 'Libvirt nodedev',
    'sockprefix': 'virtnodedevd',
    'sockets': [ 'main', 'ro', 'admin' ],
    'conflicts': true,
  }

  openrc_init_files += {
    'name': 'virtnodedevd',
    'in_file': files('virtnodedevd.init.in'),
  }

  sysconf_files += {
    'name': 'virtnodedevd',
    'file': files('virtnodedevd.sysconf'),
  }
endif
