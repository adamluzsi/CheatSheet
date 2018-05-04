## How does HTTPS work?

HTTPS is based on **public/private-key cryptography**. This basically means that there is a key pair: The public key is used for encryption and the secret private key is required for decryption.

A **certificate** is basically a public key with a label identifying the owner.

So when your browser connects to an HTTPS server, the server will answer with its certificate. The browser **checks if the certificate is valid**:

*   the owner information need to match the server name that the user requested.
*   the certificate needs to be signed by a trusted certification authority.

If one of these conditions is not met, the user is informed about the problem.

After the verification, the **browser extracts the public key** and uses it to encrypt some information before sending it to the server. The server can decrypt it because the **server has the matching private key**.

## How does HTTPS prevent man in the middle attacks?

> In this case, will G be able to get the certificate which A previously got from W?

Yes, the certificate is the public key with the label. The webserver will send it to anyone who connects to it.

> If G can get the certificate, does that mean that G will be able to decrypt the data?

No. The certificate contains the **public key of the webserver**. The malicious proxy is not in the possession of the matching private key. So if the proxy forwards the real certificate to the client, it cannot decrypt information the client sends to the webserver.

The proxy server may try to forge the certificate and provide his own public key instead. This will, however, **destroy the signature of the certification authorities**. The browser will warn about the invalid certificate.

## Is there a way a proxy server can read HTTPS?

If the **administrator of your computer cooperates**, it is possible for a proxy server to sniff https connections. This is used in some companies in order to scan for viruses and to enforce guidelines of acceptable use.

A **local certification authority** is setup and the administrator tells your browser that this **CA is trustworthy**. The proxy server uses this CA to sign his forged certificates.

Oh and of course, user tend to click security warnings away.
