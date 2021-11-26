<a href="https://tinybiggames.com" target="_blank">![PGT Logo](media/logo.png)</a>

[![Chat on Discord](https://img.shields.io/discord/754884471324672040.svg?logo=discord)](https://discord.gg/tPWjMwK) [![GitHub stars](https://img.shields.io/github/stars/tinyBigGAMES/PiroGameToolkit?style=social)](https://github.com/tinyBigGAMES/PiroGameToolkit/stargazers) [![GitHub Watchers](https://img.shields.io/github/watchers/tinyBigGAMES/PiroGameToolkit?style=social)](https://github.com/tinyBigGAMES/PiroGameToolkit/network/members) [![GitHub forks](https://img.shields.io/github/forks/tinyBigGAMES/PiroGameToolkit?style=social)](https://github.com/tinyBigGAMES/PiroGameToolkit/network/members)
[![Twitter Follow](https://img.shields.io/twitter/follow/tinyBigGAMES?style=social)](https://twitter.com/tinyBigGAMES)

## Overview
Piro Game Toolkit&trade; is a 2D indie game library that allows you to do game development in <a href="https://www.embarcadero.com/products/delphi" target="_blank">Delphi</a> for desktop PC's running Microsoft Windows® and uses OpenGL® for hardware accelerated rendering.

It's robust, designed for easy use and suitable for making all types of 2D games and other graphic simulations, You access the features from a simple and intuitive API, to allow you to rapidly and efficiently develop your projects. There is support for bitmaps, audio samples, streaming music, video playback, loading resources directly from a compressed and encrypted archive, a thin object oriented actor/scene system, entity state machine, sprite management, collision detection and much more. Piro Game Toolkit, easy, fast & fun!

## Downloads
<a href="https://pirogametoolkit.com/download/file.php?name=PGT-Dev.zip" target="_blank">**Development**</a> - This build represents the most recent development state an as such may or may not be as stable as the official release versions. If you like living on the bleeding edge, it's updated frequently (often daily) and will contain bug fixes and new features.

<a href="https://github.com/tinyBigGAMES/PiroGameToolkit/releases" target="_blank">**Releases**</a> - These are the official release versions and deemed to be the most stable.

Visit <a href="https://tinybiggames.com" target="_blank">tinyBigGAMES</a> website for the latest news, updates, downloads and licensing information.

## Features
- **Free** for commercial use. See License agreement.
- Written in **Object Pascal**
- Support Windows 64 bit platform
- Hardware accelerated with **OpenGL**
- You interact with the toolkit via **routines**, **class objects** and a thin **OOP framework**
- **Archive** (custom archive format, password protection, encryption)
- **Display** (OpenGL, anti-aliasing, vsync, viewports, primitives, blending)
- **Input** (keyboard, mouse and joystick)
- **Bitmap** (color key transparency, scaling, rotation, flipped, titled,  BMP, DDS, PCX, TGA, JPEG, PNG)
- **Video** (play, pause, rewind, OGV format)
- **Sprite** (pages, groups, animation, poly-point collision)
- **Entity** (defined from a sprite, position, scale, rotation, collision)
- **Actor** (list, scene, state machine)
- **Audio** (samples, streams, WAV, OGG/Vorbis, FLAC formats)
- **Speech** (multiple voices, play, pause)
- **Font** (true type, scale, rotate)
- **Timing** (time-based, frame elapsed, frame speed)
- **Shaders** (vertex, pixel, GLSL)
- **Misc** (collision, easing, screenshake, screenshot, starfield, colors, INI based config files, startup dialog, treeview menu)

## Minimum System Requirements
- <a href="https://www.embarcadero.com/products/delphi/starter" target="_blank">Delphi Community Edition</a>
- Microsoft Windows 10, 64 bits
- OpenGL 3

## How to use in Delphi
- Unzip the archive to a desired location.
- Add `installdir\libs`, folder to Delphi's library path so the toolkit source files can be found for any project or for a specific project add to its search path.
- See examples in the `installdir\examples` for more information about usage.
- Use `PiroArc` utility for making **.ARC** files (custom archive format, supports encryption and password protection). Running the `makearc.bat` in `installdir\examples\bin` will build `Data.arc` that is used by the examples.
- Build `PiroExamples` to showcase many of the features and capabilities of the toolkit.
- You must include **PGT.dll** in addition to any other of your own dependent files such as any **.ARC** files for example, in your project distributions.
- **NOTE:** For your assurance, all official executables in the PGT distro are code signed by tinyBigGAMES LLC. 

## Known Issues
- This project is in active development so changes will be frequent 
- Documentation is WIP. They will continue to evolve
- More examples will continually be added over time

## A Tour of Piro Game Toolkit
### Game Object
You just have to derive a new class from the `TCustomGame` base class and override a few callback methods. You access the toolkit functionality from the `PiroGameToolkit` unit.
```pascal
uses
  PiroGameToolkit;
  
const
  cArchiveFilename   = 'Data.arc';

  cDisplayTitle      = 'MyGame';
  cDisplayWidth      = 800;
  cDisplayHeight     = 480;
  cDisplayFullscreen = False;

type
  { TMyGame }
  TMyGame = class(TCustomGame)
  protected
    FFont: TFont;
  public
    procedure OnLoad; override;
    procedure OnExit; override;
    procedure OnStartup; override;
    procedure OnShutdown; override;
    procedure OnUpdate(aDeltaTime: Double); override;
    procedure OnClearDisplay; override;
    procedure OnShowDisplay; override;
    procedure OnRender; override;
    procedure OnRenderHUD; override;
  end;
```
### How to use
A minimal implementation example:
```pascal
uses
  System.SysUtils;

{ TMyGame }
procedure TMyGame.OnLoad;
begin
  // open archive file
  Piro.Archive.Open(cArchiveFilename);
end;

procedure TMyGame.OnExit;
begin
  // close archive file
  Piro.Archive.Close(cArchiveFilename);
end;

procedure TMyGame.OnStartup;
begin
  // open display
  Piro.Display.Open(cDisplayWidth, cDisplayHeight,  cDisplayFullscreen, cDisplayTitle);

  // create font
  Piro.Get(IFont, FFont);
  
  // use default mono spaced font
  FFont.Load(16)
end;

procedure TMyGame.OnShutdown;
begin
  // free font
  Piro.Release(FFont);

  // close display
  Piro.Display.Close;
end;

procedure TMyGame.OnUpdate(aDeltaTime: Double);
begin
  // process input
  if Piro.Input.KeyboardPressed(KEY_ESCAPE) then
    Piro.SetTerminate(True);
end;

procedure TMyGame.OnClearDisplay;
begin
  // clear display
  Piro.Display.Clear(BLACK);
end;

procedure TMyGame.OnShowDisplay;
begin
  // show display
  Piro.Display.Show;
end;

procedure TMyGame.OnRender;
begin
  // render any graphics here
end;

procedure TMyGame.OnRenderHUD;
var
  Pos: TVector;
begin
  // assign hud start pos
  Pos.Assign(3, 3, 0);

  // display hud text
  FFont.Print(FFont, Pos.X, Pos.Y, Pos.Z, WHITE, alLeft, 'fps %d', [Pla.GetFrameRate]);
  FFont.Print(FFont, Pos.X, Pos.Y, 0, GREEN, alLeft, 'Esc - Quit', []);
end;
```
To run your game, call
```pascal
PiroRun(TMyGame);
```
**NOTE:** For Piro to work properly, execution MUST start with `PiroRun(...)`. This call will property setup/shutdown the library and log and handle errors. Only one Piro app instance is allowed to run and will safely terminated if more than one is detected.

See the examples for more information on usage.

## Media


## Support
<table>
<tbody>
	<tr>
		<td>Project Discussions</td>
		<td><a href="https://github.com/tinyBigGAMES/PiroGameToolkit/discussions">https://github.com/tinyBigGAMES/PiroGameToolkit/discussions</a></td>
	</tr>
	<tr>
		<td>Project Tracking</td>
		<td><a href="https://github.com/tinyBigGAMES/PiroGameToolkit/projects">https://github.com/tinyBigGAMES/PiroGameToolkit/projects</a></td>
	</tr>	
	<tr>
		<td>Website</td>
		<td><a href="https://tinybiggames.com">https://tinybiggames.com</a></td>
	</tr>
	<tr>
		<td>E-Mail</td>
		<td><a href="mailto:support@tinybiggames.com">support@tinybiggames.com</a></td>
	</tr>
	<tr>
		<td>Discord</td>
		<td><a href="https://discord.gg/tPWjMwK">https://discord.io/tinyBigGAMES</a></td>
	</tr>
	<tr>
		<td>Twitter</td>
		<td><a href="https://twitter.com/tinyBigGAMES">https://twitter.com/tinyBigGAMES</a></td>
	</tr>
	<tr>
		<td>Facebook Page</td>
		<td><a href="https://facebook.com/tinyBigGAMES">https://facebook.com/tinyBigGAMES</a></td>
	</tr>
	<tr>
		<td>Facebook Group</td>
		<td><a href="https://www.facebook.com/groups/pirogametoolkit">https://www.facebook.com/groups/pirogametoolkit</a></td>
	</tr>		
	<tr>
		<td>Vimeo</td>
		<td><a href="https://vimeo.com/tinyBigGAMES">https://vimeo.com/tinyBigGAMES</a></td>
	</tr>
</tbody>
</table>

<p align="center">
 <a href="https://www.embarcadero.com/products/delphi" target="_blank"><img src="media/delphi.png"></a><br/>
 <b>Built with Delphi 11</b>
</p>

