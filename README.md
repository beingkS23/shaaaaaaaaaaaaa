## SHAAAAAAAAAAAAA This a readme file for the repository with multiple changes over the years by 2 contributors and this readme file also explains the reason and whats is in this repository.

**Note:** This project, and much of its documentation, was written in the fall of 2014, when SHA-1 was still very common and the transition was only beginning.

**As of 2016, all CAs are required to only issue SHA-2**, and SHA-1 was mostly removed from modern OSes and browsers in 2017.

This web service has been retired, but the code may still be useful for other purposes.

### What this repo is

This repository contains the code for **[shaaaaaaaaaaaaa.com](https://shaaaaaaaaaaaaa.com)**, a tool which was used to check whether your site's certificate is signed using **SHA-1** or **SHA-2**.

SHA-1 is largely gone now, and so this tool is retired. You can [read more about why I originally built this tool](https://konklone.com/post/why-google-is-hurrying-the-web-to-kill-sha-1) and why replacing SHA-1 was important.

This tool does *not* validate certificates, or test anything besides SHA-1 vs SHA-2. For that, please visit the magnificent [SSL Labs](https://www.ssllabs.com/ssltest/analyze.html) for a far more comprehensive review of your SSL configuration.

*No Longer* depends on `openssl` to download certificates. See below for a [command line version](#command-line-version).

### How do I update to SHA-2?

Now that it's 2016: just get a new certificate. As of January 1 2016, all publicly trusted CAs are required to issue only SHA-2 certificates.

To update your intermediate, check what your CA is currently offering as the default, or go through the issuance process again to see what they provide you.

If you somehow get a SHA-1 certificate, or are given a SHA-1 intermediate, [open an issue](https://github.com/konklone/shaaaaaaaaaaaaa/issues/new) and the folks on this repo will be happy to badger the CA or figure out what's going on.

### How can I help?

Check out the [issue tracker](https://github.com/konklone/shaaaaaaaaaaaaa/issues). The biggest things are:

* How about a bookmarklet, a Firefox extension, or a Chrome extension?
* Some [annoying domain errors](https://github.com/konklone/shaaaaaaaaaaaaa/issues/34) on edge cases in Google's DNS.
* Mapping out [common certificate issuers](https://github.com/konklone/shaaaaaaaaaaaaa/issues/31) so we can easily link people to replacements.
* Hunting down [more SHA-2 intermediate locations](https://github.com/konklone/shaaaaaaaaaaaaa/issues/36) than we [currently have](https://shaaaaaaaaaaaaa.com/#sha2-intermediate) on the site.
* More [unit tests](https://github.com/konklone/shaaaaaaaaaaaaa/blob/master/test/shaaaaa.js), especially for intermediate certificates and chained root certificates.
* Getting [some Internet SHA-1 stats](https://github.com/konklone/shaaaaaaaaaaaaa/issues/16) by running the [command line tool](#command-line-version) over a list of top sites, like Alexa's [[CSV download](https://s3.amazonaws.com/alexa-static/top-1m.csv.zip)].

Really, just making the site better all around.

### Running the current website

The website at https://shaaaaaaaaaaaaa.com is now a static site, using a frozen snapshot of the HTML in the `snapshot/` directory. So if you want to reproduce that, for some reason, just point a web server at the static files in that directory.

### Running the old website

The dynamic version of the app requires [Node](https://nodejs.org). Then, install dependencies:

```bash
npm install
```

And run the app:

```
node app.js
```

For best results, make sure your system is using the latest version of `openssl`.

To run the tests:

```
npm test
```

Tests use `faucet`, which should have been installed during `npm install` above.

### Command line version

To check a domain's certificate on the command line, use this repository's command line tool:

```bash
./bin/shaaaaaaaaaaaaa sha1-2017.badssl.com
```

This will exit with code `0`, and output formatted JSON to STDOUT:

```json
{
  "domain": "sha1-2017.badssl.com",
  "cert": {
    "algorithm": "sha1",
    "raw": "sha1WithRSAEncryption",
    "good": false,
    "root": false,
    "expires": "2017-01-05T12:00:00.000Z",
    "name": "*.badssl.com"
  },
  "intermediates": [
    {
      "algorithm": "sha1",
      "raw": "sha1WithRSAEncryption",
      "good": false,
      "root": false,
      "replacement": "http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt",
      "expires": "2023-03-08T12:00:00.000Z",
      "name": "DigiCert Secure Server CA"
    }
  ],
  "diagnosis": "bad"
}
```

If there's an error, you'll get some JSON with an `error` flag of `true`, and the process will exit with code `1`:

```bash
$ ./bin/shaaaaaaaaaaaaa bad-domain
```

```json
{
  "error": true,
  "domain": "bad-domain",
  "message": "Couldn't lookup hostname."
}
```


## Author

This is a tiny tool by [Eric Mill](https://twitter.com/konklone). Released under an [MIT License](LICENSE).
