# IPNS-Link-gateway

[![PR](https://img.shields.io/badge/PRs-Accepted-green)](https://github.com/ipns-link/ipns-link-gateway/pulls) ![Platform](https://img.shields.io/badge/Platform-GNU%2fLinux-blue.svg) ![Lang](https://img.shields.io/badge/Lang-Bash-cyan.svg) ![UI](https://img.shields.io/badge/UI-Command%20line-orange.svg)

Prototype implementation of [IPNS-Link-gateway](https://github.com/ipns-link/specs).

- You can either run it [locally](#self-hosting) for yourself or host it [publicly](#cloud-hosting).
- This implementation redirects almost all requests for static content to an IPFS-Gateway, in order to offload itself.

- This implementation doesn't support HTTP/2. It prefers HTTP/1.1 but can't support persistent connections 🥺. To enable HTTP2 and persistent connections, however, you can simply put a capable reverse-proxy in front 🤓. If you choose to use [Caddy](https://caddyserver.com/) for this job, which helps with automatic HTTPS too btw, this project repository contains a ready [Caddyfile](/Caddyfile).
- This implementation is optimized to save as much bandwidth as possible. While idle, viz. serving no requests, the IPFS node at the backend is paused with a SIGSTOP to stop it from consuming bandwidth.

**HELP WANTED**: Port the codebase to Go, JS, Python etc. - anything speedier than Bash 😝. [Get in touch](https://github.com/ipns-link/contribute#join-the-community) if you're interested.

## Table of Contents  
[![tocgen](https://img.shields.io/badge/Generated%20using-tocgen-blue)](https://github.com/SomajitDey/tocgen)  
  - [IPNS-Link-gateway](#ipns-link-gateway)  
      - [Self-hosting](#self-hosting)  
          - [Customize](#customize)  
          - [IPFS Companion](#ipfs-companion)  
      - [Cloud-hosting](#cloud-hosting)  
          - [General notes](#general-notes)  
          - [Prerequisites](#prerequisites)  
          - [Heroku](#heroku)  
          - [EC2 (AWS)](#ec2-aws)  
      - [Acknowledgements](#acknowledgements)  
  - [Contribute](#contribute)  
  - [Donate](#donate)  
#####   

### Self-hosting

1. Download: 

   ```bash
   git clone https://github.com/ipns-link/ipns-link-gateway
   ```

2. Go to the downloaded git-repo:

   ```bash
   cd ipns-link-gateway
   ```

3. Launch: 

   ```bash
   ./ipns-link-gateway -a "AnyAlphanumericPassword"
   ```

4. Note the PID shown. To stop the server later just do `kill "PID"`

5. Open http://www.localhost:8080 in any browser

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

### Cloud-hosting

Suppose your gateway is to have the URL: `https://domain.tld`. Just follow the [prerequisites](#prerequisites) and then carry on with your cloud setup and deployment. We provide guides for [Heroku](#heroku) and [AWS](#ec2-aws) only.

##### General notes

**Known issue**: You might experience some OOM (out-of-memory) related outage from time to time. Periodic restarts can avoid this for now. Hopefully this will be fixed soon.

**Blocking offensive content**: If you receive any request to block specific sites from being accessed using your public gateway please [let us know](https://github.com/ipns-link/gateway-registry).

##### Prerequisites

1. Purchase the domain and familiarize yourself with the DNS manager console of your domain registrar / DNS provider. Create and store an API token for your DNS provider.
2. Set up an email id with your purchased domain: `contact@domain.tld`. You might use a free email-forwarding service such as https://improvmx.com/ for this step. Also see https://improvmx.com/guides/send-emails-using-gmail/.
3. Setup your own [payment gateway](https://www.buymeacoffee.com/).
4. Edit the [index.html](/index.html) as instructed therein.
5. Create an issue at the [official public-gateway-registry](https://github.com/ipns-link/gateway-registry) to let us know of your gateway.

##### Heroku

![Pricing](https://img.shields.io/badge/Pricing-Free--Tier-brightgreen) 

1. [Verify account](https://devcenter.heroku.com/articles/account-verification)
2. [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy) 
3. [Edit config_var to](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-dashboard) : `URL: https://domain.tld` and `Password: AnyAlphanumericString`
4. [Add wild-card domain](https://devcenter.heroku.com/articles/custom-domains): `*.domain.tld`
5. [Manage SSL](https://devcenter.heroku.com/articles/ssl) or [use Cloudflare](https://support.cloudflare.com/hc/en-us/articles/205893698-Configure-Cloudflare-and-Heroku-over-HTTPS)

##### EC2 (AWS)

![Pricing](https://img.shields.io/badge/Pricing-Free--Tier-brightgreen) 

The following uses EC2 which is free for 1 year only with 750 hours/month. If you are stuck following any instruction below, or need further details, refer to the [EC2 UserGuide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html).

**Aim**: The goal is to run the gateway at EC2, as a backend/upstream of a [Caddy](https://caddyserver.com/) reverse-proxy. Caddy would take care of HTTPS and everything else a modern server needs. If you don't know what all these mean, fear not. Just follow the steps below.

1. [Sign up](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/).

2. [Go to Services and then EC2](https://console.aws.amazon.com/ec2/).

3. [Select the region you want to run the Gateway in](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/select-region.html).

4. [Create your key-pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair). [Download the key file and change permissions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html#connection-prereqs-private-key).

5. Choose "Launch an instance".

6. Choose an Amazon Machine Image (AMI) : The Free-tier-eligible Ubuntu 20.04 amd64.

7. Choose an Instance Type : The Free-tier-eligible t2.micro.

8. On the "Choose an Instance Type" page, choose "Review and Launch" to let the wizard complete the other configuration settings for you.

9. On the "Review Instance Launch" page, under "Security Groups", you'll see that the wizard created and selected a security group for you. Edit the inbound-rules to contain: 

   | Protocol | Port | IP                            |
   | -------- | ---- | ----------------------------- |
   | SSH      | 22   | Choose as appropriate for you |
   | HTTP     | 80   | 0.0.0.0/0                     |
   | HTTPS    | 443  | 0.0.0.0/0                     |

10. Choose "Launch".

11. When prompted for a key pair, select "Choose an existing key pair", then select the key pair that you created when getting set up. When you are ready, select the acknowledgement check box, and then choose "Launch Instances".

12. [Get the static public IPv4 address or public DNS name of the instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html).

13. Leaving the instance to launch, head over to your DNS provider and add to/edit your DNS records as follows:

    | Type  | Name   | Points to                           |
    | ----- | ------ | ----------------------------------- |
    | A     | @      | Static public IPv4 of your instance |
    | CNAME | www    | @                                   |
    | CNAME | ipns   | @                                   |
    | CNAME | ipfs   | @                                   |
    | CNAME | *.ipns | @                                   |
    | CNAME | *.ipfs | @                                   |

    **NOTE**: Restarting your EC2 instance doesn't change the public IP address. But everything else, such as starting/stopping or terminating and creating a new instance, does. Whenever you do such things that change the IP, simply update the IP in your DNS records.

14. Locate the private key you downloaded earlier and SSH to your instance as 

    ```bash
    ssh -i your-private-key-file ubuntu@www.domain.tld
    ```

15. Download the project repo and **move into it** 

    ```bash
    git clone https://github.com/ipns-link/ipns-link-gateway; cd ipns-link-gateway
    ```

16. Install the dependencies. The [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks) within this repository can help you here 😉

    ```bash
    bin/compile
    ```

17. Install [Caddy](https://caddyserver.com/download) for Linux amd64, custom built with your DNS provider's plugin. For example's sake, we assume your DNS provider is CloudFlare.

18. **Change to root**

    ```bash
    sudo su root
    ```

19. Launch the Gateway

    ```bash
    ./ipns-link-gateway -a "AlphanumericPassword" "https://domain.tld"
    ```

    Wait for the prompt to return.

20. Note the PID shown. To stop the server later you can simply execute: `sudo kill "PID"`.

21. **Set the environment** 

    ```bash
    export DOMAIN="gateway.tld" DNS_PROV="cloudflare" API_TKN="Your-API-Token"
    ```

22. Start Caddy

    ```bash
    caddy start
    ```

    To stop `caddy` later you just have to execute: `caddy stop`.

23. Caddy requests, installs and manages all the required SSL certificates on its own. The certificates are issued for free by [Let's Encrypt](https://letsencrypt.org/getting-started/) and [ZeroSSL](https://zerossl.com/). The Gateway can now be availed at https://www.domain.tld.

24. To update and restart the gateway and caddy periodically, and on reboot, create a [Cron job](https://opensource.com/article/17/11/how-use-cron-linux). You may use the crontab [file](/crontab) from this repo, after editing it as instructed therein. It logs in a `cron.log` file within this repo. Caddy logs in `caddy.log`.

25. Create a free uptime monitor with say [UptimeRobot](https://uptimerobot.com/) to get notified whenever your server goes down.

### Acknowledgements

The GitHub-corners in [index.html](./index.html) is courtesy of [T. Holman](https://tholman.com/github-corners/).

# Contribute

[![Contribute](https://img.shields.io/badge/Contribute%20to-IPNS--Link-brightgreen)](https://github.com/ipns-link/contribute) 

# Donate

[![Sponsor](https://www.buymeacoffee.com/assets/img/custom_images/yellow_img.png)](https://buymeacoffee.com/SomajitDey)
