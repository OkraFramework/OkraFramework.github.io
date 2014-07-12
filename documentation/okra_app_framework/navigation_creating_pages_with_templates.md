---
layout: documentation
title: Creating pages with the Visual Studio templates
---

Creating pages with the Visual Studio templates
===============================================

The Okra App Framework includes a Visual Studio extension that provides a number of item templates for creating typical Windows Store
application pages. Once you have [downloaded and installed the extension](getting_started_downloading.html) you can select Okra App
Framework specific MVVM templates from the Visual Studio **Add New Item** dialog. Many of these match the default Windows Store item templates
but include the "(MVVM)" suffix. Each item template includes a XAML page and view-model as well as a class for providing design-time data.

The provided item templates include,

* Basic Page (MVVM) - A minimal page with layout awareness, a title, and a back button
* Split Page (MVVM) - A page that displays a list of items and the details for a selected item
* Items Page (MVVM) - A page that displays a collection of item
* Item Detail Page (MVVM) - A page that displays one item in detail along with navigation to adjacent items in the same group
* Grouped Items Page (MVVM) - A page that displays item previews arranged in groups
* Group Detail Page (MVVM) - A page that displays details for a single group and previews each item in the group
* Search Contract (MVVM) - An app contract that supports displaying search results
* Share Target Contract (MVVM) - An app contract that supports receiving items shared by other apps
* Settings Pane (MVVM) - A settings pane
