**This package has been merged into nethserver-openvpn in NethServer 7.**

CA environment
==============

Prepare CA environment: ::

  mkdir /var/lib/nethserver/certs
  touch /var/lib/nethserver/certs/certindex.attr
  touch /var/lib/nethserver/certs/certindex
  echo 01 > /var/lib/nethserver/certs/serial
  echo 01 > /var/lib/nethserver/certs/crlnumber
  openssl dhparam -out /var/lib/nethserver/certs/dh1024.pem 1024


Certificate creation
====================

Create a valid certificate:

1. Create a CA (we already have it in NSRV.crt)
2. Create a certificate signing request: this will create a csr file and a key file
3. Sign the csr file

Esecute: ::

  /usr/libexec/nethserver/pki-vpn-gencert <commonName>


Certificate revocation
======================

1. Revoke and save to CRL

Execute: ::

  /usr/libexec/nethserver/pki-vpn-revoke [-d] <commonName>

If '-d' option is enabled, also delete crt, csr, pem and key files



Usefull commands
================

Read crt informations: ::

  openssl x509 -text -in /root/test.crt


Create a pcks12: ::

    openssl pkcs12  -export -in test.crt -inkey test.key -certfile /etc/pki/tls/certs/NSRV.crt  -name "test" -out test.p12

Revoke a certificate: ::

  /usr/bin/openssl ca -revoke  /var/lib/nethserver/certs/pippo2.crt  -config /var/lib/nethserver/certs/ca.cnf
  /usr/bin/openssl ca -gencrl -out /var/lib/nethserver/certs/crl.pem -config /var/lib/nethserver/certs/ca.cnf


