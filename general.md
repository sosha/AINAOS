How to NEVER hard brick Pure XL:
	Golden Rule: 
		NEVER flash preloader or scatter file with preloader.bin checked in SP Flash Tool.
		In SP Flash Tool you will see:
			preloaderlkbootsystem[x]Uncheck preloader. Always. Leave it unchecked forever. You don't need to touch it for Halium.If you leave preloader alone, you can flash garbage to every other partition and still recover with SP Flash Tool.
			
Unbrick insurance:
	Full ROM readback with SP Flash Tool: Install MTK drivers on your server (Linux: sudo apt install sp-flash-tool)Phone off, hold Vol Down, plug USBIn SP Flash Tool -> Readback tab -> Add -> Start address 0x0 length 0xE9000000 (all 64GB)This gives you pure_xl_stock.bin - your full factory backup. Store it on 2 drives.
	
	   `adb shell dd if=/dev/block/mmcblk0p2 of=/sdcard/nvram.img`
		`adb pull /sdcard/nvram.img`
This has IMEI. If you lose it, phone becomes tablet permanently.Make your recovery bulletproof:
Flash TWRP 2.8.6.0 to recovery partition ONCE and leave it. Test: power off -> Vol Up + Power -> you see TWRP. If you can get to TWRP, you can fix anything except preloader.