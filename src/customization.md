# Customization

## Descriptions

Each description in the [`COOKIE_CONSENT_SETTINGS`](cookie-consent-settings.md) configuration can be either set as `description` translatable string, or as a `description_template_name` template path with any necessary HTML markup.

## Layout options

There are five possible modal dialog layout options, set as the `dialog_position` setting:

- `center` - a dialog box is shown in the middle of the window.
- `top` - a dialog is at the full-width top bar.
- `left` - a dialog is at the left sidebar.
- `right` - a dialog is at the right sidebar.
- `bottom` - a dialog is at the full-width bottom bar.

The `show_dialog_close_button` setting tells whether to show or hide the close button for the dialog. Setting it to `False` is recommended because you need somewhat consent before activating JavaScript functionalities on your websites as soon as possible.

## Button texts

Button texts can be cusomized by the `buttons` setting:

```python
"buttons": {
    "accept_all_dialog": _("Accept all"),
    "reject_all_dialog": _("Reject all"),
    "manage_cookies": _("Manage cookies"),
    "accept_all_config": _("Accept all"),
    "reject_all_config": _("Reject all"),
    "save_preferences": _("Save preferences"),
    "save_and_close": _("Save and close"),
    "close": _("Close"),
},
```

## Styling

The modal dialog uses the fonts of the website.

Colors can be modified by CSS custom properties:

```css
body {
  --cc-primary: #007bff;
  --cc-light-gray: #ccc;
  --cc-dark-gray: #343a40;
  --cc-light: #fff;
  --cc-backdrop: rgba(0, 0, 0, 0.5);
}
```

All essential elements have CSS classes with `cc-` prefix, and you can overwrite some of their properties.

If you are using a CSS framework like Bootstrap or TailwindCSS, you can set the extra CSS classes for particular elements in the `styling` section of [`COOKIE_CONSENT_SETTINGS`](cookie-consent-settings.md):

```python
"styling": {
    "primary_button_css_classes": "",
    "secondary_button_css_classes": "",
    "provider_list_css_classes": "",
    "provider_item_css_classes": "",
    "link_css_classes": "",
    "section_anchor_css_classes": "",
},
```

If you can't implement what you need with the methods above, instead you can create a custom CSS file and link to it. And of that is still not an option for you, you can also overwrite the templates.

## Templates

The templates used for the cookie consent management can be copied to the templates directory of the project and overwritten. Mostly necessary templates are these:

- `gdpr_cookie_consent/descriptions/what_are_cookies.html` is the description of the cookies in the modal dialog and cookie management page.
- `gdpr_cookie_consent/descriptions/extra.html` is the extra HTML going at the bottom of cookie management page.
- `gdpr_cookie_consent/modal_dialog.html` is the template for the cookie consent's modal dialog.
- `gdpr_cookie_consent/cookies_management.html` is the template for the external page.

## Passing Variables to Cookie Consent Management Templates

If you don't want to hardcode some variables like API keys in the conditional HTML files, you can pass them in `extra_context` section of [`COOKIE_CONSENT_SETTINGS`](cookie-consent-settings.md):

```python
"extra_context": {
    "GOOGLE_ANALYTICS_TRACKING_ID": get_secret("GOOGLE_ANALYTICS_TRACKING_ID"),
    "STRIPE_PUBLISHABLE_KEY": get_secret("STRIPE_PUBLISHABLE_KEY"),
},
```

In this example, `get_secret()` should be a function implemented by you, that reads the secret values from the environment variables, unversioned `secrets.json` file, or elsewhere.

The variables mentioned in `extra_context` will be passed to all templates of Django GDPR Cookie Consent.

## Conditional Rendering in Templates

Enable `gdpr_cookie_consent` context processor in the template settings:

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

Now you can check if a section was chosen by the visitor in any template by this:

```html
{% if "functionality" in cookie_consent_controller.checked_sections %}
    <!-- add some optional functionality -->
{% endif %}
```

Note that this conditional rendering will happen only after page reload, not just after closing the Cookie Consent modal dialog.

## Conditional Rendering in JavaScript

If you are using ReactJS, Vue, AngularJS, or other frontend library to render HTML, you can check if a cookie section was chosen by the following technique. At first set a global variable in conditional templates:

```html
{# conditional_html/functionality.html #}
<script>
window.ENABLE_YOUTUBE_EMBEDS = true;
</script>
```

Then check for that value in some rendering functions:

```javascript
// some javascript file
if (window.ENABLE_YOUTUBE_EMBEDS) {
   embed_youtube_videos();
}
```

## Using Content-Security-Policy with Django-CSP

Django GDPR Cookie Consent is compatible with Content-Security-Policy via Django-CSP app. To use it together, you just need to do two things.

In the project's settings, enable nonces:

```python
CSP_INCLUDE_NONCE_IN = ["script-src", "style-src"]
```

In the conditional html snippets, set nonces for inline `<style>` and `<script>` tags:

```html
<style nonce="{{ request.csp_nonce }}">
    body {
        background: red;
    }
</style>

<script nonce="{{ request.csp_nonce }}">
    console.log('Conditional script loaded!');
</script>
```
