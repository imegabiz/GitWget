# GitWget

**GitWget** lets you download files from any URL using GitHub Actions – useful when only GitHub is accessible.  
It works on any forked repository, downloads files via `aria2` with parallel connections, optionally packs them into ZIP, automatically splits large files (when committing to repo), or uploads directly to GitHub Release (bypassing file size limits).

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

| Input | Description |
|-------|-------------|
| **file_url** | One or more direct URLs, separated by spaces. Example: `https://example.com/file1.zip https://example.com/file2.jpg` |
| **mode** | `normal` – keep each file as is (or split if needed). `zip` – pack all downloaded files into a single zip archive before upload. |
| **upload_target** | `commit` – push files to the repository (max ~100MB per file, automatically splits large files into chunks). `release` – create a GitHub Release with files (up to 2GB per file, no splitting needed). |
| **split_size** | Only used when `upload_target = commit`. Splits any file larger than this size into zip parts (e.g., `90m`, `95m`, `100m`). Default `90m`. |
| **connections** | Number of parallel connections per URL for faster downloads (1 to 16). Default `4`. |
| **custom_filename** | Optional. Only works when downloading a **single URL**. Overwrites the output filename. Leave empty to use the original filename from the URL. |

- Click **Run workflow**.

### 5. Get your downloaded files

- **If you used `commit`**: After the workflow finishes, go to the **Code** tab of your fork. All downloaded files are inside the `downloads/` folder. You can browse them individually or download the whole repository as ZIP (Code → Download ZIP).

- **If you used `release`**: After the workflow finishes, go to the **Releases** section of your fork (right sidebar). A new release will be created containing all downloaded files. You can download them directly from there without size restrictions (up to 2GB per file).

## Important Notes

- When using `commit` mode, any file larger than `split_size` is automatically split into `.zip`, `.z01`, `.z02` … parts. To reassemble on your local machine: use `zip -F original.zip --out combined.zip` (standard zip tool).
- The workflow commits files in batches of 5 to avoid GitHub limits (only relevant for `commit` mode).
- If you download multiple files with `custom_filename`, it will be ignored – custom naming only works for a single URL.
- The `connections` value increases download speed but may be limited by the source server. Values between 2 and 8 are usually safe.

## Credits

Created for environments with restricted internet. Uses `aria2` and standard `zip` tools.

## Author

Developed by Anonymous
