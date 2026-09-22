# XYZ Software Inc. Website: Fixes and Publishing Guide

This document explains what was wrong with the project, what was fixed, and how the website was published to GitHub Pages.

It is written for a non-technical audience, but it includes the commands used so the process can be repeated.

## Final Website

The website was published to:

<https://kaustav-tw.github.io/docs-as-code-training/>

The source code is stored in:

<https://github.com/Kaustav-TW/docs-as-code-training>

## What This Project Does

This project uses **Docusaurus** to turn Markdown documentation files into a website. Docusaurus reads the files in the `docs` folder, builds web pages from them, and publishes the finished website to GitHub Pages.

## Problems That Were Found

### 1. The website software was not installed locally

The project had a `package.json` file listing Docusaurus, but the supporting packages had not been downloaded. Because of that, this command failed:

```powershell
npm run start
```

The error was:

```text
'docusaurus' is not recognized as an internal or external command
```

**Meaning:** The project knew which software it needed, but that software was not installed in the project folder.

### 2. The sidebar file contained a JavaScript spelling error

The first line of `sidebars.js` was:

```js
cconst sidebars = {
```

The extra `c` made the file invalid. Docusaurus could not read the navigation menu until it was changed to:

```js
const sidebars = {
```

### 3. A sidebar link did not match the document filename

The sidebar referred to a document ID named `troubleshooting`, but the actual file was named:

```text
docs/trouleshooting.md
```

The filename itself contains a spelling mistake: `trouleshooting` is missing the second `b`.

To make the existing project work without renaming the file, the sidebar was changed to use:

```js
'trouleshooting'
```

### 4. Documentation images used paths Docusaurus could not resolve

The documentation used image paths like:

```markdown
![Dashboard](/docs/Images/Dashboard.png)
```

Docusaurus interpreted the leading `/docs/` as a website URL instead of a path to an image beside the document. The build therefore reported that the image did not exist.

The image links were changed to document-relative paths, for example:

```markdown
![Dashboard](./Images/Dashboard.png)
```

This was applied to the images in the following documents:

- `docs/user-guide.md`
- `docs/administration.md`
- `docs/getting-started.md`
- `docs/installation.md`
- `docs/trouleshooting.md`

### 5. Mermaid diagrams were not enabled completely

The Markdown files contained Mermaid diagram blocks such as:

````markdown
```mermaid
flowchart LR
A[Home] --> B[Employees]
```
````

Mermaid is the tool that turns those text instructions into flowcharts and diagrams. The project had Mermaid parsing enabled, but the Docusaurus Mermaid theme package was missing. As a result, the diagrams did not render as diagrams in the browser.

The fix had two parts:

1. Enable Mermaid in `docusaurus.config.js`:

   ```js
   markdown: {
     mermaid: true,
   },
   ```

2. Install and register the matching Mermaid theme:

   ```powershell
   npm install @docusaurus/theme-mermaid@3.10.2
   ```

   ```js
   themes: ['@docusaurus/theme-mermaid'],
   ```

After this fix, the diagrams rendered correctly in the browser.

### 6. GitHub Pages settings still contained template values

The original Docusaurus configuration still referred to the example Docusaurus project:

```js
organizationName: 'facebook'
projectName: 'docusaurus'
url: 'https://your-docusaurus-site.example.com'
```

These values were changed to match the actual GitHub repository:

```js
url: 'https://kaustav-tw.github.io'
baseUrl: '/docs-as-code-training/'
organizationName: 'Kaustav-TW'
projectName: 'docs-as-code-training'
```

The `baseUrl` is important because this is a project website hosted inside the `/docs-as-code-training/` path, rather than a personal website hosted at the root of the domain.

### 7. The GitHub Pages branch did not exist

Docusaurus publishes the finished website to a Git branch named `gh-pages`. That branch did not exist in the GitHub repository, so the deployment command could not clone it.

An initial `gh-pages` branch was created remotely from the current `main` branch. After that, Docusaurus could publish the website normally.

The first deployment error looked like this:

```text
Error while executing command `git clone --depth 1 --branch gh-pages ...`
```

This was not a website-build error. It meant that GitHub did not yet have the branch that Docusaurus expected to clone.

### 8. The GitHub username was not supplied to Docusaurus

Docusaurus needs the GitHub username when it publishes with HTTPS. The deployment failed until this PowerShell environment variable was set:

```powershell
$env:GIT_USER = 'Kaustav-TW'
```

The variable only lasts for the current PowerShell window. It may need to be set again in a new terminal session.

### 9. Some commands were run from the parent folder

The `package.json` file is inside:

```text
C:\Users\Kaustav.Chatterjee\docs-as-code-training\XYZ Software Inc
```

Running `npm run start` from:

```text
C:\Users\Kaustav.Chatterjee\docs-as-code-training
```

produces an error because that parent folder does not contain the project’s `package.json` file.

