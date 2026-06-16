# Uberall Support Dashboard

A live Salesforce-powered support operations dashboard deployed on GitHub Pages.

## Setup (one-time)

### 1. Create a Salesforce Connected App

1. In Salesforce → **Setup** → search **App Manager** → **New Connected App**
2. Fill in:
   - **Connected App Name**: `Support Dashboard`
   - **API Name**: `Support_Dashboard`
   - **Contact Email**: your email
3. Under **OAuth Settings**, check **Enable OAuth Settings**
4. **Callback URL**: `https://YOUR_GITHUB_USERNAME.github.io/support-dashboard/`
   - Also add `http://localhost:8080/` for local development
5. **Selected OAuth Scopes**: add `Access and manage your data (api)`
6. **Enable for Device Flow**: leave unchecked
7. Save → wait ~10 minutes for Salesforce to propagate
8. Copy the **Consumer Key** (Client ID)

### 2. Configure CORS in the Connected App

1. Back in the Connected App → **Edit**
2. Under **CORS** → **Add** your GitHub Pages origin:
   `https://YOUR_GITHUB_USERNAME.github.io`
3. Save

### 3. Update index.html

Edit the `CONFIG` block at the top of `index.html`:

```js
const CONFIG = {
  SF_CLIENT_ID: 'PASTE_YOUR_CONSUMER_KEY_HERE',
  SF_INSTANCE_URL: 'https://uberall.my.salesforce.com',
  REDIRECT_URI: 'https://YOUR_GITHUB_USERNAME.github.io/support-dashboard/',
  ALLOWED_EMAILS: ['alexander.haack@uberall.com'],
};
```

### 4. Create a GitHub repository

```bash
cd support-dashboard
git init
git add .
git commit -m "Initial support dashboard"
git remote add origin https://github.com/YOUR_USERNAME/support-dashboard.git
git push -u origin main
```

### 5. Enable GitHub Pages

1. GitHub repo → **Settings** → **Pages**
2. **Source**: `GitHub Actions`
3. Push any change to `main` to trigger the first deploy
4. Your dashboard will be live at: `https://YOUR_GITHUB_USERNAME.github.io/support-dashboard/`

---

## Usage

1. Open the dashboard URL
2. Click **Sign in with Salesforce** — log in with your Uberall Salesforce credentials
3. The dashboard loads live data. Use the **period selector** (top right) to switch between Last 7 Days, Last 30 Days, Last Month, or Last 90 Days.

### Tabs

| Tab | What you see |
|-----|-------------|
| **Overview** | Total cases, open count, CSAT avg, queue backlog, status distribution, case type breakdown |
| **Agents** | Per-agent case volume and CSAT scores, ranked table |
| **Topics** | Case type and issue type breakdown for the selected period |
| **Routing** | Cases tagged for Account Management, Partner AM, or other departments — needs forwarding |

---

## Adding more users

Update the `ALLOWED_EMAILS` array in `index.html`:

```js
ALLOWED_EMAILS: ['alexander.haack@uberall.com', 'colleague@uberall.com'],
```

> **Note**: Access is currently gated by Salesforce authentication only. Any Salesforce user at Uberall who knows the URL can authenticate. For stricter access control, a backend proxy with email allowlist enforcement can be added later.

---

## Local development

```bash
# Python
python3 -m http.server 8080

# Node
npx serve .
```

Open `http://localhost:8080` — make sure `http://localhost:8080/` is in your Connected App's callback URLs.
