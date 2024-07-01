# Module Rewrite

Welcome to mod_rewrite, voodoo of URL manipulation.

This document describes how one can use Apache's mod_rewrite to solve typical URL based problems webmasters are usually confronted with in practice. The Apache module mod_rewrite is a module which provides a powerful way to do URL manipulations. With it you can nearly do all types of URL manipulations you ever dreamed about. The price you have to pay is to accept complexity, because mod_rewrite is not easy to understand and use for the beginner.

NOTE: Depending on your server configuration it can be necessary to change the examples for your situation. Always try to understand what it really does before you use it. Bad use would lead to deadloops and will hang the server.

The most example's can be used in the .htaccess file while other ones only in the Apache htppd.conf file.


## RewriteCond

The RewriteCond directive defines a rule condition. Preserve a RewriteRule with one or more RewriteCond directives. The following rewriting rule is only used if its pattern matches the current state of the URI and if these additional conditions apply too.

You can set special flags for condition pattern by appending a third argument to the RewriteCond directive. Flags is a comma-separated list of the following flags:
[NC] (No Case)
This makes the condition pattern case insensitive, no difference between 'A-Z' and 'a-z'.

[OR] (OR next condition)
Used to combinate rule conditions with a OR.


## RewriteRule

The RewriteRule directive is the real rewriting.

You can set special flags for condition pattern by appending a third argument to the RewriteCond directive. Flags is a comma-separated list of the following flags:
[R] (force Redirect)
Redirect the URL to a external redirection. Send the HTTP response, 302 (MOVED TEMPORARILY).

[F] (force URL to be Forbidden)
Forces the current URL to be forbidden. Send the HTTP response, 403 (FORBIDDEN).

[G] (force URL to be Gone)
Forces the current URL to be gone. Send the HTTP response, 410 (GONE).

[L] (last rule)
Forces the rewriting processing to stop here and don't apply any more rewriting rules.

[P] (force proxy)
This flag forces the current URL as a proxy request and put through the proxy module mod_proxy.

## Regular expressions

Some hints about the syntax of regular expressions:

### Text:
  . Any single character
  [chars] One  of chars
  [^chars] None of chars
  text1|text2 text1 or text2

### Quantifiers:
  ? 0 or 1 of the preceding text
  * 0 or N of the preceding text (N > 0)
  + 1 or N of the preceding text (N > 1)

### Grouping:
  (text) Grouping of text

### Anchors:
  ^ Start of line anchor
  $ End of line anchor

### Escaping:
  \ char escape that particular char


## Condition pattern:

There are some special variants of CondPatterns. Instead of real regular expression strings you can also use one of the following:

```
< Condition (is lower than Condition)
Treats the Condition as a string and compares it to String. True if String is lower than Condition.

> Condition (is greater than Condition)
Treats the Condition as a string and compares it to String. True if String is greater than CondPattern.

= Condition (is equal to Condition)
Treats the Condition as a string and compares it to String. True if String is equal to CondPattern.

-d (is directory)
Treats the String as a pathname and tests if it exists and is a directory.

-f (is regular file)
Treats the String as a pathname and tests if it exists and is a regular file.

-s (is regular file with size)
Treats the String as a pathname and tests if it exists and is a regular file with size greater than zero.

-l (is symbolic link)
Treats the String as a pathname and tests if it exists and is a symbolic link.

-F (is existing file via sub request)
Checks if String is a valid file and accessible via all the server's currently configured access controls for that path. Use it with care because it decreases your servers performance!

-U (is existing URL via sub request)
Checks if String is a valid URL and accessible via all the server's currently configured access controls for that path. Use it with care because it decreases your servers performance!
```

NOTE: You can prefix the pattern string with a '!' character (exclamation mark) to specify a non-matching pattern.

## Protecting your images and files from linking

DESCRIPTION: In some cases other webmasters are linking to your download files or using images, hosted on your server as inline-images on their pages.
```
RewriteEngine On
RewriteCond %{HTTP_REFERER} !^$ [NC]
RewriteCond %{HTTP_REFERER} !^http://domain.com [NC]
RewriteCond %{HTTP_REFERER} !^http://www.domain.com [NC]
RewriteCond %{HTTP_REFERER} !^http://212.204.218.80 [NC]
RewriteRule ^.*$ http://www.domain.com/ [R,L]
```

EXPLAIN: In this case are the visitors redirect to http://www.domain.com/ if the hyperlink has not arrived from http://domain.com, http://www.domain.com or http://212.204.218.80.


## Redirect visitor by domain name

DESCRIPTION: In some cases the same web site is accessible by different addresses, like domain.com, www.domain.com, www.domain2.com and we want to redirect it to one address.
```
RewriteEngine On
RewriteCond %{HTTP_HOST} !^www.domain.com$ [NC]
RewriteRule ^(.*)$ http://www.domain.com/$1 [R,L]
```

EXPLAIN: In this case the requested URL http://domain.com/foo.html would redirected to the URL http://www.domain.com/foo.html.


## Redirect domains to other directory
```
RewriteEngine On
RewriteCond %{HTTP_HOST} ^www.domain.com$
RewriteCond %{REQUEST_URI} !^/HTML2/
RewriteRule ^(.*)$ /HTML2/$1
```

## Redirect visitor by user agent

DESCRIPTION: For important top level pages it is sometimes necesarry to provide pages dependend on the browser. One has to provide a version for the latest Netscape, a version for the latest Internet Explorer, a version for the Lynx or old browsers and a average feature version for all others.
```
# MS Internet Explorer - Mozilla v4
RewriteEngine On
RewriteCond %{HTTP_USER_AGENT} ^Mozilla/4(.*)MSIE
RewriteRule ^index\.html$ /index.IE.html [L]

# Netscape v6.+ - Mozilla v5
RewriteCond %{HTTP_USER_AGENT} ^Mozilla/5(.*)Gecko
RewriteRule ^index\.html$ /index.NS5.html [L]

# Lynx or Mozilla v1/2
RewriteCond %{HTTP_USER_AGENT} ^Lynx/ [OR]
RewriteCond %{HTTP_USER_AGENT} ^Mozilla/[12]
RewriteRule ^index\.html$ /index.20.html [L]

# All other browsers
RewriteRule ^index\.html$ /index.32.html [L]
```

EXPLAIN: In this case we have to act on the HTTP header User-Agent. If the User-Agent begins with Mozilla/4 and is MS Internet Explorer (MSIE), the page index.html is rewritten to index.IE.html and the rewriting stops. If the User-Agent begins with Mozilla/5 and is Netscape (Gecko), the page index.html is rewritten to index.NS5.html. If the User-Agent begins with Lynx/ or Mozilla/1,2, the page index.html is rewritten to index.20.html. All other browsers receive page index.32.html 

## htaccess

htaccess is a configuration file that controls Apache web servers, mod_rewrite is a rewrite engine used by web servers to modify urls before they load.

The htaccess file is a text file called .htaccess  htaccess is the file extension, there is no filename. Normally it resides in the main root directory on your server but you can also create individual htaccess files for different directories on your site.

## Canonicalization

The easiest htaccess trick is to make sure that your site doesnt have any canonicalization issues on the homepage.

A lot of websites suffer from poor search engine rankings by having a number of different versions of the homepage, for example:
```
http://www.yoursite.com
http://yoursite.com
http://www.yoursite.com/index.html
http://yoursite.com/index.html
```
These pages are all seen as different urls, despite them having exactly the same content in most cases. Google has got better at deciding which version to use over the past 12 months but you can still run into problems.

To solve this issue simply add the following to your htaccess file:
```
Options +FollowSymLinks
RewriteEngine on
RewriteCond %{HTTP_HOST} ^yoursite.com
RewriteRule (.*) http://www.yoursite.com/$1 [R=301,L]
RewriteCond %{THE_REQUEST} ^[A-Z]{3,9} /index.html HTTP/
RewriteRule ^index.html$ http://www.yoursite.com/ [R=301,L]
```
This will redirect all versions to http://www.yoursite.com

## Changing html files to php

Sometimes you might have a static html website and need to use php code on the html pages. Rather than redirecting all your html pages to the equivalent php versions you simply need to tell your server to parse html files as if they were php.
```
AddHandler application/x-httpd-php .html
```

This works with any files so if you want to create dynamic xml or asp files that behave like php files you simply edit the code as required:

```
AddHandler application/x-httpd-php .xml
AddHandler application/x-httpd-php .asp
Error pages
```

