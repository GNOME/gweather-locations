xmllint = find_program('xmllint', required: false)

if xmllint.found()
  test('Valid Locations file',
    xmllint,
    args: [
      '--path', '@0@'.format(meson.current_source_dir()),
      '--noout',
      '--valid',
      'Locations.xml',
    ],
    suite: ['lint'],
  )
endif

nomalloc = environment({'MALLOC_PERTURB_': '0'})

pylint = find_program('pylint-3', 'pylint3', 'pylint', required: false)
pylint_flags = [
  '-d', 'C0111',
  '-d', 'W0511',
  '-d', 'F0401',
  '-d', 'C0326',
]

if pylint.found()
  test('pylint-gen-locations-variant',
    pylint,
    args: [
      pylint_flags,
      gen_locations_variant.full_path(),
    ],
    env: nomalloc,
    suite: ['lint'],
  )
endif

# The generated file is architecture dependent (i.e. host endianness)
locations_bin = custom_target('locations-db',
  command: [gen_locations_variant, '@INPUT@', '@OUTPUT@' ],
  input: files('Locations.xml'),
  output: '@BASENAME@.bin',
  install: true,
  install_dir: get_option('libdir') / 'gweather-locations',
)

locations_its = meson.current_source_dir() / 'locations.its'

install_data([
    'Locations.xml',
    'locations.dtd',
  ],
  install_dir: get_option('datadir') / 'gweather-locations',
)
