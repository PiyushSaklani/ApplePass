# How to make a `.pkpass` file

[Sample Passes](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGk1eTlsSVJjcDVJSlAyTnRlYnQ4OGc1X1VaQXxBQ3Jtc0tsaElKMUhrczZJSzVtN1o5bGJjX0MzU2VxOVFUODgzdVRSOHZURmd1ZDl0cVJ2R0FicHlkUmJCZTlUV1lYWkV3TzNmbVNUV2U1ZVp6THVOaEU3eUZmcnQxTnV6eEthZTRfT0N5WnRjQzltVlJZWGtibw&q=https%3A%2F%2Fdownload.developer.apple.com%2FiOS%2FWallet_Support_Materials%2FWalletCompanionFiles.zip&v=rJZdPoXHtzI) provided by Apple.

## Overview
- Make a `CSR`.
- Generate a Certificate using CSR.
- Convert the Certificate into `.p12` format.
- Extract `signerCert` and `signerKey` from .p12 certificate.
- Make a proper `example.pass` folder structure.
- Edit the `pass.json` file.
- Make a build of signpass provided by Apple.
- Run `./signpass -p example.pass` command to make a .pkpass file.
- Now test the `.pkpass` file.

---

## How to make a `CSR`.

- Open __Keychain Access__
- Click __Request a Certificate From a Certificate Authority__. Route:`Top NavBar > Keychain Access > Certificate Assistant > Request a Certificate...`
- Please enter the email address that you used to log into your system.
- Check `Saved to disk` and continue.

---

## How to make a `Certificate` using CSR.

- Login into your [Apple Developer Account](https://developer.apple.com/account).
- Navigate to the `Certificates, Identifiers & Profiles` section.
- Inside `Identifiers` click the __Plus Icon__. Now select `Pass Type IDs` and continue.
- Enter the required information and continue. **_Make sure they remember the data entered in the `Identifier field`._**
- Now click the newly created Identifier. Upload your CSR and proceed further to download the `Certificate`.

---

## How to convert the Certificate into `.p12` format.

- Double-click the Certificate just downloaded.
- Open __Keychain Access__ navigate to __My Certificates__ secton.
- Export the Certificate in `.p12` format. Set the password for the certificate.

---

## How to extract the `signerCert` and `signerKey` from the .p12 certificate.

- Open the terminal and navigate to the folder where `.p12` certificate is stored.
- Now run this command to extract the `signerCert` from the `.p12`.

    ```bash
    openssl pkcs12 -in <cert_name>.p12 -clcerts -nokeys -out signerCert.pem -passin pass:<your_password> -legacy
    ```

- Now run this command to extract the `signerKey`.

    ```bash
    openssl pkcs12 -in <cert_name>.p12 -nocerts -out signerKey.pem -passin pass:<your_password> -passout pass:<secret_passphrase> -legacy
    ```

> If you are using the OpenSSL version greater the v3.0.0 then these commands will work.

> For lesser versions remove the `-legacy` from the end.

---

## Proper folder structure.

This folder structure is for **Store Card**.

```md
ApplePass
|
├── example.pass
   ├── icon.png
   ├── icon@2x.png
   ├── logo.png
   ├── pass.json
   ├── strip.png
   └── strip@2x.png
```

#### Make sure that the dimensions on the images must be as follows.
| Image | Dimensions | Resolution |
| :-- | :-: | :-: |
| icon | 35x35 | 72x72 |
| icon@2x | 69x69 | 72x72 |
| logo | 90x80 | 72x72 |
| strip | 312x123 | 72x72 |
| strip@2x | 624x245 | 72x72 |

---

## What to edit in `pass.json`.

- Make sure that the value of `passTypeIdentifier` is the same as the `Identifier` you created before.
- Change the `teamIdentifier` to your `Team ID`.
- Else you can edit according to your need.

---

## How to build a `signpass`.

- As you have downloaded the [sample passes](https%3A%2F%2Fdownload.developer.apple.com%2FiOS%2FWallet_Support_Materials%2FWalletCompanionFiles.zip&v=rJZdPoXHtzI) provided by Apple. It contains a `signpass` folder open it in `Xcode`.
```md
WalletCompanionFiles
|
├── SamplePasses
├── ServerReference
└── signpass
```
- In Xcode, you will find a `build` option in **Product**. Route:`Top navbar > Product > Build`.
- Now click `Show Build Folder in Finder` in **Product**.
- After that navigate to `Products > Debug > signpass`.
- Copy the signpass and paste it in the same level as the` example.pass` folder. 
```md
ApplePass
|
├── signpass
├── example.pass
   ├── icon.png
   ├── icon@2x.png
   ├── logo.png
   ├── pass.json
   ├── strip.png
   └── strip@2x.png
```

---
 
## How to run a `./signpass -p me.pass` command.

- Open the terminal and navigate to the folder where `signpass` and `example.pass` is stored.
- Then run this command.
```bash
./signpass -p example.pass
```

---

## How to test whether `.pkpass` is working fine or not.

There are two ways to test that `.pkpass` is working fine.

- First by testing it in the `IOS Simulator`. Just drag and drop the `.pkpass` file in the simulator.
- Second by mailing it. Just attach the `.pkpass` file in the email and mail it to an IOS user. Click on the .pkpass file it will open.

---
---

## Points to remember.

- This whole process will only work for `Mac users`.
- Only the `Account holder` can create the `Identifier Pass IDs` and generate a certificate from `CSR`.
- You can `export the certificate` to `.p12` format only if the `CSR` is created in your MacBook. 
