# 🔐 Checkmarx AccuKnox Integration

Easily fetch Checkmarx scan results and upload them to the AccuKnox Console using this GitHub Action.

---

## 🚀 What It Does

This GitHub Action:

1. Fetches **Checkmarx results** via Docker.
2. Uploads the resulting JSON report to the **AccuKnox Console** using API.

---

## 📦 Key Features

- ✅ Automates Checkmarx result fetching via Docker
- ✅ Uploads JSON scan report to AccuKnox for centralized analysis
- ✅ Supports direct integration with your CI/CD workflows
- ✅ Accepts custom inputs for secure and dynamic operation

---
## ⚠️ **Prerequisites**

Before using this GitHub Action, ensure the following:

1️⃣ **An AccuKnox Account** – Required for accessing the AccuKnox Console.  
2️⃣ **Checkmarx Setup** – A valid Checkmarx instance.  
3️⃣ **GitHub Repository with Actions Enabled** – Required to run GitHub workflows.  
4️⃣ **Checkmarx API Key** – Needed to fetch the project scan results.  
5️⃣ **AccuKnox API Token & Tenant ID** – Required for authentication (see [Token Generation Guide](https://help.accuknox.com/getting-started/how-to-create-tokens/)).  
6️⃣ **Docker Installed on GitHub Runner** – The action uses a Docker container to fetch scan results.

## 🔄 Usage Scenarios

### ➤ Option 1: Trigger a New Scan + Upload Results

Use [Checkmarx AST GitHub Action](https://github.com/checkmarx/ast-github-action) to initiate a scan, then use this action to fetch and upload the results.

```yaml
- name: Run Checkmarx Scan
  uses: checkmarx/ast-github-action@main
  with:
    base_uri: https://your-checkmarx-url/
    cx_client_id: ${{ secrets.CX_CLIENT_ID }}
    cx_client_secret: ${{ secrets.CX_CLIENT_SECRET }}
    cx_tenant: your-tenant
````

Then fetch and upload results:

```yaml
- name: Checkmarx AccuKnox Integration
  uses: accuknox/checkmarx-accuknox-action@v1.0.0
  with:
    api_key: ${{ secrets.CX_API_KEY }}
    project_name: "your-checkmarx-project"
    accuknox_endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
    tenant_id: ${{ secrets.TENANT_ID }}
    ak_token: ${{ secrets.ACCUKNOX_TOKEN }}
    label_id: "cmx-gh-action"
```

---

### ➤ Option 2: Only Fetch & Upload Existing Results

If your project is already scanned in Checkmarx (manually or in a separate pipeline), use this action directly to fetch & upload:

```yaml
- name: Checkmarx AccuKnox Integration
  uses: accuknox/checkmarx-accuknox-action@v1.0.0
  with:
    api_key: ${{ secrets.CX_API_KEY }}
    project_name: "your-checkmarx-project"
    accuknox_endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
    tenant_id: ${{ secrets.TENANT_ID }}
    ak_token: ${{ secrets.ACCUKNOX_TOKEN }}
    label_id: "cmx-gh-action"
```

---

## 🧰 Inputs

| Input               | Description                                       | Required |
| ------------------- | ------------------------------------------------- | -------- |
| `api_key`           | Checkmarx API key                                 | ✅ Yes    |
| `project_name`      | Checkmarx project name                            | ✅ Yes    |
| `accuknox_endpoint` | AccuKnox API endpoint URL                         | ✅ Yes    |
| `tenant_id`         | AccuKnox Tenant ID                                | ✅ Yes    |
| `ak_token`          | AccuKnox API token for authentication             | ✅ Yes    |
| `label_id`          | Label to categorize the uploaded scan in AccuKnox | ✅ Yes    |

---

## 📤 API Upload Example

The report is uploaded using the following API call:

```bash
curl --location "${accuknox_endpoint}/api/v1/artifact/?tenant_id=${tenant_id}&data_type=CX&save_to_s3=true&label_id=${label_id}" \
  --header "Tenant-Id: ${tenant_id}" \
  --header "Authorization: Bearer ${ak_token}" \
  --form "file=@CHECKMARX-*.json"
```

---

## 🛠 Requirements

- ✅ Docker must be available on the GitHub runner (`ubuntu-latest`)
    
- ✅ Valid Checkmarx project and access token
    
- ✅ AccuKnox Console API access
    

---

## 📁 Output

Produces a file like `CHECKMARX-<timestamp>.json` in the workspace, which is uploaded to the AccuKnox Console.

---

## 🔒 Security Best Practices

- Store credentials like API keys and tokens in [GitHub Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets).
    
- Avoid hardcoding secrets into your workflows or version-controlled files.
    

---

## 🧪 Example Workflow

```yaml
name: Checkmarx to AccuKnox

on:
  push:
    branches:
      - main

jobs:
  cx_ak:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
        uses: actions/checkout@v3

      - name: Checkmarx AccuKnox Integration
        uses: accuknox/checkmarx-accuknox-action@v1.0.0
        with:
          api_key: ${{ secrets.CX_API_KEY }}
          project_name: "sample-app"
          accuknox_endpoint: ${{ secrets.ACCUKNOX_ENDPOINT }}
          tenant_id: ${{ secrets.TENANT_ID }}
          ak_token: ${{ secrets.ACCUKNOX_TOKEN }}
          label_id: "gh-checkmarx"
```

---

## 📞 Support

## 📖 **Support & Documentation**

📚 **Read More:** [AccuKnox Docs](https://help.accuknox.com/)  
📧 **Contact Support:** [support@accuknox.com](mailto:support@accuknox.com)  

---

## 📝 License

[MIT License](https://chatgpt.com/c/LICENSE)

---

```

Let me know if you’d like me to package this into a `release.zip`, generate a marketplace-friendly summary, or assist with publishing.
```
