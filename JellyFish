

#include <YSI\y_hooks>//แต่ละสคริปอาจจะไม่เหมือนกันตรงนี้ //FB: TU NG

#define     JELLYFISHOBJECT       	1602
#define     JELLYFISHTEXT           	"Objective: {FF0000}แมงกระพุน\n{FFFFFF}กดปุ่ม {FFFF00}N {FFFFFF}เพื่อจับ!"
#define     JELLYFISHNAME            "แมงกระพรุน"
#define     JELLYFISHTIMER           15 // นาทีเกิด

new JELLYFISHTake[MAX_PLAYERS];

enum JELLYFISH_DATA
{
    JELLYFISHID,
    JELLYFISHObject,
    Float: JELLYFISHPosX,
	Float: JELLYFISHPosY,
	Float: JELLYFISHPosZ,
    Text3D: JELLYFISH3D,
    JELLYFISH3DMSG[80],
    JELLYFISHOn
};
new const JELLYFISHData[][JELLYFISH_DATA] =
{
	{ INVALID_STREAMER_ID, JELLYFISHOBJECT, 187.58867, -1919.77808, 0.17128, Text3D: INVALID_3DTEXT_ID, JELLYFISHTEXT, 0 },
	{ INVALID_STREAMER_ID, JELLYFISHOBJECT, 196.76282, -1914.68225, 0.17128, Text3D: INVALID_3DTEXT_ID, JELLYFISHTEXT, 0 },
	{ INVALID_STREAMER_ID, JELLYFISHOBJECT, 190.31592, -1906.52600, 0.17128, Text3D: INVALID_3DTEXT_ID, JELLYFISHTEXT, 0 },
    { INVALID_STREAMER_ID, JELLYFISHOBJECT, 181.14609, -1909.37939, 0.17128, Text3D: INVALID_3DTEXT_ID, JELLYFISHTEXT, 0 }
};

hook OnPlayerConnect(playerid)
{
	JELLYFISHTake[playerid] = 0;
	return 1;
}

hook OnPlayerDisconnect(playerid, reason)
{
	if (JELLYFISHTake[playerid] != 0)
	{
		if (JELLYFISHData[ProgressObject[playerid]][JELLYFISHOn] == 1)
		{
			JELLYFISHData[ProgressObject[playerid]][JELLYFISHOn] = 0;
			StopProgress(playerid);
		}
	}
	return 1;
}

hook OnGameModeInit()
{
	for(new i = 0; i < sizeof(JELLYFISHData); i++)
	{
		JELLYFISHData[i][JELLYFISHID] = CreateDynamicObject(JELLYFISHData[i][JELLYFISHObject], JELLYFISHData[i][JELLYFISHPosX], JELLYFISHData[i][JELLYFISHPosY], JELLYFISHData[i][JELLYFISHPosZ], 0.0, 0.0, 0.0);
		JELLYFISHData[i][JELLYFISH3D] = CreateDynamic3DTextLabel(JELLYFISHData[i][JELLYFISH3DMSG], COLOR_GREEN, JELLYFISHData[i][JELLYFISHPosX], JELLYFISHData[i][JELLYFISHPosY], JELLYFISHData[i][JELLYFISHPosZ] + 1.0, 5.0);
	}
	//CreateDynamicPickup(1239, 23, 2309.6130,-8.6164,26.7422);
	//CreateDynamic3DTextLabel("JELLYFISH:{FFFFFF} กดปุ่ม {FFFF00}N{FFFFFF}\nในการขาย", COLOR_RED, 2309.6130,-8.6164,26.7422, 5.0, INVALID_PLAYER_ID, INVALID_VEHICLE_ID, 0, -1);
}

