Changelog
=========

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).



[Unreleased]
------------

[v2.0.1] - 2022-10-31
------------------

### Fixed

- Python 3.5, 3.6, 3.7, 3.8 support.

[v2.0.0] - 2022-10-31
------------------

### Added

- Button text customization.
- Possibility to preselect a section in configuration.
- IP addresses in the logs anonymized.
- Python 3.11 support.
- Dialog position configurations: center, top, left, right, bottom.

### Changed

- Dialog close button made optional and not shown by default.

[v1.2.1] - 2022-09-14
------------------

### Added

- Pointer cursor for "Show cookie providers".

### Fixed

- gettext_lazy usage in the settings.


[v1.2.0] - 2022-08-07
------------------

### Added

- Django 4.1 support.

[v1.1.0] - 2021-12-17
------------------

### Added

- Django 4.0 support.

[v1.0.0] - 2021-11-15
------------------

### Added

- Python 3.9 and 3.10 support.

[v0.3.2] - 2021-11-14
------------------

### Fixed

- More spacious layout for the modal dialog on mobile screens.

[v0.3.1] - 2021-11-12
------------------

### Fixed

- The styling for the modal dialog close button fixed for mobile Chrome and Safari.

[v0.3.0] - 2021-11-12
------------------

### Added

- Content-Security-Policy is supported: you can use Django-CSP with nonces for inline scripts and styles.

[v0.2.4] - 2021-11-02
------------------

### Added

- HTML ids added for switch widgets for easier testability.

[v0.2.3] - 2021-11-01
------------------

### Fixed

- Modal dialog submission fix for Safari.

[v0.2.2] - 2021-11-01
------------------

### Fixed

- Multiple `<script>` loading in the conditional snippets fixed.

[v0.2.1] - 2021-10-27
------------------

### Changed

- Modal dialog centered vertically and horizontally.

[v0.2.0] - 2021-10-27
------------------

### Added

- Logging the cookie consent choices in the database because of the legal requirements.

### Fixed

- Disabling the buttons while saving the Cookie Choices so that they are not triggered more than once with slow Internet connections.

[v0.1.11] - 2021-10-24
------------------

### Fixed

- Styling of the switches.

[v0.1.8] - 2021-10-24
------------------

### Fixed

- Styling for the modal dialog close button.

[v0.1.6] - 2021-10-23
------------------

### Removed

- Samesite functionality.

[v0.1.5] - 2021-10-23
------------------

### Added

- Default extra.html template.

[v0.1.4] - 2021-10-23
------------------

### Fixed

- bump2version configuration.

[v0.1.2] - 2021-10-23
------------------

### Added

- Initial commit.


<!--
### Added
### Changed
### Deprecated
### Removed
### Fixed
### Security
-->