Custom error pages can be set up in cpanel fairly easily, if you want to create a custom error page in htaccess instead use this line:
```
ErrorDocument 404 http://www.yoursite.com/404.php
Directory Indexes
```
To avoid Google indexing your directory indexes you might need to specify an index page for your directories. This is not required on some servers.

```
DirectoryIndex index.php3
```

My preference is to redirect the directory index page to either the homepage or another suitable page. For example www.yoursite.com/images/ can normally be redirected to www.yoursite.com and www.yoursite.com/forum/ can normally be redirected to www.yoursite.com/forum/index.php
Redirecting pages

A nice simple use of htaccess is to redirect one page to another:
```
redirect 301 /old-page.php http://www.yoursite.com/new-page.php
Sending your feed to Feedburner
```

If you want to switch your feed to the Feedburner service you will need to redirect your current feed to the new http://feeds.feedburner.com/yourfeed location.

The redirect needs to apply to all users except the Feedburner spider:

```
RewriteCond %{HTTP_USER_AGENT} !FeedBurner
RewriteRule ^your-feed.xml$ http://feeds.feedburner.com/your-feed [R,L]
Advanced hotlink protection
```

If you want to block other websites from hotlinking your images, but allow indexing of your images in the Google, Yahoo and MSN image search engines, you should use the code below:
```
RewriteEngine on
RewriteCond %{HTTP_REFERER} .
RewriteCond %{HTTP_REFERER} !^http://([^.]+.)?yoursite. [NC]
RewriteCond %{HTTP_REFERER} !google. [NC]
RewriteCond %{HTTP_REFERER} !search?q=cache [NC]
RewriteCond %{HTTP_REFERER} !msn. [NC]
RewriteCond %{HTTP_REFERER} !yahoo. [NC]
RewriteCond %{REQUEST_URI} !^/hotlinker.gif$
RewriteRule .(gif|jpg|png)$ /hotlinker.gif [NC,L]
```

The hotlinker.gif image is a custom image that you have created. I suggest using something like This image was hotlinked from www.yoursite.com and your logo.

My personal preference is to allow hotlinking but to implement a solution to make use of Google Images and hotlinkers to build links to your site.
Create beautiful urls with mod_rewrite

The Apache rewrite engine is mainly used to turn dynamic urls such as www.yoursite.com/product.php?id=123 into static and user friendly urls such as www.yoursite.com/product/123

```
RewriteEngine on
RewriteRule ^product/([^/.]+)/?$ product.php?id=$1 [L]
```
Another example, rewrite from:

www.yoursite.com/script.php?product=123 to www.yoursite.com/cat/product/123/

```
RewriteRule cat/(.*)/(.*)/$ /script.php?$1=$2
Removing query strings
```

Some websites like to link to you by adding an query string, for example I could link to www.yoursite.com/index.php?source=blogstorm just so you know where your traffic came from. This creates duplicate content issue for your site so you really need to redirect back to your homepage:

RewriteCond %{QUERY_STRING} ^source= RewriteRule (.*) /$1? [R=301,L]

<IfModule mod_rewrite.c>
    # Options +FollowSymLinks -Indexes
    RewriteEngine On
    RewriteBase /ignitor/no_cms/
    #Checks to see if the user is attempting to access a valid file,
    #such as an image or css document, if this isn't true it sends the
    #request to index.php
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php/$1 [L,QSA]
</IfModule>

<IfModule !mod_rewrite.c>
    # If we don't have mod_rewrite installed, all 404's
    # can be sent to index.php, and everything works as normal.
    # Submitted by: ElliotHaughin

    ErrorDocument 404 /index.php
</IfModule>

<IfModule mod_expires.c>
    ExpiresActive on

    # Perhaps better to whitelist expires rules? Perhaps.
    ExpiresDefault                          "access plus 1 month"

    # cache.appcache needs re-requests in FF 3.6 (thx Remy ~Introducing HTML5)
    ExpiresByType text/cache-manifest       "access plus 0 seconds"

    # your document html
    ExpiresByType text/html                 "access plus 0 seconds"

    # data
    ExpiresByType text/xml                  "access plus 0 seconds"
    ExpiresByType application/xml           "access plus 0 seconds"
    ExpiresByType application/json          "access plus 0 seconds"

    # rss feed
    ExpiresByType application/rss+xml       "access plus 1 hour"

    # favicon (cannot be renamed)
    ExpiresByType image/x-icon              "access plus 1 week"

    # media: images, video, audio
    ExpiresByType image/gif                 "access plus 1 month"
    ExpiresByType image/png                 "access plus 1 month"
    ExpiresByType image/jpg                 "access plus 1 month"
    ExpiresByType image/jpeg                "access plus 1 month"
    ExpiresByType video/ogg                 "access plus 1 month"
    ExpiresByType audio/ogg                 "access plus 1 month"
    ExpiresByType video/mp4                 "access plus 1 month"
    ExpiresByType video/webm                "access plus 1 month"

    # htc files  (css3pie)
    ExpiresByType text/x-component          "access plus 1 month"

    # webfonts
    ExpiresByType font/truetype             "access plus 1 month"
    ExpiresByType font/opentype             "access plus 1 month"
    ExpiresByType application/x-font-woff   "access plus 1 month"
    ExpiresByType image/svg+xml             "access plus 1 month"
    ExpiresByType application/vnd.ms-fontobject "access plus 1 month"

    # css and javascript
    ExpiresByType text/css                  "access plus 2 months"
    ExpiresByType application/javascript    "access plus 2 months"
    ExpiresByType text/javascript           "access plus 2 months"

</IfModule>

<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/xhtml text/html text/plain text/xml text/javascript application/x-javascript text/css
</IfModule>

RewriteEngine On

# RewriteBase
# RewriteBase /

# Only needed for some hosts, if the front returns 403.
# Options +FollowSymlinks

# Memory Limit
# php_value memory_limit 256M
# php_value max_execution_time 18000

# Disable Hotlinking
# Replace "domain.tld" with your domain name
# Don't forget to add a picture "thepic.gif" on your server 
# RewriteCond %{HTTP_REFERER} !^http://(.+\.)?domain\.tld/ [NC]
# RewriteCond %{HTTP_REFERER} !^$
# Solution 1 : Displays another picture from an URL
# RewriteRule .*\.(jpe?g|gif|bmp|png)$ http://www.domain.tld/thepic.gif [L]
# Solution 2 : Displays a 403 forbidden
# RewriteRule .*\.(jpe?g|gif|bmp|png)$ - [F]

# Redirect domain.com to www.domain.com
# RewriteCond %{HTTP_HOST} !^www\.domain\.tld$ [NC]
# RewriteRule ^(.*)$ http://www.domain.tld/$1 [QSA,L,R=301]

# Redirect www.domain.com to domain.com
# RewriteCond %{HTTP_HOST} !^domain.com$ [NC]
# RewriteRule ^(.*)$ http://domain.com/$1 [L,R=301]

# Disallow .svn access if you're using SVN
# RedirectMatch 404 /\\.svn(/.*|$)

# Uncomment these lines to display the off.html page to other IP than the one bellow
# RewriteCond %{REMOTE_ADDR} !^127.0.0.1$
# RewriteCond %{REQUEST_FILENAME} !-f
# RewriteRule ^(.*) off.html

# DO NOT MODIFY ANY FOLLOWING LINE

# 
# Ionize Rules
#
# If Ionize resized outside the public folder, keep modules asset availability
# RewriteCond %{REQUEST_FILENAME} !-f
# RewriteRule ^(modules)/([a-zA-Z0-9-]+)/(assets)/(.*)$ ../$1/$2/$3/$4

# Keep these lines even in maintenance mode, to have an access to the website
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond $1 !^(index\.php|robots\.txt)
RewriteRule ^(.*)$ index.php/$1

RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(application|modules|plugins|system|themes) index.php/$1 [L]
<IfModule mod_rewrite.c>
	Options +FollowSymlinks
    RewriteEngine On

    #Removes access to the system folder by users.
    #Additionally this will allow you to create a System.php controller,
    #previously this would not have been possible.
    #'system' can be replaced if you have renamed your system folder.
    RewriteCond %{REQUEST_URI} ^dev10.*
    RewriteRule ^(.*)$ /index.php/$1 [L]

    #Checks to see if the user is attempting to access a valid file,
    #such as an image or css document, if this isn't true it sends the
    #request to index.php
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php/$1 [L]
</IfModule>

