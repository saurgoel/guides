# ================== CERTS FILES
# All the certs files are located in 
# /etc/ssl/certs
# This is required if your system wants to work on the https protocol and make connections
# do not delete these files


# ======================================= GENRATING SSL CSR FILE
# creating the server private key
openssl genrsa -des3 -out server.key 1024

# for 2048
openssl genrsa -des3 -out server.key 2048

# crete the certifcate signing request
openssl req -new -key server.key -out server.csr

# a challenge password contains only letters and digits only
https://www.youtube.com/watch?v=X9kcvvQfhvs
https://guides.spreecommerce.com/developer/requesting_and_configuring_ssl.html

# nginx configuration
http://pricees.github.io/2014/03/25/aws-elb-nginx-rails-ssl-the-final-word/


# For the certificate SSL to work
# There are 2 entities invloved
- The domain provider
- The certificate issuer

# Step 1 - Generating a CSR (certificate signin request) and a private key
# These are generated using a web control panel or using a command line tool
First generate a certificate signing request
It is a request that you need to singin a certificate
You need to generate a CSR (certificate signing request) if you are:
Once this is generated you need to send the CSR (not the private key) to the SSL provider

Common Name:* https:// amberstudent.com
Organization:*  
Department:*  Technology
City:*   Mumbai
State:*   Maharashtra
Algorithm:*  
Country:*  
Email:*  contact@amberstudent.com



The main certificate is the one with the domain name
The others are CA certifiacated that are needed for autnentication

https://www.youtube.com/watch?v=_9NKELuDvQg


# The cerifiate files
# These are the files that are the actual ceritifcates that are installed on the server
# There are serveral extensions for certificate files like
- crt 
- pem
- der
- cer 
# files


Purchase and Install an SSL Certificate 	14:10
2	Testing and Debugging SSL Certificates 	06:27
3	Self Signed SSL Certificates for Development Free!	10:28
4	Purchase & Install a Wildcard SSL Certificate 

# Rails force SSL
# It doesn't just force your browser to redirect HTTP to HTTPS. It also sets your cookies to be marked "secure", and it enables HSTS, each of which are very good protections against SSL stripping.
# Even though HTTPS protects your app at "https://example.com/yourapp" against MITM attacks, if someone gets between your client and your server they can rather easily get you to visit "http://example.com/yourapp". With neither of the above protections, your browser will happily send the session cookie to the person doing the MITM.

# Use the following url for key without requirement for password
http://stackoverflow.com/questions/9380403/what-does-ssl-ctx-use-privatekey-file-problems-getting-password-error-indica


# ceritifcation installaiotn procediure comodo
https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/789/37/certificate-installation-nginx


# ===================== Amazon Cloudfront with SNI and SSL support
# https://jaswsinc.com/s3-cloudfront-custom-ssl-free-sni/
# SNI is a cool new tehcnology that can be used to port the ssl certificates to new domain
# This is used in case of SSL certificates where we want the host (which is different) to serve and provide the SSL certiciate
# For example if you have cdn.example.com that gets resolved to something.cloudfront.com and you are hosting ssl certificates on cloudfront
# Then these can be used up by cdn.example.com

# SNI and HSTS
http://www.networking4all.com/en/ssl+certificates/faq/server+name+indication/
https://scotthelme.co.uk/setting-up-hsts-in-nginx/



# ================== USING HSTS (Strict Transport Security)
# https://scotthelme.co.uk/setting-up-hsts-in-nginx/
 # In essence, the host server returns the HSTS header with responses sent over HTTPS. Once the browser picks up on the header, it will store the response and from then on, only communicate with the host using a secure transport layer for the duration of 'max-age' set in the header. I've detailed how to issue the HSTS header in PHP, but that isn't the most ideal way.
 
 # ================== UNICORN LEVEL REDIRECTION to HTTPS
https://simonecarletti.com/blog/2011/05/configuring-rails-3-https-ssl/
