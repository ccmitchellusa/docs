.. _dijit/Dialog:

============
dijit/Dialog
============

:since: V0.1.0
:jsDoc: http://dojotoolkit.org/api/dijit.Dialog

.. contents ::
    :depth: 2

Introduction
============

Dijit's modal Dialog Box simulates a regular GUI dialog box.
The contents can be arbitrary HTML, but are most often a form or a short paragraph.
The user can close the dialog box without acting by clicking on the X button in the top-right corner.

Usage
=====

.. js ::
 
    require(["dojo/ready", "dijit/Dialog"], function(ready, Dialog){
        ready(function(){
            // create the dialog:
            myDialog = new Dialog({
                title: "My Dialog",
                content: "test content",
                style: "width: 300px"
            });
        });
    });

After creating a Dialog, the Dialog (and the underlay) moves itself right behind the <body> element within the 
DOM, so it can overlay the entire webpage.
With this move no other elements parent the Dialog.domNode.
Therefore you have to add a ``class="claro"`` attribute (or some other applicable :ref:`theme name <dijit/themes>`)
to your <body> tag, in order to show the Dialog with the right styles:

.. html ::

    <html>
    <head>
        ...
    </head>
    <body class="claro">
        ...
    </body>


Examples
========

Dialog via markup
-----------------

The first example creates a Dialog via markup from an existing DOM node:

.. code-example ::

  Require the modules we are using:

  .. js ::

    require(["dojo/parser", "dijit/Dialog", "dijit/form/Button", "dijit/layout/TabContainer", "dijit/layout/ContentPane"]);

  Some simple HTML code you need to define the place where your Dialog should be created.
  
  .. html ::

    <div id="dialogOne" data-dojo-type="dijit/Dialog" title="My Dialog Title">
        <div data-dojo-type="dijit/layout/TabContainer" style="width: 200px; height: 300px;">
            <div data-dojo-type="dijit/layout/ContentPane" title="foo">Content of Tab "foo"</div>
            <div data-dojo-type="dijit/layout/ContentPane" title="boo">Hi, I'm Tab "boo"</div>
        </div>
    </div>

    <p>When pressing this button the dialog will popup:</p>
    <button id="buttonOne" data-dojo-type="dijit/form/Button" type="button">Show me!
        <script type="dojo/method" data-dojo-event="onClick" data-dojo-args="evt">
            // Show the Dialog:
            require(["dijit/registry"], function(registry){
                registry.byId("dialogOne").show();
            });
        </script>
    </button>


Note that dialog's source markup can be hidden via specifying style="display: none", to prevent it from flashing on
the screen during page load.
However, hiding the dialog indirectly via a class won't work (in that the dialog will
remain invisible even when it's supposed to be displayed).

Dialog programmatically
-----------------------

Now let's create a dialog programmatically, and change the dialog's content dynamically

.. code-example ::

  A programmatically created dialog with no content, with content added dynamically.

  .. js ::

    require(["dojo/ready", "dijit/Dialog", "dijit/form/Button"], function(ready, Dialog, Button){
        ready(function(){
            // create the dialog
            var myDialog = new Dialog({
                title: "Programmatic Dialog Creation",
                style: "width: 300px"
            });
            
            // create a button for Dialog demonstration
            var myButton = new Button({
                label: "Show me!",
                onClick: function(){
                    myDialog.set("content", "Hey, I wasn't there before, I was added at " + new Date() + "!");
                    myDialog.show();
                }
            }, "progbutton");
        });
    });

  .. html ::
    
    <p>When pressing this button the dialog will popup. Notice this time there is no DOM node with content for the dialog:</p>
    <button id="progbutton" type="button">Show me!</button>



Coloring the Underlay
---------------------

If you wish to alter the default color for the underlay, you do so in CSS.
The underlay receives an ID to match the Dialog, suffixed with :ref:``underlay``, which you can define a CSS class for:

.. code-example ::

  Require the modules we are using:

  .. js ::

    require(["dojo/parser", "dijit/Dialog", "dijit/form/Button"]);

  And some CSS rules:

  .. css ::

    #dialogColor_underlay {
        background-color:green;
    }


  And the markup to create the Dialog:

  .. html ::

    <div id="dialogColor" title="Colorful" data-dojo-type="dijit/Dialog">
        My background color is Green
    </div>

    <p>When pressing this button the dialog will popup:</p>
    <button id="button4" data-dojo-type="dijit/form/Button" type="button">Show me!
        <script type="dojo/method" data-dojo-event="onClick" data-dojo-args="evt">
            require(["dijit/registry"], function(registry){
                registry.byId("dialogColor").show();
            });
        </script>
    </button>


