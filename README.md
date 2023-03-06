# errata2osv

## About

errata2osv is a tool to convert Red Hat Errata advisories to Open Source Vulnerability (OSV)
database format.

## Installation

1. Install requirements from Pipfile.
2. Configure ecosystem names in settings.py

## Configuration

In order to map errata collection names to OSV ecosystems, one must fill in
`collection_name_to_ecosystem` dictionary in settings.py. For example:

```python
collection_name_to_ecosystem = {
    # Almalinux 8 collection names
    r'almalinux-8-for-x86_64-appstream-rpms__8_0_default': 'Almalinux:8',
    r'almalinux-8-for-x86_64-appstream-rpms__8_1_virt': 'Almalinux:8',
    r'almalinux-8-for-x86_64-appstream-rpms__8_1_php': 'Almalinux:8',
    # Almalinux 9 collection names
    r'almalinux-9-for-x86_64-appstream-rpms__9_1_default': 'Almalinux:9',
    r'almalinux-9-for-x86_64-appstream-rpms__9_default': 'Almalinux:9',
}
```

Collection names are matched to regex pattern, so it is possible to match multiple
collections to the same ecosystem:

```python
collection_name_to_ecosystem = {
    # Almalinux 8 collection names
    r'almalinux-8.*': 'Almalinux:8',
    # Almalinux 9 collection names
    r'almalinux-9.*': 'Almalinux:9',
}
```

## Usage

General help:

```
usage: errata2osv.py [-h] [--ecosystem ECOSYSTEM] updateinfo target_dir

errata to osv converter

positional arguments:
  updateinfo            updateinfo xml with errata information
  target_dir            target directory for output OSV entries

options:
  -h, --help            show this help message and exit
  --ecosystem ECOSYSTEM
                        OSV ecosystem of output OSV entries
```

`errata2osv` accepts updateinfo.xml file and produces OSV entries in
target dir: one for each `<update>` tag in updateinfo.xml. By default, ecosystem name for
each update in deduced from collection names in `<pkglist>` tag. If `--ecosystem` option is
specified, it is used for all updates.