Wikipedia Proxy
===============

A standalone configuration bundle for OpenResty to set up a Wikipedia proxy.

**WARNING**:
This project has been superceded by
[wikiproxy](https://github.com/liweitianux/wikiproxy),
a simpler and better implementation in Go.

TODO
----
- Improve headers handling, especially `Set-Cookie`, `Referer`, `Origin`
- Better ACME integration

Usage
-----
OS: Debian Linux (bookworm/12)

1. Add OpenResty repository by creating file
   `/etc/apt/sources.list.d/openresty.sources`
   with the following content:

    ```
    # OpenResty for Debian
    # https://openresty.org/en/linux-packages.html#debian
    Enabled: yes
    Types: deb
    URIs: http://openresty.org/package/debian
    Suites: bookworm
    Components: openresty
    Architectures: amd64
    # PGP key from: https://openresty.org/package/pubkey.gpg
    Signed-By:
     -----BEGIN PGP PUBLIC KEY BLOCK-----
     .
     mQENBFkg3CEBCADG5Vem2p+1p6yV2jZfNsbJBPY1KYzR9weF/K3hmLODcrTaWfiD
     EugHwKlAptGDGBtrMsERjUiOWUNUS8IHa+R6tzhnQePG6wO7/yGWBC4J82BkCT2x
     M7zCDgldtNYkNqoBc0UfE4ln+WR/RX1DuzPM+DTBZBXLqRJVJFyFtHVJn8I5HPO2
     hj51uYqHsewTyAkGzABV4gmSIETSmcU5KDisQ9Vt5OllE0ylh7+kakDFZklyBCHT
     3IAuZhA18mw2qk1z5bnn/GpQ4fJi5w25lb9sqhhxta3ogwWWdJzXA+Nevb2dez8i
     bpzPeFnba9q0UVD2VJ25e99DpG15aPvNt+tbABEBAAG0JU9wZW5SZXN0eSBBZG1p
     biA8YWRtaW5Ab3BlbnJlc3R5LmNvbT6JATgEEwECACIFAlkg3CECGwMGCwkIBwMC
     BhUIAgkKCwQWAgMBAh4BAheAAAoJEJfbdEPV7et08k0H/iIOiZmDavSKE7NSPLxS
     ddRLh4+OGL3QW8JZh/+UaX1Z17G3q8kKSJwOemZBL/jIDkoMuHs1Hq0yp9vJ8BaS
     5unX+FRmivwdS5yvkS9s3oA3iJbHagXw0KMnr+baDDNrUwo9MeO0m9muNF/eDoRz
     FZ9SpOrgxwhz0kOOt8j+gxWk3TaQ6/JonH4rm3XtP4GMKOKQuUo6l8+pMPEfM209
     nMv82kRPAxRV2TwRYToB+TithLTQytJTBytLA+ck5Ny8sGoO5PQWRyx6gj+Bhg0O
     rFXfg7/sP2/FEeiDZcF2qn/VMDPvnC7ux2EQdI05MMGFY/pkjVYtLJC2Nb17Bcqj
     DH25AQ0EWSDcIQEIAJOoTY0vf0mr+PGUbnv0KKtk65CTzKmICmWIAkCxZaTH+o/3
     Lt9ZDtANH1ot3xVTkKg+qBuexh53jnyXyIaIfNqavH1gm+9JusrApVOad2ruODT5
     XeVamz0blq37LTmJ7A4T1i8WvB0BQ3j1vh6XkVW6xq1URzVOYyhVqNNq2UIP9M9Y
     wtiIIans5i11qmDtZwqxcSYoqSjgz+03M6Dn0UPB1OQdHjOPx7GwHG8+6sVyr+8A
     9G8SlKWre2/qdDyZNdgxalOi5ManCwWSURJRuY7s858qFUm0/5dLMAtWWEbYEmYc
     EUxbxQM2jPEaDmvvauZNup+a5DZXpRjpWcg19c0AEQEAAYkBHwQYAQIACQUCWSDc
     IQIbDAAKCRCX23RD1e3rdG0dB/9EWT8sTVPOlgFAF2WVZT3bFiqiIC9Dg6Wblt/K
     Id/p73gbDNTkeeTvGErAPPQwsKkbD1w2rIYoRzEJ1zVrLgaAbeH/frbQaYNu7c+3
     Wm93gxBxjL9Jyrs3jq5jwR4kJ5j+a/GEPtTDqtXzZHvyCP2PWDoQWANNAQDuTpYE
     LGHfDF9pmTVwuhkh2IFcH/ZBZUvcxP/w3jXqEiPti/rFN8wKSQtBgWI0pBpXGdrJ
     Tl3mIE4jLbPmkxidP1yUFx9wzEVu3soXViehMua9nOeotGOKF4DgekzCnFuXNnd3
     h2EiDJbMKk+QJcMPliIePZCP9JWj7n0ok9ccLg5XcNwiFEtn
     =U4Wk
     -----END PGP PUBLIC KEY BLOCK-----
    ```

2. Install OpenResty:

    ```
    $ sudo apt update
    $ sudo apt install openresty
    ```

3. Generate a self-signed SSL certificate (or use ACME to issue one):

    ```
    $ openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:prime256v1 \
          -out etc/ssl_ecdsa.key
    $ openssl req -x509 -sha256 -days 3650 -subj '/CN=WikiProxy' \
          -key etc/ssl_ecdsa.key -out etc/ssl_ecdsa.crt
    ```

4. Edit `wikiproxy/config.lua` accordingly, including the `wikis.host`
   domains, `proxy` SOCKS5 proxy.

5. If the SOCKS5 proxy is configured, start the proxy service.

6. Finally start the OpenResty:

    ```
    $ mkdir /tmp/wikiproxy
    $ sudo openresty -p $PWD -c $PWD/etc/nginx.conf
    ```

License
-------
MIT License