Forms and Functionality in Dialogs
----------------------------------

This example shows a Dialog containing form data.
You can get the form data as a javascript object by calling get('value') on the dialog.

To prevent the user from dismissing the dialog if there are errors in the form, add an onClick handler to your submit
button.
In order to run Dialog's execute-method the submit button has to be a dijit.form.Button, normal submit button
doesn't trigger this function.
In addition, the form has to be local, the dialog doesn't find the form values if it's
included via href attribute.

To simply close the dialog, click the Cancel button, which calls the hide() function on the Dialog.

.. code-example ::

  .. js ::

    require(["dojo/parser", "dijit/Dialog", "dijit/form/Button", "dijit/form/TextBox", "dijit/form/DateTextBox", "dijit/form/TimeTextBox"]);

  .. html ::

    <div data-dojo-type="dijit/Dialog" data-dojo-id="myFormDialog" title="Form Dialog"
        execute="alert('submitted w/args:\n' + dojo.toJson(arguments[0], true));">
        <table>
            <tr>
                <td><label for="name">Name: </label></td>
                <td><input data-dojo-type="dijit/form/TextBox" type="text" name="name" id="name"></td>
            </tr>
            <tr>
                <td><label for="loc">Location: </label></td>
                <td><input data-dojo-type="dijit/form/TextBox" type="text" name="loc" id="loc"></td>
            </tr>
            <tr>
                <td><label for="date">Start date: </label></td>
                <td><input data-dojo-type="dijit/form/DateTextBox" data-dojo-id="myStartDate" onChange="myEndDate.constraints.min = arguments[0];" type="text" name="sdate" id="sdate"></td>
            </tr>
            <tr>
                <td><label for="date">End date: </label></td>
                <td><input data-dojo-type="dijit/form/DateTextBox" data-dojo-id="myEndDate" onChange="myStartDate.constraints.max = arguments[0];" type="text" name="edate" id="edate"></td>
            </tr>
            <tr>
                <td><label for="date">Time: </label></td>
                <td><input data-dojo-type="dijit/form/TimeTextBox" type="text" name="time" id="time"></td>
            </tr>
            <tr>
                <td><label for="desc">Description: </label></td>
                <td><input data-dojo-type="dijit/form/TextBox" type="text" name="desc" id="desc"></td>
            </tr>
            <tr>
                <td align="center" colspan="2">
                    <button data-dojo-type="dijit/form/Button" type="submit"
                        data-dojo-props="onClick:function(){return myFormDialog.isValid();}">OK</button>
                    <button data-dojo-type="dijit/form/Button" type="button"
                        data-dojo-props="onClick:function(){myFormDialog.hide();}">Cancel</button>
                </td>
            </tr>
        </table>
    </div>

    <p>When pressing this button the dialog will popup:</p>
    <button id="buttonThree" data-dojo-type="dijit/form/Button" type="button">Show me!
        <script type="dojo/on" data-dojo-event="click" data-dojo-args="evt">
            myFormDialog.show();
        </script>
    </button>


If you want to handle the onSubmit event like a traditional <form> element, you will need to employ a <form> either as 
a traditional HTML element or as a ''dijit.form.Form''.
This example shows a Dialog with an embedded Form which
handles the onSubmit event, validation, and an xhrPost to the server.

.. code-example ::

  .. js ::

    require(["dojo/parser", "dijit/Dialog", "dijit/form/Form", "dijit/form/Button", "dijit/form/ValidationTextBox"]);

  .. html ::

    <div data-dojo-type="dijit/Dialog" data-dojo-id="myFormDialog" title="Form Dialog" style="display: none">
        <form data-dojo-type="dijit/form/Form" data-dojo-id="myForm">
            <script type="dojo/on" data-dojo-event="submit" data-dojo-args="e">
                require(["dojo/_base/event"], function(event){
                    event.stop(e); // prevent the default submit
                    if(!myForm.isValid()){ window.alert('Please fix fields'); return; }
                });

                window.alert("Would submit here via dojo/xhr");
                // xhr.post( {
                //      url: 'foo.com/handler',
                //      content: { field: 'go here' },
                //      handleAs: 'json'
                //      load: function(data){ .. },
                //      error: function(data){ .. }
                //  });
            </script>
            <div class="dijitDialogPaneContentArea">
                <label for='foo'>Foo:</label><div data-dojo-type="dijit/form/ValidationTextBox" data-dojo-props="required:true"></div>
            </div>
            <div class="dijitDialogPaneActionBar">
                    <button data-dojo-type="dijit/form/Button" type="submit">OK</button>
                    <button data-dojo-type="dijit/form/Button" type="button"
                        data-dojo-props="onClick:function(){myFormDialog.hide();}">Cancel</button>
            </div>
         </form>
    </div>

    <p>When pressing this button the dialog will popup:</p>
    <button id="buttonThree" data-dojo-type="dijit/form/Button" type="button">Show me!
        <script type="dojo/method" data-dojo-event="onClick" data-dojo-args="evt">
            myFormDialog.show();
        </script>
    </button>



