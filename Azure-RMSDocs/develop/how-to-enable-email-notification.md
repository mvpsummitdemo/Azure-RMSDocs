﻿---
# required metadata

title: Enabling email notification | Azure RMS
description: Email notification allows for a protected content owner to be notified when his or her content is accessed.
keywords:
author: bruceperlerms
manager: mbaldwin
ms.date: 04/28/2016
ms.topic: article
ms.prod: azure
ms.service: rights-management
ms.technology: techgroup-identity
ms.assetid: fcaa5643-64a9-4181-b29b-90211fce7ab5

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: shubhamp
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

﻿
# Enabling email notification

Email notification allows for a protected content owner to be notified when his or her content is accessed.

To setup your email notification for a given license, use [**IpcSetLicenseProperty**](/rights-management/sdk/2.1/api/win/functions#msipc_ipcsetlicenseproperty) with the property type parameter, *dwPropID*, as [**IPC\_LI\_APP\_SPECIFIC\_DATA**](/rights-management/sdk/2.1/api/win/License%20property%20types#msipc_license_property_types_IPC_LI_APP_SPECIFIC_DATA) and the application data fields formatted as an [**IPC\_NAME\_VALUE\_LIST**](/rights-management/sdk/2.1/api/win/structures#msipc_ipc_name_value_list).

## C++

    ...
    int numDataPairs = 3;

    IPC_NAME_VALUE propertyValuePairs [numDataPairs];

    // lcid field set to 0 causes the default lcid to be used

    propertyValuePairs[0] = {&quot;MS.Conetent.Name&quot;, 0, &quot;FinancialReport.docx&quot;};
    propertyValuePairs[1] = {&quot;MS.Notify.Enabled&quot;,0 , &quot;true&quot;};
    propertyValuePairs[2] = {&quot;MS.Notify.Culture&quot;,0 , “en-US”};

    IPC_NAME_VALUE_LIST emailNotificationAppData = {numDataPairs, propertyValuePairs};

    result = IpcSetLicenseProperty( licenseHandle, FALSE, IPC_LI_APP_SPECIFIC_DATA, emailNotificationAppData);
    ...    

The following table contains the application data fields, property name and value pairs, for RMS email notification.


|Property Name | Data Type | Example Value | Notes |
|--------------|-----------|---------------|-------|
|MS.Content.Name|string|“FinancialReport.docx”|This is an identifier associated with the protected content.<br><br> For protected files this value should be the name of the file without any path information.<br><br> For other types of content such as an email message it might be the subject of the email or it might be empty.|
|MS.Notify.Enabled|string|“true” &#124; “false”|If this value is set to “true” a notification email will be sent to the owner of the publishing license when someone attempts to use it to obtain an end user license.|
|MS.Notify.Culture|string|“en-US”| **Source:** System.Globalization.CultureInfo.CurrentUICulture.Name <br><br>This value is used to determine the localized language of the notification email and the date/time and number formatting that should be used in the email message.<br><br>It should be set based on user settings of the machine that the publish license is created on, or based on the preferred culture of the owner of the publish license.|
|MS.Notify.TZID|string|“Pacific Standard Time”|**Source:** TimeZoneInfo.Local.Id - Windows time zone ID.<br><br>This value is the Microsoft Windows OS time zone identifier describing a particular time zone and its characteristics.|
|MS.Notify.TZO|string|“-480”|This is the publish license owner’s time zone offset in terms of minutes from UTC time.<br><br>If a valid TZID value is provided the offset of the time zone specified by it will be used and this value will be ignored.<br><br>This value will more than likely be used by non-windows based publishing platforms that do not have access to the list of Windows OS time zone ID values.<br><br>If a TZID value is not provided this value will be used to calculate the time offset in notification messages, and the TZSN will be used (regardless of the time zone value) to indicate the name of the time zone. This will result time zone being fixed and not updating for daylight savings when it is applicable.<br><br>For example:<br><br>If TXID is blank and TZ0 is set to “-420” and the TZSN is set to “Pacific Daylight Time” all values shown in the notification email will be adjusted to "Pacific Daylight Time” and displayed as such even if daylight savings is no longer in affect currently.<br><br>On the other hand if a TZID is supplied along with both TZSN and TZDN, then the times specified in the notification email will be adjusted and displayed based on whether the date and time should be displayed in Daylight mode or Standard mode.|
|MS.Notify.TZSN|string|“Pacific Standard Time”|**Source:** TimeZoneInfo.Local.StandardName - Standard Time Zone name.<br><br>This should the localized name of the time zone’s standard time zone name.|
|MS.Notify.TZDN|string|“Pacific Daylight Time”|**Source:** TimeZoneInfo.Local.DaylightName - Daylight Time Zone name.<br><br>This should be the localized name of the time zone’s daylight savings name. It can be the same as the standard name if the time zone does not support daylight savings.|

## Related topics

* [**IpcSetLicenseProperty**](/rights-management/sdk/2.1/api/win/functions#msipc_ipcsetlicenseproperty)
* [**IPC\_LI\_APP\_SPECIFIC\_DATA**](/rights-management/sdk/2.1/api/win/License%20property%20types#msipc_license_property_types_IPC_LI_APP_SPECIFIC_DATA)
* [**IPC\_NAME\_VALUE\_LIST**](/rights-management/sdk/2.1/api/win/structures#msipc_ipc_name_value_list)
 

 