\# InstaZen



InstaZen is an automated CI/CD pipeline that builds a distraction-free version of Instagram. Using GitHub Actions and the ReVanced CLI, this repository automatically fetches the latest supported Instagram APK, strips out addictive UI elements, and publishes a securely signed release. 



By connecting this private repository to Obtainium, updates are delivered automatically to your Android device just like a native app store.



\## Modifications Applied

This build utilizes the following ReVanced patches:

\* `hide-ads`: Removes timeline and story advertisements.

\* `hide-reels`: Removes the Reels button from the bottom navigation bar.

\* `disable-reels-scrolling`: Prevents endless scrolling.

\* `hide-explore`: Nullifies the explore page grid.

\* `limit-feed-to-followed-profiles`: Ensures your home feed only shows posts from people you actually follow.

\* `custom-branding`: Renames the application to InstaZen.



\## 1. Keystore Setup (Local)

To ensure Android recognizes every weekly build as a secure update to the same app, you must sign the APK with a persistent Keystore. 



1\. Generate a Base64 Keystore string using your local terminal (PowerShell):

&#x20;  ```powershell

&#x20;  powershell -Command "\[Convert]::ToBase64String(\[IO.File]::ReadAllBytes('instazen.p12'))" > keystore\_base64.txt

&#x20;  ```

2\. Open `keystore\_base64.txt` and copy the entire text string.



\## 2. GitHub Secrets Configuration

Navigate to your repository's \*\*Settings > Secrets and variables > Actions > New repository secret\*\* and add the following exactly as named:



| Secret Name | Description / Value |

| :--- | :--- |

| `KEYSTORE\_BASE64` | The giant Base64 text string generated and copied in the previous step. |

| `KEYALIAS` | The alias chosen during Keystore generation (e.g., `instazen`). |

| `KEYPASSWORD` | The password chosen during Keystore generation. |



\## 3. Triggering the Build

The GitHub Action is scheduled to run every Sunday at midnight automatically. To trigger it manually for the first time:

1\. Go to the \*\*Actions\*\* tab in this repository.

2\. Select \*\*Build InstaZen\*\* from the left sidebar.

3\. Click the \*\*Run workflow\*\* button on the right side of the screen.



\## 4. Obtainium Integration

To receive over-the-air (OTA) updates on your Android device automatically:

1\. Install the Obtainium app on your Android phone.

