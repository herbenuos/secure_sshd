# Ansible role to harden the sshd configuration

This ansible role changes an existing sshd config to use only secure crypto.

## Example playbook

Add this role to the `requirements.yml` and install it:

```yaml
- src: https://github.com/UOS-RZ/secure_sshd.git
  scm: git
  version: main
```

Then include the role in your playbook:

```yaml
- hosts: all
  become: true
  roles:
    - role: secure_sshd
```

## Configuration options

Take a look at the [defaults](defaults/main.yml) to see what variables you can set.

## Check crypto algorithms

You can check manually, which crypto algorithms your ssh version can use.

### Key exchange

```shell
ssh -Q kex
```

### Ciphers

```shell
ssh -Q cipher
```

### MAC

```shell
ssh -Q mac
```

## Tools to check settings

You can check the ssh settings with some tools.

### nmap

```shell
nmap -p22 -n -sV --script ssh2-enum-algos <IP-ADDRESS>
```

### SSH-Audit

- [SSH-Audit (python)](https://github.com/jtesta/ssh-audit)


## License

tbd

## Author information

- [UOS-RZ](https://rz.uni-osnabrueck.de/)

## Sources

- [BSI TR-02102-4 "Kryptographische Verfahren: Teil 4 – Verwendung von Secure Shell (SSH)"](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Publikationen/TechnischeRichtlinien/TR02102/BSI-TR-02102-4.html)
