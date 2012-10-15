# FM-Crumb

FM-Crumb is a bolt-on tool for adding dynamic global navigation to FileMaker database applications.

The basic idea is that the layout names in a FileMaker file should describe the navigation structure in the file. For example, we might have a "Contacts: Form" and a "Contacts: List" layout. We should be able to parse the "Contacts" module name from the layout names. The script included with Crumb will parse the layout names in a FileMaker file, and use that information to organize global navigation for the file.

There are other global navigation schemes in FileMaker, but most of them require the developer to duplicate information about the navigation structure that's already available in "Manage Layouts" and FileMaker's design functions. Some make developers update *every* layout whenever there's a change. Others store references to layouts in records in an extra table, including permissions information. That's silly.

Crumb gets everything it needs to know from a file's layout names. It also inherits the layout permissions from FileMaker's built-in security: if the logged-in user can't see a particular layout, neither can Crumb, since the "Load Crumb Modules" script does not run will [Full Access] privileges.

I'm currently working on re-writing it from scratch for FileMaker version 12. In the mean time, you can still use the original solution for FileMaker 11.

## License

Anyone may do anything with this software. There is no warranty.