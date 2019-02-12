---
title: AEM Tagging Framework
seo-title: AEM Tagging Framework
description: Tag content and leverage the AEM Tagging infrastructure
seo-description: Tag content and leverage the AEM Tagging infrastructure
uuid: 992e6499-bda5-4e40-8aef-d6b5c0f00d7d
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: platform
content-type: reference
discoiquuid: 7d5850f2-ab45-49cc-b7ce-4300f4045985
index: y
internal: n
snippet: y
---

# AEM Tagging Framework{#aem-tagging-framework}

To tag content and leverage the AEM Tagging infrastructure :

* The tag must exist as a node of type ` [cq:Tag](#tagscqtagnodetype)` under the [taxonomy root node](#taxonomyrootnode)

* The tagged content node's NodeType must include the [ `cq:Taggable`](#taggablecontentcqtaggablemixin) mixin
* The [TagID](#tagid) is added to the content node's [ `cq:tags`](#taggedcontentcqtagsproperty) property and resolves to a node of type ` [cq:Tag](#tagscqtagnodetype)`

<!--
Comment Type: annotation
Last Modified By: mgulati
Last Modified Date: 2018-11-29T01:57:05.860-0500
Confirm about "The tag must exist as a node of type cq:Tag..."
-->

## Tags : cq:Tag Node Type  {#tags-cq-tag-node-type}

The declaration of a tag is captured in the repository in a node of type `cq:Tag.`

A tag can be a simple word (e.g. sky) or represent a hierarchical taxonomy (e.g. fruit/apple, meaning both the generic fruit and the more specific apple).

Tags are identified by a unique TagID.

A tag has optional meta information such as a title, localized titles and a description. The title should be displayed in user interfaces instead of the TagID, when present.

The tagging framework also provides the ability to restrict authors and site visitors to use only specific, predefined tags.

### Tag Characteristics {#tag-characteristics}

* node type is `cq:Tag`
* node name is a component of the ` [TagID](#tagid)`
* the ` [TagID](#tagid)` always includes a [namespace](#tagnamespace)

* optional `jcr:title` property (the Title to display in the UI)  

* optional `jcr:description` property  

* when containing child nodes, is referred to as a [container tag](#containertags)
* is stored in the repository below a base path called the [taxonomy root node](#taxonomyrootnode)

### TagID {#tagid}

A TagID identifies a path which resolves to a tag node in the repository.

Typically, the TagID is a shorthand TagID starting with the namespace or it can be an absolute TagID starting from the [taxonomy root node](#taxonomyrootnode).

When content is tagged, if it does not yet exist, the ` [cq:tags](#taggedcontentcqtagsproperty)` property is added to the content node and the TagID is added to the property's String array value.

The TagID consists of a [namespace](#tagnamespace) followed by the local TagID. [Container tags](#containertags) have sub-tags that represent a hierarchical order in the taxonomy. Sub-tags can be used to reference tags same as any local TagID. For example tagging content with "fruit" is allowed, even if it is a container tag with sub-tags, such as "fruit/apple" and "fruit/banana".

### Taxonomy Root Node {#taxonomy-root-node}

The taxonomy root node is the base path for all tags in the repository. The taxonomy root node must *not* be a node of type `  cq   :Tag`.

In AEM, the base path is `/content/  cq   :tags` and the root node is of type `  cq   :Folder`.

### Tag Namespace {#tag-namespace}

Namespaces allow to group things. The most typical use-case is to have a namespace per (web)site (for example public, internal, and portal) or per larger application (e.g. WCM, Assets, Communities) but namespaces can be used for various other needs. Namespaces are used in the user interface to only show the subset of tags (i.e. tags of a certain namespace) that is applicable to the current content.

The tag's namespace is the first level in the taxonomy subtree, which is the node immediately below the [taxonomy root node](#taxonomyrootnode). A namespace is a node of type `cq:Tag` whose parent is not a `cq:Tag`node type.

All tags have a namespace. If no namespace is specified, the tag is assigned to the default namespace, which is TagID `default` (Title is `Standard Tags),`that is `/content/cq:tags/default.`

<!--
Comment Type: annotation
Last Modified By: mgulati
Last Modified Date: 2018-11-28T08:04:42.239-0500
Doubt: confirm about cq:Tag note type from the Engineering.
-->

### Container Tags {#container-tags}

A container tag is a node of type `cq:Tag` containing any number and type of child nodes, which makes it possible to enhance the tag model with custom metadata.

Furthermore, container tags (or super-tags) in a taxonomy serve as the sub-summation of all sub-tags: for example content tagged with fruit/apple is considered to be tagged with fruit as well, i.e. searching for content just tagged with fruit would also find the content tagged with fruit/apple.

<!--
Comment Type: annotation
Last Modified By: mgulati
Last Modified Date: 2018-11-28T08:05:56.185-0500
Doubt: confirm about "node of type cq:Tag.." note type from the Engineering.
-->

### Resolving TagIDs {#resolving-tagids}

If the tag ID contains a colon ":", the colon separates the namespace from the tag or sub-taxonomy, which are then separated with normal slashes "/". If the tag ID does not have a colon, the default namespace is implied.

The standard and only location of tags is below /content/cq:tags.

Tag referencing non-existing paths or paths that do not point to a cq:Tag node are considered invalid and are ignored.  
  
The following table shows some sample TagIDs, their elements, and how the TagID resolves to an absolute path in the repository:

The following table shows some sample TagIDs, their elements, and how the TagID resolves to an absolute path in the repository :  
The following table shows some sample TagIDs, their elements, and how the TagID resolves to an absolute path in the repository :  

<!--
Comment Type: annotation
Last Modified By: mgulati
Last Modified Date: 2018-11-28T08:07:34.264-0500
Doubt: Confirm about TagID, and how is it useful and where from the engineering.
-->

<!--
Comment Type: annotation
Last Modified By: mgulati
Last Modified Date: 2018-11-28T08:08:44.698-0500
what is a cq:Tag node talked about here in 3rd sentence?
-->

<table border="1" cellpadding="2" cellspacing="2" width="100%"> 
 <tbody> 
  <tr> 
   <td><strong>TagID<br /> </strong></td> 
   <td><strong>Namespace</strong></td> 
   <td><strong>Local ID</strong></td> 
   <td><strong>Container tag(s)</strong></td> 
   <td><strong>Leaf tag</strong></td> 
   <td><strong>Repository<br /> Absolute tag path</strong></td> 
  </tr> 
  <tr> 
   <td>dam:fruit/apple/braeburn</td> 
   <td>dam</td> 
   <td>fruit/apple/braeburn</td> 
   <td>fruit, apple</td> 
   <td>braeburn</td> 
   <td>/content/cq:tags/dam/fruit/apple/braeburn</td> 
  </tr> 
  <tr> 
   <td>color/red</td> 
   <td>default</td> 
   <td>color/red</td> 
   <td>color</td> 
   <td>red</td> 
   <td>/content/cq:tags/default/color/red</td> 
  </tr> 
  <tr> 
   <td>sky</td> 
   <td>default</td> 
   <td>sky</td> 
   <td>(none)</td> 
   <td>sky</td> 
   <td>/content/cq:tags/default/sky</td> 
  </tr> 
  <tr> 
   <td>dam:</td> 
   <td>dam</td> 
   <td>(none)</td> 
   <td>(none)</td> 
   <td>(none, the namespace)</td> 
   <td>/content/cq:tags/dam</td> 
  </tr> 
  <tr> 
   <td>/content/cq:tags/category/car</td> 
   <td>category</td> 
   <td>car</td> 
   <td>car</td> 
   <td>car</td> 
   <td>/content/cq:tags/category/car</td> 
  </tr> 
 </tbody> 
</table>

### Localization of Tag Title {#localization-of-tag-title}

When the tag includes the optional title string ( `jcr:title`) it is possible to localize the title for display by adding the property `jcr:title.<locale>`.

For more details see

* [Tags in Different Languages](../../../sites/developing/using/building.md#tagsindifferentlanguages) - which describes use of the APIs
* [Managing Tags in Different Languages](../../../sites/administering/using/tags.md#managingtagsindifferentlanguages) - which describes use of the Tagging console

### Access Control {#access-control}

Tags exist as nodes in the repository under the [taxonomy root node](#taxonomyrootnode). Allowing or denying authors and site visitors to create tags in a given namespace can be achieved by setting appropiate ACLs in the repository.

Also, denying read permissions for certains tags or namespaces will control the ability to apply tags to specific content.

A typical practice includes:

* Allowing the `tag-administrators` group/role write access to all namespaces (add/modify under `/content/cq:tags`). This group comes with AEM out-of-the-box.  

* Allowing users/authors read access to all the namespaces that should be readable to them (mostly all).
* Allowing users/authors write access to those namespaces where tags should be freely definable by users/authors (add_node under `/content/cq:tags/some_namespace`)

## Taggable Content : cq:Taggable Mixin {#taggable-content-cq-taggable-mixin}

In order for application developers to attach tagging to a content type, the node's registration ([CND](https://jackrabbit.apache.org/node-type-notation.html)) must include the `cq:Taggable` mixin or the `cq:OwnerTaggable` mixin.

The `cq:OwnerTaggable` mixin, which inherits from `cq:Taggable`, is intended to indicate that the content can be classified by the owner/author. In AEM, it is only an attribute of the `cq:PageContent` node. The `cq:OwnerTaggable` mixin is not required by the tagging framework.

>[!NOTE]
>
>It is recommended to only enable tags on the top-level node of an aggregated content item (or on its jcr:content node). Examples include:
>
>* pages ( `cq:Page`) where the `jcr:content`node is of type `cq:PageContent` which includes the `cq:Taggable` mixin.
>
>* assets ( `cq:Asset`) where the `jcr:content/metadata` node always has the `cq:Taggable` mixin.  
>

### Node Type Notation (CND) {#node-type-notation-cnd}

Node Type definitions exist in the repository as CND files. The CND notation is defined as part of the JCR documentation [here](https://jackrabbit.apache.org/node-type-notation.html).

The essential definitions for the Node Types included in AEM are as follows:

```xml
[cq:Tag] > mix:title, nt:base
    orderable
    - * (undefined) multiple
    - * (undefined)
    + * (nt:base) = cq:Tag version

[cq:Taggable]
    mixin
    - cq:tags (string) multiple

[cq:OwnerTaggable] > cq:Taggable
    mixin
```

## Tagged Content: cq:tags Property {#tagged-content-cq-tags-property}

The `cq:tags` property is a String array used to store one or more TagIDs when they are applied to content by authors or site visitors. The property only has meaning when added to a node which is defined with the ` [cq:Taggable](#taggablecontentcqtaggablemixin)` mixin.

>[!NOTE]
>
>To leverage AEM tagging functionality, custom developed applications should not define tag properties other than `cq:tags`.

## Moving and Merging Tags {#moving-and-merging-tags}

The following is a description of the effects in the repository when moving or merging tags using the [Tagging console](../../../sites/administering/using/tags.md):

* When a tag A is moved or merged into tag B under `/content/cq:tags`:

    * tag A is not deleted and gets a `cq:movedTo` property.
    * tag B is created (in case of a move) and gets a `cq:backlinks` property.

* `cq:movedTo` points to tag B.  
  This property means that tag A has been moved or merged into tag B. Moving tag B will update this property accordingly. Tag A is thus hidden and is only kept in the repository to resolve tag IDs in content nodes pointing to tag A. The tag garbage collector removes tags like tag A once no more content nodes point to them.  
  A special value for the `cq:movedTo` property is `nirvana`: it is applied when the tag is deleted but cannot be removed from the repository because there are subtags with a `cq:movedTo` that must be kept.

**Note**: *The "cq:movedTo" property is only added to the moved or merged tag if either of these conditions are met:* 
*1. Tag is used in content (meaning it has a reference) OR* 
*2. Tag has children that have already been moved.*

* `cq:backlinks` keeps the references in the other direction, i.e. it keeps a list of all the tags that have been moved to or merged with tag B. This is mostly required to keep `cq:movedTo`properties up to date when tag B is moved/merged/deleted as well or when tag B is activated, in which case all its backlinks tags must be activated as well.

**Note**: *The "cq:backlinks" property is only added to the moved or merged tag if either of these conditions are met:* 
*1. Tag is used in content (meaning it has a reference) OR* 
*2. Tag has children that have already been moved.*

* Reading a `cq:tags` property of a content node involves the following resolving:

    1. If there is no match under `/content/cq:tags`, no tag is returned.
    1. If the tag has a `cq:movedTo` property set, the referenced tag ID is followed.  
       This step is repeated as long as the followed tag has a `cq:movedTo` property.
    
    1. If the followed tag does not have a `cq:movedTo` property, the tag is read.

* To publish the change when a tag has been moved or merged, the `cq:Tag` node and all its backlinks must be replicated: this is automatically done when the tag is activated in the tag administration console.  

* Later updates to the page's `cq:tags` property automatically clean up the "old" references. This is triggered because resolving a moved tag through the API returns the destination tag, thus providing the destination tag ID.
