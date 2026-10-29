
# MODS

Mods and changes I want to try in *How to Pronounce Iwan*. These are the methods and values that look useful so far. I still need to test the changes in-game to make sure they work as expected.

## GUN (Player)
**What I want:** Make the gun more accurate, shoot faster, and have unlimited ammo and very high damage.

**Where to look:** `Gun.Reload()`, `Gun.Update()`, and the fields `Accuracy`, `RateOfSpeed`, `inventoryAmmo`, `curAmmo`, `maxAmmo`, and `damage`.

Changing `inventoryAmmo` might give the gun unlimited ammo. `Update()` also checks for the reload input, so I may need to change that check too. This code is the reload check and what happens when it runs; it does not give unlimited ammo by itself.

```csharp
if (Input.GetButtonDown("Reload") & (inventoryAmmo > 0) & (curAmmo != maxAmmo))
{
	camera1.GetComponent<Camera>().fieldOfView = 60f;
	aim = false;
	GetComponent<AudioSource>().PlayOneShot(Reloaded, 0.7f);
	svumode = 4;
	camera1.GetComponent<MouseLook>().sensitivityX = 10f;
	camera1.GetComponent<MouseLook>().sensitivityY = 10f;
	GameObject.FindWithTag("Player").GetComponent<MouseLook>().sensitivityX = 15f;
}
```

The defaults shown are `Accuracy = 0.01f` and `RateOfSpeed = 0.5f`. `damage` is an integer. I need to check the shooting code to see exactly how these values affect the gun before changing them.

## HEALTH (Player)

**What I want:** Stop explosion and fire damage.

**Where to look:** `Health.Del()` and `Health.OnTriggerStay(Collider)`.

These methods lower the player's health when they take explosion or fire damage. Removing or changing those lines should stop those types of damage. Other methods may still hurt the player.

```csharp
public void Del()
{
	player_health -= explosition_damage;
}

private void OnTriggerStay(Collider Col)
{
	if (Col.tag == "Fire")
	{
		player_health -= fire_damage;
	}
}
```

## ROBOT_GUN (PvE)

**What I want:** Make the enemy gun shoot slower and less accurately, and change its sound.

**Where to look:** `Robot_Gun.Accuracy`, `Robot_Gun.RateOfSpeed`, and the `AudioSource` field called `source`.

The class has fields for accuracy, firing speed, and audio. I need to find where the gun fires and plays its sound to see which values to change. This is only part of the class:

```csharp
public class Robot_Gun : MonoBehaviour
{
	private RaycastHit Hit;
	public float Accuracy = 0.01f;
	public Health sd;
	public Transform spawn;
	public float RateOfSpeed = 0.5f;
	private float _rateofSpeed;
	private bool Shoot;
	public Transform[] _MuzzleFlash;
	public Light[] _Light;
	private float timeout = 0.2f;
	public Robot_Move robot_move;
	private AudioSource source;
}
```