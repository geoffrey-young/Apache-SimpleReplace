# NAME 

Apache::SimpleReplace - a simple template framework

# SYNOPSIS

httpd.conf:

    <Location /someplace>
       SetHandler perl-script
       PerlHandler Apache::SimpleReplace

       PerlSetVar  TEMPLATE "/templates/format1.html"
       PerlSetVar  REPLACE "the content goes here"
    </Location>  

Apache::SimpleReplace is Filter aware, meaning that it can be used
within an Apache::Filter framework without modification.  Just
include the directive

    PerlSetVar Filter On

and modify the PerlHandler directive accordingly.  As of version
0.06, Apache::SimpleReplace requires Apache::Filter 1.013 or
better - users of Apache::Filter 1.011 or less should use version
0.05.

# DESCRIPTION

Apache::SimpleReplace provides a simple way to insert content within
an established template for uniform content delivery.  While the end
result is similar to Apache::Sandwich, Apache::SimpleReplace offers
several advantages.

    o It does not use separate header and footer files, easing the
      pain of maintaining syntactically correct HTML in seperate files.

    o It is Apache::Filter aware, thus it can both accept content from
      other content handlers as well as pass its changes on to others
      later in the chain.

# EXAMPLE

/usr/local/apache/templates/format1.html:

     <html>
         <head><title>your template</title></head>
                 <title>your template</title>
         <body bgcolor="#778899">
                 some headers, banners, whatever...
                 <p>
     the content goes here
                 </p>
                 <p>some footers, modification dates, whatever...
         </body>
     </html> 

    httpd.conf:

     <Location /someplace>
        SetHandler perl-script
        PerlHandler Apache::SimpleReplace Apache::SSI

        PerlSetVar  TEMPLATE "templates/format1.html"
        PerlSetVar  REPLACE "the content goes here"
        PerlSetVar  Filter On
     </Location>

Now, a request to http://localhost/someplace/foo.html will insert
the contents of foo.html in place of "the content goes here" in the
format1.html template and pass those results to Apache::SSI
The result is a nice and tidy way to control any custom headers, 
footers, background colors, or images in a single html file.

# NOTES

As of 0.02, TEMPLATE is no longer relative to the ServerRoot.

REPLACE defaults to "|", though it may be any character or string
you like - metacharacters are disabled in the search, so sorry, no 
regex for now... 

Verbose debugging is enabled by setting
$Apache::SimpleReplace::DEBUG=1 or greater.  To turn off all debug
information, set your apache LogLevel directive above info level.

This is alpha software, and as such has not been tested on multiple
platforms or environments.  It requires PERL\_LOG\_API=1, 
PERL\_FILE\_API=1, and maybe other hooks to function properly.

# FEATURES/BUGS

If Apache::SimpleReplace finds more than one match for REPLACE in
the template, it will insert the request for the first occurrence
only.  All other replacement strings will just be stripped from the
template.

Currently, Apache::SimpleReplace will return DECLINED if the
content-type of the request is not 'text/html'.

# SEE ALSO

perl(1), mod\_perl(3), Apache(3), Apache::Filter(3)

# AUTHOR

Geoffrey Young <geoff@cpan.org>

# COPYRIGHT

Copyright (c) 2000, Geoffrey Young.  All rights reserved.

This module is free software.  It may be used, redistributed
and/or modified under the same terms as Perl itself.