Terms and Conditions Dialog
---------------------------

This example shows a Dialog that will ask the user to accept or decline the terms and conditions.

.. code-example ::

  .. js ::

    require(["dojo/parser", "dojo/ready", "dijit/Dialog", "dijit/form/Button", "dijit/form/RadioButton", "dojo/dom", "dojo/dom-style"], function(parser, ready, Dialog, Button, RadioButton, dom, domStyle){
        this.accept = function(){
            dom.byId("decision").innerHTML = "Terms and conditions have been accepted.";
            domStyle.set("decision", "color", "#00CC00");
            myFormDialog.hide();
        }

        this.decline = function(){
            dom.byId("decision").innerHTML = "Terms and conditions have not been accepted.";
            domStyle.set("decision", "color", "#FF0000");
            myFormDialog.hide();
        }
    });

  .. html ::

    <div data-dojo-type="dijit/Dialog" data-dojo-id="myFormDialog" title="Accept or decline agreement terms">
        <h1>Agreement Terms</h1>
    
        <div data-dojo-type="dijit/layout/ContentPane" style="width:400px; border:1px solid #b7b7b7; background:#fff; padding:8px; margin:0 auto; height:150px; overflow:auto;">
            Dojo is available under *either* the terms of the modified BSD license *or* the Academic Free License version 2.1. As a recipient of Dojo, you may choose which license to receive this code under (except as noted in per-module LICENSE files). Some modules may not be the copyright of the Dojo Foundation. These modules contain explicit declarations of copyright in both the LICENSE files in the directories in which they reside and in the code itself. No external contributions are allowed under licenses which are fundamentally incompatible with the AFL or BSD licenses that Dojo is distributed under. The text of the AFL and BSD licenses is reproduced below. ------------------------------------------------------------------------------- The "New" BSD License: ********************** Copyright (c) 2005-2010, The Dojo Foundation All rights reserved. Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met: * Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer. * Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
        </div>
    
        <br />
        <table>
            <tr>
                <td>
                    <input type="radio" data-dojo-type="dijit/form/RadioButton" name="agreement" id="radioOne" value="accept" data-dojo-props="onClick:accept" />
                    <label for="radioOne">
                        I accept the terms of this agreement
                    </label>
                </td>
            </tr>
            <tr>
                <td>
                    <input type="radio" data-dojo-type="dijit/form/RadioButton" name="agreement" id="radioTwo" value="decline" data-dojo-props="onClick:decline" />
                    <label for="radioTwo">
                        I decline
                    </label>
                </td>
            </tr>
        </table>
    </div>
    <p>
        When pressing this button the dialog will popup:
    </p>
        
    <label id="decision" style="color:#FF0000;">
        Terms and conditions have not been accepted.
    </label>
    <button id="termsButton" data-dojo-type="dijit/form/Button" type="button">
        View terms and conditions to accept
        <script type="dojo/on" data-dojo-event="click" data-dojo-args="evt">
            myFormDialog.show();
        </script>
    </button>


External Dialog content using HREF attribute
--------------------------------------------

You can also load dialog content from another page by setting HREF attribute for the widget.
Note that the dijit.Dialog doesn't execute script tags inline external content.
However, it parses the page for widgets,
so you can add functionality to widgets by connecting into widget extension points using declarative markup
(DojoML; e.g. ``<script type="dojo/method" data-dojo-event="onClick">``).

The :ref:`dojox.widget.DialogSimple <dojox/widget/DialogSimple>` provides the ``executeScripts`` functionality
of :ref:`dojox.layout.ContentPane <dojox/layout/ContentPane>` into ``dijit.Dialog``.

