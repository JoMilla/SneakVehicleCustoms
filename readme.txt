http://imgur.com/a/uQfhO

I expect everyone to know how to handle pbo's and getting scripts on your server so i wont explain this.

Installation:

-Database
	-import the _sql\sneak_customs.sql into your database
	-add the content of _sql\sneak_exile.ini at the end of your exile.ini (@ExileServer\extDB\sql_custom_v2\exile.ini)

-Serverside
	-compile as pbo and add to your server in: @ExileServer\addons

-mpmissions
	-add the mpmissions folder to your server
	-open the config.cpp and add 

ExileClient_gui_vehicleCustomsDialog_event_onPurchaseButtonClick = "overrides\ExileClient_gui_vehicleCustomsDialog_event_onPurchaseButtonClick.sqf";

		ExileClient_gui_vehicleCustomsDialog_event_onSkinListBoxSelectionChanged = "overrides\ExileClient_gui_vehicleCustomsDialog_event_onSkinListBoxSelectionChanged.sqf";

		ExileClient_gui_vehicleCustomsDialog_event_onVehicleDropDownSelectionChanged = "overrides\ExileClient_gui_vehicleCustomsDialog_event_onVehicleDropDownSelectionChanged.sqf";

		ExileClient_gui_vehicleCustomsDialog_show = "overrides\ExileClient_gui_vehicleCustomsDialog_show.sqf";

		ExileClient_gui_vehicleCustomsDialog_updateVehicle = "overrides\ExileClient_gui_vehicleCustomsDialog_updateVehicle.sqf";

	in class CfgCustomCode
	note: if you want to open the menu with a different key, this is the place to go.
	also add #include "addons\sneak_customs\SneakCustoms_config.cpp" at the top of your config.cpp

	-add 
		class SneakCustoms_change_skin {allowedTargets = 2;};
	to CfgRemoteExec -> CfgFunctions
	(description.ext)

-Battleye
	-im still working this out you can try everything when battleye is off, pls contact me
	if you have expierince with Battleye! (sneakcustoms@gmail.com)

Questions, bugreports or suggestions to:
sneakcustoms@gmail.com

How to use:
Just go to the standard vehicle customs trader and have fun!

How this works:
Everytime you change a skin the script will look up if its an custom skin or an exile standard.
When its standard it will just update the texture and the skinclass in the database.
When you apply a new custom skin it will add an entry to the sneak_customs database.
On server start the fn_Sneak_postInit.sqf will compare every spawned vehicle with my custom
database and update the skin to make custom skins persistent. When you revert back to a standard skin the entry in the custom databse will be deleted and value is stored in the standard database.


How to add CustomSkins:
only add additional Skins to the list or there will be duplicates!

Add the Base class of the Vehicle you want to add into the SneakCustoms_config.cpp into CfgSneakCustoms then add an array that is called "skins" which contains subarrays with ClassName for your skin and the price. (exactly like the vehicle customs entries in the standard config!)

After that create the Skinclass into CfgSneakCustoms like: 
class BaseClassName : SneakCustoms{
	skinName = "Name shown in the Vehicle customs list";
	hiddenSelectionsTextures[] = {"Texture01Path","Texture02Path"...};
};

When you want to add custom textures in your mission file the path needs to be like this:
"mpmissions\__cur_mp.MAPNAME\..."
Example:
We want to add a custom skin for the SUV:

class Exile_Car_SUV_Abstract{
	skins[] = {
		{"MySUVSkin", 500},
		{"MySUVSkin2", 500}
};

class MySUVkSkin : SneakCustoms{
	skinName = "My SUV 01";
	hiddenselectionTextures[] = {"mpmissions\__cur_mp.altis\textures\MySuv01.jpg"};
};
class MySUVkSkin2 : SneakCustoms{
	skinName = "My SUV 02";
	hiddenselectionTextures[] = {"mpmissions\__cur_mp.altis\textures\MySuv02.jpg"};
};


You can use *.paa or *.jpg as texture files.
When you add more than 1 or 2 better go for the jpg because *.paa files are huge.

NEVER start the path with a "\"!