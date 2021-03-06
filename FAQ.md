
## Frequently Asked Questions

[TOC]

### Why are we telling developers to use phone numbers? They can be hijacked, change ownership, used to track users, etc.

We aren’t telling developers to use phone number; they are already using them. Despite problems, phone numbers are useful for various purposes (see intro). In the absence of alternatives to achieve these useful things, developers will continue to use phone numbers and users will struggle with them. This proposal makes the web more usable in the short term (in particular matching functionality that already existings natively), but in the long run, we hope to move the ecosystem off of phone numbers as alternatives for the need functionality become available. Given the time-scale of such a transition, action in the short-term to reach experience parity seems necessary.

### Why is this using SMS OTP as a verification mechanism? SMS OTP are slow, expensive, phishable, etc.

SMS is the most common existing verification mechanism; this proposal aims to streamline flows that already exist by mitigating the need for user involvement/action. Although this does not address the more fundamental problems, it may make some situations better (e.g. users don’t handle OTP manually, hence less conditioned to be phished), but starting to move developers to a programmatic model would be a potential stepping stone to better mechanisms.

### Is this implementable on iOS?

Yes, iOS already uses a declarative model (form annotation “[one-time-code](https://developer.apple.com/documentation/security/password_autofill/enabling_password_autofill_on_an_html_input_element)”) and heuristically extracts and suggests OTP from SMS. They could implement an imperative API or support other alternatives like facilitating interaction with identity providers if desired.

### Is this implementable on desktop?

Yes, browser need not look for SMS on the local device. For example, if Chromesync is enabled across desktop and mobile devices, Chrome could retrieve on one device and return the SMS content on another. Or locally, a desktop instance of a browser could use Bluetooth to talk to a nearby phone and have an instance of the browser on the mobile device retrieve and return the SMS.

### Why can’t we use a model like having developer tell us the OTP and let them know if there is a match?

The developer needs something that can be verified on their server, a yes/no response is not sufficient. Claiming that the phone number is active on the device would require a response signed in some way to make it verifiable. This could be done in a number of ways, but is a major departure from the current OTP model and necessitate major changes from developers (i.e. change their backend server logic to process these claims; whereas just returning the SMS contents with the OTP means primarily only a frontend change an minimal backend work ... perhaps only changing the message template format, rather than security-sensitive backend logic). However, the longer-term intent with more comprehensive Identity APIs is to provide verifiable assertions of phone ownership, which would be compelling for developers to adopt and change their system to accept if it meant avoiding the need for sending SMS.

## Self-Review Questionnaire: Security and Privacy

https://www.w3.org/TR/security-privacy-questionnaire

### What information might this feature expose to Web sites or other parties, and for what purposes is that exposure necessary?

The feature **facilitates** the **verification** of phone numbers on the web.  Notably:

* it does not **capacitate** the verification: phone number verification via OTPs is already largely deployed in an already capable web (i.e. input boxes and copy/paste).
* it does not facilitate the **acquisition** of phone numbers on the web (e.g. `<input type="tel">`).

In that context, it is a feature that decreases the friction of verifying phone numbers programmatically with the proper user consent.

We believe the feature is valuable because it supports businesses achieving higher conversation rates in acquisition funnels (which start primarily through ads), moving the needle to the bottom line of their economic sustainability, decreasing friction for users while keeping their consent.

### Is this specification exposing the minimum amount of information necessary to power the feature?

We believe so. It currently exposes an OTP value (an alphanumeric number) that is generated by the website, which confirms the user's possession of the telephone number.

### How does this specification deal with personal information or personally-identifiable information or information derived thereof?

OTPs are ephemeral one time passwords that are generated programmatically and stored in servers, meaning that they expire quickly and can't be used to identify a specific individual effectively in case they are intercepted. They aren't global identifiers either, so they can't be used in server side joins.

### How does this specification deal with sensitive information?

The most sensitive information that is being shared is the confirmation that the user is in possession of a specific phone number (hence, accomplishing the goal of phone number verification).

For that reason, user agents should mediate that consent appropriately, typically via permission prompts.

### Does this specification introduce new state for an origin that persists across browsing sessions?

No. OTPs are ephemeral and handed back, but not stored.

### What information from the underlying platform, e.g. configuration data, is exposed by this specification to an origin?

This API doesn't expose any direct affordance available in the underlying platform. The user has to explicit consent the sharing of the OTP with the origin, so the origin cannot infer any information from a failed request (e.g. whether the user is on a cellular network or not, whether the user is on a phone or not).

### Does this specification allow an origin access to sensors on a user’s device

No.

### What data does this specification expose to an origin? Please also document what data is identical to data exposed by other features, in the same or different contexts.

This feature doesn't cross any origin boundary.

### Does this specification enable new script execution/loading mechanisms?

No.

### Does this specification allow an origin to access other devices?

No.

There is a possible implementation where devices without access to the cellular network (e.g. desktop devices) could coordinate with devices with access to the cellular network (e.g. phones), but we haven't gotten that far yet.

### Does this specification allow an origin some measure of control over a user agent’s native UI?

Yes. 

The browser intermediates the user consent flow using data passed by the origin, specifically, the OTP value (which is an alpha numeric value) but more generally the contents of the SMS.

We went through a series of UX explorations and a series of reviews with developers and users to find one that we were confident about. 

### What temporary identifiers might this this specification create or expose to the web?

None that we are aware of.

One could call an OTP an identifier, but it is (i) short lived, (ii) rotated and (iii) limited to the origin's purview, so it can't be joined.

### How does this specification distinguish between behavior in first-party and third-party contexts?

This feature isn't available in third party contexts.

### How does this specification work in the context of a user agent’s Private Browsing or "incognito" mode?

This features doesn't make any distinction between "incognito" and non "incognito" mode.

### Does this specification have a "Security Considerations" and "Privacy Considerations" section?

[Yes]([https://wicg.github.io/WebOTP/#security](https://wicg.github.io/WebOTP/#security)) and [yes]([https://wicg.github.io/WebOTP/#privacy](https://wicg.github.io/WebOTP/#privacy)).

### Does this specification allow downgrading default security characteristics?

No.