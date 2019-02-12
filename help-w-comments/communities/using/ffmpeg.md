---
title: FFmpeg for Communities
seo-title: FFmpeg for Communities
description: How to install and configure FFmpeg for Communities
seo-description: How to install and configure FFmpeg for Communities
uuid: 63995c89-4424-4c07-97d1-be854f6bbbae
contentOwner: Janice Kendall
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: administering
content-type: reference
discoiquuid: 20ea1a19-fdbb-4500-ba9a-e64b8b4805d5
index: y
internal: n
snippet: y
---

# FFmpeg for Communities{#ffmpeg-for-communities}

## Overview {#overview}

FFmpeg is a solution for converting and streaming audio and video and, when installed, is used for proper transcoding of [video assets](../../sites/authoring/using/default-components-foundation.md#video) as well as for AEM Communities' enablement feature.

FFmpeg is used in the author environment to obtain metadata for uploaded enablement resources as well as generate a thumbnail to display when listing the enablement resource.

## Installing FFmpeg {#installing-ffmpeg}

FFmpeg should be installed on the server(s) hosting the AEM *author* instance(s).

1. Go to [http://www.ffmpeg.org](http://www.ffmpeg.org/) 
1. Download the latest version of FFmpeg for your specific environment (Macintosh, Windows, or Linux)

    * it is important to keep FFmpeg up-to-date due to security vulnerabilities in older versions

1. Install FFmpeg following instructions for the OS.

    *

1. Make sure the FFmpeg executable is set in your system path.  
   You should be able to run FFmpeg from any directory in your system.

    * for example, `ffmpeg -version`

## Configure FFmpeg Transcoding Service {#configure-ffmpeg-transcoding-service}

By default, when FFmpeg is installed, multiple renditions are configured (transcodings) as per the DAM Update Asset workflow definition.

As the transcodings are CPU intensive, it is recommended to modify the list of target renditions. In most cases, transcoding is not necessary.

To modify the DAM Update Asset workflow, and in this example, to turn off transcoding :

* sign into the author instance with administrative privileges
* from global navigation : **Tools, Workflow, Models**
* locate **DAM Update Asset**
* double-click to open the workflow for edit in the Classic UI  
  resulting location : [http://localhost:4502/cf#/etc/workflow/models/dam/update_asset.html](http://localhost:4502/cf#/etc/workflow/models/dam/update_asset.html)

* double-click the **FFmpeg transcoding** step to access the Step Properties dialog
* under the `Process` tab:

    * **Arugments **: clear all entries to disable transcoding  
      default values : `profile:firefoxhq,profile:hq,profile:flv,profile:iehq`

![](assets/chlimage_1-384.png)

* select **OK** to close the `Step Properties` dialog

* select **Save** to save the `DAM Update Asset` workflow  
  (upper left corner)
