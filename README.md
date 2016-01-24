# EAGLE-bom-script
This is an User-Language-Program (ULP) script intended for the use with the layout-program EAGLE.

## Installation ##
Either copy this scipt to <EAGLE-installation-dir>/ulp, or add the path of the script to the ULP path (you can find this option in the EALGE main window -> Options -> Directories. Separate the Pathes in Windows by ';' and in Linux by ':')

## Usage ##
EAGLE runs this ULP, when you type 'run bom_TURAG' in the Schematic.
You may set up a custom shortcut in EAGLE for this. (Optionen -> Tastenbelegung)

The ULP is based of the attribute of the parts in the Schematic. It will search for order codes in the ordercode-directory (see the option --ocpath) and fill the found order codes to the part attributes !!CAUTION!! Any existing attributes will be overwritten! If you do not want this behaviour either use an empty ordercode-directory or the KEEP_AS_IS-Attibute! The BOM is then created from the attributes. 

Options:
--ocpath <path> Here you may provide an absolute path to a directory containing your ordercode-files. Without this option, this ULP will search in the order_codes subdirectory of the ULP location.
--blpath <path> Here you may provide an absolute path to your Blacklist file. The ULP will show an error, if there is any part with a blackistet order code.
Without this option, this ULP will use the 'blacklist.txt' file located in the same folder as the ULP-file.

## Ordercode-files ##
You may provide a directory with *.txt-files containing your order codes. Feel free to add as much files to the directory as you want to, but keep in mind, that the ULP will not search in supdirectories.

Each line contains one ordercode. Use the following format:
device;package;value(optional);distributor>order-code;distributor>order-code; ...
The device, package, and value fields are compared the the properties from your schematic as shown in the properties-dialog of the part. The device-properties of some parts contain values in brackets. The ULP only compartes the piece before the brackets.
If you do not provide any value-property, this will act like a wildcard and overwrite any other ordercode for the part with matching device and package options.
Lines starting with an # will be ignored.

Make sure, you provide the ordercode for a part (device/package/value) only once - otherwise the behaviour of the ULP is undefined.

## Syntax ##
Generally you may use any string as an order code. The following characters are reserved by the ULP:
: - is used as a separator fo provide more than one ordercode for a part. (e.g. for connectors, when you want to order both parts, and not only the one soldered on your pcb)
* - is used to order a product more than once. Please provide the required amount after the character.
; - is used to as a separator in the ordercode-files to seperate the entries.
> - is used as separator in the ordercode-files to seperate ordercode-string from provider-string. 

To control special behaviours for a part, there are following reserved attributes (it does only matter, whether they exist or not - the value is not checked by the ULP):
NOORDER - do not order this part. (maybe you want to sample it, or it is a SMD-Jumper, ...)
PROTOTYPE - do order this part twice. (use it for critical parts. You will get a warning, if you use this attribute)
KEEP_AS_IS - the  ULP will not update any order code for this part. (for example use it, if you need THIS special part, that must not be overwritten by the ULP in the future)

# Syntax-examples #
Attribute name | Attribute value | description
---------------+-----------------+-------------------
Distributor 1  | 123456          | The part '123456' will be ordered at 'Distributor 1'
Shop2          | AAA:BBB         | Parts 'AAA' and 'BBB' will be ordered at 'Shop2'
Buy Here       | partpart*4      | Part 'partpart' is ordered 4 times at 'Buy Here'
Buy There      | aa:bb*3:cc*2    | Order: once 'aa', three timed 'bb', twice 'cc' at the Shop 'Buy There'

## Shops/distributors ##
At the beginnin of the ULP is a string array called 'PROVIDER_NAMES'. The ULP will search for all attributes, whose names math to one on the entries in this array. You may give more than one provider there (why else, should it be an array ;-) ), but please keep in mind, that the ULP will output every ordercode it finds. So if you give more than one provider per part, you will get multiple ordercodes in your BOM.
