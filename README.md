**Guide: Deploying Your Application on Posit**

This guide outlines the steps and requirements for deploying your R Shiny or Python applications on Posit.

**I. Prerequisites**

Before you begin, ensure you have the following:

1.  **GitHub Access:**
    *   A GitHub account.
    *   Membership in the `gmi-common` GitHub organization.
    *   Membership in the following Active Directory (AD) groups (request access if needed):
        *   `GITHUB_ALL_USERS`
        *   `GITHUB_SC_DA_DNA_WRITE`
    *   **Note:** After AD group membership is granted, you may receive an automated email from GitHub to authorize your organization access. Please accept these prompts.

2.  **Standardized Project Structure:**
    Your codebase must adhere to a specific structure. The example below is for an R Shiny project, but Python projects follow a similar convention.

    **Key Files:**
    *   `app.R` (for R Shiny): Serves as the application's entry point, defining the UI and Server logic. For Python, this would be your main application script (e.g., `app.py`).
    *   `app_configs.json`: Contains application-specific parameters like name, URL, and `max_connections`.
    *   `manifest.json`: Lists the R/Python version and all project dependencies.
        *   **Important:** You must regenerate `manifest.json` every time you change project files or dependencies.
        *   **For R:** Run `rsconnect::writeManifest()` in the R console.
        *   **For Python:** Use the `rsconnect-python` package. Command:
            `rsconnect write-manifest <deployment-type> --entrypoint <python-file-name>:<function-name> .`
            *(Replace `<deployment-type>`, `<python-file-name>`, and `<function-name>` with your specific details.)*

    **Sample Project Structure:**
    You can find sample files and structure in the `r-forecast-master` directory within the `sc-da-dna-gcp/ui_pipeline/` path on the `posit-develop` branch of the `gmi-common/sc-da-dna-gcp` repository.

    **Further Reading:**
    For a comprehensive understanding, refer to the [Posit (RStudio) Connect Developer's Guide](https://gmi-pe.atlassian.net/wiki/spaces/CE/pages/267878528/Posit+RStudio+Connect+Developer+s+Guide).
    
**II. GitHub Repository Guidelines**

1.  **Repository:** All applications are deployed from `https://github.com/gmi-common/sc-da-dna-gcp/`.
2.  **Location:** Place your application code within the `ui_pipeline/` folder.
3.  **Folder Naming Convention:**
    *   R Shiny apps: `r-your-project-name`
    *   Python apps: `py-your-project-name`
4.  **Target Branch:** All changes intended for deployment *must* be pushed to the `posit-develop` branch.
5.  **Scope:** Only modify files within your project's folder inside `ui_pipeline/`. Do not alter other files or folders in the repository.

**III. Development and Deployment Workflow**

1.  **Clone & Setup:**
    *   Clone the repository: `git clone https://github.com/gmi-common/sc-da-dna-gcp/`
    *   Navigate to the main branch: `git checkout main`
    *   Ensure your main branch is up-to-date: `git pull origin main`
    *   Create a new feature branch for your work: `git checkout -b your-feature-branch-name`

2.  **Develop:**
    *   Make your code changes on `your-feature-branch-name`.
    *   **Crucially, regenerate `manifest.json` after any code or dependency changes.**
    *   Commit and push your changes to your feature branch frequently:
        ```bash
        git add .
        git commit -m "Descriptive commit message"
        git push origin your-feature-branch-name
        ```

3.  **Prepare for Deployment:**
    *   Switch to the `posit-develop` branch: `git checkout posit-develop`
    *   Ensure your local `posit-develop` is up-to-date: `git pull origin posit-develop`
    *   Merge your feature branch into `posit-develop`: `git merge your-feature-branch-name`
        *   Resolve any merge conflicts if they arise.

4.  **Deploy:**
    *   Push the merged changes to the remote `posit-develop` branch: `git push origin posit-develop`

5.  **Iterate (if needed):**
    *   For further development, switch back to your feature branch: `git checkout your-feature-branch-name`
    *   Make changes, commit, push to your feature branch, and then repeat steps 3 (Prepare for Deployment) and 4 (Deploy).

6.  **Keep Feature Branch Updated (Optional but Recommended):**
    *   Periodically, update your feature branch with the latest changes from `main`:
        ```bash
        git fetch origin main
        git merge origin/main # (While on your feature branch)
        ```

**IV. Automated Deployment**

Pushing changes to the `posit-develop` branch on the remote GitHub repository (`gmi-common/sc-da-dna-gcp`) automatically triggers GitHub Actions, which will deploy your application to the development environment on Posit. You can find your app on the link https://rsconnect-cloud-dev.genmills.com/{vanity_url_given_app_config}
