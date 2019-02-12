---
title: Image Editor
seo-title: Image Editor
description: The Image Editor is a core piece of AEM and can be leveraged by components to facilitate the manipulation of images by content authors.
seo-description: The Image Editor is a core piece of AEM and can be leveraged by components to facilitate the manipulation of images by content authors.
uuid: 0cec78a9-8896-4cac-9d1f-3386968d1d76
contentOwner: bohnert
products: SG_EXPERIENCEMANAGER/6.4/SITES
content-type: reference
topic-tags: components
discoiquuid: 10fcea28-3c4e-4b6c-a6d7-9739c3197519
index: y
internal: n
snippet: y
---

# Image Editor{#image-editor}

The Image Editor is a core piece of AEM and can be leveraged by components to facilitate the manipulation of images by content authors.

>[!CAUTION]
>
>To use the features of the Image Editor described in this article, [feature pack 24267](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/featurepack/cq-6.4.0-featurepack-24267) must be installed.

## Relative Units for Image Map {#relative-units-for-image-map}

The Image Editor persists image map areas as both absolute and relative units. Relative units are useful when provided as data attributes for dynamically resizing an image map (relative to the image size) on the client-side in a responsive image component.

### imageMap Property {#imagemap-property}

The image map coordinates are persisted to the JCR as an `imageMap` property by the Image Editor. It has the following format.

The property stores map areas as follows:

`[area1][area2][...]`

Area format:

`[SHAPE(COORDINATES)"HREF"|"TARGET"|"ALT"|(RELATIVE_COORDINATES)]`

Example:

`[rect(0,0,10,10)"http://www.adobe.com"|"_self"|"alt"|(0,0,0.8,0.8)][circle(10,10,10)"http://www.adobe.com"|"_self"|"alt"|(0.8,0.8,0.8)]`

## Support for SVG Images {#support-for-svg-images}

Scalable Vector Graphics (SVG) are supported by the Image Editor.

* Drag-and-drop of an SVG asset from DAM and upload of an SVG file upload from a local file system are both supported.

## Enabling Plugins by MIME Type {#enabling-plugins-by-mime-type}

In certain situations authoring actions must be restricted for certain MIME-types, due to lack of support in server-side processing. For example, editing SVG images may not be allowed.

Plugins in the Image Editor can be selectively enabled by MIME type by setting a `supportedMimeTypes` property on the individual plugin's configuration node.

<!--
Comment Type: annotation
Last Modified By: pid90611
Last Modified Date: 2018-07-03T05:03:26.395-0400
Going forward would document the various plugins and their options together, but for now this should be okay.
-->

### Example {#example}

As an example, let's say that the ability to crop should only be allowed for GIF, JPEG, PNG, WEBP and TIFF images.

The `supportedMimeTypes` property must then be set as a string of the allowed MIME types on the configuration node of the plugin on the `cq:editConfig` node of the image component.

`/apps/core/wcm/components/image/v2/image/cq:editConfig`

```xml
 jcr:primaryType="cq:EditConfig">
     <cq:dropTargets jcr:primaryType="nt:unstructured">
         <image ...>
            ...
         </image>
     </cq:dropTargets>
     <cq:inplaceEditing
         jcr:primaryType="cq:InplaceEditingConfig"
         active="{Boolean}true"
         editorType="image">
         <config jcr:primaryType="nt:unstructured">
             <plugins jcr:primaryType="nt:unstructured">
                 <crop
                     jcr:primaryType="nt:unstructured"
                     supportedMimeTypes="[image/gif,image/jpeg,image/png,image/webp,image/tiff]"
                     features="*">
                     <aspectRatios jcr:primaryType="nt:unstructured">
                        ...
                     </aspectRatios>
                 </crop>
                 ...
             </plugins>
             <ui jcr:primaryType="nt:unstructured">
                 ...
             </ui>
         </config>
     </cq:inplaceEditing>
 </jcr:root>
```
