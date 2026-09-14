# Path Pooja website

This is the public product site needed for Razorpay account verification. It is intentionally separate from the Razorpay webhook endpoint.

## Before publishing

Replace every `SUPPORT_EMAIL` in `site/` with a real, monitored support address. Ensure the cancellation and refund policy matches the business policy you intend to follow.

The site now includes a **Pay securely with Razorpay** button. It does not require website login. To make it open a real Razorpay-hosted checkout, create a Razorpay **Subscription Link** in the Dashboard and paste its `https://rzp.io/...` URL into `site/payment-config.js`.

Do not put a Razorpay API key secret in the website. A dashboard-created Subscription Link is hosted by Razorpay and is the safe no-login option.

## Publish with Firebase Hosting

The website is configured for the existing Firebase project `poojapath-3756c`.

```bash
firebase login
firebase deploy --only hosting
```

Firebase will give you a public website URL, normally `https://poojapath-3756c.web.app`. Put that URL in Razorpay's **Add your website link** field.

After Razorpay verifies the account, configure the separate webhook at:

```text
https://asia-south1-poojapath-3756c.cloudfunctions.net/razorpayWebhook
```

The webhook must be deployed from the Android project because its Firebase Functions source lives in `../AndroidStudioProjects/PathPooja/functions`.

## Publish with Cloudflare Pages

This project is plain static HTML; do not select a React, Android, Kotlin or other framework preset.

In **Workers & Pages → Create application → Pages → Connect to Git**, set:

| Field | Value |
| --- | --- |
| Framework preset | `None` |
| Root directory | `/` (leave blank if the repository root is selected) |
| Build command | `npm run build` |
| Build output directory | `site` |

The `build` script only checks that the static files are present; it has no dependencies and produces the deployable files directly in `site/`.

If this is a Direct Upload instead of a Git-connected Pages project, upload the **contents** of the `site/` directory. Do not upload the repository root.
