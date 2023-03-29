# secure-vm

This role applies basic security to a RHEL/CentOS VM.

## Requirements

Check: [README](../../README.md)

## Role Variables

Settable variables for this role that are in [defaults/main.yml](defaults/main.yml):

```yaml
permitRootLogin: false
ssh_port: 32122
```

## Dependencies

None.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: workstation
      roles:
         - secure-vm

## License

MIT

## Author Information

[Raphael Tholl](https://github.com/RapTho)
