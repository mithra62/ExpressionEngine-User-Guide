<!--
    This source file is part of the open source project
    ExpressionEngine User Guide (https://github.com/ExpressionEngine/ExpressionEngine-User-Guide)

    @link      https://expressionengine.com/
    @copyright Copyright (c) 2003-2021, Packet Tide, LLC (https://packettide.com)
    @license   https://expressionengine.com/license Licensed under Apache License, Version 2.0
-->

# Front-end content management

For admin members with appropriate level of access ExpressionEngine Pro adds "quick edit" links on the site's frontend that allow editing contents of each entry field individually, directly on front-end without going to the control panel.

Clicking the link opens a popup with the field that can be modified and saved; as well it presents the link to edit full entry with live preview showing the current page you are on.

The links are being placed in yout HTML right before the field's tag (or opening tag if fild tag pair is used).

## Disabling the link

There are several ways to disable front-end editing links:
 - globally with [configuration overrides](pro/configuration.md#disable_frontedit) or in [General Settings](pro/configuration.md#general-settings)
 - they can be switched off using the toggle in the Dock itself
 - can be disabled using HTML comments, ExpressionEngine template comments, or field parameter

### Disabling link in the template

There are three ways to completely prevent field edit link to be displayed in template.

### Template preferences

In template preferences under Pro Settings there is switch for "Disable front-end editing?"
When turned on, front-end editing features will be disabled in template completely.

![pro template settings](/_images/pro_template_settings.png)

#### EE Comment

Anything content wrapped in `{!-- disable frontedit --} ... {!-- //disable frontedit --}` HTML comment will not have edit links in it.

    <h1>{!-- disable frontedit --}{title}{!-- //disable frontedit --}</h1>

NOTE: **Hint:** If you need to disable front-end edit on large number of templates completely, you can wrap these comments around layout template.


#### HTML Comment

Anything content wrapped in `<!-- disable frontedit --> ... <!-- //disable frontedit -->` HTML comment will not have edit links in it.

    <h1><!-- disable frontedit -->{title}<!-- //disable frontedit --></h1>

NOTE: **Hint:** If you need to disable front-end edit on large number of templates completely, you can wrap these comments around layout template.

#### Field tag parameter
Use `disable="frontedit"` parameter on field tag to disable link for a certain field.

    {page_content disable="frontedit"}

### Customizing the link

The edit link for the field can be placed manually using **:frontedit** modifier on the field, **frontedit_link** tag if you need to have it in different section of template, custom styled - or you can even place edit link for field that is currently not on the page.

#### `:frontedit`

Inside `{exp:channel:entries}` tag, using the field name postfixed with `:frontedit` will generate edit link for that field within displayed entry.

You may find it useful if `disable_frontedit_links` configuration override is set and you need to place the links individually.

Example usage:

    {title:frontedit}

#### `{frontedit_link}`

Can be used to place edit link in arbitrary place (also outside or `exp:channel:entries` tag). The link can have custom CSS class applied to it.

| Parameter  | Description |
| ---------- | ----------- |
| entry_id   | ID of entry to edit. Required. |
| field_name | Short name of field to edit. Required, unless `field_id` is specified. |
| field_id   | ID of field to edit. Required, unless `field_name` is specified. |
| site_id    | Extra CSS class to apply to link HTML. |
| class      | Extra CSS class to apply to link HTML. |

Example usage:

    <h1>
        <!-- disable frontedit -->{title}<!-- //disable frontedit -->
        {frontedit_link entry_id="{entry_id}" field_name="title"}
    </h1>

    {page_content disable="frontedit"}
    {frontedit_link entry_id="{entry_id}" field_name="page_content" class="extra-styles"}


#### Label Custom Front-end Edit Links
When using custom links, it may be useful to label these links for the user. One way of accomplishing this is by wrapping your link in a custom HTML element and then hide/showing element with JavaScript. 

Example:

```
Template Code:
<span class="pro-edit-link" style="display:none;">Edit Homepage Property Slider:</span> {homepage_property_slider:frontedit}

Javascript:
//will only show selected elements if edit links are also visible on the page, which would indicate that ExpressionEngine Pro edit links are active. Otherwise the inline style will hide this element.
if( $(".eeFrontEdit").length > 0){
        $(".pro-edit-link").show();
    }
```
## Unsaved Changes
When a user edits data and has unsaved changes, Pro will create an autosave of the entry and alert the user via a notification on the edit window with unsaved changes. The autosave is triggerd based on the default autosave interval of 60seconds. If you want to ensure users do not accidentally loose unsaved changes adjust the `[autosave_interval_seconds]` system config override to a lower interval between autosaves. A setting of 10 seconds is the recommendation.

Example Usage:

```
$config['autosave_interval_seconds'] = '10'; 
```

## Reinitialize ExpressionEngine Pro Javascript
There may be times, such as when using AJAX, that page content is loaded after ExpressionEngine Pro has been initialized on a page. When this happens edit links may not work or even render. To fix this, you need to included `EE.pro.refresh();` in your script. 

Example usage:

```
function getData() {
    $.ajax({
        url : 'example.com',
        type: 'GET',
        success : reInitPro
    })
}

function reInitPro(){
    $.get(ajaxLink, function(data) {
}
```
