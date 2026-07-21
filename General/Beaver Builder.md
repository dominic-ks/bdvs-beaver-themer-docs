# Beaver Builder

* * *

## Summary

Beaver Builder is a drag and drop website builder for WordPress. Beaver Builder is the first choice for creating websites with Be Devious Web Development as it allows site administrators to easily manage content on the website while allowing developers to develop custom modules using HTML, CSS and JavaScript.

When working to a pre-defined design, more often than not, you should use the [BDVS Beaver Themer](BDVS%20Beaver%20Themer.md) plugin, however some examples are given on this page as to how you can and should use Beaver Builder for BDWD websites.

## Usage

Although using the builder is the same regardless of the task, note that headers, footers and single templates are managed using Beaver Themer and archives use a combination of Beaver Themer and Toolset Views. So, you will tend to only create standalone Beaver Builder pages for Pages.

## Links

This document does not intend to provide in depth documentation for the Beaver Builder plugins, there is unlimited information online that can be found. Useful links:

* Beaver Builder Website - [https://www.wpbeaverbuilder.com/](https://www.wpbeaverbuilder.com/)
* Beaver Builder Docs - [https://docs.wpbeaverbuilder.com/](https://docs.wpbeaverbuilder.com/)

## Typical Beaver Builder Modules and Setups

### Margins and Padding

As a general rule, you should leave the margins and padding as the defaults that appear in Beaver Builder. Exceptions to this normally include when using the BDVS Content Template, you may wish to set a row and content to full width and remove margins and padding from the row, column and module so that your BDVS Content Template can span the screen.

Additionally, where ever possible, use the link button to sync the padding / margin values.

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/610992129/bdvs_bb_demo_02_20210222.gif?version=1&modificationDate=1614360902614&cacheVersion=1&api=v2)

### Standard Left / Right Image / Content Row

When making a basic layout such as this it’s fine to use standard Beaver Builder modules, however if the layout becomes complex, e.g. full width row with partial width text then BDVS Beaver Themer should be used.

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/610992129/bdvs_bb_demo_01_20210222.gif?version=1&modificationDate=1614360712825&cacheVersion=1&api=v2)

### Saved and Globally Saved Rows, Columns and Modules

Rows, columns and modules can be saved to be reused elsewhere. They can also be saved globally, and doing so syncs the contents of the element anywhere it is used on the site. The global option should only be used if no change in content is required between instances of the element.

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/610992129/bdvs_bb_demo_03_20210222.gif?version=1&modificationDate=1614361121661&cacheVersion=1&api=v2)