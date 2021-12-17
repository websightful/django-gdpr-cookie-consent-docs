# Getting Started

## How to Install

### 1. Download and install the package with pip

[Get Django GDPR Cookie Consent from Gumroad](https://websightful.gumroad.com/l/django-gdpr-cookie-consent).

Create a directory `private_wheels/` in your project's repository and add the wheel file of the app there.

Link to this file in your `requirements.txt`:

```
Django==3.2
private_wheels/django_gdpr_cookie_consent-1.1.0-py2.py3-none-any.whl
```

Install the pip requirements from the `requirements.txt` file into your project's virtual environment:

```shell
(venv)$ pip install -r requirements.txt
```

Alternatively to start quickly, install the wheel file into your Django project's virtual environment right from the shell:

```shell
(venv)$ pip install /path/to/django_gdpr_cookie_consent-1.1.0-py2.py3-none-any.whl
```


### 2. Add the app to INSTALLED_APPS of your project settings

```python
INSTALLED_APPS = [
    # …
    "gdpr_cookie_consent.apps.GdprCookieConsentConfig",
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


## How to Use

### 1. Copy, paste, and modify the cookie consent configuration

Copy the example of [`COOKIE_CONSENT_SETTINGS`](cookie-consent-settings.md) to the end of your project settings, then modify it to match the cookie usage of your website.

In the example above, there are four cookie sections: __Essential__ (strictly necessary), __Functionality__ (optional), __Performance__ (optional), and __Marketing__ (optional).

### 2. Create templates for your conditional HTML snippets

Create templates for the snippets that will be loaded or rendered when a particular section is chosen, for example:

- conditional_html/functionality.html
- conditional_html/performance.html
- conditional_html/marketing.html

For testing, you can add the markup like:

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

The conditional html might include external or inline styles, external or inline  JavaScripts, and other HTML snippets.

Manage the scripts that create your __Essential (strictly necessary)__ cookies separately, unrelated to conditional html snippets.

### 3. Translate your titles and descriptions

If your website has more than one language, prepare the translations:

- Use management command `makemessages` to collect translatable strings into `django.po` files.
- Translate the strings to the languages you need.
- Then, use the management command `compilemessages` to compile them to `django.mo` files.
