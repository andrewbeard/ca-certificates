# CA Certificates
A collection of CA certificates NOT contained in the normal set of Mozilla bundles that may still be used for traffic on your network. These are mostly IoT devices, callbacks from proprietary software, and the like that have no buisness interacting with a browser.

This was mostly spawned by [Zeek](https://zeek.org) traffic analysis and [this post](http://mailman.icsi.berkeley.edu/pipermail/zeek/2019-November/014768.html) describing how to add additional certificates to the trust store for Zeek verification. I don't particularly trust Microsoft/Nest/Roku/etc but I did want to stop flagging all their traffic as anomalous and get on with looking at more interesting things.

This repo includes certificates for:

## Apple
Props to Apple for making their certificates available [here](https://www.apple.com/certificateauthority/). I do not include the majority of their CA certificates, only the ones that are visible on the wire and are not otherwise trusted by Zeek. 

## Microsoft
Microsoft also gets props for making the certificates for their services (telemetry, Windows Update, etc) available with no fuss [here](https://www.microsoft.com/pkiops/docs/repository.htm).

## Nest
From what I can tell Nest uses a single CA certificate that isn't included in their chain. You can scrape the Authority Key Identifier from an exchange, though, then look that certificate up using [Censys](https://search.censys.io/certificates/d802bd8abd42f347f2c315b088ac1e12c74db90d546dfd6dfaa2d2723b8088d9) and download it.

## Nintendo
Nintendo uses a couple CA certificates, but doesn't include anym of them in the chain. The method above for NEst would probably work, but someone seems to have made them all available [here](https://larsenv.github.io/NintendoCerts/index.html) already.

## Roku
Roku uses multiple private CAs, but includes a full certificate chain including the self-signed CA certificate with each exchange. You can grab their certificates pretty easily by using the openssl utility:

```
openssl s_client -host configsvc.cs.roku.com -port 443 -showcerts < /dev/null
openssl s_client -host liberty.logs.roku.com -port 443 -showcerts < /dev/null
```

That will dump the entire chain, but you really only need the last one in each exchange which is included in PEM format.
