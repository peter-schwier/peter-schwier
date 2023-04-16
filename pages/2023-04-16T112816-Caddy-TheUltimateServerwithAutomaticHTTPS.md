---
markdownload-timestamp: 2023-04-16T11:28:16 (UTC -05:00)
markdownload-tags: []
markdownload-source: https://caddyserver.com/
markdownload-hostname: caddyserver.com
markdownload-author: Caddy Web Server
---

# Caddy - The Ultimate Server with Automatic HTTPS

> ## Excerpt
> Caddy is a powerful, enterprise-ready, open source web server with automatic HTTPS written in Go

---
### Fewer moving parts

Caddy simplifies your infrastructure. It takes care of TLS certificate renewals, OCSP stapling, static file serving, reverse proxying, Kubernetes ingress, and more.

Its modular architecture means you can do more with a single, static binary that compiles for any platform.

Caddy runs great in containers because it has no dependencies—not even libc. Run Caddy practically anywhere.

[Documentation](https://caddyserver.com/docs/)

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/moving-parts.svg]]

### Best-in-class security

**Caddy is the only web server to use HTTPS automatically and by default.**

Caddy obtains and renews TLS certificates for your sites automatically. It even staples OCSP responses. Its novel certificate management features are the most mature and reliable in its class.

Written in Go, Caddy offers greater memory safety than servers written in C. A hardened TLS stack powered by the Go standard library serves a significant portion of all Internet traffic.

[Download](https://caddyserver.com/download)

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/caddy-circle-lock.svg]]

### Backed by Ardan

[Ardan Labs](https://www.ardanlabs.com/) is the trusted partner of the Caddy Web Server open source project, providing enterprise-grade support to our clients.

Together, we consult and train, as well as develop, install, and maintain Caddy and its plugins to ensure your infrastructure runs smoothly and efficiently. Contact us to get started!

[Let's talk](https://www.ardanlabs.com/my/contact-us?dd=caddy)

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/ardan-labs.svg]]

### File server and proxy

Caddy is both a flexible, efficient static file server and a powerful, scalable reverse proxy.

Use it to serve your static site with compression, template evaluation, Markdown rendering, and more.

Or use it as a dynamic reverse proxy to any number of backends, complete with active and passive health checks, load balancing, circuit breaking, caching, and more.

[Download](https://caddyserver.com/download)

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/proxy-file-server.svg]]

## 1-Liners

## These commands are **production-ready**. When given a domain name, Caddy will use **HTTPS by default**, which provisions and renews certificates for you.\*

\* Requires domain's public A/AAAA DNS records pointed at your machine.

`   $ caddy file-server   `

Public file server over HTTPS

`   $ caddy file-server --domain example.com   ``   $ caddy reverse-proxy --from example.com --to localhost:9000   `

Run server with Caddyfile in working directory (if present)

`   $ caddy run   `

## The Caddyfile

## A config file that's **human-readable** and **easy to write** by hand. Perfect for most common and manual configurations.

Local file server with template evaluation

`   localhost templates file_server   `

HTTPS reverse proxy with custom load balancing and active health checks

`   example.com # Your site's domain name # Load balance between three backends with custom health checks reverse_proxy 10.0.0.1:9000 10.0.0.2:9000 10.0.0.3:9000 { lb_policy random_choose 2 health_path /ok health_interval 10s }   `

HTTPS site with clean URLs, reverse proxying, compression, and templates

`   example.com # Templates give static sites some dynamic features templates # Compress responses according to Accept-Encoding headers encode gzip zstd # Make HTML file extension optional try_files {path}.html {path} # Send API requests to backend reverse_proxy /api/* localhost:9005 # Serve everything else from the file system file_server   `

## Config API

## Caddy is dynamically configurable with a **RESTful JSON API**. Config updates are **graceful**, even on Windows.

Using JSON gives you **absolute control** over the edge of your compute platform, and is perfect for **dynamic** and **automated** deployments.

`   **POST /config/** { "apps": { "http": { "servers": { "example": { "listen": ["127.0.0.1:2080"], "routes": [{ "@id": "demo", "handle": [{ "handler": "file_server", "browse": {} }] }] } } } } }   `

Export current configuration

`   **GET /config/**   `

Change only a specific part of the config

`   **PUT /id/demo/handle/0** {"handler": "templates"}   `

