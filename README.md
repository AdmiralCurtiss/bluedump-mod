# Yet Another BlueDump MOD (YABDM)
YABDM is a modification of BlueDump capable of dumping any type of title installed in your Wii console to a WAD file in a SD Card or an USB storage device. It can also backup and restore savegames in their unencrypted form, and convert content.bin files generated by the System Menu on the SD card to WAD files.

This version was heavily modified to fix bugs and add some corrections that make the WAD dumping process more accurate; that means, the resulting files now match with the WADs obtained with NUS Downloader and other PC programs. Thus, YABDM can actually recreate the original WAD used to install certain title.

Main features
--------------

* Compatibility with FAT32-formatted SD cards and USB storage devices.
* Compatibility with WiiMote, Classic Controller (Pro), GameCube Controller and Wii U Pro Controller (through libwupc).
* Enhanced WAD dumping capabilities from BlueDump that include a faster encryption/decryption process (through the use of a different AES implementation), SHA-1 verification and appropiate 16 and 64 byte padding.
* Ability to convert content.bin files stored on SD:/private/wii/title straight to WAD files.
	- They must have been generated by the Wii the application is being run on in order to work.
	- Otherwise, make sure to have a copy of the BootMii dump data (either keys.bin or nand.bin) from the original console on the root directory of the output device, and a copy of the Ticket file on /YABDM/Tickets.
* Ability to fakesign the Ticket and/or the TMD of the output WAD.
* Region-changing options that allow to modify the region of the output WAD.
* Ability to apply IOS patches at runtime if full hardware access is granted (HW_AHBPROT register disabled), through libruntimeiospatch.
* IOS selection menu which allows you to manually reload to a different IOS, automatically skipping critical system titles and stub IOSes.
	- If full hardware access is enabled, the new IOS will inherit these access rights.
	- Also, if an Hermes cIOS is loaded, the additional ehcmodule
* Compatibility with USB 2.0 devices when using a cIOS (max. 10 seconds timeout).
* Ability to read application arguments passed from the loader (0 = disabled / 1 = enabled).
	- debug: Enables the debug mode, which creates a logfile (called YABDM.log) with technical information about the application's performance.
	- wiilight: Activates the Wii disc light as a read/write indicator for the output device.
* Updater that allows you to download a new version of the application straight from the Google Code server (if available).
* Ability to generate a proper System Menu WAD even if Priiloader is already installed.
* Change between hexadecimal and ASCII Title IDs while browsing the title list.
* Improved internal name reading capabilities that are compatible not only with regular content files and savegames, but also with DLCs and content.bin files. The read names will be displayed in the language set on the Wii Options Menu (whenever possible).
* Custom 512-char font that is capable of displaying special latin characters.
* Ability to dump and rearrange DLCs with missing contents.

Usage
--------------

* **Savedata:** When you load YABDM, you'll see the main menu with different categories available. If you want to backup the savegame from a disc-based game (assuming you own it and have played it at least once), set the arrow and press A on "00010000 - Disc Savedata". Then, set the arrow on the game and press 1. An options menu will open; from there you can use the right and left arrow to switch between the available functions. Select Backup Savedata and press A. Voilà, now you have a backup of your savedata for that game in the /YABDM/Savedata directory on the output device.

	- The same things applies if you want, for example, to backup the savedata for a VC title: just go to "00010001 - Installed Channel Titles". The extracted savedata can be installed on any Wii.

* **Titles:** The procedure is the same as the one used for dumping savegames, except that you'll have to choose "Backup to WAD" on the last options menu. You'll then be prompted to select if you want to fakesign the Ticket and/or TMD of the output WAD, or to change its region. Wait for the process to end and you'll have the WAD on the /YABDM/WAD directory on the output device.

* **content.bin files:** Assuming you have inserted an SD card with content.bin files, enter the content.bin conversion mode by pressing +/R, and select the title you want to convert. The rest of the process is pretty much the same as the WAD dumping procedure.

Thanks to
--------------

* WiiPower, for helping out nicksasa.
* TriiForce and FSToolBox projects (WiiPower and nicksasa).
* JoostinOnline, for his continuous help and ideas.
* The folks at HacksDen.com, for testing every new release. Special mention to stomp_442.
* Vincent Rijmen, Antoon Bosselaers and Paulo Barreto, for their "rijndael-alg-fast" AES implementation.
* FIX94, for his libwupc library, used in the application to add support with Wii U Pro Controllers.
* Excelsiior, for his libruntimeiospatch library, used to apply IOS patches at runtime.
* Dimok, Rodries, Kwiirk and svpe, for their work on the USB storage driver.
* Hermes, for his mload implementation for PPC.

