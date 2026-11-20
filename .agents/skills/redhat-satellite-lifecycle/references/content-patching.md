# Content & Patching — Red Hat Satellite

## Content View Investigation

1. Check CV version: `hammer content-view version list --content-view "NAME"`.
2. Check promotion state: which Lifecycle Environment has which version.
3. Check Composite CV dependencies — all component CVs must be published.
4. Never assume package is available just because Product exists.

## Repository Sync Issues

1. Check sync status: `hammer repository info --id <id>`.
2. Check sync errors in task output.
3. Common causes: upstream CDN down, proxy config, space exhaustion.
4. Check sync plan schedule.

## Patching Workflow

```
Repository synced → CV published → CV promoted → AK assigns CV
  → Host has enabled repos → Erratum available → Apply patch
```

Each step must be verified independently.

## Package Not Available

1. Is the repository enabled for the Product?
2. Is the repository synced with latest content?
3. Is the CV version that contains the package published?
4. Is the CV promoted to the host's Lifecycle Environment?
5. Does the AK attach the correct CV?
6. Is the repo enabled on the host? (`subscription-manager repos --list-enabled`)

## Evidence

```bash
hammer content-view list --organization "ORG"
hammer repository list --organization "ORG"
hammer erratum list --content-view "NAME" --content-view-version-id <id>
```
