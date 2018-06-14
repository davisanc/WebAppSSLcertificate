# WebAppSSLcertificate
Using SSL certificates with LetsEncrypt

On the following post we are going to explain how quickly and easy is to assign an SSL certificate to your Azure WebApp
I have a Wordpress WebApp already running and the first thing we want to do is to create a custom hostname for my site

At this point I only have the default domain name assigned when created the WebApp

![image of custom-domain](/images/custom-domains.PNG)

We will go and add a hostname, in my case using an A record (you could use a CNAME record instead)

![image of add-hostname](/images/add-hostname.PNG)

To validate the hostname we need to create a TXT record in our DNS registrar. 

![image of txt-record-add](/images/txt-record-add.PNG)

We will add a new A record in our DNS registrar to resolve the hostname to the public IP address assigned to the WebApp. If you used a CNAME record then you would just resolve to the default domain used when creating the WebApp (davidsr.azurewebsites.net)

![image of a-record-add-new](/images/a-record-add-new.PNG)

Once this has been processed in the registrar we will see the custom domain name successfully added in the azure portal

![image of custom-domain-added](/images/custom-domain-added.PNG)

Doing a dig query to find the NS records to my custom domain name will answer with the original records from my DNS registrar

![image of dig-ns-davidsr.me](/images/dig-ns-davidsr.me.PNG)

Once TTL propagates we will be able to resolve the custom domain to the WebApp hosted in Azure 

![image of wordpress-installation](/images/wordpress-installation.PNG)

To install SSL certificates we will go to the Advanced Tools section of the WebApp, and Site Extensions

![image of advanded-tools](/images/advanded-tools.PNG)

![image of site-extensions](/images/site-extensions.PNG)

We will go the gallery and search for Lets Encrypt

![image of lets-encrypt-gallery](/images/lets-encrypt-gallery.PNG)

Install the module and go click play

![image of install-and-play-letsencrypt](/images/install-and-play-letsencrypt.PNG

If we get the following message the workaround is to stop and start the WebApp

![image of no-route-registered-letsencrypt](/images/no-route-registered-letsencrypt.PNG

If this is successful we will see the Lets Encrypt Authentication Settings

![image of Lets-encrypt-authentication-settings](/images/Lets-encrypt-authentication-settings.PNG

We will need to create a service principal for my subscription, so LetsEncrypt can access the WebApp application settings and bind the certificate

![image of create-sp](/images/create-sp.PNG

We can see the service principal on the Azure portal, on the WebApp go to Access Control and select the name of the service principal. Then under properties you will see the same values we got using the Powershell command

![image of sp-azure-portal](/images/sp-azure-portal.PNG

Now back to the authentication settings of Lets Encrypt, we will need the TenantID, SubscriptionID, ClientID, ClientSecret and RG name
ClientID is the value you get as appID when created the service principal, ClientSecret is the password

TenantID can be easily seen here:

![image of tenant-id](/images/tenant-id.PNG

ClientID can be seen here on the portal:

![image of client-id](/images/client-id.PNG

Once we pass this checks we are almost done

![image of letsencrypt-result](/images/letsencrypt-result.PNG

We will hit next and create our SSL certificate, assigned to the custom domain

![image of certificate-installed](/images/certificate-installed.PNG

Finally, we just check on the WebApp SSL bindings section that the certificate is being assigned to my custom domain

![image of sslbindings](/images/sslbindings.PNG

And the final check is to go to the site check you use HTTPS 

![image of https-working](/images/https-working.PNG
