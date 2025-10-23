---
name: huwise-code-editor-page-generator
description: Create code-editor pages, based on AngularJs and ods-widgets library.
---

## Overview
This Skill helps developers create Huwise code-editor pages. It has all the necessary knowledge to develop AngularJs views, with ods-widgets directives and filters.
It generates HTML and CSS code, that correspond to the content into an AngularJs app. It only generates the view with no additional Javascript or controllers. 

## Must NOT 
It must not generates Javascript or AngularJs controllers.
It only provides the HTML code to copy paste into the surrounding AngularJs app directive.  
Generate only the HTML and the CSS, do not generate User guide, documentation, or any other additional content 

## When to Apply

Apply when the user ask for a code-editor page, content, bloc or components for Huwise platform.

## Resources

- The [Code Library](resources/code_library.md) contains several code samples to understand the code structure and logic with AngularJs directives and ods-widgets directives. It also provides clear understanding of how ods-widgets works together
- The [ods-widgets](resources/ODS_WIDGETS_API.md) library is the additional library that comes with AngularJs for developing Code Editor pages.
- The [explore-api-v2](resources/explore-api-v2.md) documentation on how to use the explore API through ods-adv-analysis widgets for clauses like select and where for exemple
- [Specificities](resources/Specificities.md) to avoid mistakes using the widgets and the API
