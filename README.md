# InstaZen

InstaZen provides a distraction-free version of Instagram. You can choose between two architectures depending on your preference: a zero-maintenance Web Wrapper or an automated CI/CD pipeline for the native APK.

## Option A: Web Wrapper Architecture (Zero Maintenance)
If you prefer a solution that requires absolutely zero APK patching maintenance, you can build a Web Wrapper locally:
1. Create a simple mobile app using a framework like Flutter, Kotlin, or CapacitorJS.
2. Implement a `WebView` (or InAppBrowser) that loads `https://instagram.com`.
3. Inject custom JavaScript/CSS on page load to hide specific DOM elements using `display: none` (e.g., the Reels tab, explore page, and stories tray).
* **Pros:** Completely immune to native APK updates. Minimal risk to your account.
* **Cons:** Yields a mobile website experience rather than native app performance.

## Option B: Automated CI/CD Pipeline (Native App)
Using GitHub Actions and the ReVanced CLI, this repository automatically fetches the latest supported Instagram APK, strips out addictive UI elements, and publishes a securely signed release. By connecting this private repository to Obtainium, updates are delivered automatically to your Android device just like a native app store.

### Modifications Applied (Native)
This build utilizes the following ReVanced patches:
* `hide-ads`: Removes timeline and story advertisements.
* `hide-reels`: Removes the Reels button from the bottom navigation bar.
* `disable-reels-scrolling`: Prevents endless scrolling.
* `hide-explore`: Nullifies the explore page grid.
* `limit-feed-to-followed-profiles`: Ensures your home feed only shows posts from people you actually follow.
* `custom-branding`: Renames the application to InstaZen.

## 1. Keystore Setup (Local)
To ensure Android recognizes every weekly build as a secure update to the same app, you must sign the APK with a persistent Keystore. 

1. Generate a Base64 Keystore string using your local terminal (PowerShell):
   ```powershell
   powershell -Command "[Convert]::ToBase64String([IO.File]::ReadAllBytes('instazen.p12'))" > keystore_base64.txt
   ```
2. Open `keystore_base64.txt` and copy the entire text string.

## 2. GitHub Secrets Configuration
Navigate to your repository's **Settings > Secrets and variables > Actions > New repository secret** and add the following exactly as named:

| Secret Name | Description / Value |
| :--- | :--- |
| `KEYSTORE_BASE64` | The giant Base64 text string generated and copied in the previous step. |
| `KEYALIAS` | The alias chosen during Keystore generation (e.g., `instazen`). |
| `KEYPASSWORD` | The password chosen during Keystore generation. |

## 3. Triggering the Build
The GitHub Action is scheduled to run every Sunday at midnight automatically. To trigger it manually for the first time:
1. Go to the **Actions** tab in this repository.
2. Select **Build InstaZen** from the left sidebar.
3. Click the **Run workflow** button on the right side of the screen.

## 4. Obtainium Integration
To receive over-the-air (OTA) updates on your Android device automatically:
1. Install the Obtainium app on your Android phone.
2. In GitHub, generate a Personal Access Token (PAT) by going to Developer Settings. Give it Read-only access to your Repositories.
3. In Obtainium, tap **Add App** and paste the URL to this private GitHub repository.
4. Scroll down, check the box for **Use authentication**, and paste your GitHub PAT.
5. Obtainium will now monitor this repository and notify you whenever a new InstaZen release is compiled.