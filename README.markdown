# FM-Crumb

FM-Crumb is a bolt-on tool for adding dynamic global navigation to FileMaker database applications.

The basic idea is that the layout names in a FileMaker file should describe the navigation structure in the file. For example, we might have a "Contacts: Form" and a "Contacts: List" layout. We should be able to parse the "Contacts" module name from the layout names. The script included with Crumb will parse the layout names in a FileMaker file, and use that information to organize global navigation for the file.

There are other global navigation schemes in FileMaker, but most of them require the developer to duplicate information about the navigation structure that's already available in "Manage Layouts" and FileMaker's design functions. Some make developers update *every* layout whenever there's a change. Others store references to layouts in records in an extra table, including permissions information. That's usually overkill.

Crumb gets everything it needs to know from a file's layout names. It also inherits the layout permissions from FileMaker's built-in security: if the logged-in user can't see a particular layout, neither can Crumb, since the "Load Crumb Modules" script does not run with [Full Access] privileges.

I'm currently working on re-writing it from scratch for FileMaker version 12. In the mean time, you can still use the original solution for FileMaker 11.

## Format

## Delimiter Functions

The delimiter functions define text constants that can be changed to suit different layout naming conventions. They can not be changed to enable a naming convention that changes the placement of tokens in layout names, or to enable using tabs within layout names.

### CrumbHidePrefix

Returns the text used as a prefix in layout names that should not be listed in Crumb navigation tools presented to users. The tilde "~" is recommended to be consistent with the conventions at FileMakerStandards.org. Layout names beginning with a hyphen will also be treated as hidden, regardless of the value returned by this function.

### CrumbSeparator

Returns the text used as a delimiter between the module name and the page name in a layout name. ": " is used here.

## Setup Function

### CrumbSetup ( layoutNameList ; layoutIDList )

Parses the layout names in layoutNameList to determine the navigation structure of a FileMaker file, and populates the $$~CRUMB global variable with data used by the navigation functions. This function should typically be used in an OnFirstWindowOpen script:

	Set Variable [$!; Value:CrumbSetup ( LayoutNames ( "" ) ; LayoutIDs ( "" ) )]

The function returns True (1) if it parses the names successfully; it returns False (0) otherwise.

## Navigation Functions

### CrumbByLayoutID ( theLayoutID )

Returns a return-delimited list of data about a layout specified by FileMaker's internal layout ID:

1. The Crumb module number
2. The Crumb page number
3. The Crumb module name
4. The Crumb page name
5. FileMaker's internal layout ID

For layouts that consist only of the module name, the module name is repeated for both the module and page names. Modules with no non-hidden pages and hidden pages will have fractional module and page numbers.

### CrumbByPageNumber ( moduleNumber ; pageNumber )

Returns a return-delimited list of data about a layout specified by the module and page numbers assigned by Crumb:

1. The Crumb module number
2. The Crumb page number
3. The Crumb module name
4. The Crumb page name
5. FileMaker's internal layout ID

For layouts that consist only of the module name, the module name is repeated for both the module and page names. To get a module name without reference to a particular page, leave pageNumber empty.

### CrumbLayoutNumber ( moduleNumber ; pageNumber )

Returns the layout number for the specified module and page numbers. This is useful for specifying a layout number for the Go to Layout script step.

### CrumbSetLayoutVariables ( theLayoutID )

Use this function in a conditional formatting calculation on a layout to set module names and page names to locally scoped variables to be used in merge variables on the layout as labels for navigation controls.

## License

Anyone may do anything with this software. There is no warranty.