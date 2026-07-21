# Analytics

* * *

## Summary

Google Analytics is the weapon of choice for website analytics. Generally speaking on a WordPress website we would use the [Beehive Pro](General/Tools%20&%20Plugins.md#supporting--commonly-used-plugins) from WPMU Dev to add Google Analytics tags, this is for two reasons

* Using a third party plugin removes any overhead of managing the Google Analytics tags and code
* It provides some basic integrations with Google Tag Manager if extra data is required in Google Analytics

## Sending Custom Data to Google Analytics

### Add Google Tag Manager to Website

To send custom data to Google Analytics requires a Google Tag Manager account and for the Container ID to be added under _Beehive Pro > Google Tag Manager_.

### Setup Custom Data

Once added, you can provide additional data via _Beehive Pro > Google Tag Manager > Settings_. Note that from here you can make data available to Google Tag Manager from a number of pre-defined settings. If other dynamic data is required that isn’t provided here, this would need to be added manually.

- [ ] To do: Document how to add additional dynamic data

Ensure that you have a Google Analytics Tag and a Page View trigger set up in Google Tag Manager.

- [ ] To do: Document the process to add GA Tag and Page View trigger

### Set Up Receiving in Google Analytics

* Go to the Google Analytics account you are working with and choose “Admin” in the bottom left corner
* In the “Property” column choose _Custom Definitions > Custom Dimensions_ then click “+ New Custom Dimension”
* Give it a name and ID and save.

Take note of the ID here as we will need this to send the data from Tag Manager

### Add Custom Data to Tag Manager and Send to Analytics

* In Google Tag Manager, under _Variables > User-Defined Variables_ click “New”.
* Give the tag a relevant name
* Edit the variable and choose “Data Layer Variable” from the list
* Enter the “Data Layer Variable Name” as defined in Beehive Pro or the custom code, e.g. “post_author_name”

You then need to go back to the _Variables_ screen and edit the “Google Analytics Settings” variable:

* Click “Google Analytics Settings” and expand the “More Settings” section
* Click “Add Custom Dimension” and add the ID of the dimension that you added in Google Analytics
* Add the dimension value, preferably by selecting it from the list to ensure it is formatted correctly, it should look something like this - {{post_author_name}}
* At the top of the screen click save

You can then use the “Submit” button in the top right to publish the changes.

At this point the site is now set up to provide the new tag to Google Tag Manager, and Google Tag Manager is sending this data to Google Analytics.

### Verify the Data is Being Received

You can now verify if the data is coming into Analytics by going to a report, a useful general report is _Behaviour > Site Content > All Pages_. Choose today as the date range and add a second dimension to the report, then add your new dimension.

Visit a page on the website that should provide the new data dimension and verify that the data appears in the report.

Data normally takes a few minutes to appear in the report, so give it a moment and check back