## All changes made through the API are persisted to disk so they can continue to be used after restarts.

[Download](https://caddyserver.com/download) [API Docs](https://caddyserver.com/docs/api) [Tutorial](https://caddyserver.com/docs/getting-started)

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/padlock.svg]]

Secure by Default

Caddy is the only web server that uses HTTPS by default. A hardened TLS stack with modern protocols preserves privacy and exposes MITM attacks.

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/plug.svg]]

Config API

As its primary mode of configuration, Caddy's REST API makes it easy to automate and integrate with your apps.

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/box.svg]]

No Dependencies

Because Caddy is written in Go, its binaries are entirely self-contained and run on every platform, including containers without libc.

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/stack.svg]]

Modular Stack

Take back control over your compute edge. Caddy can be extended with everything you need using plugins.

✔ Static sites

✔ Dynamic sites

✔ Reverse proxy

✔ Dynamic config

✔ Extensible core

✔ Automagic TLS

![[2023-04-16T112816-caddy-theultimateserverwithautomatichttps/features.svg]]

### General

Caddy 2 was boldly engineered to simplify your infrastructure and give you control over the edge of your compute platform.

#### Architecture

##### Extensible

Caddy can embed any Go application as a plugin, and has first-class support for plugins of plugins.

##### Minimal Global State

Global state is common in servers, but tends to be error-prone and a bottleneck, so Caddy 2 uses a novel design that limits global state.

##### Lightweight

For all its features, Caddy runs lightly and efficiently with relatively low memory footprint and high throughput.

##### Multi-core

When the going gets tough, Caddy gets going on more CPUs. Go's scheduler understands Go code, and goroutines are more lightweight than system threads.

##### Static Binary

Caddy is a single executable file with no dependencies, not even libc. Literally just needs some metal and a kernel. Put Caddy in your PATH and run it. Done.

##### Cross-Platform

Caddy runs on Windows, macOS, Linux, BSD, Android, Solaris, 32-bit, amd64, ARM, aarch64, mips64... almost anything to which Go compiles.

#### Configuration

##### JSON Structure

Caddy's native config format is JSON, so it is familiar and highly interoperable with existing systems and tools.

##### REST API

Caddy's configuration is received through a REST endpoint as a single JSON document, making it highly programmable.

##### Config Files Optional

You can use config files with Caddy's CLI, which converts them to API requests for you under the hood.

##### Config Adapters

Bring your own config! Config adapters translate various config formats (Caddyfile, TOML, NGINX, etc.) into Caddy's native JSON.

##### The Caddyfile

An easy, intuitive way to configure your site. It's not scripting, and not hard to memorize. Rolls off the fingers. You'll really like it.

##### Unified Config

All configuration is contained within a single JSON document so there are fewer hidden factors affecting your config.

##### Partial Updates

When you have just small changes to make, Caddy's API lets you update just the relevant parts of its config.

##### Fine-Grained Control

Caddy's native JSON exposes the actual fields allocated in memory by the running server to give you more control.

##### Export

You can export a live copy of Caddy's current configuration with a GET request to its API.

##### Efficient Reloads

Config updates are finely tuned for efficiency so you can reload config dozens of times per second.

##### Graceful Reloads

Config changes take effect without downtime or closing sockets—even on Windows.

##### Config Validation

You can use Caddy's CLI to preview and validate configurations before applying them.

#### Basic Features

##### The Caddyfile

An easy, intuitive way to configure your site. It's not scripting, and not hard to memorize. Rolls off the fingers. You'll really like it.

##### Static Files

By default, Caddy will serve static files in the current working directory. It's so brilliantly simple and works fast.

##### Dynamic Sites

Caddy can also be used to serve dynamic sites with templates, proxying, FastCGI, and by the use of plugins.

##### Command Line Interface

Customize how Caddy runs with its simple, cross-platform command line interface; especially great for quick, one-off server instances.

##### Plugins

Caddy can be extended with plugins. All apps, Caddyfile directives, HTTP handlers, and other features are plugins! They're easy to write and get compiled in directly.

##### Multi-core

When the going gets tough, Caddy gets going on more CPUs. Go's scheduler understands Go code, and goroutines are more lightweight than system threads. So yeah, it's fast.

##### Embeddable

Writing another program or web service that could use a powerful web server or reverse proxy? Caddy can be used like a library in your Go program.

