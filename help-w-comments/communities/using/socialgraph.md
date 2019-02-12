---
title: Using Social Graph
seo-title: Using Social Graph
description: Adding a Following component to a page
seo-description: Adding a Following component to a page
uuid: 371bfe3c-a5b9-4f93-a5d9-dd78822bda6c
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: authoring
content-type: reference
discoiquuid: fe232ec3-0382-435e-b970-b36d1577d4ba
index: y
internal: n
snippet: y
---

# Using Social Graph{#using-social-graph}

### Introduction {#introduction}

The ability for a community member to follow [activities](../../communities/using/activities.md) as well as be followed is established through two components : `Follow`and `Following`.

The `Follow`component must be associated with another resource, and this association is already established for community members and features.

The `Following`component simply lists the members that are either following the current member or are being followed by the current member. This social graph of the relationships between members is included in the user profile established for a [community site](../../communities/using/overview.md#communitiessites).

### Adding Following to a Page {#adding-following-to-a-page}

If it is desired to add a `Following`component to a page in author mode, locate the component

* `Communities / Following`

and drag it into place on a page where the social graph should appear.

For necessary information, visit [Communities Components Basics](../../communities/using/basics.md).

When the [required client-side libraries](../../communities/using/essentials-socialgraph.md#essentialsforclientside) are included, this is how the `Following` component will appear :

![](assets/chlimage_1-459.png)

### Configuring Following {#configuring-following}

Currently, it is necessary to set the property to determine whether the component displays the `follows`relationship, or the `following`relationship.

### Additional Information {#additional-information}

More information may be found on the [Social Graph Essentials](../../communities/using/essentials-socialgraph.md) page for developers.