Changelog
--------------

**YABDM v1.8 (December 9th, 2014 - DarkMatterCore):**

* Fixed a bug where garbled text was being displayed for titles that use 0xFF instead of 0x00 as the preceding byte for each character in their name field (fixes MEGA MAN 9 DLC, possibly others).
* If a given title only has whitespaces on its description field, the application will no longer display them (fixes Dead Space Extraction, possibly others).
* Added compatibility with custom USB 2.0 modules from various cIOS, using a modified version of the usb2storage.c driver from WiiXplorer.
* The USB mounting process now has a 10 seconds timeout, instead of using a retry system.
* The ehcmodule will be loaded into Starlet using mload if the loaded IOS is an Hermes cIOS.
* IOS reload option added to the Settings Menu.
* If the hardware protection is disabled, the current IOS is patched before an IOS reload is performed to keep the access rights.
* Changed the default IOS selection menu value to IOS249.
* License upgraded to GNU-GPL v3.

**YABDM v1.7 (November 28th, 2014 - DarkMatterCore):**

* The application's code flow was modified to remove the overuse of the exit function and to deal with unhandled memory exceptions. Thanks to this, the goodbye() function has now been removed, and only a single call to Reboot() is performed.
* An additional settings menu has been implemented. It can be entered by pressing button -/L. There, you can either choose to switch storage devices through the Device Menu, or to update the application.
* It is now verified if the console keys collected from the BootMii data are null (zero-filled) or belong to the console the application is being run on before using them.
* Added compatibility with Priiloader. Thanks to this, the application can now generate valid System Menu WADs even if Priiloader has been installed.
* LoadTikFromDevice() is now merged with GetTicket() (it should have been like this from the beginning).
* The IOS selection menu now also skips BootMii IOS and stub IOSes.

**YABDM v1.6 (November 16th, 2014 - DarkMatterCore):**

* Added support for vWii System Menu versions 608, 609 and 610.
* Added support for mauifrog's System Menu 4.1 modified versions.
* Updated the internal name list to add support for vWii hidden channels.
* The update procedure now checks if you already have the latest YABDM version.
* The logfile now shows if the application is running under vWii.
* The reset_log() function was removed. Now, a line formed by dashes is used to divide each subsequent logging session from another.
* The application is now able to use the BootMii dump data from another Wii in the content.bin conversion process. It'll only look for the files if the content.bin's Console ID doesn't match the one from the console the application is being run on. Please keep in mind that you'll also need to have a copy of the Ticket file on your NAND, or either manually place it under /YABDM/Tickets/ in the selected output device, using the following naming scheme: TITLE_HIGH-TITLE_LOW.tik (e.g. sd:/YABDM/Tickets/00010001-57475345.tik). If you choose not to fakesign the output WAD, it'll retain the signature from its original console.
* Some other minor fixes and corrections.

**YABDM v1.5 (October 13th, 2014 - DarkMatterCore):**

* Fixed the region change procedure for DLC titles. Their Title IDs will now reflect the selected new region (in order to make them detectable by their corresponding game/channel).
* The region change prompt will now always be displayed. Keep in mind that if you enable this option, the Ticket and/or TMD will be fakesigned automatically, depending on the category of the selected title (even if you choose not to). 
* If the application detects that the Common Key Index is different than 0, it will automatically fakesign the Ticket (even if you choose not to), instead of only changing its value.
* Reimplemented the Wii disc light feature as an optional read/write indicator. You can enable it through the use of the "wiilight" argument in the meta.xml file.
* Reimplemented the ability to update the application straight from the Google Code SVN repository, by pressing buttons A and B at the same time. The meta.xml file will also get updated.

**YABDM v1.41 (September 21st, 2014 - DarkMatterCore):**

* Quick fix for the SHA-1 calculation during the content.bin conversion process.

**YABDM v1.4 (September 21st, 2014 - DarkMatterCore):**

