# flutter_multiplatform_demo

An example Flutter project.

## Build and Publish Workflow

The following workflows are included in this repository:

- [`release.yml`](./.github/workflows/release.yml)

## Setup

### Android Configuration

> [!IMPORTANT]
>
> If you haven't done so already, make sure to
> [manually upload the first release](https://support.google.com/googleplay/android-developer/answer/9859348?hl=en)
> of the Android app to the Google Play Developer console. This is required in
> order to perform automated releases later.

1. Generate a signing key, making sure to replace `YOUR_NAME` in the command.

   > **IMPORTANT:** Make sure to save the password, as it will be needed when
   > configuring GitHub Actions.

   ```bash
   $ sudo keytool -genkey -v \
     -keyalg RSA -keysize 2048 -validity 10000 \
     -keystore YOUR_NAME.keystore \
     -alias YOUR_NAME

   Password:
   Enter keystore password:
   Re-enter new password:
   Enter the distinguished name. Provide a single dot (.) to leave a
   sub-component empty or press ENTER to use the default value in braces.
   What is your first and last name?
   [Unknown]:  Nick Alteen
   What is the name of your organizational unit?
   [Unknown]:  Dev
   What is the name of your organization?
   [Unknown]:  ncalteen
   What is the name of your City or Locality?
   [Unknown]:  Example Town
   What is the name of your State or Province?
   [Unknown]:  StateName
   What is the two-letter country code for this unit?
   [Unknown]:  US
   Is CN=Nick Alteen, OU=Dev, O=ncalteen, L=Example Town, ST=StateName, C=US correct?
   [no]:  yes

   Generating 2,048 bit RSA key pair and self-signed certificate (SHA384withRSA) with a validity of 10,000 days
         for: CN=Nick Alteen, OU=Dev, O=ncalteen, L=Example Town, ST=StateName, C=US
   [Storing ncalteen.keystore]
   ```

1. Encode the generated `.keystore` file to base64, making sure to replace
   `YOUR_NAME` in the command.

   ```bash
   base64 -i ./YOUR_NAME.keystore -o ./keystore-base64.txt
   ```

1. Create a
   [GCP service account](https://cloud.google.com/iam/docs/service-accounts-create).
1. Generate a
   [service account credentials JSON file](https://cloud.google.com/iam/docs/keys-create-delete).
1. Enable the
   [Google Play Android Developer API](https://console.cloud.google.com/marketplace/product/google/androidpublisher.googleapis.com).
1. Invite the service account to your
   [Google Play Developer account](https://play.google.com/console).

### GitHub Actions Configuration

1. Create the following
   [GitHub environments](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-an-environment).

   - `staging`
   - `production`

1. Create the following secrets in **each** environment.

   > The secret names must match **exactly** as described below. If any secret
   > values are the same for both environments, they can instead be created as
   > [repository-level secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository).

   Make sure to remove any leading/trailing whitespace!

   | Name                           | Example     |
   | ------------------------------ | ----------- |
   | `ANDROID_KEYSTORE_FILE_BASE64` | `abc123...` |
   | `ANDROID_KEYSTORE_PASSWORD`    | `P4ssw0rd!` |
   | `ANDROID_SERVICE_ACCOUNT_JSON` | `{ ... }`   |
   | `ANDROID_SIGNING_KEY_ALIAS`    | `YOUR_NAME` |
   | `ANDROID_SIGNING_KEY_PASSWORD` | `P4ssw0rd!` |