hook OnPlayerKeyStateChange(playerid, newkeys, oldkeys)
{
	if (newkeys & KEY_NO && !IsPlayerInAnyVehicle(playerid))
	{
        for(new i = 0; i < sizeof JELLYFISHData; i++)
        {
            GetDynamicObjectPos(JELLYFISHData[i][JELLYFISHID], JELLYFISHData[i][JELLYFISHPosX], JELLYFISHData[i][JELLYFISHPosY], JELLYFISHData[i][JELLYFISHPosZ]);
            if(IsPlayerInRangeOfPoint(playerid, 3.0, JELLYFISHData[i][JELLYFISHPosX], JELLYFISHData[i][JELLYFISHPosY], JELLYFISHData[i][JELLYFISHPosZ]))
            {
                if(IsValidDynamicObject(JELLYFISHData[i][JELLYFISHID]) && JELLYFISHData[i][JELLYFISHOn] == 0)
                {
                    if (!Inventory_HasItem(playerid, "เสื้อชูชีพ"))
	                    return SendClientMessage(playerid, COLOR_RED, "[ระบบ] {FFFFFF}คุณไม่มีเสื้อชูชีพ");

                    new ammo = Inventory_Count(playerid, JELLYFISHNAME)+1;

                    if(ammo > playerData[playerid][pItemAmount])
                        return SendClientMessage(playerid, COLOR_RED, "[ระบบ] {FFFFFF}ความจุของ {00FF00}"#JELLYFISHNAME" {FFFFFF}ในกระเป๋าคุณเต็มแล้ว");


                    JELLYFISHData[i][JELLYFISHOn] = 1;
	                ApplyAnimation(playerid, "BOMBER", "BOM_PLANT_LOOP", 4.0, 1, 0, 0, 0, 0, 1);
	                StartProgress(playerid, 100, 0, i);
					JELLYFISHTake[playerid] = 1;
					break;
                }
            }
        }
        /*if (IsPlayerInRangeOfPoint(playerid, 2.5, 2309.6130,-8.6164,26.7422))
        {
            Dialog_Show(playerid, DIALOG_JELLYFISH, DIALOG_STYLE_TABLIST_HEADERS, "[รายการรับซื้อ]", "\
				ชื่อรายการ\tราคา\n\
				"#JELLYFISHNAME"\t{FF0000}$50","ขาย", "ออก");
        }*/
	}
	return 1;
}

/*Dialog:DIALOG_JELLYFISH(playerid, response, listitem, inputtext[])
{
	if (response)
	{
	    switch(listitem)
	    {
	        case 0:
	        {
			    new ammo = Inventory_Count(playerid, JELLYFISHNAME);
			    new price = ammo*50;

			    if (ammo <= 0)
			        return SendClientMessage(playerid, COLOR_RED, "[ระบบ] {FFFFFF}คุณไม่มี"#JELLYFISHNAME"อยู่ในตัวเลย");

		        GivePlayerMoneyEx(playerid, price);
				PlayerPlaySound(playerid, 1083, 0.0, 0.0, 0.0);
		        SendClientMessageEx(playerid, COLOR_GREEN, "[JELLYFISH] {FFFFFF}คุณได้รับเงินจำนวน {FF0000}%s {FFFFFF}จากการขาย"#JELLYFISHNAME" {00FF00}%d {FFFFFF}เหรียญ", FormatMoney(price), ammo);
				Inventory_Remove(playerid, JELLYFISHNAME, ammo);
		    }
		}
	}
	return 1;
}*/

PlayerJELLYFISHUnfreeze(playerid, number)
{
	ClearAnimations(playerid);

	JELLYFISHTake[playerid] = 0;
	JELLYFISHData[number][JELLYFISHOn] = 0;

	new id = Inventory_Add(playerid, JELLYFISHNAME, 1);

	if (id == -1)
	    return SendClientMessageEx(playerid, COLOR_RED, "[ระบบ] {FFFFFF}ความจุของกระเป๋าไม่เพียงพอ (%d/%d)", Inventory_Items(playerid), playerData[playerid][pMaxItem]);

	GivePlayerExp(playerid, 10);
	SendClientMessage(playerid, COLOR_WHITE, "คุณได้รับ {FF0000}"#JELLYFISHNAME" +1");
	return 1;
}

hook OnProgressFinish(playerid, objectid)
{
	if(JELLYFISHTake[playerid] == 1)
		PlayerJELLYFISHUnfreeze(playerid, objectid);

	return Y_HOOKS_CONTINUE_RETURN_0;
}

hook OnProgressUpdate(playerid, progress, objectid)
{
	if(JELLYFISHTake[playerid] == 1)
	{
		ApplyAnimation(playerid, "BOMBER", "BOM_PLANT_LOOP", 4.0, 1, 0, 0, 0, 0, 1);
		return Y_HOOKS_BREAK_RETURN_1;
	}

	return Y_HOOKS_CONTINUE_RETURN_0;
}
