#Jelastic certified add-ons

##Free SSL Let’s Encrypt Add-On

Let’s Encrypt Add-on for Automatic SSL Configuration of Your Jelastic Environment.
This add-on allows to configure SSL for:

 - Internal environment address
 - Custom domains
 
 See [more details about add-on installation and configuration here.](https://github.com/jelastic-jps/lets-encrypt)

```
{
  "jpsVersion": "0.6",
  "jpsType": "update",
  "id": "letsencrypt-ssl-addon",
  "name": "Let's Encrypt Free SSL",
  "categories": [
    "apps/dev-and-admin-tools"
  ],
  "targetNodes": {
    "nodeType": [
      "tomcat6",
      "tomcat7",
      "tomcat8",
      "tomcat9",
      "tomee",
      "glassfish3",
      "glassfish4",
      "jetty6",
      "apache2",
      "nginxphp",
      "apache2-ruby",
      "nginx-ruby",
      "nginx",
      "haproxy",
      "apache-lb"
    ]
  },
  "version": "1.1",
  "homepage": "https://github.com/jelastic-jps/lets-encrypt",
  "logo": "https://raw.githubusercontent.com/jelastic-jps/lets-encrypt/master/images/lets-encrypt.png",
  "description": {
    "text": "<div class='description'><b>Let's Encrypt</b> is a free and open Certificate Authority (CA), aimed to simplify and automate processes of browser-trusted SSL certificates issuing and appliance.</div><div class='description'><u>Supported stacks:</u> All Jelastic certified container templates except of Jetty 8/9, JBoss/WildFly, Node.js, Apache-Python, Varnish and Docker containers (coming soon)</div><div class='warning-lower'><b>Note</b> that Public IP address(es) will be automatically attached to all nodes within the entry point environment layer (i.e. either application server or load balancer).<br></div>",
    "short": "Free tool to configure support of secured SSL connection for an environment, by either internal or custom domain name."
  },
  "onInstall": [
    {
      "execScript": {
        "type": "js",
        "script": "https://raw.githubusercontent.com/jelastic-jps/lets-encrypt/master/scripts/create-installation-script.js",
        "params": {
          "baseDir": "https://raw.githubusercontent.com/jelastic-jps/lets-encrypt/master/scripts",
          "cronTime": "0 3 * * * *"
        }
      }
    }
  ],
  "actions": [
    {
      "log": {
        "log": "${this.message}"
      }
    },
    {
      "callScript": {
        "execScript": {
          "script": "var p = eval('(' + jelastic.dev.apps.GetApp('${env.appid}').description + ')'); p['${this.action}']=1; return jelastic.dev.scripting.Eval(p.script, p)"
        }
      }
    }
  ],
  "buttons": [
    {
      "confirmText": "Do you want to update attached SSL certificate(s)?",
      "loadingText": "Updating...",
      "procedure": "callScript",
      "caption": "Update",
      "successText": "SSL certificate files have been successfully updated!"
    }
  ],
  "onUninstall": {
    "call": {
      "action": "callScript",
      "params": {
        "action": "uninstall"
      }
    }
  },
  "settings": {
    "fields": [
      {
        "type": "radio-fieldset",
        "name": "extDomain",
        "default": "currentDomain",
        "values": {
          "currentDomain": "Environment Domain Name - get test (invalid) SSL certificate for your environment internal URL to be used in testing",
          "customDomain": "Custom Domain - get valid SSL certificate for previously attached external domain(s) to be used in production"
        },
        "showIf": {
          "customDomain": [
            {
              "type": "string",
              "name": "customDomain",
              "regex": "^(([a-zA-Z0-9]|[a-zA-Z0-9][a-zA-Z0-9\\-]*[a-zA-Z0-9])\\.)*([A-Za-z0-9]|[A-Za-z0-9][A-Za-z0-9\\-]*[A-Za-z0-9])$",
              "caption": {
                "en": "Domain"
              },
              "required": true
            }
          ]
        }
      }
    ]
  },
  "success": "<div class='description'>Your Let’s Encrypt SSL certificate(s) will remain valid for 90 days. To avoid their expiration, use the Update option at add-on’s panel (you'll get the appropriate email notification beforehand).</div><div class='description'>Starting with 4.9.5 Jelastic version, this operation is handled by the system automatically.</div><br><div>Useful links:</div><div><a href='https://github.com/jelastic-jps/lets-encrypt#how-to-renew-ssl-certificate' target='_blank'>How to renew SSL certificate</div><div><a href='https://docs.jelastic.com/custom-domain-via-cname'target='_blank'>How to bind custom domain via CNAME</a></div><div><a href='https://docs.jelastic.com/custom-domain-via-arecord' target='_blank'>How to bind custom domain via A Record</a></div>"
}
```