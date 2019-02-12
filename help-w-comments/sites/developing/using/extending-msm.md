---
title: Extending the Multi Site Manager
seo-title: Extending the Multi Site Manager
description: This page helps you extend the functionalities of the Multi Site Manager
seo-description: This page helps you extend the functionalities of the Multi Site Manager
uuid: 51dbf16f-b14c-4a76-a885-6c115bf7c59d
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: extending-aem
content-type: reference
discoiquuid: 938a2c93-7441-4972-b2bf-7310e47d6ac1
index: y
internal: n
snippet: y
---

# Extending the Multi Site Manager{#extending-the-multi-site-manager}

This page helps you extend the functionalities of the Multi Site Manager:

* Learn about the main members of the MSM Java API. 
* Create a new synchronization action that can be used in a rollout configuration.
* Remove the "Chapters" step in the Create Site wizard.
* Modify the default language and country codes.

>[!NOTE]
>
>This page should be read in conjunction with [Reusing Content: Multi Site Manager](../../../sites/administering/using/msm.md).

>[!CAUTION]
>
>The Multi Site Manager and its API are used when authoring a website, so are only intended for use on an author environment.

### Overview of the Java API {#overview-of-the-java-api}

Multi Site Management consists of the following packages:

* [com.day.cq.wcm.msm.api](/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/api/package-frame)
* [com.day.cq.wcm.msm.commons](/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/commons/package-frame)

