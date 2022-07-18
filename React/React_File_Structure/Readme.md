#### React File Structure Description

Knowing how to arrange your react project can help you write more legible code. This repository contains three file structures: beginner, intermediate, and advanced. Beginner level file structures benefit smaller react projects, whereas intermediate and advanced level file structures benefit enterprise sized software.  

#### Beginner Level File Structure

This is handful for small-sized sites, containing maybe about 5 pages, each of which is very unique to each other.    

```
.
├── public                  # Manifest, favicon and HTML file so you can tweak it.
├── src                     # Source file and files
│   ├── _tests_             # all the test files
│   ├── components          # all the components 
│   ├── hooks               # All the hooks
│   └── CSS & JS Files      # All Utility Css & Js Files
├── .gitignore              # Gitignore File
├── package-lock.json       
├── package.json              
└── Readme.md               # Project's Readme
```   

This file structure is useful for a small project like a to-do list, but it won't be useful for slightly more advanced projects, not too large, but quite large, as this next file structure will useful.   

#### Intermediate Level File Structure

This is handful for medium-sized sites, containing maybe about 10 to 15 pages, each of which is very unique to each other.   

```
.
├── public                  # Manifest, favicon and HTML file so you can tweak it.
├── src                     
│   ├── assets              # all the assets i.e. files other then js
│   ├── components          # All shared components such as buttons, checkboxes, etc. used throughout the project
│       ├── form            # form components
|       ├── ui              # ui components
|       └── js_files        # Various components that do not fit the above file description 
│   ├── context             # any kind of context will be here with their's test file
│       ├── _test_            
|       └── js_files        
│   ├── data                # contains contanst's and json's
│   ├── hooks               # all the hooks with their's test file
│       ├── _test_          
|       └── js_files        
│   ├── pages               # Components specific to each section (this can be customized according to your requirements)
│       ├── Home            # components that unique for Home with "_test_" inside
│       ├── Login           # components that unique for Login with "_test_" inside
│       ├── Settings        # components that unique for Setting with "_test_" inside
|       └── Signup          # components that unique for Signup with "_test_" inside 
│   ├── utils               # utility functions with their's test file
│       ├── _test_          
|       └── js_Files        
│   └── CSS & JS Files      # all Utility Css & Js Files
├── .gitignore              
├── package-lock.json       
├── package.json              
└── Readme.md               # Project's Readme
```   

This file structure will be useful when we have a application contains a lot of pages, maybe around 10-15 pages, but it will cause problems when you have very large enterprise style applications that have a lot of different features, you'll quickly run into problems with this structure. Issues you may face when using this framework when developing an enterprise-like application that contains many different features and has more complex components making component  pages and folders more cluttered before and debugging is not easy.    


#### Advanced Level File Structure

This is handful for large websites, i.e.  business-like applications that contain more features than the above and also scale based on user or customer needs. 

```
.
├── public                  # Manifest, favicon and HTML file so you can tweak it.
├── src                     
│   ├── assets              # all the assets i.e. files other then js
│   ├── components          # All shared components such as buttons, checkboxes, etc. used throughout the project
│       ├── form            # form components
|       ├── ui              # ui components
|       └── js_files        # Various components that do not fit the above file description 
│   ├── context             # any kind of context doesn't fits in features with their's test file
│       ├── _test_           
|       └── js_files        
│   ├── data                # contains contanst's and json's
│   ├── features            # diferent folder for diferent features implementation at one place with their's hooks component and services      
│   ├── hooks               # all the global/shared hooks and test files 
│       ├── _test_          
|       └── js_files        
│   ├── layout              # layout used in different pages many times like navigation bar, sidebar etc. (this is optional move to the component if not needed)
│   ├── lib                 # Third-party libraries used in the app
│   ├── pages               # Components specific to each section (this can be customized according to your requirements)
│   ├── utils               # utility functions with test file
│       ├── _test_          
|       └── js_Files        
│   ├── services            # api based service call
│   └── CSS & JS Files      # all Utility Css & Js Files
├── .gitignore              
├── package-lock.json       
├── package.json              
└── Readme.md               # Project's Readme
```

Therefore, anytime you create a react project, you must choose the file structure you should use so that you never have empty folders or add complexities. When creating a simple To Do List, for instance, you don't necessarily need an advanced file structure instead, you can use beginner file structure to ensure that you never run into empty folders and added complexity and intermediate file structure for slightly larger apps for the same reasons. However, when creating an enterprise level app, advanced file structures can be taken into consideration, beginner and intermediate ones are not helpful in that situation.   