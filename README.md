# Migrate 2FA from Google Authenticator
As I am trying to move away from Google for privacy issue, I have prepared a guide on how to transfer all or any account with a 2FA from Google Authenticator to any other platform.


# Table of Contents

1. <a href="#exporting-2fa-codes-from-google">Exporting 2FA Codes from Google</a>
2. <a href="#decoding-2fa-codes">Decoding 2FA Codes</a>
3. <a href="#proposed-2fa-platforms">Proposed 2FA Platforms</a>
4. <a href="#setting-up-2fa-in-a-different-platform">Setting Up 2FA in a Different Platform</a>


## Exporting 2FA Codes from Google

#### The first step will be to export or transfer all the accounts in Google using a QR Code that will be needed in the next step

To begin with, we need to first export all 2FA codes from Google Authenticator and use them somewhere else.

Open Google Authenticator, then from the left hand side, click the 3 lines and then select **Transfer codes** 

![transfer-accounts](/screenshots/google-auth/transfer-accounts.jpg)

Then, on the next screen, click on **Export Codes**

![export-accounts](/screenshots/google-auth/export-accounts.jpg)

Next, a screen showing all current accounts will be shown to select which account is needed to be exported/transfered, select all that applies. In my case, I have chosen them all as I wanted to totally migrate from Google Authenticator

![accounts](/screenshots/google-auth/accounts.jpg)


Next, a screen showing a QR Code will be shown. At this stage, take a photo of this using another phone as it will be used later on (Google Authenticator does not allow screenshots)

![qr-code](/screenshots/google-auth/qr-code.jpg)



## Decoding 2FA Codes
#### This step will allow to extract the 2FA code from the generated QR code, which will use those codes later on in another platform

Open terminal, and make sure the QR Code screenshot file is in the current working directory of your terminal. If you are not sure which folder are you in, type `$(pwd)` and take note of the output

Next, execute the command `docker run --pull always --rm -v "$(pwd)":/files:ro scit0/extract_otp_secrets [QR-CODE]]` where *[QR-CODE]* is the screenshot filename of the QR Code, in my case it is `qr-codes.jpg`. This will decode the accounts and provide the output on the terminal screen.

Alternativly, and the one I prefer, is to execute the command `docker run --pull always --rm -v "$(pwd)":/files:ro scit0/extract_otp_secrets [QR-CODE] > [LIST]` where *[QR-CODE]* is the screenshot filename of the QR Code, in my case it is `qr-codes.jpg` and *[List]* will be a CSV file containing each account's code in a list view, in my case it is `list.csv`

Once done, open the generated CSV file, and it should look like something as below

***Note that we are after the `Secret` row only***

![list](/screenshots/decode/list.png)



## Proposed 2FA Platforms

#### This step is to propsoe few 2FA alternatives I have tried and recommend. You may use anything else in a similar way.

I am an Android user, and I will propose only 2 alternatives that I have tried, and currently use (however, those might be also available for iPhone):

- [Bitwarden](https://play.google.com/store/apps/details?id=com.x8bit.bitwarden), where I have a self-hosted server, check my [other repo](https://github.com/tamimology/docker-containers#vaultwarden) for that.
The beauty of this is there is a broweser extension, when entereing passwords in a websiter using it, it will automatically copy the 2FA if it is set

- [Ente Auth](https://play.google.com/store/apps/details?id=io.ente.auth).
The beauty of this is having icons for easy visualisation, having the next code in case the first had short time expiry to remember or enter, trash folder for deleted accounts, accounts filtering, search option, and if enabled, when an account is clicked, the app minimises so it will be much easier to enter the code in its place before it expires.


## Setting Up 2FA in a Different Platform

#### This is where you set up your alternative 2FA platform

***Note: for Bitwarden, 2FA will only be doable from the mobile, not from the server***

Using 2 methods, I will be showing the steps for Ente Auth only, and all other platforms should have a similar process.

- Open the app on your mobile
- Click on the + icon at the lower right corner

**Method 1**
Select *Scan a QR code*, and then scna the exported QR code you had in step 1. This will have all selected accounts during transfer to be added automatically, usign the same account name from Google


![scan qr code](/screenshots/mobile-app/scan-qr-code.jpg)

**Method 2**
Select *Enter details manually*, and in the screen enter the following:
- *Issuer* will be the account name
- *Secret* will be the Code from the CSV generated in step 2
- *Account* will be the username used with this 2FA (optional, just in case having multiple accounts with same name, to be able to differentiate which is for which)
- *Advanced* is to create tags to filter them later on easily

![manual details](/screenshots/mobile-app/manual-details.jpg)

Regardless you went with Method 1 or 2, both should provide a final result similar to the one below (appearance might differ based on the settings of the app itself).

![ente auth](/screenshots/mobile-app/ente-auth.jpg)




## License
This document guide is licensed under the CC0 1.0 Universal license. The terms of the license are detailed in [LICENSE](/LICENSE)