.. code-example ::
  :height: 500

  .. js ::

    require(["dojo/parser", "dijit/form/Button", "dijit/Dialog"]);

  .. html ::

    <div data-dojo-id="myExternalDialog" data-dojo-type="dijit/Dialog" title="My external dialog" href="{{dataUrl}}dojo/resources/LICENSE" style="overflow:auto; width: 400px; height: 200px;">
    </div>

    <p>When pressing this button the dialog will popup loading the dialog content using an XHR call.</p>
    <button data-dojo-type="dijit/form/Button" data-dojo-props="onClick:function(){myExternalDialog.show();}" type="button">Show me!</button>



Sizing the Dialog
-----------------

A dialog by default sizes itself according to its content, just like a plain <div>.
If you want a scrollbar on a dialog, then you need to add width/height to a div *inside* the dialog, like this:

.. code-example ::

  .. js ::

    require(["dojo/parser", "dijit/form/Button", "dijit/Dialog"]);

  .. html ::

    <div data-dojo-id="mySizedDialog" data-dojo-type="dijit/Dialog" title="My scrolling dialog">
        <div style="width: 200px; height: 100px; overflow: auto;">
            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean
                semper sagittis velit. Cras in mi. Duis porta mauris ut ligula. Proin
                porta rutrum lacus. Etiam consequat scelerisque quam. Nulla facilisi.
                Maecenas luctus venenatis nulla. In sit amet dui non mi semper iaculis.
                Sed molestie tortor at ipsum. Morbi dictum rutrum magna. Sed vitae
                risus.</p>
        </div>
    </div>

    <p>When pressing this button the dialog will popup (with a scrollbar):</p>
    <button data-dojo-type="dijit/form/Button" data-dojo-props="onClick:function(){mySizedDialog.show();}" type="button">Show me!</button>


Accessibility
=============

Keyboard
--------

====================================================    =================================================
Action                                                  Key
====================================================    =================================================
Navigate to next focusable element in the dialog        tab
Navigate to previous focusable element in the dialog    shift-tab
Close the dialog                                        escape
====================================================    =================================================

Keyboard Navigation in Release 1.1 and later
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When a dialog is opened focus goes to the first focusable element within the dialog.
The first focusable element may
be an element which appears in the tab order by default such as a form field or link, an element with a tabindex 
attribute value of 0 or an element with a tabindex value greater than 0.
Elements with a tabindex value greater than 0
will appear in the tab order before elements with a tabindex of 0 or those in the tab order by default.
If the dialog does not contain a focusable item,
focus will be set to the dialog container element when the dialog is opened.
The same focus behavior has been implemented for tooltip dialog

When focus is in a dialog, pressing the tab key will move focus forward to each focusable element within the dialog.
When focus reaches the last focusable element in the dialog,
pressing tab will cycle focus back to the first focusable item.
Pressing shift-tab will move focus backwards through focusable elements within the dialog.
When the first focusable item is reached,
pressing shift-tab will move focus to the last focusable item in the dialog.

Keyboard Navigation Previous to Release 1.1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When a dialog is opened focus goes to the title section of the dialog.
This was implemented to provide screen reader
support to speak the title of the dialog when it is opened.
Likewise, when a tooltip dialog is opened, focus is placed
on the container of the tooltip dialog.
In future versions of the dialog and tooltip dialog widgets, focus will go to
the first item in the dialog or tooltip dialog.

When focus is in a dialog, pressing the tab key will move focus forward to each focusable element within the dialog.
When focus reaches the last focusable element in the dialog, pressing tab will cycle focus back to the dialog title.
Pressing shift-tab will move focus backwards through focusable elements within the dialog until the dialog title is 
reached.
If focus has previous cycled forward through all of the elements, pressing shift-tab with focus on the dialog
title will move focus to the last element in the dialog.
If focus has not previously been cycled through all of the
focusable elements in the dialog using the tab key, pressing shift-tab with focus on the dialog title will leave focus 
in the title.
The same focus cycling applies to the tooltip dialog as well with focus being set to the tooltip dialog
container since there is no dialog title.

Improved Screen Reader Support in 1.4
-------------------------------------

The dialog now supports the aria-describedby property.
If you have a description of the dialog that you would like
spoken by the screen reader when the dialog opens, add the aria-describedby property to the dialog.
Include an element containing the text you want spoken in the dialog.
The value of the aria-describedby property is the id of the element containing the text.

.. html ::

  <div data-dojo-type="dijit/Dialog" title="Example Dialog" aria-describedby="intro">
    <div id="intro">Text to describe dialog</div>
    <div>Additional dialog contents....</div>
  </div>

