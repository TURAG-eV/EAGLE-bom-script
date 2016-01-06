# EAGLE-bom-script
This is an User-Language-Program (ULP) script intended for the use with the layout-program EAGLE.

## Installation ##
Either copy this scipt to <EAGLE-installation-dir>/ulp, or add the path of the script to the ULP path (you can find this option in the EALGE main window -> Options -> Directories. Separate the Pathes in Windows by ';' and in Linux by ':')

## Usage ##
EAGLE runs this ULP, when you type 'run bom_TURAG' in the Schematic.
You may set up a custom shortcut in EAGLE for this.

Options:
--ocpath <path> Here you may provide an absolute path to your ordercode-files. Without this option, this ULP will search in the order_codes subdirectory of the ULP location.