The main MSM API objects interact as follows (see also [Terms Used](../../../sites/administering/using/msm.md#termsused)):

![](assets/chlimage_1-45.png)

* **`Blueprint`** 
  A `Blueprint` (as in [blueprint configuration](../../../sites/administering/using/msm.md#sourceblueprintsandblueprintconfigurations)) specifies the pages from which a live copy can inherit content. 

  ![](assets/chlimage_1-46.png)

    * The use of a blueprint configuration ( `Blueprint`) is optional, but:

        * Allows the author to use the **Rollout** option on the source (to (explicitly) push modifications to live copies that inherit from this source). 
        * Allows the author to use **Create Site**; this allows the user to easily select languages and configure the structure of the live copy.
        * Defines the default rollout configuration for any resultant live copies.

* 
  **`LiveRelationship`** 

  <!--
  Comment Type: remark
  Last Modified By: unknown unknown (ims-author-57F1056A4CD116590A746C15@AdobeID)
  Last Modified Date: 2017-12-04T12:01:53.977-0500
  <p>check - has changed from Geometrixx to We.Retail</p>
  -->

  The `LiveRelationship` specifies the connection (relationship) between a resource in the live copy branch and its equivalent source/blueprint resource.

    * The relationships are used when realizing inheritance and rollout.
    * `LiveRelationship` objects provide access (references) to the rollout configurations ( `RolloutConfig`), `LiveCopy`, and `LiveStatus` objects related to the relationship. 
    
    * For example, a live copy is created in `/content/copy/us` from the source/blueprint at `/content/we-retail/language-masters`. The resources `/content/we.retail/language-masters/en/jcr:content` and `/content/copy/us/en/jcr:content` form a relationship.

* 
  **`LiveCopy`** `LiveCopy` holds the configuration details for the relationships ( `LiveRelationship`) between the live copy resources and their source/blueprint resources.

    * Use the `LiveCopy` class to access to the path of the page, the path of the source/blueprint page, the rollout configurations and whether child pages are also included in the `LiveCopy`.  
    
    * A `LiveCopy` node is created each time **Create Site** or **Create Live Copy** is used.

* `LiveStatus` objects provide access to the runtime status of a `LiveRelationship`. Use to query the synchronization status of a live copy.
* A `LiveAction` is an action that is executed on each resource that is involved in the rollout.

    * LiveActions are only generated by RolloutConfigs.

* Creates `LiveAction` objects given a `LiveAction` configuration. Configurations are stored as resources in the repository.
* 
  **`RolloutConfig`** The `RolloutConfig` holds a list of `LiveActions`, to be used when triggered. The `LiveCopy` inherits the `RolloutConfig` and the result is present in the `LiveRelationship`.

    * Setting up a live copy for the very first time also uses a RolloutConfig (which triggers the LiveActions).

### Creating a New Synchronization Action {#creating-a-new-synchronization-action}

Create custom synchronization actions to use with your rollout configurations. Create a synchronization action when the [installed actions](../../../sites/administering/using/msm-sync.md#installedsynchronizationactions) do not meet your specific application requirements. To do so, create two classes:

* An implementation of the [ `com.day.cq.wcm.msm.api.LiveAction`](/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/api/LiveAction) interface that performs the action. 
* An OSGI component that implements the [ `com.day.cq.wcm.msm.api.LiveActionFactory`](/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/api/LiveActionFactory) interface and creates instances of your `LiveAction` class.

The `LiveActionFactory` creates instances of the `LiveAction` class for a given configuration:

* `LiveAction` classes include the following methods:

    * `getName`: Returns the name of the action The name is used to refer to the action, for example in rollout configurations.
    * `execute`: Performs the tasks of the action.

* `LiveActionFactory` classes include the following members:

    * `LIVE_ACTION_NAME`: A field that contains the name of the associated `LiveAction`. This name must coincide with the value that is returned by the `getName` method of the `LiveAction` class.
    
    * `createAction`: Creates an instance of the `LiveAction`. The optional `Resource` parameter can be used to provide configuration information.
    
    * `createsAction`: Returns the name of the associated `LiveAction`.

<!--
Comment Type: draft

<p>Create custom synchronization actions to use with your rollout configurations. Create a synchronization action when the <a href="../../../sites/administering/using/msm-sync.md#installedsynchronizationactions">installed actions</a> do not meet your specific application requirements. To do so, create two classes:</p>
<ul>
<li>An implementation of the <a href="/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/api/LiveAction"><span class="code">com.day.cq.wcm.msm.api.LiveAction</span></a> interface that performs the action. </li>
<li>An OSGI component that implements the <a href="/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/api/LiveActionFactory"><span class="code">com.day.cq.wcm.msm.api.LiveActionFactory</span></a> interface and creates instances of your <span class="code">LiveAction</span> class.</li>
</ul>
<p>The <span class="code">LiveAction</span> class is not registered as an OSGi service. Typically, the <span class="code">LiveAction</span> class is used only by one <span class="code">LiveActionFactory</span> so it is convenient to define the <span class="code">LiveAction</span> class as a static nested class of the <span class="code">LiveActionFactory</span> class.</p>
<ul>
<li><span class="code">LiveAction</span> classes include the following methods:
<ul>
<li><span class="code">getName</span>: Returns the name of the action The name is used to refer to the action, for example in rollout configurations.</li>
<li><span class="code">execute</span>: Performs the tasks of the action.</li>
</ul> </li>
<li><span class="code">LiveActionFactory</span> classes include the following members:
<ul>
<li><span class="code">LIVE_ACTION_NAME</span>: A field that contains the name of the associated <span class="code">LiveAction</span>. This name must coincide with the value that is returned by the <span class="code">getName</span> method of the <span class="code">LiveAction</span> class.</li>
<li><span class="code">createAction</span>: Creates an instance of the <span class="code">LiveAction</span>. The optional <span class="code">Resource</span> parameter can be used to provide configuration information.</li>
<li><span class="code">createsAction</span>: Returns the name of the associated <span class="code">LiveAction</span>.</li>
</ul> </li>
</ul>
-->

#### Accessing the LiveAction Configuration Node {#accessing-the-liveaction-configuration-node}

Use the `LiveAction` configuration node in the repository to store information that affects the runtime behaviour of the `LiveAction` instance. The node in the repository that stores the `LiveAction` configuration is available to the `LiveActionFactory` object at runtime. Therefore, you can add properties to the configuration node to and use them in your `LiveActionFactory` implementation as needed.

For example, a `LiveAction` needs to store the name of the blueprint author. A property of the configuration node includes the property name of the blueprint page that stores the information. At runtime, the `LiveAction` retrieves the property name from the configuration, then obtains the property value.

The parameter of the ` [LiveActionFactory](/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/api/LiveActionFactory).createAction` method is a `Resource` object. This `Resource` object represents the `cq:LiveSyncAction` node for this live action in the rollout configuration; see [Creating a Rollout Configuration](../../../sites/administering/using/msm-sync.md#creatingarolloutconfiguration). As usual when using a configuration node, you should adapt it to a `ValueMap` object:

```java
public LiveAction createAction(Resource resource) throws WCMException {
        ValueMap config;
        if (resource == null || resource.adaptTo(ValueMap.class) == null) {
            config = new ValueMapDecorator(Collections.<String, Object>emptyMap());
        } else {
            config = resource.adaptTo(ValueMap.class);
        }
        return new MyLiveAction(config, this);
}
```

#### Accessing Target Nodes, Source Nodes, and the LiveRelationship {#accessing-target-nodes-source-nodes-and-the-liverelationship}

The following objects are provided as parameters of the `execute` method of the `LiveAction` object:

* A [ `Resource`](/sites/developing/using/reference-materials/javadoc/org/apache/sling/api/resource/Resource) object that represents the source of the Live Copy.
* A `Resource` object that represents the target of the Live Copy.
* The [ `LiveRelationship`](/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/msm/api/LiveRelationship) object for the live copy.
* The `autoSave` value indicates whether your `LiveAction` should save changes that are made to the repository.

* The reset value indicates the rollout reset mode.

From these objects you can obtain all of the information about the `LiveCopy`. You can also use the `Resource` objects to obtain `ResourceResolver`, `Session`, and `Node` objects. These objects are useful for manipulating repository content:

In the first line of the following code, source is the `Resource` object of the source page:

```java
ResourceResolver resolver = source.getResourceResolver();
Session session = resolver.adaptTo(javax.jcr.Session.class);
Node sourcenode = source.adaptTo(javax.jcr.Node.class);
```

>[!NOTE]
>
>The `Resource` arguments may be `null` or `Resources` objects that do not adapt to `Node` objects, such as [ `NonExistingResource`](/sites/developing/using/reference-materials/javadoc/org/apache/sling/api/resource/NonExistingResource) objects.

### Creating a New Rollout Configuration {#creating-a-new-rollout-configuration}

Create a rollout configuration when the installed rollout configurations do not meet your application requirements:

* [Create the rollout configuration](#createtherolloutconfiguration).
* [Add synchronization actions to the rollout configuration](#addsynchronizationactionstotherolloutconfiguration).

The new rollout configuration is then available to you when setting rollout configurations on a blueprint or live copy page.

>[!NOTE]
>
>See also the [best practices for customizing rollouts](../../../sites/administering/using/msm-best-practices.md#customizingrollouts).

#### Create the Rollout Configuration {#create-the-rollout-configuration}

1. Open the **Tools** console in the classic UI; for example, [http://localhost:4502/miscadmin#/etc](http://localhost:4502/miscadmin#/etc)

   >[!NOTE]
   >
   >In the standard, touch-enabled UI you can navigate to the classic UI Tools console using the rail entries **Tools**, **Operations** and then **Configuration**.

1. In the folder tree, select the **Tools**, **MSM**, **Rollout Configurations** folder.
1. Click **New**, then **New Page** to define the Rollout Configuration properties:

    * **Title**: The title of the Rollout Configuration, such as My Rollout Configuration
    * **Name**: The name of the node that stores the property values, such as myrolloutconfig
    * Select **RolloutConfig Template**.

1. Click **Create**.
1. Double-click on the rollout configuration that you created to open it for further configuration.
1. Click **Edit**.
1. In the **Rollout Config** dialog, select the ** [Sync Trigger](../../../sites/administering/using/msm-sync.md#rollouttriggers)** to define the action that causes the rollout to occur.
1. Click **OK** to save the changes.

#### Add Synchronization Actions to the Rollout Configuration {#add-synchronization-actions-to-the-rollout-configuration}

Rollout configurations are stored below the `/etc/msm/rolloutconfigs` node. Add child nodes of type `cq:LiveSyncAction` to add synchronization actions to the rollout configuration. The order of the synchronization action nodes determines the order in which the actions occur.

1. Open CRXDE Lite; for example [http://localhost:4502/crx/de](http://localhost:4502/crx/de)
1. Select the `jcr:content` node below your rollout configuration node.

   For example, for the rollout configuration with the **Name** property of `myrolloutconfig`, select the node:

1. Click **Create** then **Create Node**. Then configure the following node properties and click **OK**:

    * **Name**: The node name of the synchronization action. The name must be the same as the **Action Name** in the table under [Synchronization Actions](../../../sites/administering/using/msm-sync.md#installedsynchronizationactions), for example `contentCopy` or `workflow`.
    
    * **Type**: `cq:LiveSyncAction`

1. Select the action node just created and add the following property to the node:

    * **Name**: The property name of the action. The name must be the same as the **Property Name** in the table under [Synchronization Actions](../../../sites/administering/using/msm-sync.md#installedsynchronizationactions), for example `enabled`.  
    
    * **Type**: String  
    
    * **Value**: the property value of the action. For valid values, see the **Properties** column in [Synchronization Actions](../../../sites/administering/using/msm-sync.md#installedsynchronizationactions), for example `true`.

1. Add and configure as many synchronization action nodes as you require. Rearrange the action nodes so that their order matches the order in which you want them to occur. The topmost action node occurs first.
1. Click **Save All**.

### Creating and Using a Simple LiveActionFactory Class {#creating-and-using-a-simple-liveactionfactory-class}

Follow the procedures in this section to develop a `LiveActionFactory` and use it in a rollout configuration. The procedures use Maven and Eclipse to develop and deploy the `LiveActionFactory`:

1. [Create the maven project](#createthemavenproject) and import it into Eclipse.
1. [Add dependencies](#adddependenciestothepomfile) to the POM file.
1. [Implement the `LiveActionFactory` inteface](#implementliveactionfactory) and deploy the OSGi bundle.
1. [Create the rollout configuration](#createtheexamplerolloutconfiguration).
1. [Create the live copy](#createthelivecopy).

The Maven project and the source code of the Java class is available in the public Git repository.

CODE ON GITHUB

You can find the code of this page on GitHub

* [Open experiencemanager-java-msmrollout project on GitHub](https://github.com/Adobe-Marketing-Cloud/experiencemanager-java-msmrollout)
* Download the project as [a ZIP file](https://github.com/Adobe-Marketing-Cloud/experiencemanager-java-msmrollout/archive/master.zip)

#### Create the Maven Project {#create-the-maven-project}

The following procedure requires that you have added the adobe-public profile to your Maven settings file.

* For information about the adobe-public profile, see [Obtaining the Content Package Maven Plugin](../../../sites/developing/using/vlt-mavenplugin.md#obtainingthecontentpackagemavenplugin)
* For information about the Maven settings file, see the Maven [Settings Reference](http://maven.apache.org/settings.html).

1. Open a terminal or command-line session and change the directory to point to the location of where to create the project.
1. Enter the following command:

   ```xml
   mvn archetype:generate -DarchetypeGroupId=com.day.jcr.vault -DarchetypeArtifactId=multimodule-content-package-archetype -DarchetypeVersion=1.0.0 -DarchetypeRepository=adobe-public-releases
   ```

1. Specify the following values at interactive prompt:

    * `groupId`: `com.adobe.example.msm`
    
    * `artifactId`: `MyLiveActionFactory`
    
    * `version`: `1.0-SNAPSHOT`
    
    * `package`: `MyPackage`
    
    * `appsFolderName`: `myapp`
    
    * `artifactName`: `MyLiveActionFactory package`
    
    * `packageGroup`: `myPackages`

1. Start Eclipse and [import the Maven project](../../../sites/developing/using/howto-projects-eclipse.md#importthemavenprojectintoeclipse).

#### Add Dependencies to the POM File {#add-dependencies-to-the-pom-file}

Add dependencies so that the Eclipse compiler can reference the classes that are used in the `LiveActionFactory` code.

1. From the Eclipse Project Explorer, open the file:
1. In the editor, click the `pom.xml` tab and locate the `project/dependencyManagement/dependencies` section.
1. Add the following XML inside the `dependencyManagement` element and then save the file.

   ```xml
    <dependency>
     <groupId>com.day.cq.wcm</groupId>
     <artifactId>cq-msm-api</artifactId>
     <version>5.6.2</version>
     <scope>provided</scope>
    </dependency>
    <dependency>
     <groupId>org.apache.sling</groupId>
     <artifactId>org.apache.sling.api</artifactId>
     <version>2.4.3-R1488084</version>
     <scope>provided</scope>
    </dependency>
    <dependency>
     <groupId>com.day.cq.wcm</groupId>
     <artifactId>cq-wcm-api</artifactId>
     <version>5.6.6</version>
     <scope>provided</scope>
    </dependency>
    <dependency>
     <groupId>org.apache.sling</groupId>
     <artifactId>org.apache.sling.commons.json</artifactId>
     <version>2.0.6</version>
     <scope>provided</scope>
    </dependency>
    <dependency>
     <groupId>com.day.cq</groupId>
     <artifactId>cq-commons</artifactId>
     <version>5.6.4</version>
     <scope>provided</scope>
    </dependency>
    <dependency>
     <groupId>org.apache.sling</groupId>
     <artifactId>org.apache.sling.jcr.jcr-wrapper</artifactId>
     <version>2.0.0</version>
     <scope>provided</scope>
    </dependency>
    <dependency>
     <groupId>com.day.cq</groupId>
     <artifactId>cq-commons</artifactId>
     <version>5.6.4</version>
     <scope>provided</scope>
    </dependency>
   ```

1. Open the POM file for the bundle from **Project Explorer** at `MyLiveActionFactory-bundle/pom.xml`.
1. In the editor, click the `pom.xml` tab and locate the project/dependencies section. Add the following XML inside the dependencies element and then save the file:

   ```xml
    <dependency>
     <groupId>com.day.cq.wcm</groupId>
     <artifactId>cq-msm-api</artifactId>
    </dependency>
    <dependency>
     <groupId>org.apache.sling</groupId>
     <artifactId>org.apache.sling.api</artifactId>
    </dependency>
    <dependency>
     <groupId>com.day.cq.wcm</groupId>
     <artifactId>cq-wcm-api</artifactId>
    </dependency>
    <dependency>
     <groupId>org.apache.sling</groupId>
     <artifactId>org.apache.sling.commons.json</artifactId>
    </dependency>
    <dependency>
     <groupId>com.day.cq</groupId>
     <artifactId>cq-commons</artifactId>
    </dependency>
    <dependency>
     <groupId>org.apache.sling</groupId>
     <artifactId>org.apache.sling.jcr.jcr-wrapper</artifactId>
    </dependency>
    <dependency>
     <groupId>com.day.cq</groupId>
     <artifactId>cq-commons</artifactId>
    </dependency>
   ```

#### Implement LiveActionFactory {#implement-liveactionfactory}

The following `LiveActionFactory` class implements a `LiveAction` that logs messages about the source and target pages, and copies the `cq:lastModifiedBy` property from the source node to the target node. The name of the live action is `exampleLiveAction`.

1. In the Eclipse Project Explorer, right-click the `MyLiveActionFactory-bundle/src/main/java/com.adobe.example.msm` package and click **New** > **Class**. For the **Name**, enter `ExampleLiveActionFactory` and then click **Finish**.
1. Open the `ExampleLiveActionFactory.java` file, replace the content with the following code, and save the file.

   ```java
   package com.adobe.example.msm;
   
   import java.util.Collections;
   
   import org.apache.felix.scr.annotations.Component;
   import org.apache.felix.scr.annotations.Property;
   import org.apache.felix.scr.annotations.Service;
   import org.apache.sling.api.resource.Resource;
   import org.apache.sling.api.resource.ResourceResolver;
   import org.apache.sling.api.resource.ValueMap;
   import org.apache.sling.api.wrappers.ValueMapDecorator;
   import org.apache.sling.commons.json.io.JSONWriter;
   import org.apache.sling.commons.json.JSONException;
   
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   import javax.jcr.Node;
   import javax.jcr.RepositoryException;
   import javax.jcr.Session;
   
   import com.day.cq.wcm.msm.api.ActionConfig;
   import com.day.cq.wcm.msm.api.LiveAction;
   import com.day.cq.wcm.msm.api.LiveActionFactory;
   import com.day.cq.wcm.msm.api.LiveRelationship;
   import com.day.cq.wcm.api.WCMException;
   
   @Component(metatype = false)
   @Service
   public class ExampleLiveActionFactory implements LiveActionFactory<LiveAction> {
    @Property(value="exampleLiveAction")
    static final String actionname = LiveActionFactory.LIVE_ACTION_NAME;
   
    public LiveAction createAction(Resource config) {
     ValueMap configs;
     /* Adapt the config resource to a ValueMap */
           if (config == null || config.adaptTo(ValueMap.class) == null) {
               configs = new ValueMapDecorator(Collections.<String, Object>emptyMap());
           } else {
               configs = config.adaptTo(ValueMap.class);
           }
     
     return new ExampleLiveAction(actionname, configs);
    }
    public String createsAction() {
     return actionname;
    }
    /************* LiveAction ****************/
    private static class ExampleLiveAction implements LiveAction {
     private String name;
     private ValueMap configs;
     private static final Logger log = LoggerFactory.getLogger(ExampleLiveAction.class);
   
     public ExampleLiveAction(String nm, ValueMap config){
      name = nm;
      configs = config;
     }
   
     public void execute(Resource source, Resource target,
       LiveRelationship liverel, boolean autoSave, boolean isResetRollout)
         throws WCMException {
      
      String lastMod = null;
      
      log.info(" *** Executing ExampleLiveAction *** ");
      
      /* Determine if the LiveAction is configured to copy the cq:lastModifiedBy property */
      if ((Boolean) configs.get("repLastModBy")){
       
       /* get the source's cq:lastModifiedBy property */
       if (source != null && source.adaptTo(Node.class) !=  null){
        ValueMap sourcevm = source.adaptTo(ValueMap.class);
        lastMod = sourcevm.get(com.day.cq.wcm.api.NameConstants.PN_PAGE_LAST_MOD_BY, String.class); 
       }
       
       /* set the target node's la-lastModifiedBy property */
       Session session = null;
       if (target != null && target.adaptTo(Node.class) !=  null){
        ResourceResolver resolver = target.getResourceResolver();
        session = resolver.adaptTo(javax.jcr.Session.class);
        Node targetNode;
        try{
         targetNode=target.adaptTo(javax.jcr.Node.class);
         targetNode.setProperty("la-lastModifiedBy", lastMod);    
         log.info(" *** Target node lastModifiedBy property updated: {} ***",lastMod);
        }catch(Exception e){
         log.error(e.getMessage());
        } 
       }
       if(autoSave){
        try {
         session.save();
        } catch (Exception e) {
         try {
          session.refresh(true);
         } catch (RepositoryException e1) {
          e1.printStackTrace();
         }
         e.printStackTrace();
        } 
       }   
      }
     }
     public String getName() {
      return name;
     }
   
     /************* Deprecated *************/
     @Deprecated
     public void execute(ResourceResolver arg0, LiveRelationship arg1,
       ActionConfig arg2, boolean arg3) throws WCMException {  
     }
     @Deprecated
     public void execute(ResourceResolver arg0, LiveRelationship arg1,
       ActionConfig arg2, boolean arg3, boolean arg4)
         throws WCMException {  
     }
     @Deprecated
     public String getParameterName() {
      return null;
     }
     @Deprecated
     public String[] getPropertiesNames() {
      return null;
     }
     @Deprecated
     public int getRank() {
      return 0;
     }
     @Deprecated
     public String getTitle() {
      return null;
     }
     @Deprecated
     public void write(JSONWriter arg0) throws JSONException {
     }
    }
   }
   
   ```

1. Using the terminal or command session, change the directory to the `MyLiveActionFactory` directory (the Maven project directory). Then, enter the following command:

   ```shell
   mvn -PautoInstallPackage clean install
   ```

   The AEM `error.log` file should indicate that the bundle is started.

   For example, [http://localhost:4502/system/console/status-slinglogs](http://localhost:4502/system/console/status-slinglogs).

   ```xml
   13.08.2013 14:34:55.450 *INFO* [OsgiInstallerImpl] com.adobe.example.msm.MyLiveActionFactory-bundle BundleEvent RESOLVED
   13.08.2013 14:34:55.451 *INFO* [OsgiInstallerImpl] com.adobe.example.msm.MyLiveActionFactory-bundle BundleEvent STARTING
   13.08.2013 14:34:55.451 *INFO* [OsgiInstallerImpl] com.adobe.example.msm.MyLiveActionFactory-bundle BundleEvent STARTED
   13.08.2013 14:34:55.453 *INFO* [OsgiInstallerImpl] com.adobe.example.msm.MyLiveActionFactory-bundle Service [com.adobe.example.msm.ExampleLiveActionFactory,2188] ServiceEvent REGISTERED
   13.08.2013 14:34:55.454 *INFO* [OsgiInstallerImpl] org.apache.sling.audit.osgi.installer Started bundle com.adobe.example.msm.MyLiveActionFactory-bundle [316]
   
   ```

#### Create the Example Rollout Configuration {#create-the-example-rollout-configuration}

Create the MSM rollout configuration that uses the `LiveActionFactory` that you created:

1. Create and configuration a [Rollout Configuration with the standard procedure](../../../sites/administering/using/msm-sync.md#creatingarolloutconfiguration) - and using the properties:

    1. Create:

        1. **Title**: Example Rollout Configuration
        1. **Name**: examplerolloutconfig
        1. Using the **RolloutConfig Template**.

    1. Edit:

        1. **Sync Trigger**: On Activation

#### Add the Live Action to the Example Rollout Configuration {#add-the-live-action-to-the-example-rollout-configuration}

Configure the rollout configuration that you created in the previous procedure so that it uses the `ExampleLiveActionFactory` class.

1. Open CRXDE Lite; for example, [http://localhost:4502/crx/de](http://localhost:4502/crx/de).
1. Create the following node under `/etc/msm/rolloutconfigs/examplerolloutconfig/jcr:content`:

    * **Name**: `exampleLiveAction`  
    
    * **Type**: `cq:LiveSyncAction`

   ![](assets/chlimage_1-47.png)

1. Click **Save All**.
1. Select the `exampleLiveAction` node and add the following property:

    * **Name**: `repLastModBy`
    
    * **Type**: `Boolean`
    
    * **Value**: `true`

   This property indicates to the `ExampleLiveAction` class that the `cq:LastModifiedBy` property should be replicated from the source to the target node.

1. Click **Save All**.

#### Create the Live Copy {#create-the-live-copy}

<!--
Comment Type: remark
Last Modified By: unknown unknown (ims-author-57F1056A4CD116590A746C15@AdobeID)
Last Modified Date: 2017-12-04T12:01:58.557-0500
<p>check - has changed from Geometrixx to We.Retail</p>
-->

[Create a live copy](../../../sites/administering/using/msm-livecopy.md#creatingalivecopyofapage) of the English/Products branch of the We.Retail Reference Site using your rollout configuration:

* **Source**: `/content/we-retail/language-masters/en/products`

* **Rollout Configuration**: Example Rollout Configuration

Activate the **Products** (english) page of the source branch and observe the log messages that the `LiveAction` class generates:

```xml
16.08.2013 10:53:33.055 *INFO* [Thread-444535] com.adobe.example.msm.ExampleLiveActionFactory$ExampleLiveAction  *** ExampleLiveAction has been executed.*** 
16.08.2013 10:53:33.055 *INFO* [Thread-444535] com.adobe.example.msm.ExampleLiveActionFactory$ExampleLiveAction  *** Target node lastModifiedBy property updated: admin ***
```

### Removing the Chapters Step in the Create Site Wizard {#removing-the-chapters-step-in-the-create-site-wizard}

<!--
Comment Type: remark
Last Modified By: unknown unknown (ims-author-57F1056A4CD116590A746C15@AdobeID)
Last Modified Date: 2017-12-04T12:01:58.667-0500
<p>check - has changed from Geometrixx to We.Retail</p>
-->

In some cases, the **Chapters** selection is not required in the create site wizard (only the **Languages** selection is required). To remove this step in the default We.Retail English blueprint:

1. In CRX Explorer, remove the node:  
   `/etc/blueprints/weretail-english/jcr:content/dialog/items/tabs/items/tab_chap`.

1. Navigate to `/libs/wcm/msm/templates/blueprint/defaults/livecopy_tab/items` and create a new node:

    1. **Name** = `chapters`; **Type** = `cq:Widget`.

1. Add following properties to the new node:

    1. **Name** = `name`; **Type** = `String`; **Value** = `msm:chapterPages`
    
    1. **Name** = `value`; **Type** = `String`; **Value** = `all`
    
    1. **Name** = `xtype`; **Type** = `String`; **Value** = `hidden`

### Changing language names and default countries {#changing-language-names-and-default-countries}

AEM uses a default set of language and country codes.

* The default language code is the lower-case, two-letter code as defined by ISO-639-1.
* The default country code is the lower-case or upper-case, two-letter code as defined by ISO 3166.

MSM uses a stored list of language and country codes to determine the name of the country that is associated with the name of the language version of your page. You can change the following aspects of the list if required:

* Language titles
* Country names
* Default countries for languges (for codes such as `en`, `de`, amongst others)

The language list is stored below the `/libs/wcm/core/resources/languages` node. Each child node represents a language or a language-country:

* The name of the node is the languge code (such as `en` or `de`), or the language_country code (such as `en_us` or `de_ch`).

* The `language` property of the node stores the full name of the language for the code.
* The `country` property of the node stores the full name of the country for the code.
* When the node name consists only of a language code (such as `en`), the country property is `*`, and an additional `defaultCountry` property stores the code of the language-country to indicate the country to use.

![](assets/chlimage_1-48.png)

To modify the languages:

1. Open CRXDE Lite in your web browser; for example, [http://localhost:4502/crx/de](http://localhost:4502/crx/de)
1. Select the `/apps` folder and click **Create**, then **Create Folder.**

   Name the new folder `wcm`.

1. Repeat the previous step to create the `/apps/wcm/core` folder tree. Create a node of type `sling:Folder`** **in core called `resources`.

   ![](assets/chlimage_1-49.png)

1. Right-click the `/libs/wcm/core/resources/languages` node and click **Copy**.
1. Right-click the `/apps/wcm/core/resources` folder and click **Paste**. Modify the child nodes as required.
1. Click **Save All**.
1. Click **Tools**, **Operations** then **Web Console**. From this console click **OSGi**, then **Configuration**.
1. Locate and click **Day CQ WCM Language Manager**, and change the value of **Language List** to `/apps/wcm/core/resources/languages`, then click **Save**.

   ![](assets/chlimage_1-50.png)

### Configuring MSM Locks on Page Properties (Touch-Enabled UI) {#configuring-msm-locks-on-page-properties-touch-enabled-ui}

When creating a custom page property you may need to consider whether the new property should be eligible for roll out to any live copies.

For example, if two new page properties are being added:

* Contact Email:

    * This property is not required to be rolled out, as it will be different in each country (or brand, etc).

* Key Visual Style:

    * The project requirement is that this property is to be rolled out as it is (usually) common to all countries (or brands, etc).

Then you need to ensure that:

* Contact Email:

    * Is excluded from the rolled out properties; see [Excluding Properties and Node Types from Synchronization](../../../sites/administering/using/msm-sync.md#main-pars-title-490730743).

* Key Visual Style:

    * Make sure you are not allowed to edit this property in the touch-enabled UI unless inheritance is cancelled, also that you can then reinstate inheritance; this is controlled by clicking the chain/broken-chain links that toggle to indicate the status of the connection.

Whether a page property is subject to roll out and therefore, subject to cancelling/reinstating inheritance when editing, is controlled by the dialog property:

* `cq-msm-lockable`

    * is applicable to items in a touch-enabled UI dialog  
    * will create the chain-link symbol in the dialog  
    * only allows editing if inheritance is cancelled (the chain-link is broken)
    * **Type**: `String`
    
    * **Value**: holds the name of the property under consideration (and is comparable to the value of the property `name`; for example, see  
      `/libs/foundation/components/page/cq:dialog/content/items/tabs/items/basic/items/column/items/title/items/title`

When `cq-msm-lockable` has been defined, breaking/closing the chain will interact with MSM in the following way:

* if the value of `cq-msm-lockable` is:

    * **Relative** (e.g. `myProperty` or `./myProperty`)

        * it will add and remove the property from `cq:propertyInheritanceCancelled`.
        * MSM does not operate with deep properties (e.g. `./image/fileReference`), even though the dialog’s logic does. If the chain is opened a rollout of the page will overwrite `./image/fileReference`, as the rollout of the `image` node will not "walk" up to the parent node to check `cq:propertyInheritanceCancelled`.

    * **Absolute** (e.g. `/image`)

        * breaking the chain will cancel inheritance by adding the `cq:LiveSyncCancelled` mixin to `./image` and setting `cq:isCancelledForChildren` to `true`.
        
        * closing the chain will revert inheritance.

>[!NOTE]
>
>When you re-enable inheritance, the live copy page property is not automatically synchronized with the source property. You can manually request a synchronization if this is required.
