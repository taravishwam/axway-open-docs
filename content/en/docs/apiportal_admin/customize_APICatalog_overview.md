---
title: Customize API Catalog
linkTitle: Customize API Catalog
weight: 4
date: 2019-07-30T00:00:00.000Z
description: >-
  Customize how your APIs are displayed to your API consumers and what actions
  they can perform.
---

## Customize API Catalog settings

You can customize the following in the API Catalog view of API Portal:

* Display APIs in a list or tile view.
* Show or hide the button that enables users to download API definitions. The default is shown.
* Show or hide the button that enables users to download client SDKs. The default is hidden.
* Hide tags from API Catalog.
* Show only APIs associated with specified tags. For more details on tags, see Group APIs with tags.
* Hide APIs associated with specified tags. For more details on tags, see Group APIs with tags.
* Show or hide the button that enables users to try out an API. You can show the button for all users, for authenticated users only, or hide it completely. The default is shown for all users.
* Display REST API details using the Swagger.io or AMPLIFY Swagger UI rendering tools. The default is AMPLIFY. SOAP APIs are always displayed using Swagger.io.
* Show or hide Nickname column when using AMPLIFY rendering tool. The default is hidden.
* Show or hide code examples in endpoint details when using AMPLIFY rendering tool. The default is shown.
* Order method parameters to determine the order in which the parameters for all methods are displayed.
* Set a payload size (in KB). If the response is bigger than the configured value, the response is downloaded as file. The default is blank, which means that downloads are disabled. Valid only for AMPLIFY rendering tool.
* Display REST API details using a colorful or colorless scheme when using the AMPLIFY rendering tool. You can also change the method colors when using the colorful scheme.

To change the API Catalog settings:

1. Log in to the Joomla! Administrator Interface (JAI) (`https://<API Portal_host>/administrator`).
2. Click **Menus > Main Menu**.
3. Click **APIs**.
4. Click the **API Catalog** tab.

    ![Customize API catalog](/Images/uploads/apiportal-jai-customize-api-catalog.png)

5. Change the settings as required and click **Save & Close**.

## Customize source of API descriptions

You can customize API Portal to show summaries instead of descriptions for APIs to give the app developer a quicker summary view of what the API is about instead of a long description. Using description can also have a performance impact, so it is best to use summary to improve the performance of the API Catalog view.

To change the settings:

1. In the Joomla! Administrator Interface (JAI), click **Components > API Portal > Additional Settings**.
2. In the **API Information Source** field, select `Description` or `Summary`.
3. Click **Save**.

## Customize page title or summary

You can customize the API Catalog page title, the summary text, or both.

1. In JAI, click **Menus > Main Menu**.
2. Select **APIs**, and go to the **Page Display** tab.
3. In **Masthead Title**, enter the new page title. If you leave this empty, the default title is used.
4. In **Masthead Slogan**, enter the new summary. If you leave this empty, the default text is used.
5. Click **Save & Close**.

## Group APIs with tags

You can add tags to APIs in API Manager and use them to split your API Catalog into smaller subsets. For example, you can create multiple themed API groups based on your developer communities.

For more details on adding tags to APIs, see the [API Manager User Guide](/bundle/APIManager_77_APIMgmtGuide_allOS_en_HTML5/) .

To create a dedicated API Catalog for a subset of tagged APIs, do the following:

1. Log in to Joomla! Administrator Interface (JAI).
2. Click **Menus > Main Menu > Add New Menu Item**.
3. Enter a menu title for the new API Catalog.
4. In **Menu Item Type**, click **Select > API Portal > API Catalog Page**.
5. Set **Access** to the level you want, and ensure that **Status** is set to `Published`.
6. In **Ordering**, select where in the main menu the new API Catalog appears. The menu item is placed after the item you select here.

    To access all your API Catalogs under the **APIs** menu item rather than additional menu items, set **Parent Item** to **APIs**.

7. On the **API Catalog** tab, in the **Show APIs with tags**, enter the tags to include in this API Catalog.
8. On the **API Catalog** tab, in the **Do not show APIs with tags**, enter the tags to exclude in this API Catalog.
9. On the **Page Display** tab, you can change the page title and summary text. For more details, see [Customize page title or summary](#customize-page-title-or-summary).
10. Click **Save & Close**.

Your themed API Catalog is now ready, and you can see it in your API Portal.

You can also choose to use some tags as an internal tool, and hide them from the API consumers. To hide tags, On the **API Catalog** tab, in **Hide tags**, enter the tags to hide.

### Create tags with wildcards

You can add tags using the `*` and `?` wildcards. This is helpful to list only development APIs in one API Catalog and production APIs in another. In this case you can filter them using wildcards as follows: `*dev*` will list APIs which contain `dev` somewhere in the tag, for example, `financial_development` and `development` tags. Or, to hide all tags which start with `test` and end with any other letter, for example, `test` or `tests`, you can do `test?`.

## Customize Try-it by type of request

The Try-It button is enabled for all requests to an API by default. To enable or disable it on specific types of requests (`GET`, `POST`, `PUT`, and so on):

1. In JAI, click **Components > API Portal > API Manager**.
2. In **Try-It Settings**, enable or disable the Try-It button for each request type.

    ![](/Images/uploads/api-manager-try-it-settings.png)
    
3. Click **Save**.

{{% alert title="Note" %}}
This setting applies only for this API Manager. To change it on other slave API Managers, go to **Components > API Portal > Additional API Managers**.
{{% /alert %}}
