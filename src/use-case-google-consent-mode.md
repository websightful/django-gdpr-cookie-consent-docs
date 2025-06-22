# Use Case: Django GDPR Cookie Consent with Google Consent Mode and Google Tag Manager

This guide helps you respect user privacy while using Google services. When a user accepts cookies, we’ll update **Google Consent Mode** via **Google Tag Manager (GTM)** using **Django GDPR Cookie Consent**.

**Google Tag Manager (GTM)** is a web-based console provided by Google where you manage all your tracking tags (like Google Analytics, Facebook Pixel, etc.) from one place. You add just one GTM snippet to your website, and then control what gets loaded through the GTM interface — no need to constantly update your site’s code.

**Google Consent Mode** is a JavaScript-based API that runs on your website. It lets you tell Google services whether the user has granted permission for tracking (like analytics or advertising). Based on the user's choices, it adjusts how data is collected — for example, sending anonymized data if the user declined cookies.


## What You’ll Need

* `django-gdpr-cookie-consent` is installed and working.
* You’ve added a GTM container to your site.
* You know your **Google Analytics 4 (GA4)** Measurement ID, e.g., `G-XXXXXXX`.

## 1. Setup Django Cookie Groups & GTM Snippets

### Prepare the Django Settings:

Add `gdpr_cookie_consent` to context processors in your Django settings:

```python
TEMPLATES = [
    {
        # …
        "OPTIONS": {
            "context_processors": [
                # …
                "gdpr_cookie_consent.context_processors.gdpr_cookie_consent",
            ],
        },
    },
]
```

Also prepare `performance` and `marketing` sections in `COOKIE_CONSENT_SETTINGS`:

```python
from django.utils.translation import gettext_lazy as _

COOKIE_CONSENT_SETTINGS = {
    "base_template_name": "base.html",
    "description_template_name": "gdpr_cookie_consent/descriptions/what_are_cookies.html",
    "dialog_position": "center",
    "consent_cookie_max_age": 60 * 60 * 24 * 30 * 6,
    "sections": [
        {
            "slug": "essential",
            "title": _("Essential Cookies"),
            "required": True,
            "preselected": True,
            "summary": _(
                "These cookies are always on, as they’re essential for making this website work, and making it safe. Without these cookies, services you’ve asked for can’t be provided."),
            "description": _(
                "These cookies are always on, as they’re essential for making this website work, and making it safe. Without these cookies, services you’ve asked for can’t be provided."),
            "providers": [
                {
                    "title": _("This website"),
                    "cookies": [
                        {
                            "cookie_name": "sessionid",
                            "duration": _("2 Weeks"),
                            "description": _(
                                "Session ID used to authenticate you and give permissions to use the site."),
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "csrftoken",
                            "duration": _("Session"),
                            "description": _(
                                "Security token used to ensure that no hackers are posting forms on your behalf."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "cookie_consent",
                            "duration": _("6 Years"),
                            "description": _("Settings of Cookie Consent preferences."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                    ]
                },
            ],
        },
        {
            "slug": "performance",
            "title": _("Performance Cookies"),
            "required": False,
            "summary": _(
                "These cookies help us analyse how many people are using this website, where they come from and how they're using it. If you opt out of these cookies, we can’t get feedback to make this website better for you and all our users."),
            "description": _(
                "These cookies help us analyse how many people are using this website, where they come from and how they're using it. If you opt out of these cookies, we can’t get feedback to make this website better for you and all our users."),
            "providers": [
                {
                    "title": _("Google Analytics"),
                    "description": _("Google Analytics is used to track website usage statistics."),
                    "description_template_name": "",
                    "cookies": [
                        {
                            "cookie_name": "_ga",
                            "duration": _("2 Years"),
                            "description": _("Used to distinguish users."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "_gid",
                            "duration": _("24 Hours"),
                            "description": _("Used to distinguish users."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "_ga_*",
                            "duration": _("2 Years"),
                            "description": _("Used to persist session state."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                        {
                            "cookie_name": "_gat_gtag_UA_*",
                            "duration": _("1 Minute"),
                            "description": _("Stores unique user ID."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                    ]
                },
            ],
        },
        {
            "slug": "marketing",
            "title": _("Marketing Cookies"),
            "required": False,
            "summary": _(
                "These cookies are set by our advertising partners to track your activity and show you relevant ads on other sites as you browse the internet."),
            "description": _(
                "These cookies are set by our advertising partners to track your activity and show you relevant ads on other sites as you browse the internet."),
            "providers": [
                {
                    "title": _("Google Ads"),
                    "description": _("These cookies are related to Google Ads conversion tracking."),
                    "description_template_name": "",
                    "cookies": [
                        {
                            "cookie_name": "_gac_gb_*",
                            "duration": _("90 Days"),
                            "description": _(
                                "Contains campaign related information. If you have linked your Google Analytics and Google Ads accounts, Google Ads website conversion tags will read this cookie unless you opt-out."),
                            "description_template_name": "",
                            "domain": ".example.com",
                        },
                    ]
                },
                {
                    "title": _("Facebook Pixel"),
                    "description": _(
                        "Facebook Pixel lets us track how well our ads on Facebook performed, and which ads led to registration or subscriptions."),
                    "description_template_name": "",
                    "cookies": [
                        {
                            "cookie_name": "c_user",
                            "duration": _("3 Months / Session"),
                            "description": _("Contains the user ID of the currently logged in user."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "datr",
                            "duration": _("2 Years"),
                            "description": _(
                                "Identifies the web browser being used to connect to Facebook independent of the logged in user."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "sb",
                            "duration": _("Persistent"),
                            "description": _("For storing browser details."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "dpr",
                            "duration": _("7 Days"),
                            "description": _("For delivering an optimal experience for your device’s screen."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "x-referer",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "spin",
                            "duration": _("25 Hours"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "xs",
                            "duration": _("3 Months / Session"),
                            "description": _("Stores the session ID."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "fr",
                            "duration": _("3 Months / Session"),
                            "description": _("Provides ad delivery or retargeting."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "usida",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "presence",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "_fbp",
                            "duration": _("3 Months"),
                            "description": _(
                                "Used to deliver a series of advertisement products such as real time bidding from third party advertisers."),
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                        {
                            "cookie_name": "m_pixel_ratio",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".facebook.com",
                        },
                    ]
                },
            ],
        },
    ]
}
```

