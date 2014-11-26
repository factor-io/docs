---
title: Connectors
category: Factor.io Pro
---
Here we'll look at the values that make up a Connector in Factor.io Pro.

While in **Factor.io Core** we have the `connectors.yml` for configuring the remote Connectors, and `credentials.yml` to specify the credentials needed to connect to those Connectors, in **Factor.io Pro** we use this Connectors functionality to specify the same contents as `connectors.yml` and defining the fields for prompting the user for their credentials.

# Basic description fields
Field | Default | Description
----- | -------- | -----------
Name | | User-friendly name of the connector you are adding (e.g. Github). This will appear on the Credentials page when the user registers this connector.
Description | | User-friendly name of the connector.
Logo URL | | A URL to the Image of the connector logo. A 200x200 px PNG is recommended.
Public | false | If you check this to true, the connector will be available to all organizations on the domain. By default, the Connectors you create are only visible in the organization you are using.
Docs URL | (optional) | A link to the reference docs. We recommend adding your docs to [docs.factor.io](http://docs.factor.io/connectors/overview.html).
Source Code URL | (optional) | If you chose to open-source the connector you can post the link to the source here.

# Connectors Definition
If you are familiar with the **connectors.yml** file from Factor.io Core (you can learn more from [Step 2 of the tutorial](http://docs.factor.io/learn/step_2_your_first_command.html)), then this will be very fimilar.

The contntents of this field is just like connectors.yml, but with a few differences:

- This must be formatted as JSON (not YAML)
- The first node of the hash must match the ID of the service. The ID is inferred from the name; if you are creating 'Github', then your ID is 'github'. After creation you can validate this by getting the ID form the URL.


# Credentials Definition
When running Factor.io Core (`factor s`) the runtime gets the credentials from the `credentials.yml` file and then uses them to connect to the Connector. In Pro we need a way to get the credentials from the users account too. Since it is not defined in a file like credentials.yml, we need to provide a user interface for the user to specify this.


The **Credentials Definition** is a way to specify the list of required credential fields. When the user wants to enable the connector from the Credentials page, they will be prompted to provide the values of these fields.

Field | Description
--- | ---
ID |  The ID used in the connector to get the values (e.g. "api_key")
Name | The user-friendly name of this field (e.g. "API Key")
Description | Provide a meaningful description of how the user can find this vaule. For example, with Github the user needs to specify the API Key. It is useful to provide references to to page continaing those values or ways to retrieve them.
Type | A **String** is a single line entry, like an API Key. A **Text** is a multi-line entry, like an SSH Key.