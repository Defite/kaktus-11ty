---
title: HTTPS on localhost
date: 2022-09-17
layout: layouts/post.njk
---

If for some reason you need to add ssl certificate to your localhost project, you can do the following steps to achieve that.

### Install and use `mkcert`

```bash
# Install mkcert from Homebrew
brew install mkcert

# Create new local CA (Certificate Authority)
mkcert -install

# Create certificate for localhost
mkcert localhost
```

### Install and use `local-ssl-proxy`

```bash
# Use npm (or yarn, whatever) to install package
npm install -g local-ssl-proxy

# Start your local ssl proxy
local-ssl-proxy --source 3010 --target 3000 --cert localhost.pem --key localhost-key.pem
```

This will proxy all requests in your project through `https://localhost:3010`.

