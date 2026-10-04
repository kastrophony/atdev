---
name: deploy-atdev
description: atdev deployment
---

## deploy

- You must request from the user a root domain to use for the stack if not provided
- Configure dns records if you have the ability or ask the user to create them. Provide them a list of records needed to work with the deployment target platform
- Follow the instructions in the README
- Don't expose to the public internet

## health

- Check the services are running and healthy
- Verify the relay crawls the pds with `getHostStatus` (`status` should be `active`).
