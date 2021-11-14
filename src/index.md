# Django GDPR Cookie Consent

## Why do you need it?

As stated by [GDPR cookie law](https://gdpr.eu/cookies/), websites that serve content for people from European Union must get consent from website visitors before storing any cookies that are not strictly necessary for the website to function. Not complying with GDPR laws can result in a fine of up to €20 million or 4% of the company's annual revenue, whichever is greater.

## Who is it for?

Django GDPR Cookie Consent app was created for Django developers who need to integrate a Cookie Consent dialog in their Django projects. The existing open-source Django alternatives at the time of writing were not fully compliant or not feature complete. In addition, the current external services were costly and not customizable enough.

| Feature                        | Value                                          |
|--------------------------------|------------------------------------------------|
| Supported modern browsers      | Chrome, Firefox, Safari, Opera, Microsoft Edge |
| Supported Django versions      | 2.2, 3.0, 3.1, 3.2                             |
| Supported Python versions      | 3.5, 3.6, 3.7, 3.8                             |
| Responsive layout              | ✔︎                                              |
| Translatable                   | ✔︎                                              |
| Configurable                   | ✔︎                                              |
| Unlimited websites             | ✔︎                                              |
| Continuous cookie consent logs | ✔︎                                              |
| Latest package version         | 0.3.2                                          |

## What are the benefits?

There are lots of benefits:

- Saves at least a week of development work.
- Highly flexible and configurable.
- Can be used for as many Django websites as necessary.
- No external dependencies, just Django>=2.2, Python 3, and plain modern JavaScript.
- Comes with pretty nice-looking default styling and a responsive layout.
- Typography and buttons will match your website's style (no iframes used).
- All functionality can be extended, overwritten, or replaced.
- Uses configuration in Django settings and templates.
- No need to modify or adapt scripts from third parties (most of the time).
- Logs the cookie consent choices from visitors into the database for legal compliance.
- Developed with internationalization in mind.
- Compatible with Content-Security-Policy via Django-CSP.
- MIT license applied.
- No recurring subscriptions, no limitation per domain, just a single payment.
- Designed and implemented by the author of "Web development with Django Cookbook."

## How does it work?

Django GDPR Cookie Consent app allows you to set up a modal dialog for cookie explanations and preferences. When a specific cookie section is accepted, the widget loads or renders HTML snippets related to that section. For example, if a visitor approved Performance cookies, they would get Google Analytics loaded.

Using the Django GDPR Cookie Consent app, you store the following information about the cookies in Django project settings:

- What are the cookie sections (e.g. "Essential", "Functionality", "Performance", "Marketing")? Are they bounded with any conditional HTML snippets?
- What are the cookie providers within each section (e.g., "This website," "Google Analytics," "Facebook," "Youtube," etc.)?
- What are the cookies set by each of those providers?

Descriptions for sections, providers, or cookies are translatable. User preferences are saved in a cookie too. If a particular section is unselected later, cookies related to that section are attempted to get deleted.

## Examples

Here are the recorded [Selenium tests](https://github.com/archatas/django-gdpr-cookie-consent-demo-project) for a visual preview:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/nSCdNCHQKUY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Django GDPR Cookie Consent is used in production at these websites:

- [1st things 1st](https://www.1st-things-1st.com)
- [DjangoTricks](https://www.djangotricks.com)

## Disclaimer

The actual website's compliance with the GDRP Cookie Law depends on the configuration of each use case. Django GDPR Cookie Consent app provides the mechanism to make that possible, but it's up to you how you configure and integrate it.

## Contact

For technical questions or bug reports, please contact Aidas Bendoraitis by [@archatas](https://github.com/archatas) on Github or [@DjangoTricks](https://twitter.com/DjangoTricks) on Twitter.
