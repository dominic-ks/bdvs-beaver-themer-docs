# Beaver Themer

* * *

## Summary

Beaver Themer is an add-on for Beaver Builder that allows the following:

* Creating theme parts, like Header and Footer
* Creating single templates, e.g. for posts or other CPTs
* Creating post type archives, though on BDWD, we use Toolset Archives in conjunction with Beaver Themer Headers for this…

It does this by enabling Beaver Builder for these parts and allowing the linking of data from the database.

### Useful Links

* The Beaver Themer Site - [https://www.wpbeaverbuilder.com/beaver-themer/](https://www.wpbeaverbuilder.com/beaver-themer/)
* Documentation - [https://docs.wpbeaverbuilder.com/beaver-themer/](https://docs.wpbeaverbuilder.com/beaver-themer/)

### Usage with BDVS Beaver Themer

Beaver Themer field links can also be used with fields created in Toolset Views via the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) plugin and process.

### Linking Toolset Custom Fields

When linking fields created with Toolset, it’s important to note that these should be linked as a custom field using the field slug prefixed with _wpcf-_ e.g. a field with slug “_my-custom-field”_ would be entered as “_wpcf-my-custom-field”._

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/611287049/bdvs_bb_demo_05_20210222.gif?version=1&modificationDate=1614362152157&cacheVersion=1&api=v2)

### Linking Toolset Color Custom Fields

When creating a custom field that is a colour chooser, selecting the _Custom Field_ connection type will not work, since Toolset provides the value with a # at the start of the hex value and Beaver Themer will also add a # in front of that. Therefore, instead you can use the _Toolset Color Field_ connector instead which will strip the # from the stored value.

## Setting Row Background Images

The process for setting a row background images can be seen in the below demonstration:

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/611287049/setting%20row%20backgrounds_01_20210504.gif?version=1&modificationDate=1620140362939&cacheVersion=1&api=v2)

## Setting Responsive Column Widths

The process for setting a responsive column widths can be seen in the below demonstration:

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/611287049/setting%20column%20widths_01_20210504.gif?version=1&modificationDate=1620140577657&cacheVersion=1&api=v2)