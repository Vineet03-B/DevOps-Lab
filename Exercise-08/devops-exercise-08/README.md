# Exercise 08: Creating a Hello World Jenkins Job

## Objective
Create a simple shell script, publish it to a GitHub repository, and configure a Jenkins Freestyle job to execute it.

## Prerequisites
- Git installed locally.
- A GitHub account and a public repository.
- Jenkins running and accessible at `http://localhost:8080/`.
- Jenkins configured with the required plugins, including Git support.

## Part 1: Create the GitHub Repository
Create a public repository with these details:

- **Repository name:** `devops-sample-code`
- **Description:** `A demo repository for Jenkins scripting.`
- **Visibility:** Public

A fine-grained personal access token can be created using GitHub's official guide:
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token

Do not store a personal access token in this repository or in screenshots.

## Part 2: Create the Hello World Script
Create a file named `hello-world.sh` with this content:

```bash
#!/bin/bash
echo "Hello, Jenkins!"
```

Make it executable and test it:

```bash
chmod +x hello-world.sh
./hello-world.sh
```

Expected output:

```text
Hello, Jenkins!
```

## Part 3: Initialize Git and Commit
Configure Git identity if it has not already been configured:

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Initialize the repository and commit the script:

```bash
git init -b main
git add hello-world.sh
git status
git commit -m "Add hello-world.sh"
```

## Part 4: Push the Script to GitHub
Connect the local repository to the GitHub repository you created:

```bash
git remote add origin https://github.com/<your-GitHub-username>/devops-sample-code.git
git push -u origin main
```

When prompted for HTTPS authentication, use your GitHub username and a suitable personal access token. Never paste the token into source files or commit it.

Verify that `hello-world.sh` appears in the public GitHub repository.

## Part 5: Create a Jenkins Freestyle Job
1. Open `http://localhost:8080/` and sign in.
2. Click **New Item**.
3. Enter the job name `HelloWorld`.
4. Select **Freestyle project**, then click **OK**.
5. Optionally add the description `Hello World! Jenkins job.`
6. Under **Source Code Management**, select **Git**.
7. Enter the repository URL:

   ```text
   https://github.com/<your-GitHub-username>/devops-sample-code.git
   ```

8. Under **Build Steps**, choose **Add build step → Execute shell**.
9. Enter:

   ```bash
   sh hello-world.sh
   ```

10. Click **Save**, then click **Build Now**.

## Part 6: Verify the Build
Open the build number from **Build History**, then select **Console Output**.

Expected output includes:

```text
Hello, Jenkins!
Finished: SUCCESS
```

The exact console lines and workspace path can differ by Jenkins version and configuration.

## Screenshots
The `screenshots/` folder contains the supplied evidence for:
- Creating and committing `hello-world.sh`.
- Running the script locally.
- The public `devops-sample-code` repository.
- Viewing `hello-world.sh` on GitHub.

## Result
The Hello World shell script was created and tested locally, then published to GitHub. Configure the Jenkins Freestyle job to check out the repository and run the script, then use the console output to verify success.
