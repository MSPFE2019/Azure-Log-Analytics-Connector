# Azure-Log-Analytics-Connector

## 1. Register an Azure AD Application

1. Sign in to the Azure portal at `https://portal.azure.com`  
2. Select **Azure Active Directory** → **App registrations** → **New registration**  
   - **Name**: `Azure Log Analytics Connector` (or any descriptive name)  
   - **Supported account types**: Choose **Accounts in this organizational directory only**  
   - **Redirect URI**: _Leave blank_  
3. Click **Register**.  

### 1.1 Create a Client Secret

1. In the newly registered app, go to **Certificates & secrets** → **Client secrets** → **New client secret**  
2. Enter a **Description** (e.g. `LogAnalytics-Secret`) and select an **Expiry** period.  
3. Click **Add**, then **Copy** the **Value** immediately. You will not be able to retrieve it later.  

## 2. Assign API Permissions

### 2.1 Log Analytics API

1. In the app’s **API permissions** pane, click **Add a permission** → **APIs my organization uses**  
2. Search for **Log Analytics API**, then select it.  
3. Under **Delegated permissions**, check **Data.Read**.  
4. Under **Application permissions**, check **Data.Read**.  
5. Click **Add permissions**.

### 2.2 Microsoft Graph

1. Still under **API permissions**, click **Add a permission** → **Microsoft Graph** → **Delegated permissions**  
2. Search for and select **User.Read**.  
3. Click **Add permissions**.  

### 2.3 Grant Admin Consent

1. Click **Grant admin consent for \<Your Tenant\>**.  
2. Confirm by clicking **Yes**.  

## 3. Record Your Application Details

Save the following values for later configuration:

- **Tenant ID**: found under **Azure Active Directory** → **Overview**  
- **Application (client) ID**: shown on the app’s **Overview** page  
- **Client secret**: the value you copied in **Certificates & secrets**  

## 4. Import the Swagger (OpenAPI) Definition

1. In Power Automate, navigate to **Data** → **Custom connectors** → **+ New custom connector** → **Import an OpenAPI file**.  
2. Upload your `swagger.json`.  
3. Click **Continue** and verify the operations appear as expected.  

## 5. Configure Connector Authentication

1. Go to the connector’s **Security** tab.  
2. Select **OAuth 2.0** as the **Authentication type**.  
3. Enter the following:

   | Field             | Value                                             |
   |-------------------|---------------------------------------------------|
   | **Identity provider** | Azure Active Directory                       |
   | **Client ID**         | `<Your Application (client) ID>`             |
   | **Client secret**     | `<Your Client Secret>`                       |
   | **Tenant ID**         | `<Your Tenant ID>`                            |
   | **Resource URL**      | `https://api.loganalytics.io/`                |

4. **Do not** override or modify the **Authorization URL** or **Token URL**.  

## 6. Save and Test

1. Click **Create connector** (or **Update connector** if editing).  
2. In the connector, open the **Test** tab.  
3. Click **New connection**, authenticate, and verify you can invoke an operation (for example, a simple `GET /workspaces/{workspaceId}/tables`).  
4. If the call succeeds, your connector is correctly configured.  
