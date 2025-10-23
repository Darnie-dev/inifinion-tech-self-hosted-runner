
# Infinion Self-Hosted GitHub Actions Runner Assessment

**Candidate:** Ayobami Oshokoya
---

## Table of Contents

- [Project Overview](#project-overview)
- [Setup Instructions](#setup-instructions)
- [Workflow Configuration](#workflow-configuration)
- [Git Configuration & Security](#git-configuration--security)
- [Challenges Faced & Solutions](#challenges-faced--solutions)
- [Production Considerations](#production-considerations)
- [Learning Outcomes](#learning-outcomes)
- [References](#references)

---

## Project Overview

This project demonstrates my ability to:

- Deploy a **self-hosted GitHub Actions runner** on a local PC (WSL environment).  
- Configure a **CI workflow** and optionally run a test pipeline.  
- Document the process with clear **security considerations and troubleshooting notes**.  

<img width="1499" height="222" alt="successful workflow run" src="https://github.com/user-attachments/assets/8fd6392a-ad13-4729-8d86-5ffabef95e70" />

---

## Setup Instructions

1. **Environment**  
   - WSL (Windows Subsystem for Linux) running Ubuntu.  
   - Non-root user recommended to avoid permission errors.

2. **Repository**  
   - Created a **private GitHub repository** to minimize security exposure.
   - I used a public for the sake of this assessment
   - Local setup:
     ```bash
     mkdir infinion-runner
     cd infinion-tech-self-hosted-runner
     git init
     ```
   - Cloned repository if starting from GitHub:
   <img width="1162" height="294" alt="git clone" src="https://github.com/user-attachments/assets/2c13b2fd-3f70-42b1-b142-3c14bc0d8c58" />

       ```bash
       git clone https://github.com/<username>/<repo>.git
       cd <repo>
       ```

3. **Runner Configuration**  
<img width="1904" height="1080" alt="github commands" src="https://github.com/user-attachments/assets/12437c62-6ae2-4718-8d44-e714f34a9c95" />

   - on your repo, click on actions and then runners
   - after which you select 'self-hosted runners' and select 'new runner'
   - click on linux or choose the operating system that matches your machine/needs
   - Run the commands provided by Github

     ```bash
       mkdir actions-runner && cd actions-runner
       curl -o actions-runner-linux-x64-2.305.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.305.0/actions-runner-linux-x64-2.305.0.tar.gz
     ```
   <img width="1136" height="160" alt="Screenshot 2025-10-23 030454" src="https://github.com/user-attachments/assets/a4fc9cf8-48a7-46ae-a2f7-66fd2e1afe1c" />


   - After downloading the runner package, configure the runner
     
     ```bash
      ./config.sh --url https://github.com/<username>/<repo> --token <TOKEN>
     ```

4. ** Install and Start the Runner**  
   - After configuring, you can start the runner with:
   ```bash
   ./run.sh
   ```
   -To install the runner as a service (so it starts automatically on boot), run:
   ```bash
     sudo ./svc.sh install
     sudo ./svc.sh start
   ```
   
   **Notes**:
   - Must run as **non-root**.  
   - Tokens are single-use; new tokens are generated for reconfiguration.

---

## Workflow Configuration (CI)

Example workflow file `ci.yml` or check mine here: 
[darnie-test-run.yml workflow file](https://github.com/Darnie-dev/infinion-tech-self-hosted-runner/blob/main/.github/workflows/darnie-test-run.yml):

```yaml
name: CI

on:
  # Optional triggers commented out for security reasons
  # push:
  #   branches: [ "main" ]
  # pull_request:
  #   branches: [ "main" ]
  workflow_dispatch: # Manual run

jobs:
  build:
    runs-on: ubuntu-latest #this should be on self-hosted
    steps:
      - uses: actions/checkout@v4
      - name: Run a one-line script
        run: echo "Hello, world!"
      - name: Run a multi-line script
        run: |
          echo "Add other actions to build, test, and deploy your project."
```

**Notes:**

- `workflow_dispatch` enables manual testing without exposing automatic triggers.  
- Push and pull request triggers are optional and commented out for security reasons.  

- All steps are documented to demonstrate understanding of GitHub Actions.

<img width="1261" height="736" alt="runner config confirmation" src="https://github.com/user-attachments/assets/d0b9807e-5f6c-45bc-a3db-d12dbd636cd5" />


---

## Git Configuration & Security

1. **User setup**  
     ```bash
     git config --global user.email "your.email@example.com"
     git config --global user.name "YourGitHubUsername"
     ```

2. **Authentication**  
   - **SSH-based authentication** (preferred):
       ```bash
       ssh-keygen -t ed25519 -C "your.email@example.com"
       eval "$(ssh-agent -s)"
       ssh-add ~/.ssh/id_ed25519
       ```
     - Add the public key to GitHub under **Settings → SSH and GPG keys**.

   - **HTTPS with token**:
     - Tokens can be used once; SSH is recommended for long-term workflow.

---

## Challenges Faced & Solutions
<img width="717" height="169" alt="config root error" src="https://github.com/user-attachments/assets/badfe645-ba72-4ef0-a0ae-5006ce06c253" />

| Challenge | Cause | Solution |
|-----------|-------|---------|
| `Must not run with sudo` | Runner configured as root | Switched to non-root user |
| `./config.sh: No such file or directory` | Wrong directory | Changed directory to actions-runner folder |
| `Authentication failed` | Incorrect username/email or token | Corrected Git config, generated new token |
| `Repository not found` | Typo in repo URL | Verified repository exists and corrected URL with ```git remote set url``` |
| Large file errors | GitHub max file size exceeded |Cleaned the Git history to remove the large files, and added   the ```actions-runner/ directory``` to .gitignore to ensure the repository remains strictly source-code focused (repository hygiene) |



---

## Production Considerations

- **Security**: Only enable triggers (push/pull_request) that are required.  
- **Access control**: Use SSH keys for authentication instead of plain tokens.  
- **security**: Use GitHub secrets to store delicate files.  
- **Monitoring**: Add logging and alerting for production runners.  

---

## Learning Outcomes

- Configured a **self-hosted GitHub Actions runner** successfully on WSL.  
- Gained **hands-on experience** with workflow YAML, security considerations, and CI/CD pipeline setup.  
- Learned how to **troubleshoot common Git and Actions errors**.  
- Documented all challenges, solutions, and best practices to simulate production readiness.

---

## References

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Git LFS Documentation](https://git-lfs.github.com/)
- [SSH Key Authentication](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [youtube](https://www.youtube.com/watch?v=aLHyPZO0Fy0)
# inifinion-tech-self-hosted-runner
