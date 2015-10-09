Chromium OS - Veyron build
=========================

Chromium OS build for my Asus Chromebook Flip. Built on the latest Arch. Overall install takes ages, and require loads of storage. You can use my build (from August 2015), or compile it yourself ! ArnoldTheBat update channel might work, but it's untested !

Default password on the build `pwda` (you'd better change this ! )


# Solution 1 : Use my build

Should save you a bunch of time !
	
	# Clone archive 
	cd ~ && git clone https://github.com/pldubouilh/chromiumos-veyron.git
	
	# Extract image
	7z x veyron-build.7z 
	
	# Build Dir & set python2 as main default python. May be arch only
	mkdir ~/chromiumos
	mkdir ~/bin
	
	ln -s /usr/bin/python2 ~/bin/python  
	ln -s /usr/bin/python2-config ~/bin/python-config
	
	export PATH=~/bin:$PATH     
	export PATH=~/bin:$PATH 
	
	# Git Settings
	git config --global user.email "you@example.com"
	git config --global user.name "Your Name"
	
	# Download Necessary tools & add to path
	git clone https://chromium.googlesource.com/chromium/tools/depot_tools.
	export PATH=`pwd`/depot_tools:$PATH
	
	# Need to get Google's chroot (couple GB)
	cros_sdk # Download & enter chroot
	
	# Flash image on USB drive (will ask you which one)
	cros flash usb:// ~/ChromiumOs-Veyron/R47-7408.0.2015_08_29_1855-a1/
	
	# Plug drive on the chromebook, restart pressing CTRL+E (enable dev mode first)
	
	# (Optionnal - on chromebook) System install
	/usr/sbin/chromeos-install


# Solution 2 : CIY - Compile it yourself
	
	# Build Dir & set python2 as main default python. May be arch only
	mkdir ~/chromiumos
	mkdir ~/bin
	
	ln -s /usr/bin/python2 ~/bin/python  
	ln -s /usr/bin/python2-config ~/bin/python-config
	
	export PATH=~/bin:$PATH     
	export PATH=~/bin:$PATH 
	
	# Git Settings
	git config --global user.email "you@example.com"
	git config --global user.name "Your Name"
	
	# Download Necessary tools & add to path
	git clone https://chromium.googlesource.com/chromium/tools/depot_tools.
	export PATH=`pwd`/depot_tools:$PATH
	
	# Init & sync. Go get a cuppa, need to get 12 GB !
	repo init -u https://chromium.googlesource.com/chromiumos/manifest.git --repo-url https://chromium.googlesource.com/external/repo.git
	repo sync
	
	# Almost there, need to get Google's chroot (couple more GB)
	cros_sdk # Download & enter chroot
	
	# In the chroot : set up board
	export BOARD=veyron
	./setup_board --board=${BOARD}
	
	# Still in the chroot : setup user password
	./set_shared_user_password.sh
	
	# Still in the chroot : edit buildfile to sign that bloody TOS - won't build otherwise !
	nano build_package 
	>> Change DEFINE_string accept_licenses ""
	>> To DEFINE_string accept_licenses "Google-TOS"
	
	# Add external codecs support
	export USE=”chrome_media”
	
	# Still in the chroot : build stuff. Going to take a while (~1h30 on my i7-5650U)
	./build_packages --board=${BOARD}
	
	# Still in the chroot : pack image
	./build_image --board=${BOARD} --noenable_rootfs_verification dev
	
	# Still in the chroot : flash usb (will ask you which one)
	cros flash usb:// ../build/images/veyron/latest/
	
    # Plug drive on the chromebook, restart pressing CTRL+E (enable dev mode first)
	
	# (Optionnal - on chromebook) System install
	/usr/sbin/chromeos-install