##### Caddyfile Validation

Caddy can parse and verify your Caddyfile without actually running it.

##### Process Log

Caddy can write a log of all its significant events, especially errors. Log to a file, stdout/stderr, or a local or remote system log!

##### Log Rolling

When log files get large, Caddy will automatically rotate them to conserve disk space.

### Security and Privacy

Caddy's flagship features are security and privacy. Caddy is the first and only web server to enable HTTPS automatically and by default.

#### TLS

##### TLS 1.3

TLS 1.3 is the newest standard for transport security, which is faster and more secure than its predecessors.

##### Modern Cipher Suites

Caddy uses the best crypto technologies including AES-GCM, ChaCha, and ECC by default, balancing security and compatibility. You can customize which ciphers are allowed.

##### Memory Safety

Caddy is the only web server in its class that is impervious to bugs like Heartbleed and buffer overflows because it is written in the memory-safe language of Go.

##### Client Authentication

With TLS client auth, you can configure Caddy to allow only certain clients to connect to your service.

##### Hardened Stack

Caddy is proudly written in Go, and its TLS stack is powered by the robust crypto/tls package in the Go standard library, trusted by the world's largest content distributors.

##### PCI Compliant

Companies choose Caddy because its TLS configuration is PCI-compliant by default. It has even saved some companies hours before losing certification!

##### Scalable Storage

TLS assets are stored on disk, but the storage mechanism can be swapped out for custom implementations so you can deploy and coordinate a fleet of Caddy instances.

##### Key Rotation

