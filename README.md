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
   ./ipns-link-gateway
   ```

4. Open http://www.localhost:8080 at any browser

##### Customize

See available command-line options with: 

```bash
./ipns-link-gateway -h
```

### Cloud-hosting

Suppose your gateway is to have the URL: https://example.com.

##### Heroku

![Pricing](https://img.shields.io/badge/Pricing-Free--Tier-brightgreen) 

1. [Verify account](https://devcenter.heroku.com/articles/account-verification)
2. [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy) 
3. [Edit config_var to](https://devcenter.heroku.com/articles/config-vars#using-the-heroku-dashboard) : `URL: https://example.com`
4. [Add wild-card domain](https://devcenter.heroku.com/articles/custom-domains): `*.example.com`
5. [Manage SSL](https://devcenter.heroku.com/articles/ssl) or [use Cloudflare](https://support.cloudflare.com/hc/en-us/articles/205893698-Configure-Cloudflare-and-Heroku-over-HTTPS)

### Acknowledgements

The `github-corner`s in [index.html](./index.html) is courtesy of [T. Holman](https://tholman.com/github-corners/).

# Contribute

[![Contribute](https://img.shields.io/badge/Contribute%20to-IPNS--Link-brightgreen)](https://github.com/ipns-link/contribute) 

# Donate

[![Sponsor](https://www.buymeacoffee.com/assets/img/custom_images/yellow_img.png)](https://buymeacoffee.com/SomajitDey)