<IfModule !mod_rewrite.c>
    # If we don't have mod_rewrite installed, all 404's
    # can be sent to index.php, and everything works as normal.
    # Submitted by: ElliotHaughin

    ErrorDocument 404 /index.php
</IfModule>

<IfModule mod_php5.c>
    php_value max_execution_time "18000"
    php_value upload_max_filesize "100M"
    php_value max_upload_filesize "100M"
    php_value post_max_size "100M"
    php_value memory_limit "128M"
</IfModule>

<IfModule mod_php4.c>
    php_value max_execution_time "18000"
    php_value upload_max_filesize "100M"
    php_value max_upload_filesize "100M"
    php_value post_max_size "100M"
    php_value memory_limit "128M"
</IfModule>

DirectoryIndex index.php index.html

<IfModule mod_rewrite.c>
    RewriteEngine On

	#disable directory listing for security reasons
	Options -Indexes

	#RewriteCond %{HTTP_HOST} !^local.others.com/fight [NC]
	#RewriteRule ^(.*)$ http://local.others.com/fight/$1 [L,R=301]

	#RewriteRule ^(home(\.html)?)$ http://local.others.com/fight/ [L,R=301]

	RewriteCond %{THE_REQUEST} \/index\.(php|html)\ HTTP [NC]
	RewriteRule (.*)index\.(php|html)$ /$1 [R=301,L]

    #todo::seo url for pagination: url/p/category_id/page_number
    #RewriteCond %{HTTP_HOST} !^www\.(.+)$ [NC]
    #RewriteCond %{HTTP_HOST} (.+)\.local.others\.com\fight\ [NC]
    #RewriteCond %{HTTP_HOST}%{REQUEST_URI} ^(.+)\.local.others\.com\fight\/p/(\d+)/(\d+)$ [NC]
    #RewriteRule .* http://local.others.com/fight/blog/%2/%1/%3/? [P]

    #todo::seo url for full blog post: url/blog_id-seo-friendly-url
    #RewriteCond %{HTTP_HOST} !^www\.(.+)$ [NC]
    #RewriteCond %{HTTP_HOST} (.+)\.local.others\.com [NC]
    #RewriteCond %{HTTP_HOST}%{REQUEST_URI} ^(.+)\.local.others\.com\fight/(\d+)-(.*) [NC]
    #RewriteRule .* http://local.others.com/fight/blog/%1/%2/%3/? [P]

    #todo::seo url for blog categories: category_name.url
    #RewriteCond %{HTTP_HOST} !^www\.(.+)$ [NC]
    #RewriteCond %{HTTP_HOST} (.+)\.local.others\.com [NC]
    #RewriteRule .* http://local.others.com/blog/c/%1/? [P]

	RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-l
    RewriteRule ^(.*)$ index.php?/$1 [L]

</IfModule>
# Pretty URLS
RewriteRule ^signup$ /signup.php [L]
RewriteRule ^login$ /login.php [L]

# SPAMMY IPs BLOCK
deny from 92.241.169.70
deny from 91.201.66.104
deny from 79.142.79.10
# SPAMMY IPs BLOCK END

RewriteEngine on
# Options -Indexes

# Prevent Indexing
IndexIgnore .htaccess */.?* *~ *# */HEADER* */README* */_vti*

# force www. in urls
RewriteCond %{HTTP_HOST} ^mydomain.com [NC]
RewriteRule ^(.*)$ http://www.mydomain.com/$1 [L,R=301]

Options +FollowSymLinks
RewriteCond %{SCRIPT_FILENAME} !-f
RewriteCond %{SCRIPT_FILENAME} !-d

RewriteCond %{HTTP_REFERER} !^http://mydomain.com/.*$ [NC]
RewriteCond %{HTTP_REFERER} !^http://mydomain.com$ [NC]
RewriteCond %{HTTP_REFERER} !^http://www.mydomain.com/.*$ [NC]
RewriteCond %{HTTP_REFERER} !^http://www.mydomain.com$ [NC]
RewriteRule .*\.(.[^/]*jpg|jpeg|gif|png|bmp|psd)$ - [F,NC]

<Limit GET POST>
    order deny,allow
    deny from all
    allow from all
</Limit>
<Limit PUT DELETE>
    order deny,allow
    deny from all
</Limit>

<FilesMatch "\.(ht|htaccess|htpasswd|ini|log|inc|error_log)$">
Order Allow,Deny
Deny from all
</FilesMatch>

# CACHE CONTROLS
#
# Set expiration header
# 2419200 - 4 week | 29030400 - 1 year
# ExpiresActive on
# ExpiresByType image/gif A29030400
# ExpiresByType image/png A29030400
# ExpiresByType image/jpeg A29030400
# ExpiresByType image/jpg A29030400
# ExpiresByType text/css A2419200
# ExpiresByType text/javascript A2419200
# ExpiresByType text/js A2419200

# Compress some text file types
# <FilesMatch "\.(php|html|css|js)$">
# SetOutputFilter DEFLATE
# </FilesMatch>
# AddOutputFilterByType DEFLATE text/html text/css text/xml application/xhtml+xml application/rss+xml application/x-javascript text/javascript text/js

# URL Changes - Permanent Redirects
# Redirect 301 /contact/contactus.php http://www.mydomain.com/contactus.php

# Custom Error Documents
ErrorDocument 404 /error/404.php
ErrorDocument 400 /error/400.php
ErrorDocument 401 /error/401.php
ErrorDocument 403 /error/403.php
ErrorDocument 500 /error/500.php
ErrorDocument 501 /error/501.php
ErrorDocument 502 /error/502.php
ErrorDocument 503 /error/503.php


Table of Contents

    General
        htaccess definition
        htaccess comments
        important information
        performance issues
        regex character definitions
        redirection header codes
    Essentials
        htaccess comments
        enable basic rewriting
        enable symbolic links
        enable AllowOverride
        rename the htaccess file
        retain httpd.conf rules
    Performance
        disable AllowOverride
        pass the character set
        preserve bandwidth
        disable the server signature
        set the server timezone
        set admin email address
        enable file caching
        set default language & character set
        declare specific/additional MIME types
        send headers without meta tags
        limit request methods to GET/PUT
        process files according to request method
        execute various file types via CGI script
    Security
        prevent access to htaccess
        prevent access to any file
        prevent access to multiple file types
        prevent unauthorized directory browsing
        change the default index page
        disguise script extensions
        limit access to the LAN
        secure directories by IP or domain
        deny/allow domain access for IP range
        deny/allow multiple IP addresses on one line
        miscellaneous rules for blocking/allowing
        stop hotlinking, serve alternate content
        block robots, rippers, and offline browsers
        more stupid blocking tricks
        even more scum-blocking tricks
        password-protect directories
        password-protect files, directories, and more
        require SSL (secure sockets layer)
        automatically CHMOD various file types
        disguise all file extensions
        limit file upload size
        disable script execution
    Usability
        minimize CSS image flicker in IE6
        deploy custom error pages
        provide a universal error document
        employ basic URL spelling check
        force media downloads
        display file source code
        redirect visitors during site development
        provide password-prompt during site development
        prevent access during specified time periods
    Redirects
        important note about redirecting via mod_rewrite
        redirect from www-domain to non-www-domain
        redirect from an old domain to a new domain
        redirect string variations to a specific address
        other fantastic redirect tricks
        send visitors to a subdomain
        more fun with RewriteCond & RewriteRule
        more fun with Redirect 301 & RedirectMatch 301
    WordPress
        secure wordPress contact forms
        wordpress permalinks
    Random
        activate SSI for HTML/SHTML file types
        grant CGI access in a specific directory
        disable magic_quotes_gpc for PHP enabled servers
        enable MD5 digests
        expression engine tricks
    References


General Information [ ^ ]
.htaccess Definition 1 ^

Apache server software provides distributed (i.e., directory-level) configuration via Hypertext Access files. These .htaccess files enable the localized fine-tuning of Apache's universal system-configuration directives, which are defined in Apache's main configuration file. The localized .htaccess directives must operate from within a file named .htaccess. The user must have appropriate file permissions to access and/or edit the .htaccess file. Further, .htaccess file permissions should never allow world write access — a secure permissions setting is "644", which allows universal read access and user-only write access. Finally, .htaccess rules apply to the parent directory and all subdirectories. Thus to apply configuration rules to an entire website, place the .htaccess file in the root directory of the site.
Commenting .htaccess Code ^

