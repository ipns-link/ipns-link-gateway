# IPNS-Link-gateway

[![PR](https://img.shields.io/badge/PRs-Accepted-green)](https://github.com/ipns-link/ipns-link-gateway/pulls) ![Platform](https://img.shields.io/badge/Platform-GNU%2fLinux-blue.svg) ![Lang](https://img.shields.io/badge/Lang-Bash-cyan.svg) ![UI](https://img.shields.io/badge/UI-Command%20line-orange.svg)

Prototype implementation of [IPNS-Link-gateway](https://github.com/ipns-link/specs).

## Table of Contents  
[![tocgen](https://img.shields.io/badge/Generated%20using-tocgen-blue)](https://github.com/SomajitDey/tocgen)  
  - [IPNS-Link-gateway](#ipns-link-gateway)  
      - [Self-hosting](#self-hosting)  
          - [Customize](#customize)  
      - [Cloud-hosting](#cloud-hosting)  
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

5. Open http://www.localhost:8080 at any browser

##### Customize

See available command-line options with: 

```bash
./ipns-link-gateway -h
```

### Cloud-hosting

Suppose your gateway is to have the domain `domain.tld`. So, first of all, purchase the domain and familiarize yourself with the DNS manager console of your domain registrar / DNS provider.

**Note**: We can do without an A record, simply by forwarding `domain.tld` to the canonical URL: http(s)://www.domain.tld at the DNS manager console.

##### Heroku

![Pricing](https://img.shields.io/badge/Pricing-Free--Tier-brightgreen) 

1. [Verify account](https://devcenter.heroku.com/articles/account-verification)
2. [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy) 
3. [Edit config_var to](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-dashboard) : `URL: https://domain.tld` and `Password: AnyAlphanumericString`
4. [Add wild-card domain](https://devcenter.heroku.com/articles/custom-domains): `*.domain.tld`
5. [Manage SSL](https://devcenter.heroku.com/articles/ssl) or [use Cloudflare](https://support.cloudflare.com/hc/en-us/articles/205893698-Configure-Cloudflare-and-Heroku-over-HTTPS)

##### EC2 (AWS)

![Pricing](https://img.shields.io/badge/Pricing-Free--Tier-brightgreen) 

To make sure you don't use any paid AWS services, make sure you stick to the following, if you don't know what you are doing. The following uses EC2 which is free for 1 year only with 750 hours/month. If you are stuck following any instruction below, or need help, refer to the [EC2 UserGuide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html).

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

10. Choose "Launch".

11. When prompted for a key pair, select "Choose an existing key pair", then select the key pair that you created when getting set up. When you are ready, select the acknowledgement check box, and then choose "Launch Instances".

12. [Get the public DNS name of the instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html).

13. Leaving the instance to launch, head over to your DNS provider and add the following CNAME record: `Type: CNAME ; Host: * ; Points to: your-ec2-public-DNS-name`.

14. Locate the private key you downloaded earlier and SSH to your instance as 

    ```bash
    ssh -i your-private-key-file ubuntu@www.domain.tld
    ```

15. Download the project repo and move into it 

    ```bash
    git clone https://github.com/ipns-link/ipns-link-gateway; cd ipns-link-gateway
    ```

16. Install the dependencies. The Heroku buildpack can actually help you here 😉

    ```bash
    bin/compile
    ```

17. Launch the Gateway at port 80

    ```bash
    sudo ./ipns-link-gateway -a "AlphanumericPassword" -p 80 "http://domain.tld"
    ```

18. Note the PID shown. To stop the server later just do `sudo kill "PID"`

19. The Gateway can now be availed at http://www.domain.tld.

20. If you want https, either use the free-tier of [CloudFlare](https://support.cloudflare.com/hc/en-us/articles/205893698-Configure-Cloudflare-and-Heroku-over-HTTPS) or the always free [Let's Encrypt](https://letsencrypt.org/getting-started/).

### Acknowledgements

The `github-corner`s in [index.html](./index.html) is courtesy of [T. Holman](https://tholman.com/github-corners/).

# Contribute

[![Contribute](https://img.shields.io/badge/Contribute%20to-IPNS--Link-brightgreen)](https://github.com/ipns-link/contribute) 

# Donate

[![Sponsor](https://www.buymeacoffee.com/assets/img/custom_images/yellow_img.png)](https://buymeacoffee.com/SomajitDey)
