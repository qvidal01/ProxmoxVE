# 🏷️ Setup New Labels

The autolabeler now uses additional size labels. These need to be created in your repository.

## Required Labels

### Size Labels (New)

These labels are automatically added based on the number of lines changed:

| Label      | Lines Changed | Color     | Description                   |
| ---------- | ------------- | --------- | ----------------------------- |
| `size: XS` | 1-10          | `#00ff00` | Extra Small - Minimal changes |
| `size: S`  | 11-50         | `#7fff00` | Small - Minor changes         |
| `size: M`  | 51-200        | `#ffff00` | Medium - Moderate changes     |
| `size: L`  | 201-500       | `#ff8c00` | Large - Significant changes   |
| `size: XL` | 500+          | `#ff0000` | Extra Large - Major changes   |

## Quick Setup Commands

Run these commands in your terminal to create all labels at once:

```bash
# Create size labels
gh label create "size: XS" --description "Extra Small - 1-10 lines changed" --color "00ff00"
gh label create "size: S" --description "Small - 11-50 lines changed" --color "7fff00"
gh label create "size: M" --description "Medium - 51-200 lines changed" --color "ffff00"
gh label create "size: L" --description "Large - 201-500 lines changed" --color "ff8c00"
gh label create "size: XL" --description "Extra Large - 500+ lines changed" --color "ff0000"
```

## Manual Setup (GitHub UI)

1. Go to: `https://github.com/community-scripts/ProxmoxVE/labels`
2. Click "New label" for each label
3. Fill in:
   - **Name**: Use exact names from table above
   - **Description**: Copy from table
   - **Color**: Use hex codes from table (without #)

## Verification

After creating labels, test by:

1. Creating a small PR (< 10 lines) → should get `size: XS`
2. Check that bot comment includes size in footer

## Optional: Label Sync

For automated label management, consider using [github-label-sync](https://github.com/Financial-Times/github-label-sync):

```json
{
  "size: XS": {
    "color": "00ff00",
    "description": "Extra Small - 1-10 lines changed"
  },
  "size: S": {
    "color": "7fff00",
    "description": "Small - 11-50 lines changed"
  },
  "size: M": {
    "color": "ffff00",
    "description": "Medium - 51-200 lines changed"
  },
  "size: L": {
    "color": "ff8c00",
    "description": "Large - 201-500 lines changed"
  },
  "size: XL": {
    "color": "ff0000",
    "description": "Extra Large - 500+ lines changed"
  }
}
```

---

**Note**: The autolabeler will work even if labels don't exist yet, but they won't be visible on PRs until created.
