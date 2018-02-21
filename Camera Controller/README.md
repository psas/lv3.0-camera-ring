# The LV3 Camera Ring Controller Board

## Understanding Requirements

- Numbers of cameras
   - We have hexagonal symmetry in the rocket, and never enough cameras evar, so why not 7 cameras?
- Arrangement and Field of view (FOV)
   - 3 look up, to capture the parachutes.
   - 3 look down, to capture the launch and fins.
   - Each camera needs > 120 degree horizontal FOV.
   - Each camera should have > 90 degree vertical FOV 
- Runtime
   - No controller means it would have to be on for several hours.
   - With a controller, we can keep it down to 10 minutes pre-launch + 1 minute flight + 10 minutes down = 20 mintues, round up to 1 hour just because.

## Proposed Camera and arrangement

- [RunCam Split 2](https://shop.runcam.com/runcam-split-2/).
   - 5 - 17V input
   - UART command input
   - Analog A/V out (not that we care, but?)
   - Boo: no "Recording/Low power off" GPIO input
   - 165° FOV, which translates to H = ~162° and V = ~150° according to the [The VRguy's Blog](http://vrguy.blogspot.com/2013/04/converting-diagonal-field-of-view-and.html)
   - 1080 (1920x1080) @ 60fps
   - Straight to microSD card, which is nice.
   - It has a buzzer? Why? Might be nice if we can trigger it for video sync.
- Camera arrangement
   - 3 cameras 120° apart pointed up at 45°.
   - 3 cameras 120° apart pointed down at 45°.
   - Probably the 45° is dumb, it should be more like 75 degrees, but whatever, it'll be nice to see so much of the weave of the carbon fiber.

## Power draw and Battery Pack.

- Each camera takes 5V @ 650mA (3.25W) or 12V @ 270mA (3.24W) so at a nominal voltage of 14.8V gives a current of 220 mA per camera.
- 6 * 220 mA = 1.32 amps (!) * 1 hour = 1.32 Ahr and then 1.5x to save the batteries (save the batteries!) so that gives us ~ 2 Ahr.
- We find: [Tattu 1800mAh 4S1P LiPo Battery Pack](https://www.getfpv.com/tattu-1800mah-4s-75c-lipo-battery.html) which is 200 g and 110 x 34 x 25 mm.
- This is the second time we've found this battery, so, yay, we must loves it.

## Camera Communication

- Oh look, they have an [RunCam API](https://support.runcam.com/hc/en-us/articles/115011786628-RunCam-Split-Communication-protocol), kind of.
- 115200 8N1. Sure.
- `0x55 0x01 0x02 0xf5 0xaa` is the command we send to turn on/off recording.
- There's no other commands? Really? Lame. There's some kind of On-Screen Display (OSD)... can we control that?

- Hackerating:
   - There are three pushbuttons! We can hackarate them! TODO! FIXME! TBD!
   
## Behavior

- Look for shore power.
   - Turn on recording when shore power goes off.
   - Turn off recording if shore power comes back on.
- We should also have some ability to to manually turn it on and off. 

