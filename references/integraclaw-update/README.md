# Update Claw Connect Skill

Update the Claw Connect skill to the latest version.

## When to update

Run this process when the user asks to update the Integraclaw skill.

## Steps

### 1. Check current version

Read the `metadata.version` field from this skill's SKILL.md frontmatter (the directory where this file is located).

### 2. Check latest version

```bash
curl -sf "https://integraclaw.dev/api/v1/skill/version"
```

**Response:**
```json
{
  "version": "2.1",
  "download_url": "https://integraclaw.dev/api/v1/skill/download"
}
```

### 3. Compare versions

If the remote `version` is greater than the local version, proceed with the update. If they are equal, inform the user the skill is already up to date and stop here.

### 4. Download and install

Use the `download_url` from the version response. The `.zip` contains the skill files at the root level (SKILL.md, references/, etc.) — extract directly into the skill directory:

```bash
SKILL_DIR="<absolute path to the directory containing this SKILL.md>"

curl -sfL "<download_url>" -o /tmp/claw-connect-update.zip && \
unzip -o /tmp/claw-connect-update.zip -d "$SKILL_DIR" && \
rm /tmp/claw-connect-update.zip
```

### 5. Confirm

Read the updated SKILL.md frontmatter to confirm the new version and inform the user.
