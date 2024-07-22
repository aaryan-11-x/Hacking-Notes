# Enable Credential-based Login

Remove the "!--" and "--" for both *authorizationStrategy* and *securityRealm*, then save the file.
```sh
root@jenkins:~# egrep 'denyAnonymousReadAccess|disableSignup|enableCaptcha' -C1 /var/lib/jenkins/config.xml
  <authorizationStrategy class="hudson.security.FullControlOnceLoggedInAuthorizationStrategy"> 
    <denyAnonymousReadAccess>true</denyAnonymousReadAccess> 
  </authorizationStrategy> 
  <securityRealm class="hudson.security.HudsonPrivateSecurityRealm"> 
    <disableSignup>true</disableSignup> 
    <enableCaptcha>false</enableCaptcha> 
  </securityRealm>
```