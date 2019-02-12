---
title: Configuring validation messages
seo-title: Configuring validation messages
description: Learn how to specify how validation messages are displayed and their location relative to the form returned in the web browser.
seo-description: Learn how to specify how validation messages are displayed and their location relative to the form returned in the web browser.
uuid: 3bd713cb-7c00-4d16-a918-ca3a09c4c25d
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/configuring_forms
products: SG_EXPERIENCEMANAGER/6.4/FORMS
discoiquuid: d2c11ba4-a4d0-4c3c-a427-b8f59afc55f6
index: y
internal: n
snippet: y
---

# Configuring validation messages{#configuring-validation-messages}

For forms that are rendered as HTML, form validation errors that occur are displayed for the user. You can customize how validation messages are displayed. Depending on where the validation messages are displayed, you can also control the location of the message in the form and the size of the frame border.

## Specify how validation messages are displayed {#specify-how-validation-messages-are-displayed}

1. In administration console, click Services &gt; forms.
1. Under Validation Output, in the Reporting list, select one of the following options:

   **Message box:** To display validation messages in a separate dialog box.

   **Frame:** To display validation messages within a frame of the same window.

   **No Frame:** To display validation messages in the same window. This value is the default.

   **Via API (with data):** To return the validation messages through the API (with data). The validation messages are not displayed on the screen.

   **Via API (with form):** To return the validation messages through the API (with the form). The validation messages are not displayed on the screen.

   **None:** To not display validation messages.

1. Click Save.

## Specify the location of validation messages relative to the form returned in the web browser {#specify-the-location-of-validation-messages-relative-to-the-form-returned-in-the-web-browser}

When Reporting is set to Frame or No Frame, you can specify the location of validation messages.

1. Under Validation Output, in the Position list, select one of the following options:

   **Left:** To display validation messages on the left side of the web browser.

   **Right:** To display validation messages on the right side of the web browser.

   **Top**: To display validation messages at the top of the web browser. This value is the default.

   **Bottom**: To display validation messages at the bottom of the web browser.

1. Click Save.

## Specify the frame border size {#specify-the-frame-border-size}

When Reporting is set to Frame, you can specify the frame border size.

1. Under Validation Output, in the Border Size box, type the size of the frame border, in pixels.

   The border size must be equal to or greater than 0. The default value is 1.

1. Click Save.
