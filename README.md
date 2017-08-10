# systemd-udevd

This role configures systemd-udevd and creates hwdb entries and rules

## Requirements

systemd with udevd compiled in

## Role Variables

| Name                 | Default/Required | Description                                                  |
|----------------------|:----------------:|--------------------------------------------------------------|
| `udevd_log_level`    | `info`           | udevd log level to set                                       |
| `udevd_hwdb_entries` |                  | Array of custom hwdb entries, see below for more information |
| `udevd_rules`        |                  | Array of custom rules, see below for more information        |

#### Custom hwdb entries

Each hwdb entry consists of any number of matches and any number of properties to set if a device matches.
Both values can either be a string, or a list of strings.
They must not be surrounded by any whitespace, indentation is automatically done by this role.

| Name         | Default/Required | Description                           |
|--------------|:----------------:|---------------------------------------|
| `matches`    |                  | String to match or list of strings    |
| `properties` |                  | Property to set or list of properties |

#### Custom rules

Each rule consists of any number of matches and any number of assignments.
They are sorted so the matches come before the assignments.
The matches use `==` to compare, while the assignments use `=` to set variables.

| Name          | Default/Required | Description         |
|---------------|:----------------:|---------------------|
| `matches`     |                  | List of matches     |
| `assignments` |                  | List of assignments |

## Dependencies

None

## Example Playbook

```yml
- hosts: all
  roles:
  - systemd-udevd
    udevd_log_level: err
    udevd_hwdb_entries:
      - matches: [ "mouse:*:name:*Trackball*:", "mouse:*:name:*trackball*:" ]
        properties: "ID_INPUT_TRACKBALL=1"
    udevd_rules:
      - matches: [ 'KERNEL=="hdb"', 'DRIVER=="ide-disk"' ]
        assignmens: [ 'NAME="myDisk"' ]
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

## Author Information

- [Janne Heß](https://github.com/dasJ)
