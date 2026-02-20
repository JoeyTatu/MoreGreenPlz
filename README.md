#MoreGreen
* Look like you're active on GitHub and make it more green by automating editing and pushing a file to a repo by running a bash script using Cron.   
* This is a repo that adds the current date and time to the dates.log via a cron job on an AWS Ubuntu server.
* The AWS Ubuntu instance user is "root". You can find out how to access the AWS Ubuntu instance as the root user by clicking [here](https://tecadmin.net/how-to-enable-ssh-as-root-on-aws-ubuntu-instance/).

Setup:
1. Create the MoreGreen repo on GitHub.

2. Still in GitHub, get an access token:<br>
  a. Click Pic on top right > Settings > Developer settings (bottom left) > Personal access token > Token (classic) > Generate new token > Generate new token (classic)<br>
  or<br>
  b. Go here: [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)
  
3. In the AWS Ubuntu instance, clone the repo:<br>
  <code>git clone http://github.com/{your-username}/MoreGreen.git</code>
  
4. Move into the MoreGreen folder:<br> 
  <code>cd MoreGreen</code>
  
5. Remove the remote origin:<br>
  <code>git remote remove origin</code>
  
6. Re-add the remote origin with youe username and access token:<br>
  <code>git remote add origin https://{your-username):{your-access-token}@github.com/{your-username}/MoreGreen.git</code><br>
  (This will prevent being asked for your username and password (i.e. access token) when pushing to the repo every time.)
  
7. Set your username and email as per your GitHub account:<br>
  <code>git config --global user.name "{your-name}"</code><br>
  <code>git config --global user.email "{your-email-address}"</code>
 
8. Move out of the MoreGreen folder:<br>
  <code>cd .. </code>
  
9. Nano was used to edit files, but other text editors can be used:<br>
  <code> nano gitupdate.sh</code>
  
10. a. Edit the file to the below. This also checks if dates.log exists.:
    <code>
          #!/bin/bash

        FILE="dates.log"
          
        cd MoreGreen

        # Check if the file exists
        if [ ! -e "$FILE" ]; then
            # If it doesn't exist, create it
            touch "$FILE"
        fi
          
        git pull
        echo "$(date +'%A %d %B %Y %H:%M:%S %Z')" >> "$FILE"
        git add .
        git commit -m "$(date +'%A %d %B %Y %H:%M:%S %Z')"
        git push -u origin main
    </code>
    <br><br>
    b. Ensure date is enclosed in backticks, like this: \`date\` or \`date +"%D"\`)<br>
        The text after <code>date</code> is formatting. If you want to format your date and time differently, check out [this link](https://phoenixnap.com/kb/linux-date-command).<br>
        %Z is the time zone.
  
12. Save the file and exit the editor:<br>
  CTRL + O, CTRL + X.
  
13. Begin crontab editing:<br>
  <code>crontab -e</code>
  
14. Add this line to the end:<br>
  <code>*/15 * * * * ./gitupdate.sh</code>
  This runs the bash file every 15 minutes. Crontab variables can be generated [here](https://crontab.guru/).
  
15. Save the file and exit the editor:<br>
  CTRL + O, CTRL + X.
  
16. Finally, ensure the gitupdate.sh file is readable by running:<br><code>chmod 777 gitupdate.sh</code>

17. If all done right, it will update every 15 minutes and add the current date to the dates.log file.
