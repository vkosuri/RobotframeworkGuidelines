Introduction
------------

A custom library was imported globally in addition in each test case.  A single import in the *** Settings *** section is all that should be required.  The ‘Import Library’ keyword within test cases is only necessary when the Libary to import is not known at the time of the test being parsed.

 1. Custom libraries should be placed in their own package directory so that file path imports are not required.
 2. http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#id473
 3. In the link above, the desired style is to import Using the Library Name, not the physical path to the librarys

Test Suite Setup/Teardown
-------------------------
There should be a Suite Teardown setting to disconnect from any open external connections (Database, UUTs, Open Files, etc...)

 1. http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#suite-setup-and-teardown
 2. You can use the ‘Suite Setup’ and ‘Suite Teardown’ settings in the *** Settings *** section to do this correctly.
 3. The Suite Setup/Teardown settings only take a single keyword with arguments.  To accomplish more than one action you can define your own keywords in the *** Keywords *** section to accomplish all necessary tasks, you can even make compound keywords there if necessary.
 4. As time goes on, these user keywords would be transferred to a library and the “Setup” Library would be imported.

Custom Libraries
----------------
 1. We’ll be using the Static API method to create keywords: http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#creating-static-keywords
 2. We’ll be using the ‘under_score’ style for method names and the CamelCase style for Class Names.
 3. Private methods that should not be keywords must start with an underscore
    - def this_is_a_keyword():
    - def _this_is_not():
 4. All importable libraries will be classes, no keywords should be defined in a Python module.
    - This avoids the problem of imported methods in your python library becoming keywords unintentionally.
 5. Most library python modules will only contain one class, and that class should have the same name as the module file.
    - I.E. if your file is ‘MyLibrary.py’, then your class should be ‘class MyLibrary’.
    - This can then be imported into a Robot Test simply via ‘Library MyLibrary’
    - Additional classes in ‘MyLibary.py’ module can be imported into a Robot Test file via ‘Library MyLibrary.SecondClass’
 6. Once a Custom Library is imported you can use the Keywords without specifiying the Library you imported, but to avoid name collisions and to improve readability, we should require using the Library name, then keyword.  For example, if we imported a Libaray called ‘MyLibrary’ with a keyword ‘My Keyword’, then to call it in a test case you should:
    - Do this: MyLibrary.My Keyword
    - Not This: My Keyword

Variables
---------
 1. When a scalar variable is used in a test data cell with anything else (constant strings or other variables), its value is first converted into a Unicode string and then catenated to whatever is in that cell. Converting the value into a string means that the object's method __unicode__ (in Python, with __str__ as a fallback)
    - More info http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#scalar-variables
 2. Robot Framework variables, similarly as keywords, are case-insensitive, and also spaces and underscores are ignored. However, it is recommended to use capital letters with global variables (for example, ${PATH} or ${TWO WORDS}) and small letters with variables that are only available in certain test cases or user keywords (for example, ${my var} or ${myVar}). Much more importantly, though, cases should be used consistently.
 3. Robot Framework ${/} variable would use forward slash as folder separator. File separator applicable both Windows and Linux Operating system. more information http://robotframework.org/robotframework/latest/libraries/OperatingSystem.html#Path%20separators

Decorators
----------
 1. When writing static keywords, it is sometimes useful to modify them with Python's decorators. However, decorators modify function signatures, and can confuse Robot Framework's introspection when determining which arguments keywords accept. This is especially problematic when creating library documentation with Libdoc and when using RIDE. To avoid this issue, either do not use decorators, or use the handy decorator module to create signature-preserving decorators.
    - http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#using-decorators