* Removed the use of strcpy/strncpy to avoid possible buffer overflows.
* Now, argv[0] is also checked when looking for the "debug" argument, since it appears it gets passed here if Wiiload is launched from a batch script.
* Added BC-NAND (1-200) and BC-WFS (1-201) vWii titles to the internal name list, previously listed as IOS 512 and 513, respectively. Thanks to larsenv for noticing this.
* SHA-1 hash verification is now performed during the dumping process for every regular content file.
* Improved the logfile code. Now, the debug file is only opened/closed when absolutely necessary; each consequent debug string is flushed to the file while it is kept open, which greatly reduces the time needed to write info to the output device. Strings longer than 256 characters are now also allowed. Thanks again to JoostinOnline!
* Added compatibility with Wii U Pro Controllers. Thanks to FIX94 for his libwupc library! Also thanks to my good friend Dimitry Valencia for testing.

**YABDM v1.3 (June 25th, 2014 - DarkMatterCore):**

* Fixed the directory sorting function by doing a string comparison without case sensivity, using stricmp(). I don't know how didn't I notice that before.
* Fixed a bug where the content.bin conversion mode would not be entered, with no error message displayed.
* Directory entries without a content.bin file won't be shown anymore.

**YABDM v1.2 (May 16th, 2014 - DarkMatterCore):**

* Updated all the debug code to use the Carriage Return + Line Feed combo, in order to make the logfile readable under Windows.
* Unpadded Ticket size is now always set to 0x2A4 bytes. Some PC applications aren't even compatible with bigger Ticket sizes.
* If a DLC content (type 0x4001) for the selected title is not present, it is now skipped and its info struct is removed from the TMD (which is also fakesigned in the process). The output file is also rearranged when all the available contents have been dumped. Thanks to szczuru from GBAtemp for doing **a lot** of testing.

**YABDM v1.1 (May 5th, 2014 - DarkMatterCore):**

* Fixed the left/right buttons behaviour in the content.bin conversion menu (oops).
* An output device selection menu will now be displayed after pressing -/L, instead of just remounting the storage devices. Device swapping is also allowed, but only in the main menu.
* Compiled with the latest devkitPPC, libOGC (with custom font) and libFAT.

**YABDM v1.0 (March 25th, 2014 - DarkMatterCore):**

* Eliminated the need for realloc() in pad_data(), since allocate_memory() generates buffers with sizes rounded to a 64-bite boundary; this is most likely safer. The function is now used only to write zeros.
* The IOS buffered list is now freed before reloading to a new IOS.
* Fixed a bug in the directory navigation code where garbled text would be printed if there were no items in the selected directory (after pressing A).
* Fixed left/right buttons functionality for lists with equal to or less than 4 elements.
* The region change prompt now won't be displayed if a system title was selected.
* Button -/L can now be used to remount the storage devices.
* Some minor changes to the AES code.

**YABDM v0.93 (March 1st, 2014 - DarkMatterCore):**

* The RemoveIllegalCharacters() function now also replaces non-ASCII characters with an underscore (_), since libFAT apparently has problems opening/writing files with names that include them. Thanks to erickmaniche from Emudesc for letting me know.
* The getdir_info() and getdir_device() functions will now end early if no files are detected when reading the directory the first time. This fixes some small problems related to save dumping/restoring.
* Logfile output cleanup (mostly, missing newlines).
* General code cleanup. Fixed some small things I previously overlooked.
* Exit sequence cleanup: instead of calling Unmount_Devices() and Reboot() all over again, now just goodbye() is used.

**YABDM v0.92 (February 27th, 2014 - DarkMatterCore):**

* Reimplemented the DetectInput() function, now fixed by JoostinOnline.
* Rewrote the GetASCII() function to discard non-valid ASCII characters.
* Button A can no longer be used in the content.bin conversion mode if the selected directory doesn't have a content.bin file inside.

**YABDM v0.91 (February 26th, 2014 - DarkMatterCore):**

* Fixed a very silly regression bug in the name reading code.
* Fixed GCN Controller compatibility. For some odd reason, the controller input acts erratically using the DetectInput() function, so for now I'm just reverting back to the old waitforbuttonpress(). I swear I received my controller just a moment ago; otherwise, I wouldn't have noticed that.

**YABDM v0.9 (February 26th, 2014 - DarkMatterCore):**

* Rewrote the title name reading functions from scratch. The application can now read multilingual names and descriptions from contents stored on the NAND filesystem and content.bin files using the IMET and WIBN structs from Wiibrew, and the __convertWiiString() function from AnyTitle Deleter by bushing.
* Improvements to the save dumping/flashing code.
* Minor cleanup to the WAD dumping code. The application will now also check if the Ticket and/or TMD data is already fakesigned before trying to wipe the signature.
* If you choose to fakesign the TMD, the application will now display a prompt asking if you want to change the WAD region. This is done by modifying the value @ 0x19D in the TMD, just like FreeTheWads on PC.
* A "(vWii)" suffix will be displayed along with the System Menu version if the application is running under vWii (thanks to JoostinOnline and crediar for their IsWiiU() check).
* Updated libruntimeiospatch to v1.5.1.
* Updated application headline and meta.xml credits to reference HacksDen.com.