Always move into the `XYZ Software Inc` folder first.

There are two relevant folders in this project:

```text
Repository root: C:\Users\Kaustav.Chatterjee\docs-as-code-training
Website project: C:\Users\Kaustav.Chatterjee\docs-as-code-training\XYZ Software Inc
```

Git commands can be run from the repository root. npm and Docusaurus commands must be run from the website project folder because that is where `package.json` is located.

## Commands Used to Fix the Project

### Install the project dependencies

Run this once after downloading the project or whenever `node_modules` is missing:

```powershell
Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training\XYZ Software Inc'
npm install
```

### Check the sidebar JavaScript

```powershell
node --check .\sidebars.js
```

### Remove generated files and caches

This is useful after changing Docusaurus configuration:

```powershell
npm run clear
```

### Build the website

This checks that the documentation can be converted into website files:

```powershell
npm run build
```

A successful build ends with output similar to:

```text
Use `npm run serve` command to test your build locally.
```

### Preview the website locally

```powershell
npm run start
```

Then open:

<http://localhost:3000/>

To prevent Docusaurus from opening a browser automatically:

```powershell
npm run start -- --no-open
```

Stop the server with `Ctrl+C` in the terminal.

## Commands Used to Publish to GitHub Pages

### Check the repository connection

```powershell
Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training'
git remote -v
git branch --show-current
```

The project is connected to:

```text
https://github.com/Kaustav-TW/docs-as-code-training.git
```

### Save and upload source changes

Run these commands from the `XYZ Software Inc` folder:

```powershell
git add .
git commit -m "Update documentation website"
git push origin main
```

What these commands mean:

- `git add .` prepares changed files.
- `git commit` records the changes locally.
- `git push origin main` uploads the source files to GitHub.

The repository root contains the documentation guide created during this work. To include that guide as well, run the same commands from the repository root instead:

```powershell
Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training'
git add .
git commit -m "Document project fixes and publishing steps"
git push origin main
```

### Create the `gh-pages` branch if it is missing

Check whether the branch already exists:

```powershell
Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training'
git ls-remote --heads origin gh-pages
```

If the command produces no output, create the branch remotely using an initial commit based on `main`:

```powershell
$tree = git rev-parse 'main^{tree}'
$commit = git commit-tree $tree -m 'Initialize gh-pages branch'
git push origin "$($commit):refs/heads/gh-pages"
```

This command does not switch branches or change the files in your working folder. If `gh-pages` already exists, skip this step.

### Publish the website

```powershell
Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training\XYZ Software Inc'
$env:GIT_USER = 'Kaustav-TW'
npm run deploy
```

The deployment command performs these actions automatically:

1. Builds the website into the `build` folder.
2. Clones the remote `gh-pages` branch into a temporary folder.
3. Replaces its contents with the new website files.
4. Creates a deployment commit.
5. Force-pushes the generated website to `gh-pages`.

The source files remain on `main`; the generated website files are stored on `gh-pages`.

If Docusaurus reports that `GIT_USER` is missing, set it in the same PowerShell window before running deploy:

```powershell
$env:GIT_USER = 'Kaustav-TW'
```

## GitHub Pages Settings

If GitHub Pages has not been configured yet:

1. Open the repository on GitHub.
2. Select **Settings**.
3. Select **Pages** in the left menu.
4. Under the publishing source, choose **Deploy from a branch**.
5. Select the `gh-pages` branch.
6. Select the `/ (root)` folder.
7. Select **Save**.

GitHub may take a short time to publish the first version.

## Repeatable Publishing Checklist

Use this checklist whenever documentation changes:

```powershell
Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training\XYZ Software Inc'
npm run build

Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training'
git add .
git commit -m "Update documentation"
git push origin main

Set-Location 'C:\Users\Kaustav.Chatterjee\docs-as-code-training\XYZ Software Inc'
$env:GIT_USER = 'Kaustav-TW'
npm run deploy
```

Then visit:

<https://kaustav-tw.github.io/docs-as-code-training/>

## Warnings That Did Not Stop the Website

### npm security audit warnings

Installing packages reported dependency vulnerabilities. These warnings did not prevent the website from building or publishing. They should be reviewed separately before using the project in a production business environment.

To view the details:

```powershell
npm audit
```

Do not run `npm audit fix --force` automatically, because it can upgrade packages in ways that may break the website. Review the proposed changes first.

### GitHub Pages trailing slash warning

Docusaurus displayed a warning recommending an explicit `trailingSlash` setting. The website still built and deployed successfully. This warning concerns how direct page URLs and relative links behave on GitHub Pages; it is not the reason deployment failed.

## Current Result

The project now:

- Installs its required software correctly.
- Builds successfully.
- Resolves the documentation images.
- Displays Mermaid diagrams.
- Uses the correct GitHub Pages URL configuration.
- Publishes to the `gh-pages` branch.
- Is available at <https://kaustav-tw.github.io/docs-as-code-training/>.
