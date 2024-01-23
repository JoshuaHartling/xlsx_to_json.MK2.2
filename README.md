# xlsx_to_json.MK2
This project is meant to assist in uploading mass amounts of metavariable data to FortiManager v7.2
or later.  The "XLSX2JSON.exe" executable takes a specifically formatted Excel file and converts
it to a JSON file that FortiManager will recognize for importing Metadata.
Alternatively, the "JSON2XLSX.exe" executable takes a JSON file, presumably downloaded
from FortiManager, and updates/creates an Excel file.

Update the "user_settings.yml" file with settings specific to your use case.  This is the only file
you need to edit.
    
## What's Included
* *example.xlsx* is an example Excel file that shows how to format your metavariable data.
    * The first two columns should be your hostname and vdom assignment respectively.  Leave vdom entry
    blank to use global vdom.
        * You can put columns to the left of hostname/vdom columns for notes, but they will not be picked up
        by the script.
    * The first two rows should be metavariable name and its global value respectively.  Leave global entry
    blank to not use a global value.
* *example_metadata_variables.json* is an example file of the format FortiManager expects metavariable
data to be in.  It was pulled directly from FortiManager v7.2.3.
* *user_settings.yml* contains user configuration parameters.
* *XLSX2JSON.exe* converts your Excel file into the properly formatted JSON.
* *JSON2XLSX.exe* is a script that imports a JSON file (usually downloaded from FortiManager) and
updates your Excel file.  It also creates a backup of the current Excel file before making 
changes to it.
* *example_output.json* is an example of the output of *XLSX2JSON.exe*.

## User Input
There are several parameters that you can set within the *user_settings.yml* file,
though the only parameter you always must set is the file_path.
* **adom:** This is the ADOM on FortiManager that you wish to upload metadata to.  If ADOM's have
not been enabled on FortiManager, leave it as "root" as that is the default ADOM.
  * When running *JSON2XLSX.exe*, this value needs to match the ADOM on FortiManager the metadata is
  downloaded from.  This is a simple check to ensure you're working with the right data.
* **file_path:** This variable is the directory path to your Excel file.
It is the file you import metadata from when running *XLSX2JSON.exe*
and the file you export metadata to when running *JSON2XLSX.exe*.
Typically you'll want to use absolute path of the file.
* **active_sheet:** This is the sheet in your Excel File that holds your metadata.  Set this
if your metavariable data is not on the active (first) sheet of your Excel File.
* **device_list:** is a filter that can be set to only include specific devices.
The filter can be either a string or a list.  Leave it as 'null' to not use it.
* **negate_devices:** is a boolean that when 'true' reverses the filter, turning it into a blacklist
instead of a whitelist.  All devices not in the filter will be added to the .json/.xlsx file.
* **variable_list:** is a filter that can be set to only include specific variables.
The filter can be either a string or a list.  Leave it as 'null' to not use it.
* **negate_variables:** is a boolean that when 'true' reverses the filter, turning it into a blacklist
instead of a whitelist.  All variables not in the filter will be added to the .json/.xlsx file.
* **update_defaults:** when 'true' (default) the global values AKA default values of each variable
 will be included when generating the .json file.  Set to 'False' if you wish to ignore the default values.
* **json_output_file:** the name of the .json file that will be created when *XLSX2JSON.exe*
is run.
* **json_input_file:** *(only applicable to JSON2XLSX.exe)* the name of the JSON file
that will be imported when *JSON2XLSX.exe* is run.
* **add_new_vars:** *(only applicable to JSON2XLSX.exe)* when this boolean is true,
variables in the .json file that are absent from the Excel file will be added to it.
* **add_new_devices:** *(only applicable to JSON2XLSX.exe)* when this boolean is true,
devices in the .json file that are absent from the Excel file will be added to it.

## Upload to FortiManager
To upload the .json file to FortiManger, log into FortiManager and navigate to the appropriate ADOM
(if ADOM's have been enabled).  Navigate to Policy & Objects > Object Configurations > Advanced >
Metadata Variables.  *Note: you may have to enable Metadata Variables using Feature Visibility
under Tools.*

Click on the 'More' dropdown menu, then select 'Import Metadata Variables'.  Either drag and drop
your *output.json* file or click 'Browse' to navigate to it.  Click 'Import'.

**Remember to always review your *output.json* file before uploading it to FortiManager.**
