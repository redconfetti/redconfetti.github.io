---
layout: post
title: Mounting an Encrypted Volume on Mac OS X
date: '2014-09-11 21:45:36 -0700'
categories:
- Mac OS X
tags:
- mac-osx
- security
- encrypted volume
- mount
---
<p>Note: The following was performed on Mac OS X (10.8.5 - Mountain Lion)</p>
<p>It is our policy to ensure that any confidential information stored on our machines be stored in an encrypted file storage area on our laptops. Should a laptop become missing, we know that sensitive content stored in that volume will be safe from unauthorized access.</p>
<p>With the deprecation of TrueCrypt, I needed to pursue an alternative method of mounting the encrypted volume automatically upon login. This guide contains my method.</p>
<h2>Create Protected Volume</h2><br />
The first step is to open the 'Disk Utility' program. This is available under Applications > Utilities > Disk Utility. Alternatively, you can use ⌘ + Space Bar to access the 'Spotlight' and search for 'Disk Utility' and open the program.</p>
<p>Once the program is open, click on the 'New Image' button at the top of the program.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.02.26-PM.png"><img class="alignnone wp-image-1813 size-full" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.02.26-PM.png" alt="Disk Utility - New Image button" width="749" height="204" /></a></p>
<p>Make sure that none of the options, such as the 'Macintosh HD' or any other DVDs shown in the left window are selected, or else the program will try to create a new disk image from the selected hard drive. Navigate to the proper location you wish to save the file in, and then in the 'Save As' field give the Apple Disk Image (DMG) file an appropriate name. In my case I've saved the image in my home directory (not shown below) with the name 'private_volume'.</p>
<p>For the 'Name' field you'll want to give the image a proper name. When you setup this image to mount to a part of the file system, this will be the name of the directory which is mounted. I recommend simply 'private', so that when it's mounted to your home directory you end up with a path of ~/private/.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.06.45-PM.png"><img class="alignnone size-full wp-image-1814" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.06.45-PM.png" alt="Encrypted volume settings" width="719" height="595" /></a></p>
<p>The size of the image depends on your preference. I found that in practice I've never used more than 1 MB for all the text files containing sensitive keys and passwords, so the 40 MB 'Size' option would be fine. Just in case, I choose '100 MB'. Since I'm using such a small encrypted volume, I've decided to use the strongest encryption (256-bit AES), single Apple partition map, with read/write image format, with Mac OS Extended (Journaled) format.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.07.58-PM.png"><img class="alignnone size-full wp-image-1815" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.07.58-PM.png" alt="encrypted volume password entry" width="460" height="344" /></a></p>
<p>The password you set for the volume does allow spaces, so I highly recommend the <a href="http://preshing.com/20110811/xkcd-password-generator/" target="_blank">XKCD password format</a>.</p>
<p>After you press 'OK' the drive will be automatically mounted, much like a USB drive or DVD is mounted in the Finder. I recommend opening the Finder and then 'Eject' the disk image from the 'Devices' area.</p>
<h2>Creating a Mounting Application</h2><br />
Next open the 'Automator' program from the Applications folder. When prompted to 'Choose a type for your document', choose 'Application' and then click on the 'Choose' button.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-1.51.24-PM.png"><img class="alignnone size-full wp-image-1817" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-1.51.24-PM.png" alt="Automator - Application document type" width="555" height="537" /></a></p>
<p>Within the 'Actions' area on the left, click inside of the search field at the top and type 'shell'. This should narrow the options down to 'Run Shell Script'.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.24.06-PM.png"><img class="alignnone size-full wp-image-1819" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.24.06-PM.png" alt="Run Shell Script" width="435" height="184" /></a></p>
<p>Double click on 'Run Shell Script' to view the script input area on the right hand side. Paste in the following script, with the paths to your disk image and your home directory modified.</p>
<pre class="brush:shell">/usr/bin/hdiutil attach /Users/myusername/protected_volume.dmg -mountroot /Users/myusername/<br />
</pre><br />
<a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.27.26-PM.png"><img class="alignnone size-full wp-image-1820" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.27.26-PM.png" alt="Shell Script" width="613" height="195" /></a></p>
<p>After you've done this, go to the File menu and select 'Save'. Save the new script to the 'Applications' folder with a name such as 'MountProtectedImage'.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.29.41-PM.png"><img class="alignnone size-full wp-image-1821" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.29.41-PM.png" alt="MountProtectedImages Application" width="437" height="209" /></a></p>
<p> </p>
<h2>Mounting upon Startup</h2><br />
I would have advised you to simply create a shell script in ~/bin with executable permissions, but that simply doesn't work with this next step. Under the Apple menu in the upper left corner of your screen, select to view the System Preferences, and then select 'Users and Groups'.</p>
<p>Make sure your own user account is selected, and then click on the 'Login Items' button at the top.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.32.50-PM.png"><img class="alignnone size-full wp-image-1822" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.32.50-PM.png" alt="Login Items" width="674" height="167" /></a></p>
<p>This will display the various programs that are setup to start up when you login to your account. Click on the '+' button below the list of applications, then select the 'MountProtectedImage' application you created in your Applications folder and click on the 'Add' button.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.34.35-PM.png"><img class="alignnone size-full wp-image-1823" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.34.35-PM.png" alt="MountProtectedImage" width="722" height="434" /></a></p>
<p>After you've done this, you'll see the new application show up in the list. You can close the System Preferences window at this point.</p>
<h2>Testing</h2><br />
Now lets see if this worked properly. Close all the programs and log out of your account. When you log back in, you may be prompted to allow the diskimages-helper to use the password from your keychain. Choose 'Always Allow' to ensure that this prompt doesn't occur again when you login in the future.</p>
<p><a href="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.38.04-PM.png"><img class="alignnone size-full wp-image-1825" src="http://www.rubycoloredglasses.com/wp-content/uploads/2014/09/Screen-Shot-2014-09-11-at-2.38.04-PM.png" alt="Allow disk images helper" width="453" height="197" /></a></p>
<p>Keep in mind that your keychain is associated with your user account. Make sure that your Mac OS X user account uses a strong password, and also configure your <a href="https://developer.apple.com/library/mac/documentation/security/conceptual/keychainServConcepts/02concepts/concepts.html#//apple_ref/doc/uid/TP30000897-CH204-SW8" target="_blank">keychain to lock itself when not in use</a>.</p>
<p> </p>
