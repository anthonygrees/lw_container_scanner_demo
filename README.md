# Lacework Inline Scanner GitHub Actions Container Demo.
GitHub Actions demo for Lacework Inline Scanner.
  
### About
This action pulls a PacMan Container and then Scan's it with the Lacework Inline CVE scanner.  
  
### Expected Inputs
It required two inputs as GitHub Secrets:
- secrets.LW_ACCOUNT_NAME
- secrets.LW_ACCESS_TOKEN
  
In your GitHub repo, go into `Settings` -> `Secrets` and create Repository Secrets as follows.  They are encrypted by default.
  
![Secrets](/images/lw_sec.png)

### Integration and Token
In the Lacework UI, se;ect `Settings` -> `Account Settings` -> `Integrations` -> `Container Registries`   
  
Create a `New` and choose a `Registry Type` of `Inline Scanner`.  You cannot use an Agent Token or API Key.
  
![Registration](/images/lw_reg.png)
  
### Lacework Scanner
This version uses a GitHub action that is published to provide the LW Scanner   
`timarenz/lw-scanner-action@main`  
  
The code to make a CI/CD pipeline work is [here.](https://github.com/timarenz/lw-scanner-action/blob/main/docker-entrypoint.sh)
  
You will need to add the following `step` to your GitHub Actions workflow:  
```yml
      - name: Scan container images for vulnerabitilies using Lacework
        uses: timarenz/lw-scanner-action@main
        env:
          LW_ACCOUNT_NAME: ${{ secrets.LW_ACCOUNT_NAME }} 
          LW_ACCESS_TOKEN: ${{ secrets.LW_ACCESS_TOKEN }}
        with:
          image_name: uzyexe/pacman
          image_tag: latest
          fail_if_critical_vulnerabilities_found: true
          fail_only_if_vulnerabilities_fixable: true
          save_build_report: true
```
  
### Lacework Inline Scanner Output
The CLI output will look like this:
  
```bash
                                  CONTAINER IMAGE DETAILS                                          VULNERABILITIES          
------------------------------------------------------------------------------------------+---------------------------------
    ID          sha256:c691d574154e15eb43c7078e977e97b35d84a2cc08e7ccf4a58f4a10b32aa4a4       SEVERITY   COUNT   FIXABLE    
    Digest      sha256:647f6d4b8f08950fa42a206e83ee2b65de97d040441d8f85dee721c093cd882a     -----------+-------+----------  
    Registry    remote_scanner                                                                Critical      10        10    
    Repository  uzyexe/pacman                                                                 High          14        14    
    Size        66.4 MB                                                                       Medium         8         8    
    Created At  2016-06-28T06:06:28.637Z                                                      Low            0         0    
    Tags        latest                                                                        Info           0         0    
                                                                                                                            
      CVE ID       SEVERITY   PACKAGE    CURRENT VERSION   FIX VERSION                            INTRODUCED IN LAYER                           
-----------------+----------+----------+-----------------+-------------+------------------------------------------------------------------------
  CVE-2017-8105    Critical   freetype   2.6.2-r0          2.6.3-r0      addgroup -S nginx && adduser -D -S -h /var/cache/nginx                 
                                                                         -s /sbin/nologin -G nginx nginx && apk add --no-cache                  
                                                                         --virtual .build-deps gcc libc-dev make openssl-dev                    
                                                                         pcre-dev zlib-dev linux-headers curl gnupg                             
......                 
                                                                         apk del .build-deps && rm -rf /usr/src/nginx-$NGINX_VERSION            
                                                                         && apk add --no-cache gettext && ln -sf /dev/stdout                    
                                                                         /var/log/nginx/access.log && ln -sf /dev/stderr                        
                                                                         /var/log/nginx/error.log                                               
-----------------+----------+----------+-----------------+-------------+------------------------------------------------------------------------
  CVE-2016-9843    Critical   zlib       1.2.8-r2          1.2.11-r0     ADD                                                                    
                                                                         file:86864edb9037700501e6e016262c29922e0c67762b4525901ca5a8194a078bfb  
                                                                         in /                                                                   
-----------------+----------+----------+-----------------+-------------+------------------------------------------------------------------------
  CVE-2016-6912    Critical   gd         2.1.1-r1          2.2.4-r0      addgroup -S nginx && adduser -D -S -h /var/cache/nginx                 
                                                                         -s /sbin/nologin -G nginx nginx && apk add --no-cache                  
                                                                         --virtual .build-deps gcc libc-dev make openssl-dev                    
                                                                         pcre-dev zlib-dev linux-headers curl gnupg                                      
                                                                         /var/log/nginx/error.log                                               
-----------------+----------+----------+-----------------+-------------+------------------------------------------------------------------------
  CVE-2016-10166   Critical   gd         2.1.1-r1          2.2.4-r0      addgroup -S nginx && adduser -D -S -h /var/cache/nginx                 
                                                                         -s /sbin/nologin -G nginx nginx && apk add --no-cache                  
                                                                         --virtual .build-deps gcc libc-dev make openssl-dev                    
                                                                         pcre-dev zlib-dev linux-headers curl gnupg                                                                    
....
                                                                         /usr/lib/nginx/modules/*.so | awk '{ gsub(/,/, "\nso:", $2);           
                                                                         print "so:" $2 }' | sort -u | xargs -r apk info --installed            
                                                                         | sort -u )" && apk add --virtual .nginx-rundeps $runDeps &&                           
                                                                         /var/log/nginx/access.log && ln -sf /dev/stderr                        
                                                                         /var/log/nginx/error.log                                               
-----------------+----------+----------+-----------------+-------------+------------------------------------------------------------------------
  CVE-2016-8859    Critical   musl       1.1.12-r5         1.1.12-r6     ADD                                                                    
                                                                         file:86864edb9037700501e6e016262c29922e0c67762b4525901ca5a8194a078bfb  
                                                                         in /                                                                   
-----------------+----------+----------+-----------------+-------------+------------------------------------------------------------------------

```


### Lacework Vulnerability Report
Once the GitHub action flow is complete, you will see the container scan report in Lacework.  Under `Vulnerabilities` -> `Container` select the `remote_scanner` in the drop-down menu at the top.  
  
![Vuls](/images/lw_vul.png)
