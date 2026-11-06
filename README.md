# renovate-config

This repository contains shared Renovate configuration used in my own projects.

## Usage

To use the configuration in another repository add the following in the Renovate configuration:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github.com>andrewheberle>renovate-config"]
}
```
