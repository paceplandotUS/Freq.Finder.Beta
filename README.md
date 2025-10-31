# Freq. Finder Setup 

Version: 251031.1600 From Branch: Issue2-Dates

This guide provides quick instructions for running the pre-release Windows version(s) of Freq.Finder

## Disclaimers
This is a new, pre-release program in active development. Use at your own risk. It should not be relied on as authoritative. Always consult the FCC's own ULS for authoritative answers. 

Notably, this is not a product of, warranted or endorsed by, recommended by, or affiliated with the Commonwealth of Virginia. 

## Downloading
https://github.com/paceplandotUS/Freq.Finder.Beta

Unzip the downloaded file into a clean directory/folder. You should end up with an .exe and a folder call _internal.


## Running
Execute the program
1. note - your antivirus may fight you - proceed accordingly and at your discretion. 
This SHOULD be just becuase it's a novel/new .exe, but I make no safety/security warranty or guarantee.

2. You should get a command window / console that opens up. Eventually you'll see something like: 

    Warning: Bundled file 'uls_data.db' not found in MEIPASS.
    Warning: Bundled file 'uls_data.db' not found in MEIPASS.
    pywebview not found, running in standard server mode.
    Access the application at http://127.0.0.1:7223
    * Serving Flask app 'app'
    * Debug mode: off
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on http://127.0.0.1:7223
    Press CTRL+C to quit

Open a browser to that URL. Port 7223 is "PACE" on a phone keypad. Get it? :) 

3. The first time you run, you'll need to download FCC data. Go to Data Maintenance. Click Start Data Update.
**When it says "can take a significant amount of time" it means it. Like 20+ minutes depending on your connection and PC speed.**
The "building database" part can seem like nothings happening for many minutes. Don't Panic. 

4. Once it tells you it's done, click "Back to Query Tool"

5. There's an instructions link that should give you the basics. 

6. When you're done, you can "CTRL-C" in the console window or just close it to kill it. 


## Support
Honestly, there really isn't any. makestuff@paceplan.us and I'll try to help. More soon!    