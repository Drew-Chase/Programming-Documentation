# Class Library

Next we are going to create a class library to hold the majority of our code.  This allows are code to be more reusable in the future. Even if you plan to delete this project once your done, this is still a good practice to follow.

1. Open your solution explorer and right click your solution, the solution is the top-most item in the solution explorer.\
   ![](<../../../.gitbook/assets/image (2).png>)
2. Then under the "Add" subsection, click "New Project"\
   ![](<../../../.gitbook/assets/image (7).png>)
3. Now select "Class Library" from the templates list\
   ![](<../../../.gitbook/assets/image (6).png>)
4. Name it "Library"
5. Set the Framework to the same as the parent _(.NET 6.0 (LTS) for this example)_
6. Click Create!
7. Now delete the default class that is created!

## Folder Structure

Now create two folders/namespaces inside the class library\
See: [namespaces.md](../../undestanding-csharp/namespaces.md "mention")

* Controller
* Model

The controller namespace is used to handle logic classes. Every class in this namespace should be named by this convension: "{Name}Controller.cs". \
For example if we wanted a class to handle the FileSystem we would say "FileSystemController.cs"

The model namespace is used to handle structure classes, these are classes that don't do anything but instead hold data. Every class in this namespace should be named by this convension: "{Name}Model.cs". \
For example if we wanted a class to hold FileData we would say "FileModel.cs"

![](<../../../.gitbook/assets/image (4).png>)
