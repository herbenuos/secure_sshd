# Ansible role to harden the sshd configuration

This ansible role changes an existing sshd config to use only secure crypto as recommended by the [BSI TR 02102](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/Technische-Richtlinien/TR-nach-Thema-sortiert/tr02102/tr-02102.html) and the following parameters:


| Parameter | Value |
| --- | --- |
| Loglevel | VERBOSE |
| PermitRootLogin | no |
| PubkeyAuthentication | yes |
| PasswordAuthentication | no |
| Protocol | 2 |
| LoginGraceTime | 1m |
| MaxAuthTries | 6 |
| UsePAM | yes |
| MaxSessions | 2 |
| MaxStartups | 10:30:60 |
| StrictModes | yes |
| X11Forwarding | no |
| Compression | no |
| TCPKeepAlive | no |
| PrintMotd | yes |
| PrintLastLog | yes |
| AllowAgentForwarding | no |
| AllowTcpForwarding | no |
| PermitEmptyPasswords | no |
| GSSAPIAuthentication | no |
| KerberosAuthentication | no |
| RekeyLimit | 250M 1800 |
| PermitTunnel | no |

## Example playbook

Add this role to the `requirements.yml` and install it:

```yaml
- src: https://github.com/UOS-RZ/secure_sshd.git
  scm: git
  version: 1.1.1
```
### THIS MIGHT BREAK YOUR CONNECTIVITY TO THE SERVER(S)
### CHECK IF YOU HAVE A RSA SSH-KEY WITH LESS THAN 3072 BIT!! 

```bash
ssh-keygen -lf ~/.ssh/id_rsa
3072 SHA256:l0VdUfWSxaNg0/X/g8RIacN2afYt8rkxuABBlTE+JMw user@host (RSA)
 ^ RSA KEY SIZE
```

Then include the role in your playbook:
```yaml
- hosts: all
  roles:
    - role: secure_sshd
```

If you ***need*** x11 forwarding enable it like so:
```yaml
- hosts: all
  roles:
    - role: secure_sshd
      vars:
        secure_sshd_allow_x11_forwarding: 'yes' # default: no
```

For a full example playbook with step-by-step instructions, take a look at

- [Example Ansible playbook including secure\_sshd](example_playbook.yml)

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

[BSD-3-Clause](LICENSE)

## Author information

- [UOS-RZ](https://rz.uni-osnabrueck.de/)

## Sources

- [BSI TR-02102-4 "Kryptographische Verfahren: Teil 4 – Verwendung von Secure Shell (SSH)"](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Publikationen/TechnischeRichtlinien/TR02102/BSI-TR-02102-4.html)
