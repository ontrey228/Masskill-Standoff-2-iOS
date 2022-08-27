## Masskill Standoff 2 0.19.4 iOS

<b> Example: </b>
```
#include "Vector3.h"

int (*PlayerController_get_team)(void*);
bool (*PlayerController_get_isLocal)(void*);
Vector3 (*Transform_get_position)(void*);
void (*Transform_set_position)(void*, Vector3);
void *(*Component_get_transform)(void*);

void *me = NULL;
void *enemy = NULL;

void (*old_Player_Update)(void *player); 
void Player_Update(void *player) { 
	if(PlayerController_get_isLocal(player)) {
		me = player;
	}

	if(me != NULL) {
		if(PlayerController_get_team(me) != PlayerController_get_team(player)) {
			enemy = player;
		}

		if([switches isSwitchOn:@"Masskill"]) {
			Vector3 mypos = Transform_get_position(Component_get_transform(me));
			Transform_set_position(Component_get_transform(enemy), Vector3(mypos.X-1, mypos.Y, mypos.Z));
		} 
	} 
    old_Player_Update(player); 
} 
 
void setup() {
 
PlayerController_get_team = (int (*)(void *))getRealOffset(0x1A25E24);
PlayerController_get_isLocal = (bool (*)(void *))getRealOffset(0x1A26264);
Transform_get_position = (Vector3 (*)(void*))getRealOffset(0x270100C);
Transform_set_position = (void (*)(void*, Vector3))getRealOffset(0x27010BC);
Component_get_transform = (void* (*)(void*))getRealOffset(0x26D3288);

HOOK(0x1A25490, Player_Update, old_Player_Update);

[switches addSwitch:@"Masskill" description:@"Teleport enemy to me"];

}

```