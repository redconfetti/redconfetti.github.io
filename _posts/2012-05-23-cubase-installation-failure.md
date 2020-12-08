---
layout: post
published: true
title: Cubase Installation Failure
date: '2012-05-23 03:48:01 -0700'
date_gmt: '2012-05-23 07:48:01 -0700'
categories:
- computing tips
tags:
- cubase
- package scripts
- install failure
---

I recently ran into issues installing Cubase 4 on my Mac running Snow Leopard.
I uninstalled Cubase 5 Essential, thinking that this was causing a conflict, and
thus stopping me from installing an older version. This wasn't the case
I tried to install Cubase 5 Essential again, and I got the same type of error
with it's installer. I received Cubase 6 in the mail today and tried to install
it...only to receive the same type of error:
<!--more-->

``` shell
5/23/12 12:09:29 AM installd[483] PackageKit: Install Failed: PKG: post-install scripts for "de.steinberg.vstsounds.halionsonicseadvancedcontent"
Error Domain=PKInstallErrorDomain Code=112 UserInfo=0x1005f1690 "An error occurred while running scripts from the package 'vstsounds_HALion Sonic SE Advanced Content.pkg'." {
    NSFilePath = "./postinstall";
    NSLocalizedDescription = "An error occurred while running scripts from the package \U201cvstsounds_HALion Sonic SE Advanced Content.pkg\U201d.";
    NSURL = "./Contents/Packages/vstsounds_HALion%20Sonic%20SE%20Advanced%20Content.pkg -- file://localhost/Volumes/Cubase%206/Cubase%206%20for%20Mac%20OS%20X/Cubase%206.mpkg/";
    PKInstallPackageIdentifier = "de.steinberg.vstsounds.halionsonicseadvancedcontent";
}
5/23/12 12:09:29 AM com.apple.ReportCrash.Root[541] 2012-05-23 00:09:29.688 ReportCrash[541:2803] Saved crash report for installd[540] version ??? (???) to /Library/Logs/DiagnosticReports/installd_2012-05-23-000929_localhost.crash
5/23/12 12:09:30 AM Installer[439]  Install failed: The Installer encountered an error that caused the installation to fail. Contact the software manufacturer for assistance.
```

After many attempts to repair the permissions and the disk itself using the
Disk Utility, I identified the cause of the issue after inspecting the crash
report that the Cubase 6 installer provided. It turns out that Steinberg uses
Ruby scripts to install packages. I had renamed the symlink located at
`/usr/bin/ruby as /usr/bin/ruby.old`, and created a new one that pointed to
`/opt/local/bin/ruby` (the location of the Ruby interpreter I had previously
installed using MacPorts).

After removing this symlink, and restoring the old one (which pointed to
`/System/Library/Frameworks/Rubyframework/Versions/Current/usr/bin/ruby`), the
installation of Cubase 6 was successful.

HORRAY!
