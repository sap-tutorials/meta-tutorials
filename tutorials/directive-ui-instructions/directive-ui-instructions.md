---
parser: v2
auto_validation: false
time: 15
tags: [ tutorial>beginner, software-product>sap-business-technology-platform ]
primary_tag: software-product>sap-business-technology-platform
author_name: SAP Tutorial Team
author_profile: https://github.com/sap-tutorials
---

# UI Instruction Directives
<!-- description --> Demonstrate the UI-instruction directives (`:menu`, `:ui`, `:icon`, `:kbd`, `:ask`) that render consistent, styled references to menu paths, UI controls, icons, and keyboard shortcuts — shown here in a realistic "Configure an SAP BTP Destination" walkthrough.

## You will learn
- How to reference a menu path with `:menu[A > B > C]`
- How to render a UI control (button, field) with `:ui[Label]{type=button|field}`
- How to show an icon with `:icon[name]{label="..."}` and a keyboard shortcut with `:kbd[Mod+S]`
- How to offer an inline follow-up question with `:ask[...]`

## Prerequisites

- An SAP BTP subaccount
- Editor access to the BTP cockpit

---

### Step 1: Open the Destinations panel

1. Log in to the [SAP BTP Cockpit](https://cockpit.btp.cloud.sap) and navigate to your subaccount.

2. In the left navigation, choose :menu[Connectivity > Destinations].

3. Click :ui[New Destination]{type=button} in the toolbar.

:ask[What types of destinations are supported in SAP BTP Connectivity?]

### Step 2: Enter the destination details

1. Fill in the **Name** field. Destination names must be unique within a subaccount and may only contain letters, digits, hyphens, and underscores.

2. Set the **Type** to :ui[HTTP]{type=field}.

3. Enter the URL of your target API in the :ui[URL]{type=field} field.

4. Under **Authentication**, select :ui[BasicAuthentication]{type=field} and provide the credentials.

5. Click :ui[Save]{type=button} to store the destination.

:ask[How do I configure OAuth2 client credentials authentication for a BTP destination?]

### Step 3: Test the connection

1. In the destination list, find your new destination and click the :icon[play]{label="Test Connection"} button to the right of the row.

2. A panel appears with the connection test result. If the test succeeds, the status reads **200 OK**. If it fails, check that the URL is reachable from the BTP connectivity proxy — some on-premise targets require the Cloud Connector.

3. To bind this destination to your CAP application locally, press :kbd[Mod+`] in Business Application Studio to open a terminal and run:

   ```bash
   cds bind --to your-destination-name
   ```

4. Save the binding file with :kbd[Mod+S].

Congratulations — your destination is configured and ready to use from your CAP application via the `@sap/cds` Connectivity API.
