# cmacc-ui
CommonAccord Code
=================
This library is included in to Common Accord web sites.
It repleaces the original cmacc library.
It is the code that runs the siste.

There are two parts to every CommonAccord site:

1. Documents and configuration code
2. Code that runs the website (this library)
 
New Fetures
-----------
The "Edit and Complete" feature has been revised so a
user friendly series of pages and web forms (with help text) that can be filled out by a user.
These pages and forms replace a textbox or ascii editor of input lines that was difficult to understand.

This improved user esperiance (UI) is generated by a shadow file that contains codeing and HTML.
The shadow files name is the same as the documents name, but ends with `.dot`


Demonstration
-------------
After installing 

1. Create the file `Doc/H4KC/Form/form.md` with the following line in it

    ````
    =[H4KC/Form/Master_DSA.md]
    ````
    
2. Create the file `form.dot` with the following content.  
    Lines with starting with a period can not have leading spaces.

    ````
    .page Q1|Questions
    <P>Mauris massa mauris, vulputate eget mattis sit amet, semper non massa. Donec pretium,
   
    .field City.Name.Full
    .field_label City Name
    .field_description Legal City Name
    .field_place_holder City of Kansas City, Missouri
   
    .field City Department
    .field_label Department
    .field_place_holder Water    
    .field_description Donec pretium, ipsum id elementum finibus, nibh odio molestie ante.

    .field Address of City Department
    .field_label Address
    .field_description Mailing address of the department
    ````
 3. Now when you goto /index.php?action=openedit&file=/H4KC/Form/test.md
    you should see

![form](https://github.com/UMKC-Law/cmacc-ui/blob/master/assets/cmacc-ui-example.jpg)



Installing
==========

To create a development site:

1. Create a local web site that will serve PHP code.  Apache is the most common server for this.  
2. Do not forget to add the site to your `/etc/localhosts` file
3. In the directory of the web site, clone the repository that you will wnat to work with

    ````
    git clone git@github.com:CommonAccord/Site-DataShare.git .
    ````
    
4. Replace `.gitignore` with the following:

````
.idea
.idea/
newlogs
/logs
vendor
vendor/
/vendor/
````
 
5. Remove the current `vendor` folder
6. Update git

````
git add .
git commit -m 'Added vendor to .gitignore'
````

7. Configure composer to include this library instead of the standard library.  
    In `composer.json` change the `url` line in the `repositories` section and the
    `require` section.  The file should contain the following lines:

    ````
    {
        "name": "CommonAccord/CommonAccord",
        "repositories": [
            {
                "type": "vcs",
                "url": "https://github.com/UMKC-Law/cmacc-ui.git"
            }
        ],
        "require": {
            "CommonAccord/cmacc-ui": "dev-master",
            "components/jquery": "^2.1",
            "twbs/bootstrap": "^3.3"
        },
        "scripts": {
            "post-update-cmd": [
                "mkdir -p public/bootstrap",
                "mkdir -p public/jquery",
                "mkdir -p public/js",
                "cp -R vendor/twbs/bootstrap/dist/ public/bootstrap/",
                "cp -R vendor/twbs/bootstrap/docs/assets/ public/bootstrap/",
                "cp -R vendor/components/jquery/ public/jquery/",
                "cp -R vendor/CommonAccord/cmacc-ui/js/ public/js/"
            ]}
    }
~       
    ````
   
8. Remove the file `composer.lock`
9. Regenerate the `vendor` folder by running Composer
    
    ````
    composer update
    ````

    you should see the following lines
    
    ````
    Loading composer repositories with package information
    Updating dependencies (including require-dev)      
      - Installing commonaccord/cmacc-ui (dev-master 4053b4f)
        Cloning 4053b4fce9057f33d61f0233041c46ef9b07ce4a

      - Installing components/jquery (2.1.4)
        Loading from cache

      - Installing twbs/bootstrap (v3.3.5)
        Loading from cache

    Writing lock file
    Generating autoload files
    > mkdir -p public/bootstrap
    > mkdir -p public/jquery
    > mkdir -p public/js
    > cp -R vendor/twbs/bootstrap/dist/ public/bootstrap/
    > cp -R vendor/twbs/bootstrap/docs/assets/ public/bootstrap/
    > cp -R vendor/components/jquery/ public/jquery/
    > cp -R vendor/CommonAccord/cmacc-ui/js/ public/js/
    ````
    
    where xxxxxx is replaced by a random number.
    
10. Verify that `index.php` points to the new `cmacc-ui` library.  
Make sure the `LIB_PATH` line is:
    
    ````
    DEFINE('LIB_PATH', ROOT . '/vendor/CommonAccord/cmacc-ui/library');
    ````
11. Make sure that the following lines are in `index.php`

    ````
    DEFINE('EDIT_FORM_MESSAGE', 'Edit Form');   // Tab


    DEFINE('TEXT_EDIT_WINDOW_SIZE', 'cols=120 rows=30'); 
    DEFINE('TEXT_EDIT_AREA_STYLE', 'font-size: 16px; padding:10px;');  
    DEFINE('LANDING_MD', 'ZZZ/landing.md'); // The repo home
    DEFINE('SOURCE_TAB_MESSAGE', 'Source'); 
    DEFINE('EDIT_TAB_MESSAGE', 'Edit'); 
    DEFINE('COMPLETE_TAB_MESSAGE', 'Edit and Complete');    
    DEFINE('DOC_TAB_MESSAGE', 'Document');  
    DEFINE('PRINT_TAB_MESSAGE', 'Print');
    ````

You should now be able to browse to your new site.


Technologies
------------
* PHP
* Composer
* jQuery
* Bootstrap



