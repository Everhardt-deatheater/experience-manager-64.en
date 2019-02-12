---
title: Using Liking
seo-title: Using Liking
description: Adding and configuring the Liking component
seo-description: Adding and configuring the Liking component
uuid: c03b2499-302b-4f7d-8228-906001b1afbb
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: authoring
content-type: reference
discoiquuid: b0c81d4a-195e-44b6-9a96-e6fbb0e66030
index: y
internal: n
snippet: y
---

# Using Liking{#using-liking}

The `Liking`component is a useful tool that allows users to express an opinion about a particular piece of content, such as an comment within a forum. With the `Liking`component, members select the heart icon to indicate a positive opinion.

### Adding Liking to a Page {#adding-liking-to-a-page}

To add a `Liking` component to a page in author mode, use the component browser to locate

* `Communities / Liking`

and drag it into place on a page, such as a position relative to the feature for users to like.

For necessary information, visit [Communities Components Basics](../../communities/using/basics.md).

When the [required client-side libraries](../../communities/using/essentials-liking.md#essentialsforclientside) are included, this is how the `Liking` component will appear.

![](assets/chlimage_1-99.png)

### Configuring Liking {#configuring-liking}

Select the placed `Liking` component to access and select the `Configure` icon which opens the edit dialog.

![](assets/chlimage_1-100.png)

Under the **Texts & Labels **tab, specify the properties used to record likes.

![](assets/chlimage_1-101.png)

* **Positive Response Label** 
  (*Required*) The property name for a positive response.

* **Negative Response Label** 
  (*Required*) The property name for a negative response.

* **Tally Name** 
  (*Required*) The internal, identifiable property name for this instance of a voting component.

### Site Visitor Experience {#site-visitor-experience}

#### Members {#members}

Members may change their like at any time.

#### Anonymous {#anonymous}

Anonymous liking is not supported. Site visitors must register (become a member) and sign in to participate in liking.

### Additional Information {#additional-information}

More information may be found on the [Liking Essentials](../../communities/using/essentials-liking.md) page for developers.