**YABDM v0.8 (September 24th, 2013 - DarkMatterCore):**

* Added support for title 00010000-HAZA. It is the responsible for controlling the Photo Channel "swap" between versions 1.0 and 1.1. When it is present, v1.1 is displayed in the System Menu; otherwise, v1.0 is used. Since it gets installed to the savedata directory, it was not being treated as a title, thus its dumping was not allowed; now this behaviour is fixed. Thanks to Iwatasan for noticing this!
* Compiled with the latest libogc git revision from 09/04/2013.
* Now using the latest libruntimeiospatch revision. The previous version was not properly patching the hash_old[] pattern because it had the same value as new_hash_old[]. I informed Excelsiior about this and the change has now been made to the GitHub repository.

**YABDM v0.7 (September 15th, 2013 - DarkMatterCore):**

* Changed application name to YABDM (Yet Another BlueDump MOD).
* Added a proper fix for DetectInput(): now calling VIDEO_WaitVSync() instead of using a while loop (thanks to JoostinOnline!).
* Changed the AES / Rijndael implementation from Mike Scott in favor of a faster one by Vincent Rijmen, Antoon Bosselaers and Paulo Barreto (slightly modified by Jouni Malinen), known as "rijndael-alg-fst". This one uses previously created AES tables instead of generating them everytime an encryption / decryption operation is done, and it also allows the use of a single memory block to be modified 'in-place'. Thus, the dumping process is now faster.
* The OTP memory is now mounted once instead of accessing its data everytime Content_bin_Dump() is called in the application.
* The application now generates a name list for all the entries in a directory everytime the current path is changed. This is mainly done to reduce loading times, and to avoid reading ALL the names again when a D-Pad button is pressed. Because of this, the names for the content.bin files saved in the SD card are now properly displayed on screen; you'll only have to wait a bit after pressing +/R.
* Thanks to the previous point, I noticed a memory leak in getdir_info(), because the *ent pointer was used to allocate memory with malloc() regardless if it already pointed to a memory address. It is now freed before reusing it.
* Some other optimizations.

**MOD v0.6 (September 7th, 2013 - DarkMatterCore and JoostinOnline):**

* Added compatibility with content.bin files stored in SD:/private/wii/title. You can switch between normal WAD dumping and content.bin conversion modes with +/R.
* Added code to read the OTP contents and copy them to a previously aligned memory block. This is needed to access both the PRNG Key and the Console ID, which are used in the content.bin conversion process.
* Added compatibility with Classic Controllers (thanks to JoostinOnline!).
* Optimized name reading functions. Instead of allocating a memory block and then returning it (which is unsafe), now a single global character string is used, with its current value being updated by using snprintf() (thanks to JoostinOnline!).
* Added meta.xml argument support to enable or disable the logfile creation without having to recompile the entire application (thanks to JoostinOnline!).
* New sexy, smexy icon (thanks to JoostinOnline!).
* Some other changes and simplifications that reduce the overall CPU use (thanks to JoostinOnline!).

**MOD v0.5 (August 27th, 2013 - DarkMatterCore):**

* read_name_from_banner_app() and read_dlc_name() functions are now merged.
* Fixed a bug in the read_name() function which would return wrong text if the title has contents smaller than the size of the chunk read into memory (e.g. YouTube Channel, it has two 'dummy' contents of 0x40 bytes each).
* Fixed a *possible* buffer overflow in the read_name*() functions, since they were allocating 'length+10' bytes for the name buffer and then the description string was concatenated to it (which is usually longer than 10 bytes). Now, they're just allocating enough space for what they need.
* Fixed left button behaviour.
* More code changes.

**MOD v0.4 (August 26th, 2013 - DarkMatterCore):**

* Ability to read the internal channel descriptions. Not all channels have this, though, but it still is a nice feature.
* Button 2 / X is now used to change the view mode between hexadecimal IDs and ASCII IDs.
* Added padding to 64-byte boundary for the footer data (I noticed that the official WADs from game discs have this, so why not?).
* Mostly, code optimizations.

**MOD v0.3 (August 24th, 2013 - DarkMatterCore):**