### Update your `base.html` template:

Add these two scripts in the `<head>` section:

```html
<script nonce="{{ request.csp_nonce }}">
   window.dataLayer = window.dataLayer || [];
   function gtag(){dataLayer.push(arguments);}
   
   gtag('consent', 'default', {
      'analytics_storage': '{% if "performance" in cookie_consent_controller.checked_sections %}granted{% else %}denied{% endif %}',
      'ad_storage': '{% if "marketing" in cookie_consent_controller.checked_sections %}granted{% else %}denied{% endif %}',
      'ad_user_data': '{% if "marketing" in cookie_consent_controller.checked_sections %}granted{% else %}denied{% endif %}',
      'ad_personalization': '{% if "marketing" in cookie_consent_controller.checked_sections %}granted{% else %}denied{% endif %}',
      'functionality_storage': 'denied',
      'personalization_storage': 'denied',
      'security_storage': 'granted'  // usually okay to grant by default
   });
</script>

<!-- Google Tag Manager (head) -->
<script nonce="{{ request.csp_nonce }}">(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXXX');</script>
```

Add this iframe in the beginning of the `<body>` tag:

```html
<!-- Google Tag Manager (body) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-XXXXXXX"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
```

Add the modal dialog of **Django GDPR Cookie Consent** at the end of the `<body>` tag:

```django
<!-- Show cookie banner -->
{% include "gdpr_cookie_consent/includes/cookie_consent.html" %}
```

Replace `GTM-XXXXXXX` with your real GTM container ID.

## 2. Create JavaScript custom event handlers for granding and denying consent

In a JavaScript file that is included in your base template, add these custom event handlers:

