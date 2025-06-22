# Getting Started

## How to Install Django GDPR Cookie Consent

### 1. Download and install the package with pip

[Get Django GDPR Cookie Consent from Gumroad](https://websightful.gumroad.com/l/django-gdpr-cookie-consent).

Create a directory `private_wheels/` in your project's repository and add the wheel file of the app there.

Link to this file in your `requirements.txt`:

```
Django==5.2
file:./private_wheels/django_gdpr_cookie_consent-4.0.0-py2.py3-none-any.whl
```

Install the pip requirements from the `requirements.txt` file into your project's virtual environment:

```shell
(venv)$ pip install -r requirements.txt
```

Alternatively to start quickly, install the wheel file into your Django project's virtual environment right from the shell:

```shell
(venv)$ pip install /path/to/django_gdpr_cookie_consent-4.0.0-py2.py3-none-any.whl
```


### 2. Add the app to INSTALLED_APPS of your project settings

```python
INSTALLED_APPS = [
    # …
    "gdpr_cookie_consent",
    # …
]
```

### 3. Add path to urlpatterns

```python
from django.urls import path, include

urlpatterns = [
    # …
    path(
        "cookies/",
        include("gdpr_cookie_consent.urls", namespace="cookie_consent"),
    ),
    # …
]
```

### 4. Include the widget to your template(s)

Load the CSS somewhere in the `<head>` section:

```html
<link href="{% static 'gdpr-cookie-consent/css/gdpr-cookie-consent.css' %}" rel="stylesheet" />
```

Include the widget just before the closing `</body>` tag:

```html
{% include "gdpr_cookie_consent/includes/cookie_consent.html" %}
```

Link to the cookie management view, for example, in the website's footer:

```html
{% url "cookie_consent:cookies_management" as cookie_management_url %}
<a href="{{ cookie_management_url }}" rel="nofollow">
    {% trans "Manage Cookies" %}
</a>
```

### 5. Copy, paste, and modify the cookie consent configuration

Copy the example of [`COOKIE_CONSENT_SETTINGS`](cookie-consent-settings.md) to the end of your project settings, then modify it to match the cookie usage of your website.

In the example above, there are four cookie sections: __Essential__ (strictly necessary), __Functionality__ (optional), __Performance__ (optional), and __Marketing__ (optional).

### 6. Create templates for your conditional HTML snippets

To respect visitor choices, certain cookies—such as those for analytics or marketing—should only be set after obtaining proper consent. One way to manage this is by using conditional templates that render cookie-related code only when a user has opted in.

For example, analytics providers often recommend placing their tag in the `<head>` section. However, if you're using Django GDPR Cookie Consent to let visitors choose whether to accept analytics cookies, then the analytics tag (which sets those cookies) must be included conditionally—only if the visitor has given consent.

The conditional HTML can include external or inline styles, JavaScript files or blocks, and other HTML snippets.

Create templates for the snippets that will be loaded or rendered when a particular section is chosen, for example:

* `conditional_html/functionality.html`
* `conditional_html/performance.html`
* `conditional_html/marketing.html`

For testing, you can add markup like:

```html
<script>
    console.log('Conditional functionality code loaded.');
</script>
```

```html
<script>
    console.log('Conditional performance code loaded.');
</script>
```

```html
<script>
    console.log('Conditional marketing code loaded.');
</script>
```

**Manage the scripts that create your strictly necessary cookies separately, unrelated to the Django GDPR Cookie Consent app.**

Note: Since version 4.0.0, using conditional templates is optional. You can instead rely on the provided context processor or special JavaScript to check which cookie categories are active and control rendering accordingly.

### 7. Add event handlers if you need to track cookie consent changes

In your base template, include a JavaScript code that handles tracking of cookie consent preference changes:

```javascript
document.addEventListener('grantGDPRCookieConsent', (e) => {
    console.log(`${e.detail.section} cookies granted`);
});

document.addEventListener('denyGDPRCookieConsent', (e) => {
    console.log(`${e.detail.section} cookies denied`);
});

document.addEventListener('changeGDPRCookieConsent', (e) => {
    const comparison = {};
    Object.keys(e.detail.consentPreferences).forEach(key => {
        comparison[key] = {
            before: e.detail.previousConsentPreferences[key],
            after: e.detail.consentPreferences[key],
            changed: e.detail.previousConsentPreferences[key] !== e.detail.consentPreferences[key]
        };
    });
    console.table(comparison);
});
```

Custom JavaScript events:

`grantGDPRCookieConsent` and `denyGDPRCookieConsent` - triggered for each section individually when a user makes a choice.

   - `e.detail.section`: slug of the section.

`changeGDPRCookieConsent` - triggered after all changes are saved.

   - `e.detail.consentPreferences`: an object mapping section slugs to `true` or `false`.
   - `e.detail.previousConsentPreferences`: preferences before the change - an object mapping section slugs to `true`, `false`, or `null`.

For example, these events can be used with [Google Consent Mode](use-case-google-consent-mode.md).

### 8. Check if your setup is correct

Check the correctness of your configuration with the following:

```shell
(venv)$ python manage.py check gdpr_cookie_consent
```

You will get errors about misconfigurations, for example:

> (gdpr\_cookie\_consent.E009) Section "essential" must have at least one provider.

If your intention is to keep the configuration incomplete and Django GDPR Cookie Consent works as is, you can silence the errors by adding them to `SILENCED_SYSTEM_CHECKS` in the settings, e.g.:

```python
SILENCED_SYSTEM_CHECKS = [
    "gdpr_cookie_consent.E009",
]
```

Here is an overview of all errors with generalized descriptions:

- **gdpr\_cookie\_consent.E001**: `COOKIE_CONSENT_SETTINGS` is not defined in the settings.
- **gdpr\_cookie\_consent.E002**: `["base_template_name"]` is not defined in `COOKIE_CONSENT_SETTINGS`.
- **gdpr\_cookie\_consent.E003**: Template defined in `["*_template_name"]` doesn't exist.
- **gdpr\_cookie\_consent.E004**: You cannot set both, `["*"]` and `["*_template_name"]`.
- **gdpr\_cookie\_consent.E005**: `["dialog_position"]` must be one of `"center"`, `"top"`, `"left"`, `"right"`, `"bottom"`.
- **gdpr\_cookie\_consent.E006**: `["sections"]` must contain at least one section.
- **gdpr\_cookie\_consent.E007**: Each section must have a `["slug"]` defined.
- **gdpr\_cookie\_consent.E008**: Slugs for sections must be unique.
- **gdpr\_cookie\_consent.E009**: `["providers"]` for each section must contain at least one provider.
- **gdpr\_cookie\_consent.E010**: `["cookies"]` for each provider must contain at least one cookie.

### 9. Translate your titles and descriptions

If your website has more than one language, prepare the translations:

- Use management command `makemessages` to collect translatable strings into `django.po` files.
- Translate the strings to the languages you need.
- Then, use the management command `compilemessages` to compile them to `django.mo` files.
