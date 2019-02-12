---
title: Internationalizing UI Strings 
seo-title: Internationalizing UI Strings 
description: Java and Javascript APIs enable you to internationalize strings
seo-description: Java and Javascript APIs enable you to internationalize strings
uuid: c7e8faaa-52dc-4c10-ad07-5b860f391288
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.4/SITES
content-type: reference
topic-tags: components
discoiquuid: 07eb085c-41d3-44af-8617-946878d555da
index: y
internal: n
snippet: y
---

# Internationalizing UI Strings {#internationalizing-ui-strings}

<!--
Comment Type: remark
Last Modified By: unknown unknown (sbroders@adobe.com)
Last Modified Date: 2017-11-30T05:24:59.963-0500
<p>Content requires massaging when the xgettext-maven-plugin tool is released to the public. When the tool is used, the code actually creates the strings in addition to looking them up in the dictionary.</p>
-->

Java and Javascript APIs enable you to internationalize strings in the following types of resources:

* Java source files.
* JSP scripts.
* Javascript in client-side libraries or in page source.
* JCR node property values used in dialogs and component configuration properties.

For an overview of the internationalization and localization process, see [Internationalizing Components](../../../sites/developing/using/i18n.md).  

### Internationalizing Strings in Java and JSP Code {#internationalizing-strings-in-java-and-jsp-code}

The `com.day.cq.i18n` Java package enables you to display localized strings in your UI. The `I18n` class provides the `get` method that retrieves localized strings from the AEM dictionary. The only required parameter of the `get` method is the string literal in the English language. English is the default langauge for the UI. The following example localizes the word `Search`:

`i18n.get("Search");`

Identifying the string in the English language differs from typical internationalization frameworks where an ID identifies a string and is used to reference the string at runtime. Using the English string literal provides the following benefits:

* Code is easy to understand.
* The string in the default language is always available.

#### Determining the User's Language {#determining-the-user-s-language}

There are two ways to determine the language that the user prefers:

* For authenticated users, determine the language from the preferences in the user account.
* The locale of the requested page.

The language property of the user account is the preferred method because it is more reliable. However, the user must be logged in to use this method.  

#### Creating the I18n Java object {#creating-the-i-n-java-object}

The I18n class provides two constructors. How you determine the user's preferred language determines the constructor to use.

