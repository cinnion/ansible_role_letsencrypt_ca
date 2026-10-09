letsencrypt_ca
=========

An Ansible Role which installs the Let's Encrypt CA certificates to hosts,
rebuilds the SSL CA certificate bundle, and then restarts the SSSD process on
RHEL hosts.

For the actual distribution of certificates for services, see my [letsencrypt_certs](https://github.com/cinnion/ansible_role_letsencrypt_certs)
role.

Requirements
------------

This role has been developed using Ansible 2.20, and presently only works with
RHEL/CentOS.

It requires downloading the CA certificates for Let's Encrypt from their
[Chain of Trust](https://letsencrypt.org/certificates/) page and placing them
in the `files` directory of this role, with their extension changed to just
'.pem'

N.B.: At this time, the certificates being pushed are the

* [The ISRG Root X1 (self-signed) certificate.](https://letsencrypt.org/certs/isrgrootx1.pem)
* [The ISRG Root X1 (cross-signed by DST Root CA X3) certificate.](https://letsencrypt.org/certs/isrg-root-x1-cross-signed.pem)
* [The ISRG Root X2 (self-signed) certificate.](https://letsencrypt.org/certs/isrg-root-x2.pem)
* [The ISRG Root X2 (cross-signed by ISRG Root X1) certificate.](https://letsencrypt.org/certs/gen-y/root-x2-by-x1.pem)
* [The ISRG Root YE (self-signed) certificate.](https://letsencrypt.org/certs/gen-y/root-ye.pem)
* [The ISRG Root YE (cross signed by ISRG Root X2) certificate.](https://letsencrypt.org/certs/gen-y/root-ye-by-x2.pem)
* [The ISRG Root YR (self signed) certificate.](https://letsencrypt.org/certs/gen-y/root-yr.pem)
* [The ISRG Root YR (cross-signed by ISRG Root X1) certificate.](https://letsencrypt.org/certs/gen-y/root-yr-by-x1.pem)
* [The Let's Encrypt YE1 certificate.](https://letsencrypt.org/certs/gen-y/int-ye1.pem)
* [The Let's Encrypt YE2 certificate.](https://letsencrypt.org/certs/gen-y/int-ye2.pem)
* [The Let's Encrypt YR1 certificate.](https://letsencrypt.org/certs/gen-y/int-yr1.pem)
* [The Let's Encrypt YR2 certificate.](https://letsencrypt.org/certs/gen-y/int-yr2.pem)

This role also attempts to download new versions of the certificates, and if the
`.git` directory is present, will automatically do the commit of the new
certificates.

When copied to the target host, the files are renamed to include the prefix `letsencrypt-`.

Role Variables
--------------

The following variable is defined in the defaults (see `defaults/main.yml`)

```yaml
letsencrypt_ca_certificate_urls:
    - https://letsencrypt.org/certs/isrgrootx1.pem
    - https://letsencrypt.org/certs/isrg-root-x1-cross-signed.pem
    - https://letsencrypt.org/certs/isrg-root-x2.pem
    - https://letsencrypt.org/certs/gen-y/root-x2-by-x1.pem
    - https://letsencrypt.org/certs/gen-y/root-ye.pem
    - https://letsencrypt.org/certs/gen-y/root-ye-by-x2.pem
    - https://letsencrypt.org/certs/gen-y/root-yr.pem
    - https://letsencrypt.org/certs/gen-y/root-yr-by-x1.pem
    - https://letsencrypt.org/certs/gen-y/int-ye1.pem
    - https://letsencrypt.org/certs/gen-y/int-ye2.pem
    - https://letsencrypt.org/certs/gen-y/int-yr1.pem
    - https://letsencrypt.org/certs/gen-y/int-yr2.pem
```

A list of the certificates to download and install on the target server.

---------------------------------------------------------------------------

The following platform-specific variables are defined in the files under the
`vars` directory (see `vars/RedHat.yml`).

    ca_trusted_dir: /etc/pki/ca-trust/source/anchors

The directory where CA certificates are placed for incorporation into the
CA bundle.

    ca_update_command: update-ca-trust

The command to be run to rebuild the CA bundle.

Dependencies
------------

A working `git` installation, if the `.git` directory is present in the role.

Example Playbook
----------------

```yaml
    - hosts: servers
      roles:
         - cinnion.letsencrypt_ca
```

License
-------

BSD-3-Clause

Author Information
------------------

This role was created 2018 Dec 01 by [Douglas Wade Needham](https://www.ka8zrt.com)
and repackaged on 2026 Oct 08 to use a standard template.
