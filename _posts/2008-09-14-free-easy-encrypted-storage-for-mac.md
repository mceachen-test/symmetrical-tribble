---
ID: 142
post_title: >
  Free and Easy Encrypted Storage for Mac
  OS X and MobileMe or DropBox
author: matthew
post_date: 2008-09-14 23:14:41
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/free-easy-encrypted-storage-for-mac-142.html
published: true
syntaxhighlighter_encoded:
  - "1"
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "160993384"
---
Online storage services like <a href="http://www.apple.com/mobileme/features/idisk.html">iDisk</a> (part of MobileMe) or <a href="http://www.getdropbox.com/">DropBox</a> are very convenient for sharing and backing up files--but when you put files into the cloud you're assuming that:
<ul>
	<li>those companies, and</li>
	<li>all their employees, and</li>
	<li>all their contractors, and</li>
	<li>all their collocation facility employees</li>
</ul>
will be morally upstanding. And that they will securely erase old drives. And that their systems will never be cracked. Ever.

My point is that <strong>sensitive stuff, like tax documents and passwords, must be encrypted before storing them in the cloud</strong>.

The solution? Make an encrypted file on your computer that can be mounted as a "secure hard drive".

<strong>(Update August 2009): <a href="http://www.truecrypt.org/">TrueCrypt</a> is an easy-to-use and cross-platform solution to this issue. The following is a tutorial that is a Mac OS X specific alternative to TrueCrypt.</strong>

[caption id="attachment_148" align="alignright" width="74"]<img class="size-full wp-image-148" title="crypto-dmg" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/09/crypto-dmg.jpg" alt="Example disk image" width="74" height="81" /> Example disk image[/caption]

Using the disk image is easy -- when you double click your encrypted disk image, you'll be prompted for the image's password, and then the image will be mounted, looking just like another hard drive was attached to your computer. (With TrueCrypt you must "mount" the file using the TrueCrypt software, so it isn't quite as user-friendly).

You can drag-and-drop private files into the disk image, and even edit them just like a normal disk. When you're done, eject the drive, and your files won't be readable until the image's password is re-entered.

The disk image can be stored on your MobileMe drive for backups, but you make sure you un-mount/eject the drive before you lose your network connection to prevent the disk image from losing your most recent changes.

So -- here's how to create one of these encrypted disk images.

[caption id="attachment_1243" align="alignright" width="150"]<a href="http://matthew.mceachen.us/blog/wp-content/uploads/2008/09/sparse-bundle-encrypted-disk-image.png"><img src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/09/sparse-bundle-encrypted-disk-image-150x150.png" alt="" title="sparse-bundle-encrypted-disk-image" width="150" height="150" class="size-thumbnail wp-image-1243" /></a> Disk Utility's disk image creation dialog[/caption]

<ol>
	<li>Open <code>Disk Utility</code> (found in <code>/Applications/Utilities/</code>)</li>
	<li>Click "New Image" in the toolbar</li>
	<li>The "Save As:" field will be the name of the disk image <strong>file</strong>, so it should be something like "secret" or "private"</li>
	<li>The "Volume Name:" is what the name of the disk will be when it's mounted.</li>
	<li>Use a volume size of 500 MB -- we'll be creating a "sparse" bundle, so this is only the upper bound to what you can store in this image.</li>
	<li>Choose format: <strong>Mac OS Extended (Journaled)</strong> (or ExFAT if you want to use it on windows too)</li>
	<li>Choose encryption: <strong>256-bit AES</strong></li>
	<li>Choose partions: <strong>Hard disk</strong></li>
	<li>Choose image format: <strong>sparse bundle disk image</strong>. The other options will work, but take more disk space.</li>
	<li>And finally click <strong>Create</strong></li>
</ol>
[caption id="attachment_144" align="alignnone" width="346"]<img class="size-full wp-image-144" title="creating-cryptodisk" src="http://matthew.mceachen.us/blog/wp-content/uploads/2008/09/creating-cryptodisk.jpg" alt="Settings in Disk Utility to create an encrypted disk image" width="346" height="240" /> Settings in Disk Utility to create an encrypted disk image[/caption]

I'd recommend <strong>NOT</strong> storing the password in your keychain, so when asked for the password for the disk image, unselect that option, and make sure you remember the password to your new image--because you can't open it without the password.

Disk Utility will automatically mount the disk image as soon as it's created. Remember to eject it when you're done (by right-clicking on the disk image and choosing "Eject"). In the future double-click the image to re-mount it and get access to your secret files again.