To present the string in the language that is specified in the user account, use the following constructor (after importing `com.day.cq.i18n.I18n)`:

```java
I18n i18n = new I18n(slingRequest);
```

The constructor uses the `SlingHTTPRequest` to retrieve the user's language setting.

To use the page locale to determine the language, you first need to obtain the ResourceBundle for the language of the requested page:

```java
Locale pageLang = currentPage.getLanguage(false);
ResourceBundle resourceBundle = slingRequest.getResourceBundle(pageLang);
I18n i18n = new I18n(resourceBundle); 
```

#### Internationalizing a String {#internationalizing-a-string}

Use the `get` method of the `I18n` object to internationalize a string. The only required parameter of the `get` method is the string to internationalize. The string corresponds with a string in a Translator dictionary. The get method looks up the string in the dictionary and returns the translation for the current language.

The first argument of the `get` method must comply with the following rules:

* The value must be a string literal. A variable of type `String` is not acceptable.
* The string literal must be expresse on a single line.
* The string is case-sensitive.

```xml
i18n.get("Enter a search keyword");
```

#### Using Translation Hints {#using-translation-hints}

<!--
Comment Type: remark
Last Modified By: unknown unknown (sbroders@adobe.com)
Last Modified Date: 2017-11-30T05:25:00.133-0500
<p>When the xgettext maven plugin is released, this section requires an explanation of why you'd use translation hints, because the get command will essentially create the hint. The similar text in the javascript and JCR sections must also be massaged.</p>
-->

Specify the [translation hint](../../../sites/developing/using/i18n-translator.md#main-pars-title-5) of the internationalized string to distinguish between duplicate strings in the dictionary. Use the second, optional parameter of the `get` method to provide the translation hint. The translation hint must exactly match the Comment property of the item in the dictionary.

For example, the dicationary contains the string `Request` twice: once as a verb and once as a noun. The following code includes the translation hint as an argument in the `get` method:

```java
i18n.get("Request","A noun, as in a request for a web page");
```

#### Including Variables in Localized Sentences {#including-variables-in-localized-sentences}

Include variables in the localized string to build contextual meaning into a sentence. For example, after logging into a web application, the home page displays the message "Welcome back Administrator. You have 2 messages in your inbox." The page context determines the user name and the number of messages.

[In the dictionary](../../../sites/developing/using/i18n-translator.md#main-pars-title-5), the variables are represented in strings as bracketed indexes. Specify the values of the variables as arguments of the `get` method. The arguments are placed following the translation hint, and the indexes correspond with the order of the arguments:

```xml
i18n.get("Welcome back {0}. You have {1} messages.", "user name, number of messages", user.getDisplayName(), numItems); 
```

The internationalized string and the translation hint must exactly match the string and comment in the dictionary. You can omit the localization hint by providing a `null` value as the second argument.

#### Using the Static Get Method {#using-the-static-get-method}

The `I18N` class defines a static `get` method that is useful when you need to localize a small number of strings. In addition to the parameters of an object's `get` method, the static method requires the `SlingHttpRequest` object or the `ResourceBundle` that you are using, according to how you are determining the user's preferred language:

* Use the user's language preference: Provide the SlingHttpRequest as the first parameter. 
* Use the page language: Provide the ResourceBundle as the first parameter.

### Internationalizing Strings in Javascript Code {#internationalizing-strings-in-javascript-code}

The Javascript API enables you to localize strings on the client. As with [Java and JSP](../../../sites/developing/using/i18n.md#main-pars-title-0) code, the Javascript API enables you to identify strings to localize, priovide localization hints, and include variables in the localized strings.

The `granite.utils` [client library folder](../../../sites/developing/using/clientlibs.md) provides the Javascript API. To use the API, include this client library folder on your page. Localization functions use the `Granite.I18n` namespace.

Before you present localized strings, you need to set the locale using the `Granite.I18n.setLocale` function. The function requires the language code of the locale as an argument:

```
Granite.I18n.setLocale("fr");
```

To present a localized string, use the `Granite.I18n.get` function:

```
Granite.I18n.get("string to localize");
```

The following example internationalizes the string "Welcome back":

```
Granite.I18n.setLocale("fr");
Granite.I18n.get("string to localize", [variables], "localization hint");
```

The function parameters are different than the Java I18n.get method:

* The first parameter is the string literal to localize.
* The second parameter is an array of values to inject into the string literal.
* The third parameter is the localization hint.

The following example uses Javascript to localize the "Welcome back Administrator. You have 2 messages in your inbox." sentence:  

```
Granite.I18n.setLocale("fr");
Granite.I18n.get("Welcome back {0}. You have {1} new messages in your inbox.", [username, numMsg], "user name, number of messages");
```

### Internationalizing Strings from JCR Nodes {#internationalizing-strings-from-jcr-nodes}

UI strings are often based on JCR node properties. For example, the `jcr:title` property of a page is typically used as the content of the `h1` element in the page code. The `I18n` class provides the `getVar` method for localizing these strings.

The following example JSP script retrieves the `jcr:title` property from the repository and displays the localized string on the page:

```java
<% title = properties.get("jcr:title", String.class);%>
<h1><%=i18n.getVar(title) %></h1>
```

<!--
Comment Type: draft

<note type="note">
<p>You must also configure xgettext-maven-plugin to extract the string to the XLIFF file.</p>
</note>
-->

#### Specifying Translation Hints for JCR Nodes {#specifying-translation-hints-for-jcr-nodes}

Similar to [translation hints in the Java API](../../../sites/developing/using/i18n.md#main-pars-title-12), you can provide translation hints to distinguish duplicate strings in the dictionary. Provide the translation hint as a property of the node that contains the internationalized property. The name of the hint property is comprised of the name of the internationalized property name with the `_commentI18n` suffix:

`${prop}_commentI18n`

For example, a `cq:page` node includes the jcr:title property which is being localized. The hint is provided as the value of the property named jcr:title_commentI18n.

### Testing Internationalization Coverage {#testing-internationalization-coverage}

Test whether you have internationalized all of the strings in your UI. To see which strings are covered, set the user languge to zz_ZZ and open the UI in the web browser. The internationalized strings appear with a stub tranlsation in the folloiwng format:

`USR_*Default-String*_尠`

The following image shows the stub translation for the AEM home page:

![](assets/chlimage_1-1.jpeg)

To set the language for the user, configure the language property of the preferences node for the user account.

The preferences node of a user has a path like this:

`/home/users/<letter>/<hash>/preferences`

![](assets/chlimage_1-2.jpeg)
