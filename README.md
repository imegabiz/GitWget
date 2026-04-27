# GitWget

**GitWget** lets you download files from any URL using GitHub Actions – useful when only GitHub is accessible.  
It works on any forked repository, downloads files via `aria2`, optionally packs them into ZIP, and splits large files into chunks under 100MB.

## How to Use (after forking)

### 1. Fork this repository
Click the **Fork** button (top right) to create your own copy.

### 2. Enable write permissions for Actions
In your forked repo:
- Go to **Settings** → **Actions** → **General**
- Under **Workflow permissions**, choose **Read and write permissions**
- Click **Save**

### 3. Enable GitHub Actions (if disabled)
Forks often have Actions disabled by default. To enable:
- Go to the **Actions** tab of your fork
- Click the green button **I understand my workflows, go ahead and enable them** (only needed once)

### 4. Run the workflow
- Go to the **Actions** tab
- Select **Download from User Input** (left sidebar)
- Click **Run workflow** (right side)
- Fill in the form:
  - **file_url** – one or more space‑separated direct URLs
  - **mode** – `normal` (keep files separate) or `zip` (pack all into one archive)
  - **split_size** – chunk size for large files (default `90m`, e.g. `95m`, `100m`)
- Click **Run workflow**

### 5. Get your downloaded files
After the workflow finishes (green checkmark), go to the **Code** tab of your fork.  
All downloaded files appear inside the `downloads/` folder. You can now download the whole repository as ZIP (Code → Download ZIP) or browse individual files.

## Important Notes
- Files larger than the `split_size` are automatically split into `.zip`, `.z01`, `.z02` … parts.  
  To reassemble: use `zip -F archive.zip --out combined.zip` (or any standard ZIP tool).
- The workflow commits files in batches of 5 to avoid GitHub limits.
- No extra files (README, license) are added to the `downloads/` folder – only your downloaded content.

## Credits
Created for environments with restricted internet. Uses `aria2` and standard `zip` tools.
