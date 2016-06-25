---
ID: 731
post_title: Macports fails to compile p5-perlmagick
author: matthew
post_date: 2009-11-29 13:30:37
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/macports-fails-to-compile-p5-perlmagick-731.html
published: true
syntaxhighlighter_encoded:
  - "1"
dsq_thread_id:
  - "161305219"
---
If you need <a href="http://www.imagemagick.org/script/perl-magick.php">PerlMagick</a> on a Mac, there was (and still is) a <code>p5-perlmagick</code> package. If you try to install that package now, you'll find it doesn't compile (!!). You don't have to resort to compiling from source, though.

<!--more-->

According to <a href="http://trac.macports.org/ticket/13234">this MacPorts ticket</a>, p5-perlmagick has been deprecated in favor of recompiling the imagemagick port with the +perl variant:

<pre lang="bash">
sudo port -nu uninstall p5-perlmagick imagemagick
sudo port clean imagemagick
sudo port install imagemagick +perl
</pre>

Before you just add the perl variant, though, you might want these other goodies:

<pre lang="bash">
$ port variants imagemagick
ImageMagick has the variants:
   graphviz: Support Graphviz
   gs: Include Ghostscript library support
   hdri: Support High Dynamic Range Imaging using OpenEXR
   jbig: Support JBIG
   jpeg2: Support JPEG-2000 using JasPer
   lcms: Support the Little Color Management System
   lqr: Support Liquid Rescale (experimental)
   mpeg: Support MPEG-1 and MPEG-2 video
   no_plus_plus: Do not install Magick++
   no_x11: Disable support for X11
   perl: Install PerlMagick
[+]q16: Use 16 bits per pixel quantum
     * conflicts with q32 q8
   q32: Use 32 bits per pixel quantum
     * conflicts with q16 q8
   q8: Use 8 bits per pixel quantum
     * conflicts with q16 q32
   rsvg: Support SVG using librsvg
   universal: Build for multiple architectures
   wmf: Support the Windows Metafile Format
</pre>

To make it easier for search engines to find this, here's the compilation failure:

<pre lang="bash">
$ sudo port install p5-perlmagick
Password:
--->  Computing dependencies for p5-perlmagick
--->  Fetching p5-perlmagick
--->  Verifying checksum(s) for p5-perlmagick
--->  Extracting p5-perlmagick
--->  Configuring p5-perlmagick
--->  Building p5-perlmagick
Error: Target org.macports.build returned: shell command " cd "/opt/local/var/macports/build/_opt_local_var_macports_sources_rsync.macports.org_release_ports_perl_p5-perlmagick/work/PerlMagick-6.32" && /usr/bin/make -j2 all " returned error 2
Command output: Magick.xs:10918: error: request for member 'severity' in something not a structure or union
Magick.xs:10918: error: 'ErrorException' undeclared (first use in this function)
Magick.xs:10919: error: 'struct Methods' has no member named 'exception'
Magick.xs:10920: warning: implicit declaration of function 'GetImageException'
Magick.xs:10922: error: dereferencing pointer to incomplete type
Magick.xs:10922: error: request for member 'image_info' in something not a structure or union
Magick.xs:10922: error: 'struct Methods' has no member named 'adjoin'
Magick.xs:10929: error: request for member 'severity' in something not a structure or union
Magick.xs:10929: error: 'UndefinedException' undeclared (first use in this function)
Magick.xs:10929: error: request for member 'severity' in something not a structure or union
Magick.xs:10929: error: request for member 'reason' in something not a structure or union
Magick.xs:10929: error: request for member 'severity' in something not a structure or union
Magick.xs:10929: error: request for member 'reason' in something not a structure or union
Magick.xs:10929: warning: pointer/integer type mismatch in conditional expression
Magick.xs:10929: error: request for member 'description' in something not a structure or union
Magick.xs:10929: error: request for member 'description' in something not a structure or union
Magick.xs:10929: error: request for member 'severity' in something not a structure or union
Magick.xs:10929: error: request for member 'description' in something not a structure or union
Magick.xs:10929: warning: pointer/integer type mismatch in conditional expression
Magick.xs:10929: error: request for member 'description' in something not a structure or union
Magick.xs:10929: warning: passing argument 2 of 'Perl_sv_catpv' from incompatible pointer type
Magick.xs:10929: warning: unused variable 'message'
Magick.xs:10856: warning: unused variable 'filename'
Magick.c:10784: warning: unused variable 'ref'
Magick.c:10777: warning: unused variable 'ix'
Magick.xs: In function 'boot_Image__Magick':
Magick.xs:2122: warning: implicit declaration of function 'InitializeMagick'
Magick.xs:2123: warning: implicit declaration of function 'SetWarningHandler'
Magick.xs:2124: warning: implicit declaration of function 'SetErrorHandler'
make: *** [Magick.o] Error 1

Error: Status 1 encountered during processing.
</pre>