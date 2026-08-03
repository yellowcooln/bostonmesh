---
id: meshcore-regions
title: MeshCore Regions
sidebar_label: MeshCore Regions
---

# MeshCore Regions for Repeaters

Region management is for **repeater deployments**. The New England region plan is now stable enough for Boston metro and Eastern Massachusetts repeater operators to begin configuring regions while continuing to allow wildcard (`*`) flood traffic.

Using consistent regions helps with:

- Keeping flood behavior predictable
- Making repeater intent clear to other operators
- Coordinating regional deployment decisions

## Find the regions for your repeater

Use the [interactive New England Mesh region map](https://newenglandme.sh/regions/map) to find the region codes for a repeater's GPS location. Click the repeater's location on the map to see which regions should be configured.

For an explanation of the map, boundary sources, region status, and repeater commands, see the [New England Mesh region guide](https://newenglandme.sh/regions/).

### Boston metro and Eastern Massachusetts

Repeaters in Boston metro and Eastern Massachusetts should use:

- `bos` (Boston / Eastern Massachusetts)
- `northeast` (Northeast region)
- `east` (East Coast region)

The `bos` boundary is a high-level outline and may receive minor adjustments as deployment needs become clearer. New England's neighboring region plans are also firming up; the broader regions that currently overlap Boston / Eastern Massachusetts are `northeast` and `east`.

If you are outside this area or near a boundary, use the [region map](https://newenglandme.sh/regions/map) rather than assuming a region from the state alone.

## How scope forwarding works

Each channel can have a scope. If a message is sent on a scoped channel, that packet carries the same scope.

A repeater will only forward that packet when:

- The repeater has that region configured
- Flooding is allowed for that region

Important caveat: every repeater on the path must have the same region configured for forwarding to continue end-to-end.

Example:

- If someone sends on a channel with `bos` scope, every repeater along the route needs `bos` configured and allowed.
- Traffic scoped to `northeast` or `east` can only continue along paths whose repeaters have the matching region configured and allowed.

## Rollout note for flood permissions

Continue to allow flood traffic on `*` for now. Do not disable wildcard flooding when adding the new regions.

As region adoption increases, operators may begin disabling wildcard flooding or reducing hop counts for unscoped floods. That transition is expected to happen gradually over the coming weeks or months rather than immediately.

Optionally, an operator can restrict unscoped flood packets to a suggested maximum of 12 hops:

```
set flood.max.unscoped 12
```

These settings can be changed remotely over the mesh after a repeater is deployed.

## How to set region in the UI

1. Open the MeshCore app.
2. Connect to your companion.
3. Log into the repeater as admin.
4. Go to `Settings`.
5. Select `Manage Regions`.
6. Click the add button at the top right.
   ![Manage Regions screen](./assets/region-1.png)
7. Enter `bos` and click the check mark at the top right. Repeat this to add `northeast` and `east`.
8. Click the 3 dots next to `bos` and select `Allow Flood`. Do the same for `northeast` and `east`. Keep flood allowed for `*`.
9. Click the check box to confirm region settings.

## How to set allowed regions (CLI)

For an Eastern Massachusetts or Boston repeater, run:

```
region put bos
region put northeast
region put east

region allowf bos
region allowf northeast
region allowf east

region save
```

Keep wildcard flooding enabled during the rollout. If `*` is not already allowed, run:

```
region allowf *
region save
```

Optional verification:

```
region list allowed
```

Confirm that `bos`, `northeast`, `east`, and `*` appear in the allowed list.

## Questions and coordination

- Ask local questions in the [Greater Boston Mesh Discord channel](https://discord.com/channels/1380981251200254107/1399550558523887616).
- Follow broader regional coordination in the [New England Mesh Discord channel](https://discord.com/channels/1515187762771263598/1515207194457673808).
- Check the [interactive New England Mesh region map](https://newenglandme.sh/regions/map) before configuring a repeater, especially near a boundary.

For the full region command list, see the official MeshCore CLI reference:
[https://docs.meshcore.io/cli_commands/](https://docs.meshcore.io/cli_commands/)
