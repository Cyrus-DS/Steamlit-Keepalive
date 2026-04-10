## Keeping the Finance App Awake

This finance application is built using Streamlit and hosted on Streamlit Community Cloud.

To ensure a smooth experience for members recording and viewing transactions, a scheduled workflow via GitHub Actions is used to prevent the app from going idle.

---

## Why This Matters

This app is used to:
- Record member transactions  
- Track balances  
- Maintain simple financial records  

Since activity is **low but important**, users may access the app after long periods of inactivity. By default, Streamlit apps sleep when idle, which can cause:

- Delayed loading times  
- Poor user experience when recording transactions  
- Potential confusion for non-technical users  

This workflow ensures the app is **always responsive when needed**

---

## How It Works

A background workflow runs every **6 hours** and sends a request to the app’s internal health endpoint:

https://ndundu-finance-appgit-5xrzkytijl5kizazc5uxav.streamlit.app/_stcore/health

This:
- Wakes the app if it is asleep  
- Keeps it active if already running  

---

## Workflow Configuration

```yaml
name: Keep Streamlit Awake

on:
  workflow_dispatch:
  schedule:
    - cron: '17 */6 * * *'

jobs:
  ping:
    runs-on: ubuntu-latest

    steps:
      - name: Ping Streamlit health endpoint
        run: |
          curl -sS -o /dev/null -w "HTTP %{http_code}\n" "https://ndundu-finance-appgit-5xrzkytijl5kizazc5uxav.streamlit.app/_stcore/health"
