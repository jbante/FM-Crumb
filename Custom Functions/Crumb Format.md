A "Crumb" object represents a FileMaker layout, and information about where it fits in the global navigation scheme of a FileMaker application. The Crumb scheme assumes a hierarchical structure limited to two levels: there are _modules_, and there are _pages_ (layouts) within modules; there are no modules within modules.

The Crumb function parses the following data from a layout name:

- module:	The module name
- page:	The page name
- isVisible:	A value of True (1) indicates that the layout should be exposed to the user in global navigation. False (0) indicates that the layout should not be exposed.

Layout names should follow this pattern:

	[<not isVisible indicator>]<module>[<separator>][<page>]

- <not isVisible indicator> represents whatever string the file uses as a prefix to indicate that a layout should not be included in global navigation. If the layout should be visible, this can simply be omitted.
- <module> represents the module name for the layout
- <separator> represents whatever string the file uses to separate the module name from the page name. It is optional if the module name is also the page name.
- <page> represents the page name for the layout.

In a file where "~" is the <not isVisible indicator> (to be consistent with conventions from FileMakerStandards.org), "Contacts" is the module, ": " is the separator, and "Form" is the page, we have:

	~Contacts: Form

If "Home" is both the module and the page name, which should be visible in the global navigation, we have simply:

	Home

Because a hyphen "-" is used for the names of layout separators, Crumb will treat any layout beginning with a hyphen as if it is hidden.

The Crumbs for a given system are stored in a data structure in a single global variable for reference by various calculations and scripts.

moduleList:	A return-delimited list of the names of modules within the file
<moduleName>:	A return-delimited list of the Crumb objects for each page in moduleName