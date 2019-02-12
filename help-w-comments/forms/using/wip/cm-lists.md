---
title: [DO NOT PUBLISH] Lists
seo-title: Lists
description: Document Fragments, such as Text, lists, conditions, and layout fragments, in Correspondence Management let you form the static, dynamic, and repeatable components of customer correspondence.
seo-description: Document Fragments, such as Text, lists, conditions, and layout fragments, in Correspondence Management let you form the static, dynamic, and repeatable components of customer correspondence.
page-status-flag: never-activated
uuid: 6d3fef03-9a90-4439-9777-708d80719910
products: SG_EXPERIENCEMANAGER/6.4/FORMS
discoiquuid: b5c8f815-7ddd-4c59-81c8-c8ac6b8c9bc3
index: y
internal: n
snippet: y
---

# [DO NOT PUBLISH] Lists{#do-not-publish-lists}

## List {#list}

A list is a group of document fragments, including text, (other) lists, conditions, and images. The order of the list elements can be fixed or editable. While creating a letter, you can use some or all of the list elements to replicate a reusable pattern of elements. Lists basically behave as targets that can be nested within other targets.

### Implementing lists {#implementing-lists}

Implementing lists consists of two steps:

1. Defining core properties such as name, description, data dictionary.
1. Section of content that is part of the list, and then setting properties such as lock order and library access for the list.

### Create a list {#create-a-list}

A list is a group of related content that can be used in a letter template as a single unit. Any kind of content can be added to a list. Lists can also be nested. List modules can be specified as:

* **ORDERED**: The order cannot be changed in the Create Correspondence runtime.
* **Library Access**: Users can add modules to the list. This flag specifies whether library access is enabled. If enabled (open) the user can add modules to the list while previewing the letter.
* When creating a list, you can specify a type, such as:
* **Plain**: No additional style formatting is applied to the list.
* **Bulleted**: A list formatted with a simple bullet.
* **Numbered**: A numeric list with the choice of Standard (1,2,...), Upper Roman (I, II, ...), and Lower Roman (i, ii,...) numerals.
* **Lettered**: An alphabetical list with the choice of lowercase (a,b,...) and uppercase (A,B,...) letters.
* **Custom**: You can create any Numbered/Lettered type and prefix and suffix values of your choice.

1. Select **Forms** &gt; **Document Fragments**.  

1. Select **Create **&gt; **List**.  

1. Specify the following information for the list:

    * **Title (Optional): Enter** the title for the list. Title needs not be unique and can have special characters and non-english characters. Lists are referred by their titles (when available) such as in thumbnails and asset properties.
    * **Name: **The unique name for the list. No two assets (text, condition, or list) in any state can exist with the same name. In the Name field, you can enter only English language characters, numbers, and hyphens. The Name field is automatically populated with the value in the Title field. The special characters, spaces, numbers, and non-English characters entered in the Title field are replaced with hyphens in the Name field. Although the value in the Title field is automatically copied to the Name, you can edit the value.
    * **Description (Optional)**: Type a description of the asset.
    * **Data Dictionary (Optional)**: Optionally, select the data dictionary to which to connect. Only assets that use the same data dictionary as the list, or assets that have no data dictionary assigned, can be added to the list. Assigning a data dictionary to a list makes it easier for the person creating a letter template to find the appropriate list.
    * **Tags (Optional)**: Select the tags to apply. You can also type in a new tag’s name and create it. (The new tag is created when you tap **Save**.)

1. Tap **Next**.
1. Tap **Add Asset**. 
1. To add assets to the list, select them in the Select Assets page and tap **Done**.

   ![Select assets to add to the list](assets/SelectAssets.png)

1. The assets are added to the List Items page.  
   To change the order of the assets within the list, tap and hold the arrows icon ( ![](assets/DragnDrop.png) ) and drag-and-drop. When the user opens a letter template in the Create Correspondence user interface, the content is assembled in the order you defined here.

   ![Reorder and configure assets in a list](assets/ListItems.png)

1. You can select the following options to specify how the list behaves in the CCR user interface:

    * **Library Access**: To enable library access for adding assets, tap Library Access. When Library Access is enabled, the claims adjustor can add more content to the list. Otherwise, the Claims Adjustor is limited to the content you have defined for the list.
    * **Lock** **Order**: To lock the order of the assets in the list so that the Claims Adjustor cannot change the order, tap Lock Order. If you do not select this option, the Claims Adjustor can change the order of the list items.
    
    * **Add Bullets**: Use this option to apply a bullet or numbering style to the module. You can use either a predesigned list style or a custom one. You can also specify the text to be displayed before and after each of the list items. 
    * **Page Break**: Select this option ( ![](assets/Break.png)) to add a page break between the list contents. When this option is not selected ( ![](assets/NoBreak.png)), if the contents of the list are overflowing to the next page, the whole list is shifted to the next page instead of breaking in the page between the list. 
    
    * **Assignment Configuration**: Use this option to specify minimum and maximum number of assets that can be added to the list.

1. Tap **Save**.

### Best practices/tips and tricks {#best-practices-tips-and-tricks}

* Use a consistent naming convention to avoid duplication.
* Use appropriate data dictionary binding
* The following rules apply when using the List Editor to change a list:

    * Update of properties: Allowed
    * **Change of data dictionary: **Allowed until no item that uses the data dictionary is associated with it. You cannot change the data dictionary on update.

>[!MORE_LIKE_THIS]
>
>* [Create correspondence](../../../forms/using/create-correspondence.md)
>* [Manage agent signature images](../../../forms/using/manage-agent-signature-images.md)
>* [Layout design details](../../../forms/using/layout-design-details.md)
>* [Expression builder](../../../forms/using/expression-builder.md)
>* [Correspondence Management configuration properties](../../../forms/using/cm-configuration-properties.md)
>* [Post processing of letters](../../../forms/using/submit-letter-topostprocess.md)
>* [Add custom action to Asset Listing view](../../../forms/using/add-custom-action-asset-listing-view.md)
>* [Add custom properties to Correspondence Management assets](../../../forms/using/add-custom-properties-cm-assets.md)
>* [Add custom action/button in Create Correspondence UI](../../../forms/using/add-action-button-in-create-correspondence-ui.md)
>* [Customize Create Correspondence UI logo](../../../forms/using/customize-create-correspondence-ui.md)