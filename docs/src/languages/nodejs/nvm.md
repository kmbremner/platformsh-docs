---
title: "Alternative Node.js install"
weight: 1
toc: false
---

## How to use NVM to run different versions of Node.js

[Node Version Manager](https://github.com/creationix/nvm) or NVM is a tool for managing multiple versions of Node.js in one installation.

You can use NVM with any of our container types that have node installed to change or update the version. This may be useful, for example, where a container has a Long Term Release (LTS) version available, but you would like to use the latest.

Installing NVM is done in the build hook of your `.platform.app.yaml`, which some additional calls to ensure that environment variables are set correctly.

```yaml
variables:
    env:
        # Update these for your desired NVM and Node versions.
        NVM_VERSION: v0.35.2
        NODE_VERSION: 14

hooks:
    build: |
        unset NPM_CONFIG_PREFIX
        curl -o- https://raw.githubusercontent.com/creationix/nvm/$NVM_VERSION/install.sh | dash
        export NVM_DIR="$PLATFORM_APP_DIR/.nvm"
        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
        nvm install $NODE_VERSION
 ```

And in a `.environment` file in the root of your project:

```bash
# This is necessary for nvm to work.
unset NPM_CONFIG_PREFIX
# Disable npm update notifier; being a read only system it will probably annoy you.
export NO_UPDATE_NOTIFIER=1
# This loads nvm for general usage.
export NVM_DIR="$PLATFORM_APP_DIR/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```
