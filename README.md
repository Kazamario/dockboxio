# DockBoxIO

This docker is an updated version of hjhart's "Docker Downloader for seedbox.io" project. The original documenation below is reference for getting this working on Windows machines.

### Windows 10 Stack Essentials
[Windows 10](https://www.microsoft.com/en-us/software-download/windows10): Clean, jank-free, fully functional copy of Windows 10 is of the utmost importance. If you have any jank in there, it might cause problems, as you are touching a lot of layers here - (OS, Hyper-V, Containers, etc.). This didn't work correctly for me until I completely wiped my system, although you shouldn't have to in most cases!

Plex - Lorem Ipsum

Plex Media Server - Loren Ipsum

[Seedbox.io Seedbox](https://seedbox.io/): This won't work without one of these, please [read up on them](https://seedboxgui.de/guides/what-is-a-seedbox/) so you have a full understanding of what these do. It comes with EVERYTHING you need, and it's also the thing in the screenshot when you set your CompletedDirectory folder. More on that later.

How to set Environment Variables on your Windows 10 system: This one is no biggie as long as you RTFD, but very, very important you get it right. These are the meat and potatoes of this stack, and if they aren't set correctly, your in for a real treat.

Make sure you set the following variables in your [Environment Path in Windows 10](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/):

* [ ]  SEEDHOST - Put YOUR Seedbox.io URL information in here, minus the [http://] part. It should look something very close to > (psv23232.seedbox.io). Yours will be different, so don't cut and paste and expect miracles!
* [ ]  MOVIE_DIR - This value is the File Explorer path to your movies. (Example: M:\Movies)
* [ ]  TV_SHOW_DIR - This value is the File Explorer path to your tv shows. (Example: T:\TVShows)
* [ ]  STORAGE_DIR - This value is the File Explorer path to your CompletedDownloads folder that Resilio Sync will use

[Resilio Storage Sync](https://www.resilio.com/individuals/): You'll need this to sit between your containers and Plex.

Basic coding skills: As long as you can change directories, and remember where you put your folders, you'll be ok! Oh, and we'll be doing some Powershell and Bash CLI too! Fun stuff!

[Docker for Windows](https://docs.docker.com/docker-for-windows/install/): This is where all the magic happens. It puts all the cool things in one nifty container. More on Docker [here](https://en.wikipedia.org/wiki/Docker_(software)).

[Powershell CLI](https://docs.microsoft.com/en-us/powershell/scripting/components/console/powershell.exe-command-line-help?view=powershell-6): This ships with Windows, but I included a link to the documentation in case you're just starting out!

[Git](https://git-scm.com/download/win): You'll need this for the bash terminal to export your environment SEEDHOST variable. Once this is installed on your system if you install Visual Studio Code, [you can create a bash terminal inside of the IDE itself](https://stackoverflow.com/questions/42606837/how-do-i-use-bash-on-windows-from-the-visual-studio-code-integrated-terminal), very cool!

### Windows 10 Stack Nice-To-Haves
[Visual Studio Code](https://code.visualstudio.com/): You don't need this per se, but you can select multiple terminals right from one place, making this a whole lot easier! It also integrates with Docker!

**Click on the links below to learn more about:**

[Switching Terminals](https://code.visualstudio.com/docs/editor/integrated-terminal)

[Official Visual Studio Code Docker Extension](https://code.visualstudio.com/docs/editor/integrated-terminal)

[Visual Studio Marketplace Docker Extensions](https://marketplace.visualstudio.com/search?term=docker&target=VSCode&category=All%20categories&sortBy=Relevance)




# Docker Downloader for seedbox.io

This docker set up allows you to set up a local dockerized Sonarr, Radarr, Jackett, and Resilio Sync, that will automatically hook up to your seedbox.io seedbox instance, straight out of the box. It even has an nginx reverse proxy sitting on port 80 to give you a front page to your media services.

### Caveat:

**Warning:** This whole README uses the same hostname, psv23232.seedbox.io. Anywhere you see that, change it to your seedbox.io hostname.


# Installation

Set your seedbox.io name as an environment variable.

```
export SEEDHOST=psv23232.seedbox.io
open https://$SEEDHOST/dav
```

* Create a directory called `CompletedDownloads`


```
open https://$SEEDHOST
```

* Click on the settings icon. Go to "Autotools". Make sure AutoMove is checked, if torrent's label matches filter /.*/ with the directory, /home/psv23232/files/CompletedDownloads. Also check the "Add torrent's label to path"
![rTorrent Settings](https://github.com/hjhart/docker-downloader/blob/master/assets/rtorrent_settings.png)

Now go add the `CompletedDownloads` directory.

```
open https://$SEEDHOST/resilio
```

* Add that directory in Resilio Sync, copy the read-only secret.

```
cp .env.example .env
```

Now edit the .env file to include your newly generated Resilio Sync secret as RSLSYNC_SECRET.

While you're in the .env file, add in correct data for MOVIE_DIR, TV_SHOW_DIR, and STORAGE_DIR. These represent where you want you movies, your tv shows, and where your completed downloads should go.

## Configure Radarr:

* Add indexers that you want to add. (Jackett can be configured / found on port 9117)
* Add rTorrent as a Download Client to point to your seedbox.io. [See this article](https://panel.seedbox.io/index.php?rp=/knowledgebase/41/How-to-connect-Sonarr-to-your-service.html)
* Toggle "Advanced Settings" in the Download Client tab.
* Add a new Remote Path Mapping like this:

```
   host                               remote path                                        local path
psv23232.seedbox.io       /home/psv23232/files/CompletedDownloads/radarr/              /downloads/radarr/

```

## Configure Sonarr: 

* Add indexers that you want to add. (Jackett can be configured / found on port 9117)
* Add rTorrent as a Download Client to point to your seedbox.io. [See this article](https://panel.seedbox.io/index.php?rp=/knowledgebase/41/How-to-connect-Sonarr-to-your-service.html)
* Add a new Remote Path Mapping like this:

```
   host                               remote path                                        local path
psv23232.seedbox.io       /home/psv23232/files/CompletedDownloads/sonarr/              /downloads/sonarr/

```

## Congratulations!

Now you should be all set up. If you have any problems, leave an issue in github and I'll get back to you.


TODO:


* Figure out how to configure Download Clients automatically and check it into version control
* (Essentially figure out how to remove the "Configure Radarr / Sonarr" manual steps)
* Add download clients automatically
