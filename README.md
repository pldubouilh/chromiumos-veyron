Chromium OS - Veyron build
=========================

Chromium OS build for my Asus Chromebook Flip. Built on the latest Arch - the overall build took ages, but good news you can use my build (from August 2015) ! ArnoldTheBat update channel might work, but it's untested.

Default password on the build `pwda` (you'd better change this ! )

	# Get image 
	cd ~ && curl -OL https://github.com/pldubouilh/chromiumos-veyron/releases/download/DiskImage/veyron-build.7z
	
	# Extract image
	7z x veyron-build.7z 
	
	dd if=~/R47-7408.0.2015_08_29_1855-a1/chromiumos_image.bin of=/dev/SDyourdrive 
	
	# (Enable dev mode on Chrome OS)
	# Plug drive on the chromebook, restart pressing CTRL+U
	
	# (Optionnal - on chromebook) System install
	/usr/sbin/chromeos-install
