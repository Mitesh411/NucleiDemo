# NucleiDemo

[![CI Workflow](https://github.com/NucleiDemo/actions/workflows/CI/badge.svg)](../../actions)

**NucleiDemo** is a demonstration repository showing how to automate web application vulnerability scanning using ProjectDiscovery's [Nuclei](https://github.com/projectdiscovery/nuclei) tool integrated with GitHub Actions.

---

## 📌 Overview

Nuclei is a fast, template-based vulnerability scanner used to detect security weaknesses in web applications, network protocols, and cloud infrastructure. This repository provides an automated GitHub Actions CI pipeline that executes Nuclei vulnerability scans against target environments and preserves scan findings as workflow artifacts.

---

## 🛠️ Architecture & Workflow Diagram

The workflow automates code checkout, vulnerability scanning, and artifact collection upon git triggers or manual execution:

```mermaid
flowchart TD
    A[Event Trigger] -->|Push to main| B(Start CI Workflow)
    A -->|Pull Request to main| B
    A -->|Workflow Dispatch| B

    subgraph GitHub Actions Runner (ubuntu-latest)
        B --> C[Checkout Repository\nactions/checkout@v4.2.2]
        C --> D[Run Nuclei Vulnerability Scan\nprojectdiscovery/nuclei-action@v3.1.1]
        D -->|Generate nuclei.log| E[Upload Artifacts\nactions/upload-artifact@v4.6.0]
    end

    E --> F[Artifact stored: nuclei.log]
```

---

## ⚙️ CI Workflow Details

The GitHub Actions workflow (`.github/workflows/blank.yml`) performs the following steps:

1. **Trigger Options (`on`)**:
   - `push`: Triggered on direct commits pushed to the `main` branch.
   - `pull_request`: Triggered on pull requests targetting the `main` branch.
   - `workflow_dispatch`: Allows manual execution from the GitHub Actions tab with optional input parameters (e.g. `comment`).

2. **Jobs & Actions**:
   - **Checkout**: Uses `actions/checkout@v4.2.2` to fetch repository source code.
   - **Nuclei Scan**: Uses `projectdiscovery/nuclei-action@v3.1.1` to scan the configured target URL (`http://testphp.vulnweb.com/`).
   - **Artifact Upload**: Uses `actions/upload-artifact@v4.6.0` to save `nuclei.log` as a workflow artifact downloadable from the workflow run summary.

---

## 🚀 Usage

### Running Manually via GitHub UI

1. Navigate to the **Actions** tab in the repository.
2. Select the **CI** workflow from the left sidebar.
3. Click **Run workflow**, specify any comment or input, and click the green **Run workflow** button.

### Viewing Scan Logs & Artifacts

1. Select a completed workflow run.
2. Scroll down to the **Artifacts** section at the bottom of the summary page.
3. Download `nuclei.log` to review detected vulnerabilities and scan details.

---

## 📂 Repository Structure

```
.
├── .github/
│   └── workflows/
│       └── blank.yml    # GitHub Actions workflow definition
└── README.md            # Repository documentation and workflow diagram
```
