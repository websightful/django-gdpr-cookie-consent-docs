# The Example of Cookie Consent Settings

Copy this to your project's settings and modify according to your website's needs:

```python
from django.utils.translation import gettext_lazy as _

COOKIE_CONSENT_SETTINGS = {
    # Base template will be used for the cookie management view.
    # It should contain {% block content %}{% endblock %}
    "base_template_name": "base.html",

    # Description will be used in the modal dialog and cookie management view.
    # Provide either a translatable string or a HTML template name.
    "description": "",
    "description_template_name": "gdpr_cookie_consent/descriptions/what_are_cookies.html",

    # Extra information will be used at the end of cookie management view.
    # Provide either a translatable string or a HTML template name.
    "extra_information": "",
    "extra_information_template_name": "gdpr_cookie_consent/descriptions/extra.html",
    
    # Button texts can be customized.
    "buttons": {
        "accept_all_dialog": _("Accept all recommended"),
        "reject_all_dialog": _("Reject non-required"),
        "manage_cookies": _("Manage cookies"),
        "accept_all_config": _("Accept all"),
        "reject_all_config": _("Reject all"),
        "save_preferences": _("Save preferences"),
        "save_and_close": _("Save and close"),
        "close": _("Close"),
    },

    # Should it be possible to close the dialog without choosing a consent?
    "show_dialog_close_button": True,
    
    # Dialog position: center, top, left, right, bottom
    "dialog_position": "center",
    
    # All important elements will have CSS classes with `cc-` prefix,
    # by which you can target them and overwrite their styling.
    # But you can attach some CSS classes to certain elements too.
    "styling": {
        "primary_button_css_classes": "",
        "secondary_button_css_classes": "",
        "provider_list_css_classes": "",
        "provider_item_css_classes": "",
        "link_css_classes": "",
        "section_anchor_css_classes": "",
    },
    
    # Any variables that will be passed to conditional HTML snippets
    "extra_context": {
        "API_KEY_1": "abcdefg12345",
        "API_KEY_2": "12345abcdefg",
    },

    # Consent cookie max age say how many seconds to keep the cookie consent preferences.
    # For example, it can be approximately six months
    "consent_cookie_max_age": 60 * 60 * 24 * 30 * 6,

    # Sections define the purposes of cookie groups.
    # For example: Essential, Functionality, Performance, and Marketing
    "sections": [
        {
            # The slug will be used as a readable identifier of the section
            "slug": "essential",

            # Translatable title of the section
            "title": _("Essential Cookies"),

            # Required sections will be already selected and read-only
            "required": True,

            # Preselected sections will be selected in the configuration page before the consent is given.
            # (all required sections will be preselected too and this setting is ignored for those)
            # N.B. Laws of the United Kingdom, Germany, and France require that
            # the opt-in consent for cookies must not be pre-enabled,
            # so consult your lawyers before enabling this setting.
            "preselected": True,

            # Section summary will be shown in the modal dialog and preferences form.
            # Provide either a translatable string or a HTML template name.
            "summary": _(
                "These cookies are always on, as they’re essential for making this website work, and making it safe. Without these cookies, services you’ve asked for can’t be provided."),
            "summary_template_name": "",

            # Section description will be used at the extended cookie explanation in the cookie management view.
            # Provide either a translatable string or a HTML template name.
            "description": _(
                "These cookies are always on, as they’re essential for making this website work, and making it safe. Without these cookies, services you’ve asked for can’t be provided."),
            "description_template_name": "",

            # Cookie providers are websites that set the cookies on your website
            "providers": [
                {
                    # Translatable title of the provider
                    "title": _("This website"),

                    # Provider description will be used at the extended cookie explanation in the cookie management view.
                    # Provide either a translatable string or a HTML template name.
                    "description": "",
                    "description_template_name": "",

                    # A list of cookies set by the provider
                    "cookies": [
                        {
                            # Cookie name can include wildcard syntax like "abc_*"
                            "cookie_name": "sessionid",

                            # Human-readable translated duration of the cookie
                            "duration": _("2 Weeks"),

                            # Cookie description will be used at the extended cookie explanation in the cookie management view.
                            # Provide either a translatable string or a HTML template name.
                            "description": _(
                                "Session ID used to authenticate you and give permissions to use the site."),
                            "description_template_name": "",

                            # Cookie domain will be used to delete cookies if a section is unchecked
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
            "slug": "functionality",
            "title": _("Functionality Cookies"),

            # Conditional HTML snippet will be loaded or rendered if this section is selected
            "conditional_html_template_name": "conditional_html/functionality.html",
            "required": False,
            "summary": _(
                "These cookies help us provide enhanced functionality and personalisation, and remember your settings. They may be set by us or by third party providers."),
            "summary_template_name": "",
            "description": _(
                "These cookies help us provide enhanced functionality and personalisation, and remember your settings. They may be set by us or by third party providers."),
            "description_template_name": "",
            "providers": [
                {
                    "title": _("Youtube"),
                    "description": _(
                        "Youtube is a video platform from which we are embedding video tutorials and other informational videos."),
                    "description_template_name": "",
                    "cookies": [
                        {
                            "cookie_name": "CONSENT",
                            "duration": _("2 Years"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".youtube.com",
                        },
                        {
                            "cookie_name": "VISITOR_INFO1_LIVE",
                            "duration": _("6 Months"),
                            "description": "",
                            "description_template_name": "",
                        },
                        {
                            "cookie_name": "YSC",
                            "duration": _("Session"),
                            "description": "",
                            "description_template_name": "",
                        },
                        {
                            "cookie_name": "1P_JAR",
                            "duration": _("1 Month"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".google.com",
                        },
                        {
                            "cookie_name": "NID",
                            "duration": _("6 Months"),
                            "description": "",
                            "description_template_name": "",
                            "domain": ".google.com",
                        },
                    ]
                },
            ],
        },
        {
            "slug": "performance",
            "title": _("Performance Cookies"),
            "conditional_html_template_name": "conditional_html/performance.html",
            "required": False,
            "summary": _(
                "These cookies help us analyse how many people are using this website, where they come from and how they're using it. If you opt out of these cookies, we can’t get feedback to make this website better for you and all our users."),
            "summary_template_name": "",
            "description": _(
                "These cookies help us analyse how many people are using this website, where they come from and how they're using it. If you opt out of these cookies, we can’t get feedback to make this website better for you and all our users."),
            "description_template_name": "",
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
                            "cookie_name": "_gac_gb_*",
                            "duration": _("90 Days"),
                            "description": _(
                                "Contains campaign related information. If you have linked your Google Analytics and Google Ads accounts, Google Ads website conversion tags will read this cookie unless you opt-out."),
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
            "conditional_html_template_name": "conditional_html/marketing.html",
            "required": False,
            "summary": _(
                "These cookies are set by our advertising partners to track your activity and show you relevant ads on other sites as you browse the internet."),
            "summary_template_name": "",
            "description": _(
                "These cookies are set by our advertising partners to track your activity and show you relevant ads on other sites as you browse the internet."),
            "description_template_name": "",
            "providers": [
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
