# IPNS-Link-gateway

[![PR](https://img.shields.io/badge/PRs-Accepted-green)](https://github.com/ipns-link/ipns-link-gateway/pulls) ![Platform](https://img.shields.io/badge/Platform-GNU%2fLinux-blue.svg) ![Lang](https://img.shields.io/badge/Lang-Bash-cyan.svg) ![UI](https://img.shields.io/badge/UI-Command%20line-orange.svg)

Prototype implementation of [IPNS-Link-gateway](https://github.com/ipns-link/specs).

- You can either run it locally for yourself or host it publicly.
- This implementation redirects almost all requests for static content to an IPFS-Gateway, in order to offload itself.

- This implementation doesn't support HTTP/2. It prefers HTTP/1.1 but can't support persistent connections. To enable HTTP2 and persistent connections, however, you can simply put a capable reverse-proxy in front. If you choose to use [Caddy](https://caddyserver.com/) for this job, which helps with automatic HTTPS too, this project repository contains a ready [Caddyfile](/Caddyfile). See below for a setup guide.
- This implementation is optimized to save as much bandwidth as possible. While idle, viz. serving no requests, the IPFS node at the backend is paused with a SIGSTOP.

**HELP WANTED**: Port the codebase to Go. [Get in touch](https://github.com/ipns-link/contribute#join-the-community) if you're interested. 

## Table of Contents  
[![tocgen](https://img.shields.io/badge/Generated%20using-tocgen-blue)](https://github.com/SomajitDey/tocgen)  
  - [IPNS-Link-gateway](#ipns-link-gateway)  
      - [Local hosting (HTTP)](#local-hosting-http)  
          - [Customize](#customize)  
          - [IPFS Companion](#ipfs-companion)  
      - [Remote hosting (HTTPS)](#remote-hosting-https)  
      - [Cloud hosting](#cloud-hosting)  
          - [Heroku](#heroku)  
          - [EC2 (AWS)](#ec2-aws)  
      - [Public hosting](#public-hosting)  
      - [Contact us whenever needed](#contact-us-whenever-needed)  
  - [Contribute](#contribute)  
  - [Donate](#donate)  
#####   

### Local hosting (HTTP)

1. Download: 

   ```bash
   git clone https://github.com/ipns-link/ipns-link-gateway
   ```

2. Go to the downloaded git-repo:

   ```bash
   cd ipns-link-gateway
   ```

3. Install the non-standard dependencies. The [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks) within this repository can be used here

   ```bash
   bin/compile
   ```

4. Launch: 

   ```bash
   ./ipns-link-gateway -a "AnyAlphanumericPassword"
   ```

5. Note the PID shown. To stop the server later, just do `kill "PID"`

6. Open http://www.localhost:8080 in any browser

##### Customize

See available command-line options with: 

```bash
./ipns-link-gateway -h
```

##### IPFS Companion

If you have [IPFS Companion](https://github.com/ipfs/ipfs-companion) running, you can integrate this gateway with it as follows.

1. Run an IPFS node and thus a local IPFS Gateway at say `localhost:8080`. This is to serve static content.

2. Point your IPNS-Link-gateway to this local IPFS-Gateway as :

   ```bash
   export IPFS_SUBD_GW="http://localhost:8080" IPFS_PATH_GW="http://localhost:8080"
   ```

3. Launch your IPNS-Link-gateway at say port 8000. This is to serve dynamic content.

   ```bash
   ./ipns-link-gateway -p 8000
   ```

4. Point your IPFS Companion to http://localhost:8000 as the local gateway and activate the use of subdomains.

5. To browse an IPNS-Link-exposed site now, you just simply access the URI: `ipns://IPNSNameOfTheSite`

### Remote hosting (HTTPS)

1. Head over to your DNS provider and add to/edit your DNS records as follows:

   | Type  | Name   | Points to                       |
   | ----- | ------ | ------------------------------- |
   | A     | @      | Static public IPv4 of your host |
   | CNAME | www    | @                               |
   | CNAME | ipns   | @                               |
   | CNAME | ipfs   | @                               |
   | CNAME | *.ipns | @                               |
   | CNAME | *.ipfs | @                               |

2. Generate an API token for programatically editing your DNS records.
3. Test if gateway is running locally. This makes sure you've all the dependencies in place. Once tested, kill the gateway using its PID.
4. Open ports 443 (HTTPS) and 80 (HTTP) for incoming traffic.
5. Install [Caddy](https://caddyserver.com/download) for Linux amd64, custom built with your DNS provider's plugin. For example's sake, we shall assume your DNS provider is CloudFlare. **Tip**: Download the binary with `curl -Lo caddy $URL` instead of `wget`. Then,`chmod +x caddy ; sudo install caddy /usr/local/bin`
6. Edit the [crontab file](/crontab) within this repo by putting appropriate values for all the environment variables
7. Set up a cronjob for root as instructed in the crontab file.
8. Launch the script : `sudo /path/to/edited/crontab/file`. This would launch the gateway and caddy with progress logged in `cron.log` and `caddy.log` within the repo.
9. In a browser, test if the gateway is accessible at your domain.

[Let us know](mailto:contact@ipns.live) anytime you're stuck. We'll help.

### Cloud hosting

##### Heroku

1. [Verify account](https://devcenter.heroku.com/articles/account-verification)
2. [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy) 
3. [Edit config_var to](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-dashboard) : `URL: https://domain.tld` and `Password: AnyAlphanumericString`
4. [Add wild-card domain](https://devcenter.heroku.com/articles/custom-domains): `*.domain.tld`
5. [Manage SSL](https://devcenter.heroku.com/articles/ssl) or [use Cloudflare](https://support.cloudflare.com/hc/en-us/articles/205893698-Configure-Cloudflare-and-Heroku-over-HTTPS)

##### EC2 (AWS)

1. Refer to the [EC2 UserGuide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) to set up your instance.
2. Follow the remote hosting guide above.

### Public hosting

If you're hosting a public gateway, please note these.

1. Set up an email id with your purchased domain: `contact@domain.tld`. You might use a free email-forwarding service such as https://improvmx.com/ for this step. Also see https://improvmx.com/guides/send-emails-using-gmail/.
2. Setup your own [payment portal](https://www.buymeacoffee.com/).
3. Edit the html files to reflect your contact details and payment portals.
4. Create an issue at the [official public-gateway-registry](https://github.com/ipns-link/gateway-registry) to let us know of your gateway.
5. If you receive any request to block specific sites from being accessed using your public gateway please [let us know](https://github.com/ipns-link/gateway-registry).
6. You might experience some OOM (out-of-memory) related outage from time to time. Periodic restarts can avoid this for now. Hopefully, this will be fixed soon.

### Contact us whenever needed

Email: [contact@ipns.live](mailto:contact@ipns.live)

[Other channels](https://github.com/ipns-link/contribute#join-the-community)

# Contribute

[![Contribute](https://img.shields.io/badge/Contribute%20to-IPNS--Link-brightgreen)](https://github.com/ipns-link/contribute) 

# Donate

We cannot sustain this project and our public gateways without your support.

[![Donate](https://img.shields.io/badge/Donate%20to-IPNS--Link-brightgreen)](https://github.com/ipns-link/contribute/blob/main/donate.md)


