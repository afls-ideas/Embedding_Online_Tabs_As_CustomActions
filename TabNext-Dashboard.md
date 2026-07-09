# TabNext Dashboard — Home Screen Custom Action

This guide documents how to add the **TabNext Dashboard** custom action to the LSC mobile
app **Home screen**, opening an embedded Tableau dashboard from the floating action
button (FAB).

Targets an LSC4CE org (API version 66.0). Replace the `your-my-domain` host in the URLs
below with your org's My Domain.

The working metadata is included under
[`force-app/`](./force-app/main/default/lifeSciConfigRecords/CustomAction_TabNextDashboard.lifeSciConfigRecord-meta.xml).

---

## What we are building

A tappable **TabNext Dashboard** action on the Home screen that opens the Tableau
`SP_Analyzer_Dashboard` inside an in-app webview.

![TabNext Dashboard custom action opening an embedded Tableau dashboard on the Home screen](./assets/Embedded_TabNext.gif)

Because it lives on the Home screen, there is **no record in context** — it is a plain
static URL, so no parameters, no QuickAction, and no page-layout change are required.

---

## Key concepts

A Home-screen LSC custom action needs only **one** metadata component:

| Piece | Purpose |
|-------|---------|
| **`LifeSciConfigRecord`** (Custom Action) | Defines the action's label, URL, and target behavior. Managed via the Admin Console or the LSC MCP tools. |

> Home-page custom actions render directly on the FAB — **no QuickAction and no layout
> change required**. (SObject/record actions are different: they additionally need a
> QuickAction backed by `placeholderLwc` and a page-layout entry.)

### Target Type

| Value | Behavior |
|-------|----------|
| `Inline` | Opens the URL in an in-app modal webview. **Used here.** |
| `External` | Opens in the device browser (Safari). |
| `Internal` | Full-screen navigation within the app. Does **not** work for some Lightning `/lightning/n/...` pages, which can return a "content is blocked" error. |

> **Note:** `Inline` is not offered by the `create_custom_action` MCP tool (which only
> accepts `Internal`/`External`). If creating via the tool, create the action as
> `External`, then update `TargetType` to `Inline`.

---

## Configuration values

**Custom Action** (`CustomAction_TabNextDashboard`)

| Field | Value |
|-------|-------|
| Label (`masterLabel`) | `TabNext Dashboard` |
| Entity Type | `HomePage` |
| Action Type | `URL` |
| Action Target | `https://your-my-domain.lightning.force.com/tableau/dashboard/SP_Analyzer_Dashboard/view` |
| Target Type | `Inline` |
| Active | `true` |

### Metadata

[`force-app/main/default/lifeSciConfigRecords/CustomAction_TabNextDashboard.lifeSciConfigRecord-meta.xml`](./force-app/main/default/lifeSciConfigRecords/CustomAction_TabNextDashboard.lifeSciConfigRecord-meta.xml):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<LifeSciConfigRecord xmlns="http://soap.sforce.com/2006/04/metadata">
    <fieldValues>
        <dataType>PICKLIST</dataType>
        <fieldName>EntityType</fieldName>
        <hasBooleanValue>false</hasBooleanValue>
        <picklistValue>HomePage</picklistValue>
    </fieldValues>
    <fieldValues>
        <dataType>PICKLIST</dataType>
        <fieldName>TargetType</fieldName>
        <hasBooleanValue>false</hasBooleanValue>
        <picklistValue>Inline</picklistValue>
    </fieldValues>
    <fieldValues>
        <dataType>TEXT</dataType>
        <fieldName>ActionTarget</fieldName>
        <hasBooleanValue>false</hasBooleanValue>
        <textValue>https://your-my-domain.lightning.force.com/tableau/dashboard/SP_Analyzer_Dashboard/view</textValue>
    </fieldValues>
    <fieldValues>
        <dataType>PICKLIST</dataType>
        <fieldName>ActionType</fieldName>
        <hasBooleanValue>false</hasBooleanValue>
        <picklistValue>URL</picklistValue>
    </fieldValues>
    <isActive>true</isActive>
    <isOrgLevel>false</isOrgLevel>
    <lifeSciConfigCategory>CustomAction</lifeSciConfigCategory>
    <masterLabel>TabNext Dashboard</masterLabel>
</LifeSciConfigRecord>
```

---

## Steps

1. **Create the custom action** — either:
   - **Admin Console** → *Quick and Custom Action Administration* → *Custom Actions* → set
     Label `TabNext Dashboard`, Entity Type `HomePage`, Action Type `URL`, Action Target
     the Tableau URL above, Target Type `Inline`; **or**
   - **Deploy the metadata** — place the `.lifeSciConfigRecord-meta.xml` under a
     `lifeSciConfigRecords/` folder in an SFDX project and deploy:
     ```bash
     sf project deploy start --source-dir force-app --target-org <your-org-alias>
     ```
2. **Set Target Type = Inline** (if you created via the MCP tool, which defaults to
   `External`).
3. **Regenerate the mobile metadata cache** for the relevant profile(s).
4. **Hard-sync the device** — fully close and reopen the app (a pull-to-refresh is not
   always enough).

---

## Deploy checklist

- [ ] `CustomAction_TabNextDashboard` — HomePage / URL / Inline / Tableau dashboard URL
- [ ] Action **Active** = true
- [ ] **Regenerate** mobile metadata cache for the profile(s)
- [ ] **Hard-sync** the device (fully close + reopen)
- [ ] **Verify** the action appears on the Home FAB and the dashboard renders

---

## Troubleshooting

| Symptom | Cause / Fix |
|---------|-------------|
| Action does not appear on the Home FAB | Action inactive, cache not regenerated, or device not hard-synced. Set Active = true, regenerate cache, hard-sync. |
| **"This content is blocked" in the webview** | Some Lightning Experience pages (`*.lightning.force.com/...`) do not load inside the in-app `Inline` webview. **Fix:** switch `TargetType` to `External` so the dashboard opens in the device browser. |
| Tap does nothing | Relative path in Action Target, or an unreachable URL. Use a full `https://` URL. |
| Changes not reflected on device | Cache not regenerated, or device not hard-synced. Regenerate cache and fully close/reopen the app. |

---

## Reference: configured values

| Component | Developer Name | Key values |
|-----------|----------------|------------|
| Home action | `CustomAction_TabNextDashboard` | HomePage · URL · Inline · `https://your-my-domain.lightning.force.com/tableau/dashboard/SP_Analyzer_Dashboard/view` |