Comments are essential to maintaining control over any involved portion of code. Comments in .htaccess code are fashioned on a per-line basis, with each line of comments beginning with a pound sign #. Thus, comments spanning multiple lines in the .htaccess file require multiple pound signs. Further, due to the extremely volatile nature of htaccess voodoo, it is wise to include only alphanumeric characters (and perhaps a few dashes and underscores) in any .htaccess comments.
Important Notes for .htaccess Noobs ^

As a configuration file, .htaccess is very powerful. Even the slightest syntax error (like a missing space) can result in severe server malfunction. Thus it is crucial to make backup copies of everything related to your site (including any original .htaccess files) before working with your Hypertext Access file(s). It is also important to check your entire website thoroughly after making any changes to your .htaccess file. If any errors or other problems are encountered, employ your backups immediately to restore original functionality.
Performance Issues ^

.htaccess directives provide directory-level configuration without requiring access to Apache's main server cofiguration file (httpd.conf). However, due to performance and security concerns, the main configuration file should always be used for server directives whenever possible. For example, when a server is configured to process .htaccess directives, Apache must search every directory within the domain and load any and all .htaccess files upon every document request. This results in increased page processing time and thus decreases performance. Such a performance hit may be unnoticeable for sites with light traffic, but becomes a more serious issue for more popular websites. Therefore, .htaccess files should only be used when the main server configuration file is inaccessible. See the "Performance Tricks" section of this article for more information.
Regex Character Definitions for htaccess 2 ^

#
    the # instructs the server to ignore the line. used for including comments. each line of comments requires it's own #. when including comments, it is good practice to use only letters, numbers, dashes, and underscores. this practice will help eliminate/avoid potential server parsing errors.
[F]
    Forbidden: instructs the server to return a 403 Forbidden to the client.
[L]
    Last rule: instructs the server to stop rewriting after the preceding directive is processed.
[N]
    Next: instructs Apache to rerun the rewrite rule until all rewriting directives have been achieved.
[G]
    Gone: instructs the server to deliver Gone (no longer exists) status message.
[P]
    Proxy: instructs server to handle requests by mod_proxy
[C]
    Chain: instructs server to chain the current rule with the previous rule.
[R]
    Redirect: instructs Apache to issue a redirect, causing the browser to request the rewritten/modified URL.
[NC]
    No Case: defines any associated argument as case-insensitive. i.e., "NC" = "No Case".
[PT]
    Pass Through: instructs mod_rewrite to pass the rewritten URL back to Apache for further processing.
[OR]
    Or: specifies a logical "or" that ties two expressions together such that either one proving true will cause the associated rule to be applied.
[NE]
    No Escape: instructs the server to parse output without escaping characters.
[NS]
    No Subrequest: instructs the server to skip the directive if internal sub-request.
[QSA]
    Append Query String: directs server to add the query string to the end of the expression (URL).
[S=x]
    Skip: instructs the server to skip the next "x" number of rules if a match is detected.
[E=variable:value]
    Environmental Variable: instructs the server to set the environmental variable "variable" to "value".
[T=MIME-type]
    Mime Type: declares the mime type of the target resource.
[]
    specifies a character class, in which any character within the brackets will be a match. e.g., [xyz] will match either an x, y, or z.
[]+
    character class in which any combination of items within the brackets will be a match. e.g., [xyz]+ will match any number of x's, y's, z's, or any combination of these characters.
[^]
    specifies not within a character class. e.g., [^xyz] will match any character that is neither x, y, nor z.
[a-z]
    a dash (-) between two characters within a character class ([]) denotes the range of characters between them. e.g., [a-zA-Z] matches all lowercase and uppercase letters from a to z.
a{n}
    specifies an exact number, n, of the preceding character. e.g., x{3} matches exactly three x's.
a{n,}
    specifies n or more of the preceding character. e.g., x{3,} matches three or more x's.
a{n,m}
    specifies a range of numbers, between n and m, of the preceding character. e.g., x{3,7} matches three, four, five, six, or seven x's.
()
    used to group characters together, thereby considering them as a single unit. e.g., (perishable)?press will match press, with or without the perishable prefix.
^
    denotes the beginning of a regex (regex = regular expression) test string. i.e., begin argument with the proceeding character.
$
    denotes the end of a regex (regex = regular expression) test string. i.e., end argument with the previous character.
?
    declares as optional the preceding character. e.g., monzas? will match monza or monzas, while mon(za)? will match either mon or monza. i.e., x? matches zero or one of x.
!
    declares negation. e.g., "!string" matches everything except "string".
.
    a dot (or period) indicates any single arbitrary character.
-
    instructs "not to" rewrite the URL, as in "...domain.com.* - [F]".
+
    matches one or more of the preceding character. e.g., G+ matches one or more G's, while "+" will match one or more characters of any kind.
*
    matches zero or more of the preceding character. e.g., use ".*" as a wildcard.
|
    declares a logical "or" operator. for example, (x|y) matches x or y.
\
    escapes special characters ( ^ $ ! . * | ). e.g., use "\." to indicate/escape a literal dot.
\.
    indicates a literal dot (escaped).