```javascript
document.addEventListener('grantGDPRCookieConsent', (e) => {
    console.log(`${e.detail.section} cookies granted`);
    if (e.detail.section === 'performance') {
        gtag('consent', 'update', {
            'analytics_storage': 'granted'
        });
        dataLayer.push({'event': 'analytics_consent_granted'});
    } else if (e.detail.section === 'marketing') {
        gtag('consent', 'update', {
            'ad_storage': 'granted',
            'ad_user_data': 'granted',
            'ad_personalization': 'granted'
        });
        dataLayer.push({'event': 'marketing_consent_granted'});
    }
});

document.addEventListener('denyGDPRCookieConsent', (e) => {
    console.log(`${e.detail.section} cookies denied`);
    if (e.detail.section === 'performance') {
        gtag('consent', 'update', {
            'analytics_storage': 'denied'
        });
        dataLayer.push({'event': 'analytics_consent_denied'});
    } else if (e.detail.section === 'marketing') {
        gtag('consent', 'update', {
            'ad_storage': 'denied',
            'ad_user_data': 'denied',
            'ad_personalization': 'denied'
        });
        dataLayer.push({'event': 'marketing_consent_denied'});
    }
});
```

## 3. Set Up GTM to React to Consent

You’ll use the **Google Tag Manager (GTM) web interface** — a dashboard at [tagmanager.google.com](https://tagmanager.google.com) — to add and manage all your tracking scripts (called *tags*). You won’t have to edit your website code every time you want to change something — GTM handles it all from one place.

Here’s how to configure GTM to load scripts **only after the user gives consent**, using the signals from **Google Consent Mode**.

### GA4 Configuration Tag

This tag initializes Google Analytics 4 (GA4) tracking.

* **Tag Type**: GA4 Configuration
* **Measurement ID**: Your GA4 ID (`G-XXXXXXX`)
* **Trigger**: `Consent Initialization – All Pages` (this runs very early, before other tags)

This setup allows GA4 to start in a consent-aware way. It reads the **default denied** state at page load and will automatically adjust when the user accepts cookies later.

### GA4 Event Tags

These are additional GA4 tags to track specific actions (like form submissions or button clicks).

* **Tag Type**: GA4 Event
* **Trigger**: Choose based on the action (e.g., form submit, button click)

You don’t need to check for consent manually here — **GA4 automatically tracks or holds data based on the user's consent** provided through Google Consent Mode.

### Google Ads Tags

For conversion tracking and remarketing (showing ads to users who visited your site):

* **Tag Type**: Google Ads Conversion Tracking or Remarketing
* **Trigger**: Set this to fire after a successful action (like a purchase or sign-up)

These tags **respect consent choices**, like whether the user allowed `ad_storage`.

### Facebook Pixel or Other Custom Tools

If you’re using tools that don’t automatically understand Google Consent Mode (like Facebook Pixel), you’ll need to set them up to run **only when the user gives marketing consent**.

1. **Create a Variable**:
    * **Type**: Data Layer Variable
    * **Name**: `event`
2. **Create a Trigger**:
    * **Type**: Custom Event
    * **Event Name**: `marketing_consent_granted`
3. **Create a Tag**:
    * **Type**: Custom HTML
    * **Code**: Paste in your Facebook Pixel code
    * **Trigger**: Use the `marketing_consent_granted` trigger

This setup ensures your custom script only runs if the user actively agrees to marketing cookies.

## 4. Test and Debug

### Use GTM’s Preview Mode:

1. [Enable Preview in GTM](https://tagassistant.google.com/).
2. Visit your site.
3. Accept cookies.
4. You should see these events:
   * `analytics_consent_granted` → GA4 tag should fire.
   * `marketing_consent_granted` → Facebook tag should fire.
5. Check which tags fired — they should match the user’s choices.
6. The Consent tab of each event should show the correct preferred consent choices.

### Use Browser's Developer Tools:

* Open **Network tab**
* GA4: Look for requests to `/g/collect` or `analytics.google.com`
* Facebook: Look for requests to `fbevents.js` or `facebook.com/tr/`
* Confirm they include `gcs` or `gcd` parameters in GA4 Consent Mode (that means consent data was sent)

## You’re Done!

Now your Django site is:

* Respecting user privacy.
* Compliant with **Google Consent Mode**.
* Only firing analytics and marketing tags after consent.

## Next Steps

If you add more scripts via **Google Tag Manager** in the future, make sure that you list all the cookies that these scripts create, in the `COOKIE_CONSENT_SETTINGS`.

For more information, check this article about [EU-focused data and privacy using Google Analytics](https://support.google.com/analytics/answer/12017362?hl=en).
