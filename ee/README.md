# Custom execution-environment

## SSH key for Mac podman users
If you're using podman on a Mac, your podman runs within a virtual machine. You thus need to copy the ssh-key to a location where the virtual machine has access to.

Create your podman machine with a mounted volume
```
mkdir -p ~/podman_volumes/.ssh
podman machine init -v ~/podman_volumes/:$HOME
cp .ssh/id_rsa_ansible ~/podman_volumes/.ssh/id_rsa_ansible
```

## Login to registry.redhat.io

```
podman login quay.io
```

## OPTIONAL: Add your Python, Ansible or system dependencies
Add your dependencies to the specific files
```
requirements.txt = python dependencies
requirements.yml = ansible dependencies
bindep.txt = system dependencies
```
## OPTIONAL: Modify any of the files
If you changed any of the ansible.cfg, requirements files etc. you need to rebuild the context folder
```
ansible-builder create
```

Replace the custom Containerfile with the generated Containerfile
```
cp Containerfile context/Containerfile
```

## Build execution-environment

```
podman build -t ee-custom:1.0 -f context/Containerfile --layers=false context
```