/*
    zero or more slashes.
.*
    zero or more arbitrary characters.
^$
    defines an empty string.
^.*$
    the standard pattern for matching everything.
[^/.]
    defines one character that is neither a slash nor a dot.
[^/.]+
    defines any number of characters which contains neither slash nor dot.
http://
    this is a literal statement — in this case, the literal character string, "http://".
^domain.*
    defines a string that begins with the term "domain", which then may be proceeded by any number of any characters.
^domain\.com$
    defines the exact string "domain.com".
-d
    tests if string is an existing directory
-f
    tests if string is an existing file
-s
    tests if file in test string has a non-zero value

Redirection Header Codes ^

    301 – Moved Permanently
    302 – Moved Temporarily
    403 – Forbidden
    404 – Not Found
    410 – Gone

Essentials [ ^ ]
Commenting your htaccess Files ^

It is an excellent idea to consistenly and logically comment your htaccess files. Any line in an htaccess file that begins with the pound sign ( # ) tells the server to ignore it. Multiple lines require multiple pounds and use letters/numbers/dash/underscore only:

# this is a comment
# each line must have its own pound sign
# use only alphanumeric characters along with dashes - and underscores _
Enable Basic Rewriting ^

Certain servers may not have "mod_rewrite" enabled by default. To ensure mod_rewrite (basic rewriting) is enabled throughout your site, add the following line once to your site's root htaccess file:

# enable basic rewriting
RewriteEngine on
Enable Symbolic Links ^

Enable symbolic links (symlinks) by adding the following directive to the target directory's htaccess file. Note: for the FollowSymLinks directive to function, AllowOverride Options privileges must be enabled from within the server configuration file (see proceeding paragraph for more information):

# enable symbolic links
Options +FollowSymLinks
Enable AllowOverride ^

For directives that require AllowOverride in order to function, such as FollowSymLinks (see above paragraph), the following directive must be added to the server configuration file. For performance considerations, it is important to only enable AllowOverride in the specific directory or directories in which it is required. In the following code chunk, we are enabling the AllowOverride privs only in the specified directory (/www/replace/this/with/actual/directory). Refer to this section for more information about AllowOverride and performance enhancement:

# enable allowoverride privileges
<Directory /www/replace/this/with/actual/directory>
 AllowOverride Options
</Directory>
Rename the htaccess File ^

Not every system enjoys the extension-only format of htaccess files. Fortunately, you can rename them to whatever you wish, granted the name is valid on your system. Note: This directive must be placed in the server-wide configuration file or it will not work:

# rename htaccess files
AccessFileName ht.access

Note: If you rename your htaccess files, remember to update any associated configuration settings. For example, if you are protecting your htaccess file via FilesMatch, remember to inform it of the renamed files:

# protect renamed htaccess files
<FilesMatch "^ht\.">
 Order deny,allow
 Deny from all
</FilesMatch>
Retain Rules Defined in httpd.conf ^

Save yourself time and effort by defining replicate rules for multiple virtual hosts once and only once via your httpd.conf file. Then, simply instruct your target htaccess file(s) to inherit the httpd.conf rules by including this directive:

RewriteOptions Inherit
Performance [ ^ ]
Improving Performance via AllowOverride ^

Limit the extent to which htaccess files decrease performance by enabling AllowOverride only in required directories. For example, if AllowOverride is enabled throughout the entire site, the server must dig through every directory, searching for htaccess files that may not even exist. To prevent this, we disable the AllowOverride in the site's root htaccess file and then enable AllowOverride only in required directories via the server config file (refer to this section for more information). Note: if you do not have access to your site's server config file and also need AllowOverride privileges, do not use this directive:

# increase performance by disabling allowoverride
AllowOverride None
Improving Performance by Passing the Character Set ^

Prevent certain 500 error displays by passing the default character set parameter before you get there. Note: replace the "utf-8" below with the charset that your site is using:

# pass the default character set
AddDefaultCharset utf-8
Improving Performance by Preserving Bandwidth ^

To increase performance on PHP enabled servers, add the following directive:

# preserve bandwidth for PHP enabled servers
<ifmodule mod_php4.c>
 php_value zlib.output_compression 16386
</ifmodule>
Disable the Server Signature ^

Here we are disabling the digital signature that would otherwise identify the server:

# disable the server signature
ServerSignature Off
Set the Server Timezone ^

Here we are instructing the server to synchronize chronologically according to the time zone of some specified state:

# set the server timezone
SetEnv TZ America/Washington
Set the Email Address for the Server Administrator ^

Here we are specifying the default email address for the server administrator:

# set the server administrator email
SetEnv SERVER_ADMIN default@domain.com
Improve Site Transfer Speed by Enabling File Caching ^

The htaccess genius over at askapache.com explains how to dramatically improve your site's transfer speed by enabling file caching 3. Using time in seconds* to indicate the duration for which cached content should endure, we may generalize the htaccess rules as such (edit file types and time value to suit your needs):

# cache images and flash content for one month
<FilesMatch ".(flv|gif|jpg|jpeg|png|ico|swf)$">
Header set Cache-Control "max-age=2592000"
</FilesMatch>

# cache text, css, and javascript files for one week
<FilesMatch ".(js|css|pdf|txt)$">
Header set Cache-Control "max-age=604800"
</FilesMatch>

# cache html and htm files for one day
<FilesMatch ".(html|htm)$">
Header set Cache-Control "max-age=43200"
</FilesMatch>

# implement minimal caching during site development
<FilesMatch "\.(flv|gif|jpg|jpeg|png|ico|js|css|pdf|swf|html|htm|txt)$">
Header set Cache-Control "max-age=5"
</FilesMatch>

# explicitly disable caching for scripts and other dynamic files
<FilesMatch "\.(pl|php|cgi|spl|scgi|fcgi)$">
Header unset Cache-Control
</FilesMatch>

# alternate method for file caching
ExpiresActive On
ExpiresDefault A604800 # 1 week
ExpiresByType image/x-icon A2419200 # 1 month
ExpiresByType application/x-javascript A2419200 # 1 month
ExpiresByType text/css A2419200 # 1 month
ExpiresByType text/html A300 # 5 minutes
# disable caching for scripts and other dynamic files
<FilesMatch "\.(pl|php|cgi|spl|scgi|fcgi)$">
ExpiresActive Off
</FilesMatch>

    * Convert common time intervals into seconds:
    300 = 5 minutes
    2700 = 45 minutes
    3600 = 1 hour
    54000 = 15 hours
    86400 = 1 day
    518400 = 6 days
    604800 = 1 week
    1814400 = 3 weeks
    2419200 = 1 month
    26611200 = 11 months
    29030400 = 1 year = never expires

Set the default language and character set ^

Here is an easy way to set the default language for pages served by your server (edit the language to suit your needs):

# set the default language
DefaultLanguage en-US

Likewise, here we are setting the default character set (edit to taste):

# set the default character set
AddDefaultCharset UTF-8
Declare specific/additional MIME types ^

# add various mime types
AddType application/x-shockwave-flash .swf
AddType video/x-flv .flv
AddType image/x-icon .ico
Send character set and other headers without meta tags ^

# send the language tag and default character set
# AddType 'text/html; charset=UTF-8' html
AddDefaultCharset UTF-8
DefaultLanguage en-US
Limit server request methods to GET and PUT ^

# limit server request methods to GET and PUT
Options -ExecCGI -Indexes -All
RewriteEngine on
RewriteCond %{REQUEST_METHOD} ^(TRACE|TRACK|OPTIONS|HEAD) RewriteRule .* - [F]
Selectively process files according to server request method ^

# process files according to server request method
Script PUT /cgi-bin/upload.cgi
Script GET /cgi-bin/download.cgi
Execute various file types through a cgi script ^

For those special occasions where certain file types need to be processed with some specific cgi script, let em know who sent ya:

# execute all png files via png-script.cgi
Action image/png /cgi-bin/png-script.cgi
Security [ ^ ]
Prevent Access to .htaccess ^

Add the following code block to your htaccess file to add an extra layer of security. Any attempts to access the htaccess file will result in a 403 error message. Of course, your first layer of defense to protect htaccess files involves setting htaccess file permissions via CHMOD to 644:

# secure htaccess file
<Files .htaccess>
 order allow,deny
 deny from all
</Files>
Prevent Access to a Specific File ^

To restrict access to a specific file, add the following code block and edit the file name, "secretfile.jpg", with the name of the file that you wish to protect:

# prevent viewing of a specific file
<files secretfile.jpg>
 order allow,deny
 deny from all
</files>
Prevent access to multiple file types ^

To restrict access to a variety of file types, add the following code block and edit the file types within parentheses to match the extensions of any files that you wish to protect:

<FilesMatch "\.(htaccess|htpasswd|ini|phps|fla|psd|log|sh)$">
 Order Allow,Deny
 Deny from all
</FilesMatch>
Prevent Unauthorized Directory Browsing ^

Prevent unauthorized directory browsing by instructing the server to serve a "xxx Forbidden – Authorization Required" message for any request to view a directory. For example, if your site is missing it's default index page, everything within the root of your site will be accessible to all visitors. To prevent this, include the following htaccess rule:

# disable directory browsing
Options All -Indexes

Conversely, to enable directory browsing, use the following directive:

# enable directory browsing
Options All +Indexes

Likewise, this rule will prevent the server from listing directory contents:

# prevent folder listing
IndexIgnore *

And, finally, the IndexIgnore directive may be used to prevent the display of select file types:

# prevent display of select file types
IndexIgnore *.wmv *.mp4 *.avi *.etc
Change Default Index Page ^

This rule tells the server to search for and serve "business.html" as the default directory index. This rule must exist in the htaccess files of the root directory for which you wish to replace the default index file (e.g., "index.html"):

# serve alternate default index page
DirectoryIndex business.html

This rule is similar, only in this case, the server will scan the root directory for the listed files and serve the first match it encounters. The list is read from left to right:

# serve first available alternate default index page from series
DirectoryIndex filename.html index.cgi index.pl default.htm
Disguise Script Extensions ^

To enhance security, disguise scripting languages by replacing actual script extensions with dummy extensions of your choosing. For example, to change the ".foo" extension to ".php", add the following line to your htaccess file and rename all affected files accordingly:

# serve foo files as php files
AddType application/x-httpd-php .foo

# serve foo files as cgi files
AddType application/x-httpd-cgi .foo
Limit Access to the Local Area Network (LAN) ^

# limit access to local area network
<Limit GET POST PUT>
 order deny,allow
 deny from all
 allow from 192.168.0.0/33
</Limit>
Secure Directories by IP Address and/or Domain ^

In the following example, all IP addresses are allowed access except for 12.345.67.890 and domain.com:

# allow all except those indicated here
<Limit GET POST PUT>
 order allow,deny
 allow from all
 deny from 12.345.67.890
 deny from .*domain\.com.*
</Limit>

In the following example, all IP addresses are denied access except for 12.345.67.890 and domain.com:

# deny all except those indicated here
<Limit GET POST PUT>
 order deny,allow
 deny from all
 allow from 12.345.67.890
 allow from .*domain\.com.*
</Limit>

This is how to block unwanted visitors based on the referring domain. You can also save bandwidth by blocking specific file types — such as .jpg, .zip, .mp3, .mpg — from specific referring domains. Simply replace "scumbag" and "wormhole" with the offending domains of your choice:

# block visitors referred from indicated domains
<IfModule mod_rewrite.c>
 RewriteEngine on
 RewriteCond %{HTTP_REFERER} scumbag\.com [NC,OR]
 RewriteCond %{HTTP_REFERER} wormhole\.com [NC,OR]
 RewriteRule .* - [F]
</ifModule>
Prevent or allow domain access for a specified range of IP addresses ^

There are several effective ways to block a range of IP addresses via htaccess. This first method blocks an IP range specified by their CIDR (Classless Inter-Domain Routing) number. This method is useful for blocking mega-spammers such as RIPE, Optinet, and others. If, for example, you find yourself adding line after line of Apache deny directives for addresses beginning with the same first few numbers, choose one of them and try a whois lookup. Listed within the whois results will be the CIDR value representing every IP address associated with that particular network. Thus, blocking via CIDR is an effective way to eloquently prevent all IP instances of the offender from accessing your site. Here is a generalized example for blocking by CIDR (edit values to suit your needs):

# block IP range by CIDR number
<Limit GET POST PUT>
 order allow,deny
 allow from all
 deny from 10.1.0.0/16
 deny from 80.0.0/8
</Limit>

Likewise, to allow an IP range by CIDR number:

# allow IP range by CIDR number
<Limit GET POST PUT>
 order deny,allow
 deny from all
 allow from 10.1.0.0/16
 allow from 80.0.0/8
</Limit>

Another effective way to block an entire range of IP addresses involves truncating digits until the desired range is represented. As an IP address is read from left to right, its value represents an increasingly specific address. For example, a fictitious IP address of 99.88.77.66 would designate some uniquely specific IP address. Now, if we remove the last two digits (66) from the address, it would represent any address beginning with the remaining digits. That is, 99.88.77 represents 99.88.77.1, 99.88.77.2, … 99.88.77.99, …etc. Likewise, if we then remove another pair of digits from the address, its range suddenly widens to represent every IP address 99.88.x.y, where x and y represent any valid set of IP address values (i.e., you would block 256*256 = 65,536 unique IP addresses). Following this logic, it is possible to block an entire range of IP addresses to varying degrees of specificity. Here are few generalized lines exemplifying proper htaccess syntax (edit values to suit your needs):

# block IP range by address truncation
<Limit GET POST PUT>
 order allow,deny
 allow from all
 deny from 99.88.77.66
 deny from 99.88.77.*
 deny from 99.88.*.*
 deny from 99.*.*.*
</Limit>

Likewise, to allow an IP range by address truncation:

# allow IP range by address truncation
<Limit GET POST PUT>
 order deny,allow
 deny from all
 allow from 99.88.77.66
 allow from 99.88.77.*
 allow from 99.88.*.*
 allow from 99.*.*.*
</Limit>
Block or allow multiple IP addresses on one line ^

Save a little space by blocking multiple IP addresses or ranges on one line. Here are few examples (edit values to suit your needs):

# block two unique IP addresses
deny from 99.88.77.66 11.22.33.44
# block three ranges of IP addresses
deny from 99.88 99.88.77 11.22.33

Likewise, to allow multiple IP addresses or ranges on one line:

# allow two unique IP addresses
allow from 99.88.77.66 11.22.33.44
# allow three ranges of IP addresses
allow from 99.88 99.88.77 11.22.33
Miscellaneous rules for blocking and allowing IP addresses ^

Here are few miscellaneous rules for blocking various types of IP addresses. These rules may be adapted to allow the specified IP values by simply changing the deny directive to allow. Check 'em out (edit values to suit your needs):

# block a partial domain via network/netmask values
deny from 99.1.0.0/255.255.0.0

# block a single domain
deny from 99.88.77.66

# block domain.com but allow sub.domain.com
order deny,allow
deny from domain.com
allow from sub.domain.com
Stop Hotlinking, Serve Alternate Content ^

To serve 'em some unexpected alternate content when hotlinking is detected, employ the following code, which will protect all files of the types included in the last line (add more types as needed). Remember to replace the dummy path names with real ones. Also, the name of the nasty image being served in this case is "eatme.jpe", as indicated in the line containing the RewriteRule. Please advise that this method will also block services such as FeedBurner from accessing your images.

# stop hotlinking and serve alternate content
<IfModule mod_rewrite.c>
 RewriteEngine on
 RewriteCond %{HTTP_REFERER} !^$
 RewriteCond %{HTTP_REFERER} !^http://(www\.)?domain\.com/.*$ [NC]
 RewriteRule .*\.(gif|jpg)$ http://www.domain.com/eatme.jpe [R,NC,L]
</ifModule>

Note: To deliver a standard (or custom, if configured) error page instead of some nasty image of the Fonz, replace the line containing the RewriteRule in the above htaccess directive with the following line:

# serve a standard 403 forbidden error page
RewriteRule .*\.(gif|jpg)$ - [F,L]

Note: To grant linking permission to a site other than yours, insert this code block after the line containing the "domain.com" string. Remember to replace "goodsite.com" with the actual site domain:

# allow linking from the following site
RewriteCond %{HTTP_REFERER} !^http://(www\.)?goodsite\.com/.*$ [NC]
Block Evil Robots, Site Rippers, and Offline Browsers ^

Eliminate some of the unwanted scum from your userspace by injecting this handy block of code. After such, any listed agents will be denied access and receive an error message instead. Please advise that there are much more comprehensive lists available this example has been truncated for business purposes. Note: DO NOT include the "[OR]" on the very last RewriteCond or your server will crash, delivering "500 Errors" to all page requests.

# deny access to evil robots site rippers offline browsers and other nasty scum
RewriteBase /
RewriteCond %{HTTP_USER_AGENT} ^Anarchie [OR]
RewriteCond %{HTTP_USER_AGENT} ^ASPSeek [OR]
RewriteCond %{HTTP_USER_AGENT} ^attach [OR]
RewriteCond %{HTTP_USER_AGENT} ^autoemailspider [OR]
RewriteCond %{HTTP_USER_AGENT} ^Xaldon\ WebSpider [OR]
RewriteCond %{HTTP_USER_AGENT} ^Xenu [OR]
RewriteCond %{HTTP_USER_AGENT} ^Zeus.*Webster [OR]
RewriteCond %{HTTP_USER_AGENT} ^Zeus
RewriteRule ^.* - [F,L]

Or, instead of delivering a friendly error message (i.e., the last line), send these bad boys to the hellish website of your choice by replacing the RewriteRule in the last line with one of the following two examples:

# send em to a hellish website of your choice
RewriteRule ^.*$ http://www.hellish-website.com [R,L]

Or, to send em to a virtual blackhole of fake email addresses:

# send em to a virtual blackhole of fake email addresses
RewriteRule ^.*$ http://english-61925045732.spampoison.com [R,L]

You may also include specific referrers to your blacklist by using HTTP_REFERER. Here, we use the infamously scummy domain, "iaea.org" as our blocked example, and we use "yourdomain" as your domain (the domain to which you are blocking iaea.org):

RewriteCond %{HTTP_REFERER} ^http://www.iaea.org$
RewriteRule !^http://[^/.]\.yourdomain\.com.* - [F,L]
More Stupid Blocking Tricks ^

Note: Although these redirect techniques are aimed at blocking and redirecting nasty scumsites, the directives may also be employed for friendly redirection purposes:

# redirect any request for anything from spamsite to differentspamsite
RewriteCond %{HTTP_REFERER} ^http://.*spamsite.*$ [NC]
RewriteRule .* http://www.differentspamsite.com [R]

# redirect all requests from spamsite to an image of something at differentspamsite
RewriteCond %{HTTP_REFERER} ^http://.*spamsite.*$ [NC]
RewriteRule .* http://www.differentspamsite/something.jpg [R]

# redirect traffic from a certain address or range of addresses to another site
RewriteCond %{REMOTE_ADDR} 192.168.10.*
RewriteRule .* http://www.differentspamsite.com/index.html [R]
Even More Scum-Blocking Tricks ^

Here is a step-by-step series of code blocks that should equip you with enough knowledge to block any/all necessary entities. Read through the set of code blocks, observe the patterns, and then copy, combine and customize to suit your specific scum-blocking needs:

# set variables for user agents and referers and ip addresses
SetEnvIfNoCase User-Agent ".*(user-agent-you-want-to-block|php/perl).*" BlockedAgent
SetEnvIfNoCase Referer ".*(block-this-referrer|and-this-referrer|and-this-referrer).*" BlockedReferer
SetEnvIfNoCase REMOTE_ADDR ".*(666.666.66.0|22.22.22.222|999.999.99.999).*" BlockedAddress

# set variable for any class B network coming from a given netblock
SetEnvIfNoCase REMOTE_ADDR "66.154.*" BlockedAddress

# set variable for two class B networks 198.25.0.0 and 198.26.0.0
SetEnvIfNoCase REMOTE_ADDR "198.2(5|6)\..*" BlockedAddress

# deny any matches from above and send a 403 denied
<Limit GET POST PUT>
 order deny,allow
 deny from env=BlockedAgent
 deny from env=BlockedReferer
 deny from env=BlockedAddress
 allow from all
</Limit>
Password-Protect Directories ^

Here is an excellent online tool for generating the necessary elements for a password-protected directory:

# password protect directories
htaccess Password Generator
Password-protect Files, Directories, and More.. ^

Secure site contents by requiring user authentication for specified files and/or directories. The first example shows how to password-protect any single file type that is present beneath the directory which houses the htaccess rule. The second rule employs the FilesMatch directive to protect any/all files which match any of the specified character strings. The third rule demonstrates how to protect an entire directory. The fourth set of rules provides password-protection for all IP's except those specified. Remember to edit these rules according to your specific needs.

# password-protect single file
<Files secure.php>
AuthType Basic
AuthName "Prompt"
AuthUserFile /home/path/.htpasswd
Require valid-user
</Files>

# password-protect multiple files
<FilesMatch "^(execute|index|secure|insanity|biscuit)*$">
AuthType basic
AuthName "Development"
AuthUserFile /home/path/.htpasswd
Require valid-user
</FilesMatch>

# password-protect the directory in which this htaccess rule resides
AuthType basic
AuthName "This directory is protected"
AuthUserFile /home/path/.htpasswd
AuthGroupFile /dev/null
Require valid-user

# password-protect directory for every IP except the one specified
# place in htaccess file of a directory to protect that entire directory
AuthType Basic
AuthName "Personal"
AuthUserFile /home/path/.htpasswd
Require valid-user
Allow from 99.88.77.66
Satisfy Any
Require SSL (Secure Sockets Layer) ^

Here is an excellent method for requiring SSL (via askapache.com 3):

# require SSL
SSLOptions +StrictRequire
SSLRequireSSL
SSLRequire %{HTTP_HOST} eq "domain.tld"
ErrorDocument 403 https://domain.tld

# require SSL without mod_ssl
RewriteCond %{HTTPS} !=on [NC]
RewriteRule ^.*$ https://%{SERVER_NAME}%{REQUEST_URI} [R,L]
Automatically CHMOD Various File Types ^

This method is great for ensuring the CHMOD settings for various file types. Employ the following rules in the root htaccess file to affect all specified file types, or place in a specific directory to affect only those files (edit file types according to your needs):

# ensure CHMOD settings for specified file types
# remember to never set CHMOD 777 unless you know what you are doing
# files requiring write access should use CHMOD 766 rather than 777
# keep specific file types private by setting their CHMOD to 400
chmod .htpasswd files 640
chmod .htaccess files 644
chmod php files 600
Disguise all file extensions ^

This method will disguise all file types (i.e., any file extension) and present them as .php files (or whichever extension you choose):

# diguise all file extensions as php
ForceType application/x-httpd-php
Protect against denial-of-service (DOS) attacks by limiting file upload size ^

One method to help protect your server against DOS attacks involves limiting the maximum allowable size for file uploads. Here, we are limiting file upload size to 10240000 bytes, which is equivalent to around 10 megabytes. For this rule, file sizes are expressed in bytes. Check here for help with various file size conversions. Note: this code is only useful if you actually allow users to upload files to your site.

# protect against DOS attacks by limiting file upload size
LimitRequestBody 10240000
Secure directories by disabling execution of scripts ^

Prevent malicious brainiacs from actively scripting secure directories by adding the following rules to the representative htaccess file (edit file types to suit your needs):

# secure directory by disabling script execution
AddHandler cgi-script .php .pl .py .jsp .asp .htm .shtml .sh .cgi
Options -ExecCGI
Usability Tricks [ ^ ]
Minimize CSS Image Flicker in IE6 ^

Add the following htaccess rules to minimize or even eliminate CSS background-image "flickering" in MSIE6:

# minimize image flicker in IE6
ExpiresActive On
ExpiresByType image/gif A2592000
ExpiresByType image/jpg A2592000
ExpiresByType image/png A2592000
Deploy Custom Error Pages ^

Replicate the following patterns to serve your own set of custom error pages. Simply replace the "/errors/###.html" with the correct path and file name. Also change the "###" preceding the path to summon pages for other errors. Note: your custom error pages must be larger than 512 bytes in size or they will be completely ignored by Internet Explorer:

# serve custom error pages
ErrorDocument 400 /errors/400.html
ErrorDocument 401 /errors/401.html
ErrorDocument 403 /errors/403.html
ErrorDocument 404 /errors/404.html
ErrorDocument 500 /errors/500.html
Provide a Universal Error Document ^

# provide a universal error document
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^.*$ /dir/error.php [L]
Employ Basic URL Spelling Check ^

This bit of voodoo will auto-correct simple spelling errors in the URL:

# automatically corect simple speling erors
<IfModule mod_speling.c>
 CheckSpelling On
</IfModule>
Instruct browser to download multimedia files rather than display them ^

Here is a useful method for delivering multimedia file downloads to your users. Typically, browsers will attempt to play or stream such files when direct links are clicked. With this method, provide a link to a multimedia file and a dialogue box will provide users the choice of saving the file or opening it. Here are a few htaccess rules demonstrating the technique (edit file types according to your specific needs):

# instruct browser to download multimedia files
AddType application/octet-stream .avi
AddType application/octet-stream .mpg
AddType application/octet-stream .wmv
AddType application/octet-stream .mp3
Instruct server to display source code for dynamic file types ^

There are many situations where site owners may wish to display the contents of a dynamic file rather than executing it as a script. To exercise this useful technique, create a directory in which to place dynamic files that should be displayed rather than executed, and add the following line of code to the htaccess file belonging to that directory. This method is known to work for .pl, .py, and .cgi file-types. Here it is:

RemoveHandler cgi-script .pl .py .cgi
Redirect visitors to a temporary site during site development ^

During web development, maintenance, or repair, send your visitors to an alternate site while retaining full access for yourself. This is a very useful technique for preventing visitor confusion or dismay during those awkward, web-development moments. Here are the generalized htaccess rules to do it (edit values to suit your needs):

# redirect all visitors to alternate site but retain full access for you
ErrorDocument 403 http://www.alternate-site.com
Order deny,allow
Deny from all
Allow from 99.88.77.66
Provide a password prompt for visitors during site development ^

Here is another possible solution for "hiding" your site during those private, site-under-construction moments. Here we are instructing Apache to provide visitors with a password prompt while providing open access to any specifically indicated IP addresses or URL's. Edit the following code according to your IP address and other development requirements (thanks to Caleb at askapache.com for sharing this trick 3):

# password prompt for visitors
AuthType basic
AuthName "This site is currently under construction"
AuthUserFile /home/path/.htpasswd
AuthGroupFile /dev/null
Require valid-user
# allow webmaster and any others open access
Order Deny,Allow
Deny from all
Allow from 111.222.33.4
Allow from favorite.validation/services/
Allow from googlebot.com
Satisfy Any
Prevent file or directory access according to specified time periods ^

Prevent viewing of all pictures of Fonzi during the midnight hour — or any files during any time period — by using this handy htaccess ruleset:

# prevent access during the midnight hour
RewriteCond %{TIME_HOUR} ^12$
RewriteRule ^.*$ - [F,L]

# prevent access throughout the afternoon
RewriteCond %{TIME_HOUR} ^(12|13|14|15)$
RewriteRule ^.*$ - [F,L]
Redirect Tricks [ ^ ]
Important Note About Redirecting via mod_rewrite ^

For all redirects using the mod_rewrite directive, it is necessary to have the RewriteEngine enabled. It is common practice to enable the mod_rewrite directive in either the server configuration file or at the top of the site's root htaccess file. If the mod_rewrite directive is not included in either of these two places, it should be included as the first line in any code block that utilizes a rewrite function (i.e., mod_rewrite), but only needs to be included once for each htaccess file. The proper mod_rewrite directive is included here for your convenience, but may or may not also be included within some of the code blocks provided in this article:

# initialize and enable rewrite engine
RewriteEngine on
Redirect from http://www.domain.com to http://domain.com ^

This method uses a "301 redirect" to establish a permanent redirect from the "www-version" of a domain to its respectively corresponding "non-www version". Be sure to test immediately after preparing 301 redirects and remove it immediately if any errors occur. Use a "server header checker" to confirm a positive 301 response. Further, always include a trailing slash "/" when linking directories. Finally, be consistent with the "www" in all links (either use it always or never).

# permanently redirect from www domain to non-www domain
RewriteEngine on
Options +FollowSymLinks
RewriteCond %{HTTP_HOST} ^www\.domain\.tld$ [NC]
RewriteRule ^(.*)$ http://domain.tld/$1 [R=301,L]
Redirect from http://old-domain.com to http://new-domain.com ^

For a basic domain change from "old-domain.com" to "new-domain.com" (and folder/file names have not been changed), use the Rewrite rule to remap the old domain to the new domain. When checking the redirect live, the old domain may appear in the browser's address bar. Simply check an image path (right-click an image and select "properties") to verify proper redirection. Remember to check your site thoroughly after implementing this redirect.

# redirect from old domain to new domain
RewriteEngine On
RewriteRule ^(.*)$ http://www.new-domain.com/$1 [R=301,L]
Redirect String Variations to a Specific Address ^

For example, if we wanted to redirect any requests containing the character string, "perish", to our main page at http://perishablepress.com/, we would replace "some-string" with "perish" in the following code block:

# redirect any variations of a specific character string to a specific address
RewriteRule ^some-string http://www.domain.com/index.php/blog/target [R]

Here are two other methods for accomplishing string-related mapping tasks:

# map URL variations to the same directory on the same server
AliasMatch ^/director(y|ies) /www/docs/target

# map URL variations to the same directory on a different server
RedirectMatch ^/[dD]irector(y|ies) http://domain.com
Other Fantastic Redirect Tricks ^

Redirect an entire site via 301:

# redirect an entire site via 301
redirect 301 / http://www.domain.com/

Redirect a specific file via 301:

# redirect a specific file via 301
redirect 301 /current/currentfile.html http://www.newdomain.com/new/newfile.html

Redirect an entire site via permanent redirect:

# redirect an entire site via permanent redirect
Redirect permanent / http://www.domain.com/

Redirect a page or directory via permanent redirect:

# redirect a page or directory
Redirect permanent old_file.html http://www.new-domain.com/new_file.html
Redirect permanent /old_directory/ http://www.new-domain.com/new_directory/

Redirect a file using RedirectMatch:

# redirect a file using RedirectMatch
RedirectMatch 301 ^.*$ http://www.domain.com/index.html

Note: When redirecting specific files, use Apache's Redirect rule for files within the same domain. Use Apache's RewriteRule for any domains, especially if they are different. The RewriteRule is more powerful than the Redirect rule, and thus should serve you more effectively.

Thus, use the following for a stronger, harder page redirection (first line redirects a file, second line a directory, and third a domain):

# redirect files directories and domains via RewriteRule
RewriteRule http://old-domain.com/old-file.html http://new-domain.com/new-file.html
RewriteRule http://old-domain.com/old-dir/ http://new-domain.com/new-dir/
RewriteRule http://old-domain.com/ http://new-domain.com/
Send visitors to a subdomain ^

This rule will ensure that all visitors are viewing pages via the subdomain of your choice. Edit the "subdomain", "domain", and "tld" to match your subdomain, domain, and top-level domain respectively:

# send visitors to a subdomain
RewriteCond %{HTTP_HOST} !^$
RewriteCond %{HTTP_HOST} !^subdomain\.domain\.com$ [NC]
RewriteRule ^/(.*)$ http://subdomain.domain.tld/$1 [L,R=301]
More fun with RewriteCond and RewriteRule ^

# rewrite only if the file is not found
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.+)special\.html?$ cgi-bin/special/special-html/$1

# rewrite only if an image is not found
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule images/special/(.*).gif cgi-bin/special/mkgif?$1

# seo-friendly rewrite rules for various directories
RewriteRule ^(.*)/aud/(.*)$ $1/audio-files/$2 [L,R=301]
RewriteRule ^(.*)/img/(.*)$ $1/image-files/$2 [L,R=301]
RewriteRule ^(.*)/fla/(.*)$ $1/flash-files/$2 [L,R=301]
RewriteRule ^(.*)/vid/(.*)$ $1/video-files/$2 [L,R=301]

# broswer sniffing via htaccess environmental variables
RewriteCond %{HTTP_USER_AGENT} ^Mozilla.*
RewriteRule ^/$ /index-for-mozilla.html [L]
RewriteCond %{HTTP_USER_AGENT} ^Lynx.*
RewriteRule ^/$ /index-for-lynx.html [L]
RewriteRule ^/$ /index-for-all-others.html [L]

# redirect query to Google search
Options +FollowSymlinks
RewriteEngine On
RewriteCond %{REQUEST_URI} .google\.php*
RewriteRule ^(.*)$ ^http://www.google.com/search?q=$1 [R,NC,L]

# deny request according to the request method
RewriteCond %{REQUEST_METHOD} ^(TRACE|TRACK|OPTIONS|HEAD)$ [NC]
RewriteRule ^.*$ - [F]

# redirect uploads to a better place
RewriteCond %{REQUEST_METHOD} ^(PUT|POST)$ [NC]
RewriteRule ^(.*)$ /cgi-bin/upload-processor.cgi?p=$1 [L,QSA]
More fun with Redirect 301 and RedirectMatch 301 ^

# seo friendly redirect for a single file
Redirect 301 /old-dir/old-file.html http://domain.com/new-dir/new-file.html

# seo friendly redirect for multiple files
# redirects all files in dir directory with first letters xyz
RedirectMatch 301 /dir/xyz(.*) http://domain.com/$1

# seo friendly redirect entire site to a different domain
Redirect 301 / http://different-domain.com
WordPress Tricks [ ^ ]
Secure WordPress Contact Forms ^

Protect your insecure WordPress contact forms against online unrighteousness by verifying the domain from whence the form is called. Remember to replace the "domain.com" and "contact.php" with your domain and contact-form file names, respectively.

# secure wordpress contact forms via referrer check
RewriteCond %{HTTP_REFERER} !^http://www.domain.com/.*$ [NC]
RewriteCond %{REQUEST_POST} .*contact.php$
RewriteRule .* - [F]
WordPress Permalinks ^

In our article, The htaccess rules for all WordPress Permalinks, we revealed the precise htaccess directives used by the WordPress blogging platform for permalink functionality. Here, for the sake of completeness, we repeat the directives only. For more details please refer to the original article:

If WordPress is installed in the site's root directory, WordPress creates and uses the following htaccess directives:

# BEGIN WordPress
<IfModule mod_rewrite.c>
 RewriteEngine On
 RewriteBase /
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule . /index.php [L]
</IfModule>
# END WordPress

If WordPress is installed in some subdirectory "foo", WordPress creates and uses the following htaccess directives:

# BEGIN WordPress
<IfModule mod_rewrite.c>
 RewriteEngine On
 RewriteBase /foo/
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule . /foo/index.php [L]
</IfModule>
# END WordPress
Random Tricks [ ^ ]
Activate SSI for HTML/SHTML file types: ^

# activate SSI for HTML and or SHTML file types
AddType text/html .html
AddType text/html .shtml
AddHandler server-parsed .html
AddHandler server-parsed .shtml
AddHandler server-parsed .htm
Grant CGI access in a specific directory: ^

# grant CGI access in a specific directory
Options +ExecCGI
AddHandler cgi-script cgi pl
# to enable all scripts in a directory use the following
SetHandler cgi-script
Disable magic_quotes_gpc for PHP enabled servers: ^

# turn off magic_quotes_gpc for PHP enabled servers
<ifmodule mod_php4.c>
 php_flag magic_quotes_gpc off
</ifmodule>
Enable MD5 digests: ^

Note: enabling this option may result in a relative decrease in server performance.

# enable MD5 digests via ContentDigest
ContentDigest On
Expression Engine Tricks: ^

# send Atom and RSS requests to the site docroot to be rewritten for ExpressionEngine
RewriteRule .*atom.xml$ http://www.yoursite.com/index.php/weblog/rss_atom/ [R]
RewriteRule .*rss.xml$ http://www.yoursite.com/index.php/weblog/rss_2.0/ [R]

# cause all requests for index.html to be rewritten for ExpressionEngine
RewriteRule /.*index.html$ http://www.domain.com/index.php [R]