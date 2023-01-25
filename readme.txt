	LMSW: An emulator inside LM
		version 1.10
	     by Alcaro, TheBiob
	(Original software by Alcaro)

This is an emulator directly inside Lunar Magic. It is intended to allow quicker testing of the
 levels by dropping a bunch of time spent savestating on the overworld and between the levels.

Note that it requires Lunar Magic 2.00 or newer. Download the latest Lunar Magic from
 <http://fusoya.eludevisibility.org/> and put the contents of this archive in the same folder as LM.

The relevant buttons are in File->Emulator. Just play around with it for a while, it should be
 obvious enough.

Common problems:
 LMSW is running too slow!
  If your computer can't run the bundled emulator (Snes9x, some in-between version newer than 1.53),
  your computer is most likely too old to handle this tool properly. Just stick to ZSNES, or get rid
  of that old ENIAC.
 LMSW is running choppily!
  Set LM to run the animations at 60 frames per second. LM even says so when you boot up LMSW if the
   setting is wrong.
 My ROM looks crappy and/or doesn't boot and/or throws up "ROM crashed" messages in LMSW!
  LMSW takes plenty of shortcuts to speed up loading the level: It creates synthetic screen exits,
   and skips plenty of game modes. This means that several routines are skipped:
  The ROM initialization routine is executed until game mode ($7E:0100) #$02. Note that the Nintendo
   Presents fadeout timer is set to zero instantly.
  However, when this is done, LMSW instantly jumps into the routine to load level 0C5, instead of
   loading the title screen. LevelASM INIT is allowed to execute here. Note that SRAM is never saved
   when using LMSW, and that level 0C5 is used even if the sprite 19 fix is installed.
  When this is done, a fake screen exit to the relevant level is created and entered. $7E:13BF will
   not be accurate, but the routine that the levelnum patch hijacks will be executed, so you can set
   it there if you know what it should be and if it's important.
  Note that LMSW is incompatible with the Free RAM $7F:8000 patch, due to RAM conflicts.
  For a few compatibility options check lmsw.cfg.
 The message boxes are screwed up!
  Set the correct $13BF through LevelASM INIT. LMSW can't find which overworld level number some
   levels have, so it defaults to garbage on some of them.
 The timer is wrong!
  Tick the "force time limit reset" flag in the timer bypass window. LMSW enters all levels as
   through a screen exit, and the normal bypass flag for that doesn't run in that case.
 I want to detect whether I'm running inside LMSW or an external emulator.
  Check the internal ROM title, $00FFC0. LMSW sets it to "LMSW ENABLED ROM     "; if it's anything
   else (for example "SUPER MARIOWORLD     "), you're in an external emulator.


Notes on different emulation cores:
LMSW uses a libretro to run the game. It will try to load retro.dll which by default is snes9x 1.59.2. If you want a different core replace retro.dll with a different file (either the 1.53 version provided or another libretro core) LMSW will then try to load this file instead of the original. The new core requires given features listed below
Requirements for different cores: 
	SNES core
	RAM Access
	Savestate support
	VRAM Access
	BW-RAM Access*
	I-RAM Access*

* SA-1 only, these are non standard, for cores not provided in this download you will need to compile them with these features yourself. 
   To do this retro_get_memory_data must return a pointer to BW-RAM for a value of 0x2B1D and a pointer to I-RAM for 0x2B1E.
   Other cores without these features will still work for non SA-1 ROMs

Changelog

v1.10 (07/23/2019, TheBiob)
* SA-1 Support
* RAM watches
* LMSW crashes no longer freeze Lunar Magic
* Pausing the game and switching between sprite and block mode no longer crashes
* Bunch of internal changes in an attempt to make it more stable
* Now updates graphics in levels with LM3 level sizes
* Options on when the config is reloaded
* Options to disabled skipping transitions
* retro.dll > snes9x 1.59.2 + sa-1

v1.05, v1.04 (05/16/2013, 03/13/2013, Alcaro)
* initial releases

Credits list:
 FuSoYa - Alpha and beta testing, support in LM, various helpful input. This tool would've been
  much lower quality without his cooperation (though it would still exist).
 WhiteYoshiEgg, imamelia - Beta testing, various helpful input.
 byuu, Themaister, the Snes9X developers - Making linkable libraries out of bsnes and Snes9x,
  removing the need for me to hack up the emulators directly. It's much easier this way.