For earlier Dojo versions, you can add an onshow event handler that adds the aria-describedby property:

.. html ::

  <div data-dojo-type="dijit/Dialog" title="Example Dialog" onShow="dojo.attr(this.domNode, 'aria-describedby', 'info');">
    <div id="intro">Text to describe dialog</div>
    <div>Additional dialog contents....</div>
  </div>

Known Issues
------------

* On Windows, In Firefox 2, when in High Contrast mode, the dialog with display correctly, but the underlying page  
  will not be seen.
* Dialogs with an input type=file as the only focusable element will not work with the keyboard. This is because input 
  type=file elements require two tab stops - one in the textbox and the other on the "Browse" button. Rather 
  than clutter the dialog box widget with code to special case for this one condition, dialog boxes with an input 
  type=file as the only focusable element are not supported.
* Dialogs with an input type=file element as the first focusable element in Firefox (and there are additional 
  focusable elements).
  Programmatically setting focus to an input type=file element behaves oddly in Firefox. In this case the focus is set 
  onto the textbox field and then immediately moved onto the browse button of the input type=file field. This causes 
  problems in Firefox when setting focus to an input type=file element as the first element as a dialog. For this 
  reason, in Firefox if the first
  focusable item in a dialog is an input type=file, focus will be set onto the dialog container rather than the input 
  element.
  For these reasons it is recommended that input type=file elements not be added as the only or first focusable item 
  within a dialog in Firefox.
* Even though the dialog is marked with the proper ARIA role of dialog, there are issues with screen readers. Due to 
  these issues , it is important that the instructions or label for a trigger element that opens a dialog to indicate 
  via text that a dialog will be opened.

  * JAWS 9 does not speak "dialog" when the dialog is opened in Firefox or IE 8.
  * In Firefox 2 even though the focus is on the first focusable item in the dialog, the information about that item 
    is also not spoken.
  * In Firefox 3 with JAWS 9 the dialog is also not announced but the information about the item in the dialog which 
    gets focus is spoken. The issue has been fixed in JAWS 10 with Firefox 3.
  * In IE 8 with JAWS 10 and JAWS 11 the dialog information and title is not spoken. This is due to the fact that IE 8 
    does not support the ARIA labelledby property that is used to assign the title to the dialog.
* There are focus issues when the dialog is created via an href. Due to timing issues focus may not be properly set 
  nor properly trapped
  in the dialog. For accessibility reasons, dialogs created via href are not recommended. This issue has been 
  addressed in the 1.5 release.
* When loading Dialog content with the href property, there can be issues with scrolling in IE7: If the loaded content 
  contains dijit.layout elements and the Dialog content is larger than the size of the dialog, the layout dijits do 
  not scroll properly in IE7. The workaround for this issue is to set the 'position:relative' style to the 
  dialog.containerNode:
* Dialogs with an iframe as the contents will cause a focus trap and are not supported. This because the dialog code 
  can not traverse within the iframe contents to find all of the focusable elements to know the first and last   
  focusable element within the contents.
* Dialogs with no focusable items cause problems for screen readers.  If the dialog has no focusable items, set the 
  tabindex="0" on the container element of the text.  This will set focus to that container when the dialog is opened 
  and will cause JAWS to speak the title of the dialog and the user will know that a dialog has been opened.

.. js ::

  require(["dojo/ready", "dijit/Dialog", "dojo/dom-style"], function(ready, Dialog, domStyle){
      dialogObj = new Dialog({
          id: 'dialogWithHref',
          title: 'The title',
          href: "/url/to/dialog/content/including/layout/dijit/"
      });
  
      domStyle.set(dialogObj.containerNode, {
          position: 'relative'
      });
  });


See also
========

* :ref:`dojox.widget.DialogSimple <dojox/widget/DialogSimple>` - mixin of dijit.Dialog and dojox.layout.ContentPane
* :ref:`dojox.widget.Dialog <dojox/widget/Dialog>` - more flexible dialog allowing animations and different styles/theme support
*  :ref:`dojox.image.Lightbox <dojox/image/Lightbox>` - a dojo-based Lightbox implementation
* :ref:`dojox.image.LightboxNano <dojox/image/LightboxNano>` - a simple "nano" version of the dojox.image.Lightbox (without captions, descriptions...)
* :ref:`dijit/TooltipDialog <dijit/TooltipDialog>` - modal tooltip with rich content for user interactions (forms ...)
