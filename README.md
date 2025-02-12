Use of open-source intelligence and passive reconnaissance
Whois www.google.com
Nslookup www.google.com
Git clone https://github.com/Tuhinshubhra/RED_HAWK 
Cd RED_HAWK
Php rhawk.php
intitle: index of /concrete/Password
intitle:"index of" "/usernames"
filetype:csv intext:"Secret access key"
intext:"user" filetype:php intext:"account" inurl:/admin
inurl:"live/cam.html"

allintext:username filetype:log “oracle”

Practical on enumerating host,port and service scanning.
nmap scanme.nmap.org
nmap -p80, 443 scanme.nmap.org
nmap -sV scanme.nmap.org
nmap -vv -O 192.168.10.123
masscan –regress
masscan –help
masscan –p 80,53 192.168.10.123
nmap -v -p 0-65535 -A 192.168.10.123 -oA metasploitable2
ls
mkdir target (Making Directory)
mv metasploitable2.* target/
cd target/ (Change Directory)
cat metaspoitable2.nmap

Practical on vulnerability scanning and assessment.

1.ping indiamart.com
nslookup indiamart.com
sudo nmap –vv –O 192.168.126.2
VirusTotal.com
HTTPStatus.io
MXToolbox.com
Shodan.io
Ssl:”indiamart.com”

sudo apt-get install nikto
sudo nikto -h
sudo apt-get update
sudo nikto -h http://www.facebook.com/
sudo apt-get install uniscan  
sudo uniscan -u http://www.facebook.com/


Practical on use of Social Engineering Toolkit.

curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null

echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list

sudo apt update
sudo apt install ngrok
Open firefox browser in kali linux, visit https://dashboard.ngrok.com/signup  & create a ngrok account to generate a “Ngrok Auth Token”

Go to Setup & Installation Section and Copy the “Ngrok Auth Token” & paste in the terminal & Hit Enter

Setoolkit 
Option 1 2 3 2  192.168.126.128
Account.spotify.com/en/login

