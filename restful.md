Barrel Development Best Practices

API/REST/Server Best Practices
------------------

- Try to avoid using plugins (unsure if this can be a best practice, but apis change fast so it's nice to be incontrol)
- When pulling data from a third party api (ex. twitter, facebook), always have a backup for if/**when** the api fails, this could be a db to pull the last posts from, etc. 

## Apache Configuration

### .htaccess

Here are a couple of .htaccess rules used in several sites to improve caching.

1.  Vary: Accept-Encoding - *More research is needed to clearly understand what this rule does.*

        <IfModule mod_headers.c>
        	<FilesMatch "\.(js|css|svg|woff|xml|gz)$">
        		Header append Vary: Accept-Encoding
        	</FilesMatch>
        </IfModule> 

2.  Mod Expires - *More research is needed to determine which approach is better*

        <FilesMatch "\.(gif|jpg|jpeg|png|swf|svg|css)$">
        	ExpiresActive on
        	ExpiresByType image/png "access plus 1 month"
        	ExpiresByType image/gif "access plus 1 month"
        	ExpiresByType image/jpeg "access plus 1 month"
        	ExpiresByType image/svg+xml "access plus 1 month"
        	ExpiresByType text/css "access plus 7 days"
        	Header append Cache-Control "public"
        </FilesMatch>

  **OR**

        <IfModule mod_expires.c>
        	ExpiresActive On
        	<FilesMatch "\.(ico|pdf|flv|jpg|jpeg|png|gif|js|css|svg|swf)$">
        		ExpiresDefault "access plus 1 year"
        	</FilesMatch>
        </IfModule>

##CDN, Sharding, or cookieless domain

More research required.