* Fixed a very silly bug in the GetContent() function that would improperly dump content files smaller than 16KB using a null IV in the encryption process (I used the Mario Kart Channel to test this behaviour, since it has two contents that match this case).
* The WAD backup section of the dump_menu() function was completely revamped to avoid redundancy (it seriously had a lot of unnecessary code). Plus, the name scheme of the dumped WADs was changed to be the same from CustomizeMii and similar applications.
* Now using conditional operators for most checks on the 'isSD' value.
* Some other little corrections.

**MOD v0.2 (August 15th, 2013 - DarkMatterCore):**

* Now using libruntimeiospatch by Excelsiior to apply patches to the current IOS.
* Fixed illegal FAT characters on dumped savegames. They are now replaced with an underscore (_).
* Added full support for contents bigger than 45MB. The GetContent() function was completely rewritten to dump each regular content file in chunks of 16KB, which are read, encrypted and saved to the output WAD. Because of this, the encrypt_buffer() function is not used anymore in GetContent(), since we need to change the Initialization Vector (IV) for each chunk.
* Other minor fixes and corrections.

**MOD v0.1 (August 1st, 2013 - DarkMatterCore):**

* Full hardware access through the HW_AHBPROT flag (must use HBC v1.1.0 or later!). The IOS patches applied include the "KillAntiSysTitleInstall" set from damysteryman, so the application is also fully compatible with the vWii mode available in Wii U.
* Added an IOS selection screen at startup (you can select whether you want to use the full hardware access, or a patched IOS).
* Full compatibility with USB storage devices and GCN controllers.
* Ability to choose if you want to fakesign the WAD ticket and/or TMD before starting the dumping process (no secondary version needed anymore).
* If you select to fakesign the WAD ticket, the application now wipes the console ID and the ECDH data from it; both of those can prevent a successful WAD installation on another Wii. It is also verified if the Common Key Index value (0x1F1) is set to 0x01 or 0x02; if so, it is changed to 0x00.
* The certificate chain (cert.sys) is now embedded on the code, to avoid reading it every time from the NAND.
* Complete overhaul in the name reading functions for channel banners and banner.bin file from savegames. Plus, the application now can also read names from installed DLCs.
* The WAD footer is now added to every title dumped from the application. Its size is added to the WAD header @ 0x1C, too.
* The content.map file is now read into memory once for every dumping process, instead of accessing it every time the application dumps a shared content.
* Compatibility with content type 0x4001 (which is only present in DLCs, AFAIK).
* Fixed a bug in GetContent() where the application would allocate memory for two different buffers with the same size, which could exceed the console memory limit if the content size was greater than ~22.5 MB. Now, a single memory buffer is used for all the dumping, padding and encrypting operations.
* The pad_data_32() function is now called pad_data(). It uses a bool value to determine if the input buffer will be padded to a 16-byte (0x10) boundary, or a 64-byte boundary (0x40). To avoid memory problems, it now uses realloc() to change the size of the already-allocated input buffer, instead of allocating memory for a new buffer.
* The decrypted memory buffer is now padded to a 16-byte boundary before encrypting. Then, the encrypted memory buffer is padded to a 64-byte boundary. That way, the WAD structure used by NUS Downloader can be easily recreated, with amazing results (just see it by yourself).
* Before reading the internal name of a given title, it is now verified which type of title is it to use the appropiate function for its case. That way, the logfile won't be full of ISFS_Open() -106 errors when trying to read the banner.bin file for everything. lol
* Added a switch case function to properly output the System Menu version in the browser screen (e.g. "v513" is now displayed as "v4.3U").
* Ability to use the LEFT/RIGHT buttons in the browser screen (why didn't nicksasa add that...?).
* Output font replaced with another that has full Unicode support. That way, the title names can be displayed properly if you have set your console in Spanish or a different language.
* Compiled with the latest libogc, devkitPPC and libfat versions (as of August 1st, 2013: v1.8.12, r26 and v1.0.12, respectively).
* Compatibility with the new WiiMote Plus (RVL-CNT-01-TR).

**Alpha 3:**

* Show IOS names (for example, IOS36).
* Show IOS versions (for example, IOS 249 v18).
* Disabled logging by default.
* Code cleanup.
* Creating folders correctly now if they don't exist.
* Some more stuff I forgot.

**Alpha 2:**

* Backup savedata from VC/WiiWare and Disc games.
* Restore savedata from VC/WiiWare and Disc games.
* Backup title's to WAD (correctly signed, will only work on your own wii if you use the normal version!).

**Alpha 1:**

* Never released.
