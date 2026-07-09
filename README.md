# AFLS Quick and Custom Actions

Guides and working metadata for configuring Quick Actions and Custom Actions in
Life Sciences Cloud (LSC) mobile — for the Home screen.  This is for customers that want to launch some functionality available online, but do not want their users leaving the AFLS app.  That is, keep the users in the flow of work.

### Use cases

1. **Tableau Next / Tableau** — launch a Tableau dashboard as a modal from the Home screen.
2. **Salesforce Maps** — launch the Salesforce Maps app as a modal from the Home screen.
3. **Other AFLS tabs that are online only** — surface online-only AFLS to be launched directly from the mobile app
4. **LWCs that need to be full screen** — e.g. launch custom apps that customers have built.
5. **External 3rd-party apps that cannot be embedded in any iframe** — open them in a
   browser-level window rather than an embedded frame.


> **⚠️ Disclaimer**
>
> This is **not** an official Salesforce repository, and it is not supported by
> Salesforce. All information, guides, and metadata are provided **AS IS**, without
> warranty of any kind, express or implied. Customers must perform their own due
> diligence and testing before using any of this content in a production org. Use at
> your own risk.

![TabNext Dashboard custom action opening an embedded Tableau dashboard on the Home screen](./assets/Embedded_TabNext.gif)

## Guides

| Guide | Description |
|-------|-------------|
| [TabNext Dashboard](./TabNext-Dashboard.md) | Add a Home-screen custom action that opens an embedded Tableau dashboard (Inline webview). |
| [Sample Inventory Management](./Sample-Inventory-Management.md) | Add a Home-screen custom action (**Sample IM**) that opens the LSC Sample Management landing page (Inline webview). |
| [SF Maps](./SF-Maps.md) | Add a Home-screen custom action (**SF Maps**) that opens the Salesforce Maps app (Inline webview). |
| [Patient Journey](./Patient-Journey.md) | Add a Home-screen custom action (**Patient Journey**) that opens the Patient Journey Simulator (Inline webview). |

_More Quick Action guides will be added here over time._

## What the Custom Action looks like

To view or edit a custom action, navigate to **Admin Console > Quick and Custom Action
Administration > Custom Actions**.

![Edit Custom Action dialog in the Admin Console, showing the TabNext Dashboard custom action](./assets/CustomAction_EditView_Admin_Console.png)

> **Note:** The **Target Type** field shows up blank ("Select an Option") because when we
> deploy, we set it to `Inline`. This will be corrected in the future to accept the value
> of `Internal`.

## Advanced configuration

This is a more advanced configuration. The **Target Type** cannot currently be set to
`Inline` through the Admin Console UI — the configurator must update the metadata Target
Type to `Inline` directly. This is done by **deploying the available `LifeSciConfigRecord`
XML files** in [`force-app/`](./force-app/main/default/lifeSciConfigRecords/):

```bash
sf project deploy start --source-dir force-app --target-org <your-org-alias>
```

After deploying, regenerate the mobile metadata cache for the relevant profile(s) and
hard-sync the device.

## Repository layout

```
TabNext-Dashboard.md                 ← TabNext dashboard guide
Sample-Inventory-Management.md       ← Sample IM (Sample Inventory Management) guide
SF-Maps.md                           ← SF Maps guide
Patient-Journey.md                   ← Patient Journey guide
assets/                              ← images / GIFs used by the guides
force-app/                           ← deployable SFDX source (lifeSciConfigRecords)
```
