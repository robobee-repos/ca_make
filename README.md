-   ![](https://anrisoftware.com/projects/attachments/download/217/apache2.0-small.gif)
    (© 2016 Erwin Müller)
-   [Source github.com](https://github.com/devent/ca_make)
-   [Archive
    github.com](https://github.com/devent/ca_make/archive/master.zip)
-   `git`anrisoftware.com:ca\_make.git@
-   `git`github.com:devent/ca\_make.git@

CA Make
=======

Introduction
------------

This project contains scripts to create a simple Certificate Authority
that can issue and sign certificates and keeps track of them by a simple
serial number system. The project is based on `make` scripts, because we
need to keep track of files and run commands on the shell. The `make`
tool is originally designed to run source code build tools like `gcc`,
but it is perfectly suited to build certificates.

The project is designed to be cloned and to be run on a specific branch
for a specific CA. The certificates and keys can also be committed to
the repository. If a repository is used it should be held private
because the scripts also keep track of the private key passphrases in
plain text files. The proposed use case for the project is it to clone
it and to checkout a new branch as shown in the example below. As an
alternative, the project sources can also be downloaded and extracted in
a new directory.

The project can also be used to create just the certificate key and the
certificate signing request to be used by an external CA. For that,
first the `openssl.cnf` should be changed to set the country, province,
city, organization, etc., then the root and intermediate CA should be
created. Then finally in the `./root` directory the certificate key and
the certificate signing request can be generated. A password-less key is
also generated and saved with the `_insecure` label.

    git clone git@github.com:devent/ca_make.git
    git checkout -b nttdata
    ... create server certificates

The project creates a root CA and an intermediate CA that can create
server certificates and sign them. It can also revoke server
certificates and regenerate the certificate revocation lists. Basic
functions can be accessed by the main `./Makefile` in the base directory
and more advanced functions are located in the intermediate CA directory
`./intermediate/Makefile`.

-   `./Makefile`
-   `./root/Makefile`
-   `./root/openssl.cnf`
-   `./intermediate/Makefile`
-   `./intermediate/openssl.cnf`

Credits
-------

The project is based on a tutorial from Jamie Nguyen that can be found
on
[jamielinux.com](https://jamielinux.com/docs/openssl-certificate-authority/index.html)

Dependencies
------------

The project requires the following tool.

-   `make` to run Makefile scripts;
-   `openssl` to create and sign certificates;
-   `pwgen` to generate passphrases for the certificate keys.

Configuration
-------------

The `root/openssl.cnf` and `intermediate/openssl.cnf` file contains the
main configuration of the Openssl client. The top of the file contains
some default values to create server certificates and should be changed
to one's own needs.

    # Change those for you own needs.
    KEY_SIZE               = 2048
    KEY_COUNTRY            = DE
    KEY_PROVINCE           = BAVARIA
    KEY_CITY               = MUNICH
    KEY_ORG                = ERWIN MUELLER
    KEY_ORGUNIT            = ERWIN MUELLER Certificate Authority
    KEY_COMMON_NAME        = ERWIN MUELLER Root CA
    KEY_EMAIL              = admin@muellerpublic.de

    # Set the CRL distribution URL.
    CRL_URL                = URI:https://muellerpublic.de/intermediate.crl.pem

    # Set the OCSP distribution URL.
    OCSP_URL               = OCSP;URI:http://ocsp.muellerpublic.de

Usage
-----

### ./Makefile

-   `make help`

Shows the available targets with a description.

-   `make`

Creates the root and the intermediate CA.

-   `make regenerate`

Regenerates the certificate revocation lists.

-   `make show_crl`

Shows the certificate revocation lists.

-   `make ocsp`

Creates the OCSP cryptographic pair.

### root/Makefile

-   `make create_key SERVER_NAME=test.muellerpublic.de`
-   `make create_key SERVER_NAME=test.muellerpublic.de SERVER_KEY_SIZE=2048`

Creates a new certificate key. Optionally, the key size can be specified
by setting SERVER\_KEY\_SIZE.

-   `make create_crt SERVER_NAME=test.muellerpublic.de`

Creates a new certificate signing request.

### intermediate/Makefile

-   `make server SERVER_NAME=test.muellerpublic.de`

Creates the server certificate signed by the intermediate CA.

-   `make revoke_server SERVER_NAME=test.muellerpublic.de`

Revokes the server certificate that was signed by the intermediate CA.

-   `make start_ocsp`

Starts an OCSP responder on localhost.

-   `make query_ocsp`

Query the OCSP responder on localhost.

Generated Files
---------------

<table>
<thead>
<tr class="header">
<th>File</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>certs/</td>
<td>Directory of the signed certificates.</td>
</tr>
<tr class="even">
<td>certs/ca_cert.pem</td>
<td>The root CA certificate.</td>
</tr>
<tr class="odd">
<td>certs/ca_chain_cert.pem</td>
<td>The intermediate CA chain certificate.</td>
</tr>
<tr class="even">
<td>certs/intermediate_cert.pem</td>
<td>The intermediate CA certificate.</td>
</tr>
<tr class="odd">
<td>certs/ocsp.muellerpublic.de_cert.pem</td>
<td>The OCSP certificate.</td>
</tr>
<tr class="even">
<td>crl/</td>
<td>Directory of the certificate revocation lists.</td>
</tr>
<tr class="odd">
<td>csr/</td>
<td>Directory of the certificate requests.</td>
</tr>
<tr class="even">
<td>passwords/</td>
<td>Directory of the private key passphrases.</td>
</tr>
<tr class="odd">
<td>private/</td>
<td>Directory of the private keys.</td>
</tr>
<tr class="even">
<td>private/ca_key.pem</td>
<td>The root CA private key.</td>
</tr>
<tr class="odd">
<td>private/intermediate_key.pem</td>
<td>The intermediate CA private key.</td>
</tr>
<tr class="even">
<td>private/ocsp.muellerpublic.de_key.pem</td>
<td>The OCSP private key.</td>
</tr>
</tbody>
</table>

License
-------

Licensed under a [Apache 2.0
License.](http://www.apache.org/licenses/LICENSE-2.0) Permissions beyond
the scope of this license may be available at
`erwin.mueller`deventm.org@ or `erwin.mueller`nttdata.com@.