Caddy is cited as the [only web server](https://jhalderm.com/pub/papers/forward-secrecy-imc16.pdf) to rotate TLS session ticket keys by default. This helps preserve forward secrecy, i.e. visitor privacy.

##### Server Name Indication

Caddy uses the TLS extension Server Name Indication (SNI) to be able to host multiple sites on a single interface. Like most features, this just works.

##### Redirect HTTP to HTTPS

Caddy's [automatic HTTPS](https://caddyserver.com/docs/automatic-https) feature includes redirecting HTTP to HTTPS for you by default.

#### Certificates

##### Auto Obtain

Caddy obtains certificates for you automatically using Let's Encrypt. Any ACME-compatible CA can be used. Caddy was the first web server to implement this technology.

##### Auto Renew

Never deal with certificates again! Certificates are automatically renewed in the background before they get close to expiring.

##### Dynamic Cert Loading

Caddy is the only web server that can obtain certificates during a TLS handshake and use it right away.

##### Bring Your Own

If you still prefer to manage certificates yourself, you can give Caddy your certificate and key files (PEM format) like you're used to.

##### Bulk Cert Loading

If you manage many certificates yourself, you can give Caddy an entire folder to load certificates from.

##### Easy Self-Signed Certs

For easy local development and testing, Caddy can generate and manage self-signed certificates for you without any hassle.

##### SAN Certificates

Caddy fully accepts SAN certificates for times when you may be managing your own SAN certificates and wish to use those instead.

##### Cluster Support

Caddy can share managed certificates stored on disk with other instances and synchronize renewals in fleet deployments.

##### Scalable

Caddy's certificate management scales well up to tens of thousands of sites and tens of thousands of certificates per instance.

##### Wildcards

When needed, Caddy can obtain and renew wildcard certificates for you when you have many related subdomains to serve.

#### OCSP

##### Stapling

Caddy staples OCSP responses to every qualifying certificate by default. Caddy's OCSP stapling is more robust against network failure than other web servers.

##### Caching

Every OCSP response is cached on disk to preserve integrity through restarts, in case the responder goes down or the network link is being attacked.

##### Must-Staple

Caddy can be configured to obtain Must-Staple certificates, which requires that certificate to always have the OCSP response stapled.

##### Background Updates

Unlike other web servers, Caddy updates OCSP responses in the background, asynchronously of any requests, well before their expiration.

##### Pre-Validated

An OCSP response will not be stapled unless it checks out for validity first, to make sure it's something clients will accept.

##### Revocation Handling

If a managed certificate is discovered by OCSP to be revoked, Caddy will automatically try to replace the certificate.

#### ACME Protocol

##### HTTP Challenge

Caddy can solve the HTTP challenge to obtain certificates. You can also configure Caddy to proxy these challenges to other processes.

##### TLS-ALPN Challenge

Caddy solves the TLS-ALPN challenge which happens on port 443 and does not require opening port 80 at all.

##### Fleet Coordination

Caddy coordinates the obtaining and renewing of certificates in cluster configurations for both HTTP and TLS-ALPN challenges!

##### DNS Challenge

Caddy solves the DNS challenge which does not involve opening any ports on the machine. There are integrations for all major DNS providers!

##### Revocation

If one of your private keys becomes compromised, you can use Caddy to easily revoke the affected certificates.

##### Customizable CA

Caddy is designed to be used with any ACME-compatible certificate authority, which you can customize with a single command line flag.

##### Robust to Failures

Caddy is the only web server and only major ACME client that was not disrupted by CA changes and outages, or OCSP responder hiccups.

### HTTP Server

Caddy's HTTP server has a wide array of modern features, high performance, and is easy to deploy.

#### Site Features

##### Directory Browsing

List files and folders with Caddy's attractive, practical design or according to your own custom template.

##### Virtual Hosts

Serve multiple sites from the same IP address with the Caddyfile.

##### Configurable Binding

You can select which network interfaces to which you bind the listener, giving you more access control over your site.

##### Markdown

Let Caddy render your Markdown files as HTML on-the-fly. You can embed your Markdown in a template and parse out front matter.

##### Templates

A powerful and improved alternative to Server-Side Includes, templates allow you to make semi-dynamic sites quickly and easily.

##### Custom Error Pages

Show user-friendly error pages when things go wrong, or write the error details to the browser for dev environments.

##### Logging

Caddy takes copious notes according to your favorite log format. Log errors and requests to a file, stdout/stderr, or a local or remote system log.

##### Request Size Limits

You can limit the size of request bodies that go through Caddy to prevent abuse of your network bandwidth.

##### Timeouts

Enabling timeouts can be a good idea when your server may be prone to slowloris attacks or you want to free up resources from slow networks.

#### Web Protocols

##### HTTP/1.1

Still commonly used in plaintext, development, and debug environments, Caddy has solid support for HTTP/1.1.

##### HTTP/2

It's time for a faster web. Caddy uses HTTP/2 right out of the box. No thought required. HTTP/1.1 is still used when clients don't support HTTP/2.

##### HTTP/3

With the IETF-standard-draft version of QUIC, sites load faster and connections aren't dropped when switching networks.

##### WebSockets

Caddy proxies WebSocket upgrades with ease.

##### IPv6

Caddy supports both IPv4 and IPv6. In fact, Caddy runs full well in an IPv6 environment without extra configuration.

##### FastCGI

Serve your PHP site behind Caddy securely with just one simple line of configuration. You can even specify multiple backends.

#### HTTP Spec

##### Basic Authentication

Protect areas of your site with HTTP basic auth. It's simple to use and secure over HTTPS for most purposes.

##### Redirects

Caddy can issue HTTP redirects with any 3xx status code, including redirects using `<meta>` tags if you prefer.

##### Headers

Customize the response headers so that some headers are removed or others are added.

#### Reverse Proxy

##### Basic Proxying

Caddy can act as a reverse proxy for HTTP requests. You can also proxy transparently (preserve the original Host header) with one line of config.

##### Load Balancing

Proxy to multiple backends using a load balancing policy of your choice: random, least connections, round robin, IP hash, or header.

##### SSL Termination

Caddy is frequently used as a TLS terminator because of its powerful TLS features.

##### WebSocket Proxy

Caddy's proxy middleware is capable of proxying websocket connections to backends as well.

##### Health Checks

Caddy marks backends in trouble as unhealthy, and you can configure health check paths, intervals, and timeouts for optimal performance.

##### Retries

When a request to a backend fails to connect, Caddy will try the request with other backends until one that is online accepts the connection.

##### Header Controls

By default, most headers will be carried through, but you can control which headers flow upstream and downstream.

##### Dynamic Backends

Proxy to arbitrary backends based on request parameters such as parts of the domain name or header values.

#### Amenities

##### Clean URIs

Elegantly serve files without needing the extension present in the URL. These look nicer to visitors and are easy to configure.

##### Rewrites

Caddy has powerful request URI rewriting capabilities that support regular expressions, conditionals, and dynamic values.

##### Response Status Codes

Send a certain status code for certain requests.

##### Compression

Compress content on-the-fly using gzip, Zstandard, or brotli.
