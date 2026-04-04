## Lab 2: Anonymous Blob Access Detection & Misconfiguration Analysis

### Lab Notes
⚠️ Anonymous blob access is a common cloud misconfiguration that exposes sensitive data without authentication. Attackers routinely scan for publicly accessible containers.

### 🔧 Storage and Container Misconfiguration Set Up

- Set Public Access Level Blob Storage to intentionally allow anonymous read access.
- Set Public Access Level Blob Container to intentionally allow anonymous read access.
- Uploaded a test file (test.txt) to generate activity.

#### Screenshot of Storage Blob set to anonymous access
![anonymousblob](./screenshots/1-blob-set-to-anonymous-access-enabled.png)

### 🕵️ Anonymous Access Testing

- Copied the blob URL to a private browser window and refreshed multiple times to generate repeated anonymous events. The blob was accessed successfully without authentication, confirming the container was publicly accessible.

**Observations**
- Anonymous access does not require SAS tokens, keys, or Azure AD credentials.
- When accessing the blob anonymously, the browser prompted a ‘Save As’ dialog box. This confirms that the blob was publicly accessible without authentication.
- Confirmed that diagnostic settings were sending: StorageBlobLogs, StorageRead and StorageWrite

#### 📊 Key Fields:

- AuthenticationType == "Anonymous"
- OperationName == "GetBlob"
- CallerIpAddress
- Uri

#### 🛠️ Remediation
• 	Disabled public access at both account and container levels
• 	Re-ran detection query to confirm no further anonymous reads


### 🧠  Commentary
The lab mirrors real world  incidents where public access leads to data exposure. Anonymous reads, container listings, and repeated access attempts were all captured as expected. **Public blob access** should be disabled unless explicitly required.

