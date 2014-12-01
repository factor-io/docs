---
title: "Step 7: Best Practices"
category: Build a custom connector
order: 7
---


- When using Factor::Connector.service the name of the service should match the filename
- The connectors should be placed in `lib/factor/connector/` path
- Add strict validation of the parameters. The more validation you can do upfront, the better the error messages will be to the user before the execution continues.
- Tests should be functional/integration, not unit tests.
- Use CodeClimate and Travis to automate tests
- Include badges with build, test, etc status in your Readme.md in the repo.
