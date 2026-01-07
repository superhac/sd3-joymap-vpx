Helps figure out your joystick mappings for use with vpinball.  Provides the VPX logical mappings.

<img width="435" height="649" alt="image" src="https://github.com/user-attachments/assets/2b6f4228-99bc-4763-82eb-38e2dc95b9de" />

From VPinballX.ini:
```
Device.SDLJoy_030081b85e0400008e02000014010000.Name = Xbox 360 Controller #028e
Device.SDLJoy_030081b85e0400008e02000014010000.Element0 = 512;A;Left Stick X
Device.SDLJoy_030081b85e0400008e02000014010000.Element1 = 513;A;Left Stick Y
Device.SDLJoy_030081b85e0400008e02000014010000.Element2 = 514;A;Left Trigger
Device.SDLJoy_030081b85e0400008e02000014010000.Element3 = 515;A;Right Stick X
Device.SDLJoy_030081b85e0400008e02000014010000.Element4 = 516;A;Right Stick Y
Device.SDLJoy_030081b85e0400008e02000014010000.Element5 = 517;A;Right Trigger
Device.SDLJoy_030081b85e0400008e02000014010000.Element6 = 0;B;A
Device.SDLJoy_030081b85e0400008e02000014010000.Element7 = 1;B;B
Device.SDLJoy_030081b85e0400008e02000014010000.Element8 = 2;B;X
Device.SDLJoy_030081b85e0400008e02000014010000.Element9 = 3;B;Y
Device.SDLJoy_030081b85e0400008e02000014010000.Element10 = 6;B;Back
Device.SDLJoy_030081b85e0400008e02000014010000.Element11 = 8;B;Guide
Device.SDLJoy_030081b85e0400008e02000014010000.Element12 = 7;B;Start
Device.SDLJoy_030081b85e0400008e02000014010000.Element13 = 9;B;Left Stick
Device.SDLJoy_030081b85e0400008e02000014010000.Element14 = 10;B;Right Stick
Device.SDLJoy_030081b85e0400008e02000014010000.Element15 = 4;B;Left Shoulder
Device.SDLJoy_030081b85e0400008e02000014010000.Element16 = 5;B;Right Shoulder
Device.SDLJoy_030081b85e0400008e02000014010000.Element17 = 258;B;DPad Up
Device.SDLJoy_030081b85e0400008e02000014010000.Element18 = 259;B;DPad Down
Device.SDLJoy_030081b85e0400008e02000014010000.Element19 = 256;B;DPad Left
Device.SDLJoy_030081b85e0400008e02000014010000.Element20 = 257;B;DPad Right
```

Compile:
```
sudo apt install libsdl3-dev libncurses-dev libncursesw5-dev
gcc sdl3-joymap-vpx.c -o sdl3-joymap-vpx `pkg-config --cflags --libs sdl3 ncursesw`
./sdl3-joymap-vpx
```
