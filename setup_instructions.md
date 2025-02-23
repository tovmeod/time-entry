# Time Entry Application Setup Instructions

## Google Cloud Console Configuration

### 1. Create OAuth 2.0 Client ID
1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the Google Sheets API for your project
4. Go to "Credentials" and create an OAuth 2.0 Client ID
5. Configure the OAuth consent screen
6. Add the following Authorized JavaScript origins:
   ```
   http://localhost:8000
   http://127.0.0.1:8000
   https://YOUR_GITHUB_USERNAME.github.io
   ```
7. Add the following Authorized redirect URIs:
   ```
   http://localhost:8000
   http://127.0.0.1:8000
   storagerelay://http/localhost:8000
   storagerelay://http/127.0.0.1:8000
   https://YOUR_GITHUB_USERNAME.github.io
   https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME
   storagerelay://https/YOUR_GITHUB_USERNAME.github.io
   ```

8. Configure OAuth consent screen:
   - Go to "OAuth consent screen"
   - Choose "External" user type
   - Fill in the required fields:
     * App name
     * User support email
     * Developer contact information
   - Add the following scopes:
     * Google Sheets API (`https://www.googleapis.com/auth/spreadsheets`)
   - Add test users (your email and any other users who need access)
   - Click "Save and Continue"
   - Complete the configuration and publish the app

9. Create an API key:
   - Go to "Credentials"
   - Click "Create Credentials" > "API Key"
   - Restrict the API key:
     * Set application restrictions to "HTTP referrers"
     * Add your authorized domains (localhost:8000 and your GitHub Pages domain)
     * Restrict the key to Google Sheets API only
10. Add test users to https://console.cloud.google.com/auth/audience?inv=1&invt=AbqWTg&project=time-entry-451814

### 2. Update Application Credentials

1. Open `time_entry_app.html`
2. Replace `YOUR_CLIENT_ID` with your OAuth 2.0 client ID
3. Replace `YOUR_API_KEY` with your API key

### 3. OAuth Troubleshooting

If you encounter OAuth-related errors:

1. Verify Redirect URIs:
   - All redirect URIs must match exactly, including the storage relay URIs
   - For local development: `storagerelay://http/localhost:8000`
   - For GitHub Pages: `storagerelay://https/YOUR_GITHUB_USERNAME.github.io`

2. Check OAuth Consent Screen:
   - Ensure your email is added as a test user
   - Verify the required scope is added
   - Make sure the app is either published or in testing mode

3. Common Errors:
   - "redirect_uri_mismatch": Add the missing redirect URI from the error message
   - "invalid_client": Check that the client ID is correctly copied to the HTML file
   - "unauthorized_client": Ensure you're added as a test user
   - "access_denied": Check OAuth consent screen configuration

4. API Key Issues:
   - Verify the key is restricted to your domains
   - Check that the Google Sheets API is enabled
   - Ensure the key is restricted to only necessary APIs

## Local Development Setup

### Running Locally
1. Open terminal in the project directory
2. Start a local HTTP server:
   ```bash
   # For Python 3
   python3 -m http.server 8000

   # For Python 2
   python -m SimpleHTTPServer 8000
   ```
3. Open in browser: http://localhost:8000/time_entry_app.html

## GitHub Pages Deployment

1. Update the repository settings to enable GitHub Pages
2. Make sure to replace `YOUR_GITHUB_USERNAME` and `YOUR_REPO_NAME` in the authorized URIs in Google Cloud Console
3. Deploy to GitHub Pages
4. Access your application at: https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/time_entry_app.html

## Security Considerations

### Client-Side Credentials
- The CLIENT_ID and API_KEY are visible in the client-side code
- This is acceptable for this application because:
  * The OAuth flow requires client-side implementation
  * These credentials are restricted by domain in Google Cloud Console
  * Additional security is provided by OAuth consent screen

### Best Practices
1. Set up proper domain restrictions in Google Cloud Console
2. Regularly monitor API usage in Google Cloud Console
3. Keep the API key's scope limited to necessary APIs only
4. Never commit actual credentials to version control
5. Use environment variables or configuration files for local development

## Troubleshooting

### Common Issues
1. "gapiLoaded is not defined" or "gisLoaded is not defined"
   - Make sure the Google API scripts are properly loaded
   - Check that the functions are defined before they're called

2. "container is null"
   - Verify that all HTML elements exist in the DOM
   - Check element IDs match between HTML and JavaScript

3. OAuth errors
   - Verify all authorized origins and redirect URIs are properly configured
   - Check that the credentials are correctly copied into the application
   - Ensure the Google Sheets API is enabled in your project
