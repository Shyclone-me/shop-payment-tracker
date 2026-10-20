# Play Console: what to answer (Policy > App content, and the SMS declaration)

Wording in the Console changes over time. Where this file says "closest option", pick the option that matches the meaning.

## 1. Permissions declaration: SMS (App content > Sensitive app permissions > SMS and Call Log)
Google's policy (support.google.com/googleplay/android-developer/answer/10208820) lists an exception, subject to review, for "SMS-based money management" apps that track and manage budget. Choose that.

Use this text (adjust only if the app changes):

**Core functionality**
Shop Payment Tracker is a money-management ledger for small shopkeepers. Its core function is to read incoming payment-credit SMS alerts from the bank or wallet senders that the owner explicitly selects, and record the amount, time, sender and message text in an on-device ledger with daily and all-time totals.

**RECEIVE_SMS** is used to detect a new payment alert as it arrives from a selected sender. Messages from any other sender are discarded immediately and never stored.

**READ_SMS** is used (1) once, during setup or when the owner presses "Add account", to scan the last 7 days of the inbox and list sender names so the owner can choose which senders are payment accounts. The scan result is held in memory and discarded when the screen closes; only counts and one short preview of a payment-like message are shown to the owner; and (2) when the owner adds a sender, to import that day's received-payment messages from that sender only.

**Data handling**
All data stays on the device. The app has no server and no network permission, and it does not share, sell or transmit SMS content. The owner can remove a sender or delete all data at any time in Settings.

**Why no alternative works**
The SMS Retriever and SMS User Consent APIs only deliver one-time verification codes. There is no alternative API that lets an app see payment confirmation messages from the owner's chosen bank or wallet senders.

**Default handler**
The app is not the default SMS handler and does not send, delete or modify messages.

### Demo video (Google asks for a link, usually a YouTube "unlisted" video)
Record the phone screen, about 30 to 60 seconds, in this order:
1. Open the app, show the SMS screen with the disclosure text, tap Allow, and show Android's permission prompt being accepted.
2. Show the sender list and pick one sender.
3. Receive a test SMS from that sender (for example "Rs. 500 received from Test Payer" sent from another phone) and show it appearing in the ledger.
4. Show Settings: Tracked accounts, remove, and Delete data.

### Instructions for the reviewer (App access)
"No login is required. On first launch, grant SMS access when asked. The app lists senders who texted in the last 7 days; tick one. To see a payment, send an SMS containing a credit word and an amount, for example: 'Rs. 500 received from Test Payer', from that sender's number. It appears in the dashboard. Settings has Remove account and Delete data."

## 2. Prominent disclosure (Google checks that the app shows this before the permission prompt)
Onboarding step 3 already does this: it explains what is read and why (including the one-time 7-day sender scan), lists the three privacy points, and only then shows the Allow button, which triggers Android's prompt. Keep it that way: do not move the permission request earlier, and do not shorten the text.

## 3. Data safety form
The app does not send any data off the device, so under Play's definition nothing is "collected" or "shared".
- Does your app collect or share any of the required user data types? **No**
- (Closest option for "data is processed only on device".) If the form asks about SMS specifically: SMS is accessed but only processed on the device and not transmitted.
- Data encrypted in transit: not applicable (no transmission).
- Can users request deletion: the app stores data only on the phone; users delete it in-app (Settings > Delete data) or by uninstalling.
- Privacy policy URL: your hosted privacy-policy.html
Re-check this form if you ever add analytics, crash reporting, ads, cloud backup or any network feature.

## 4. Other App content forms
- **App access:** "All or some functionality is restricted" is not needed for login, but add the reviewer instructions above.
- **Ads:** No ads.
- **Content rating:** answer the IARC questionnaire honestly: no violence, sexual content, gambling, user-generated content or location sharing. Expect a low rating.
- **Target audience:** 18 and over (business tool). Do not select child age groups.
- **News app:** No. **COVID-19:** No. **Government app:** No.
- **Financial features declaration:** the app does not hold, move, lend or invest money. Choose the closest "personal finance / money management tracking" option if listed, otherwise "none of the above"; the app only records messages the owner already received.
- **Health, advertising ID, photo/video permissions:** not applicable (the app requests none; confirm in the AAB permission check in the release steps).

## 5. Things to avoid in the listing
Do not call it an SMS reader or say it "reads your messages aloud". Describe it as a payment ledger with optional voice announcements, as the listing text already does.
