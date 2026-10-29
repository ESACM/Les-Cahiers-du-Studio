# Les Cahiers du Studio

A time-based documentation tool to collaboratively keep notes and capture media from live experiences.

![dmk4q_fxkae_i70](https://user-images.githubusercontent.com/1948417/32667094-f8f3b43e-c639-11e7-98e2-22de0502fa96.png)

The documentation for this app can be found [here (in french)](https://latelier-des-chercheurs.fr/docs/manuel-les-cahiers-du-studio).

# License

This app is under a [Creative Commons BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/) license. It can be used, modified and shared freely as long as no commercial use is made and the attribution and link to [L’Atelier des Chercheurs](https://latelier-des-chercheurs.fr/) are kept.

The typeface embedded in this tool, _STVitry_, was created specifically for the Studio-Théâtre of Vitry. Its use is expressly restricted to this tool, and it cannot be used anywhere else without prior authorisation by the Studio-Théâtre or Maxime Fittes, the creator of this font.

# Credits and origin

This tool was made for and in collaboration with the [Studio-Théâtre of Vitry](http://www.studiotheatre.fr/), a place for theatrical creation and research that welcomes and hosts theatre companies. It is installed on a computer inside the Studio-Théâtre and allows companies to keep track of their time and process inside the walls of the Studio-Théâtre. Studio-Théâtre of Vitry is supported by la ville de Vitry-sur-Seine, le Département du Val-de-Marne, la Région île-de-France et la DRAC île-de-France.

The creation, design and code for this app was done by [L’Atelier des Chercheurs](https://latelier-des-chercheurs.fr/) following a series of workshops and field studies at the Studio-Théâtre, and meetings with persons in charge there (specifically Bérangère Vantusso, Florence Kremper and Ella Gouet).

Furthermore, it was developed with the precious help from the following persons:

* Vincent Desclaux (who participated in the creation of the user interface)

* Maxime Fittes (who created the typeface _STVitry_)

* Nolwenn Maudet, Benoît Verjat, Émilie Coquard, Nicolas Taffin, Julien Taquet, Thibéry Maillard, Anthony Masure, Mike Sabbagh, and others who gave feedbacks and helped refine the concept and use cases of this app.

Finally, we should mention that this app wouldn’t be the same if it weren’t for two other, older free and open-source apps: [Rekall](http://www.rekall.fr/) and [Ligne de Temps](http://www.iri.centrepompidou.fr/outils/lignes-de-temps/). Both had very interesting ideas and use cases that we drew inspiration from to make Les Cahiers du Studio.

# Install Les Cahiers du Studio without running Electron

To run this app as a server application running on Debian 13, please follow these steps

#### Setup environment

Make sure your installation is up to date and prepare the packages needed
```
sudo apt update && sudo apt upgrade -y
sudo apt install git -y
```

[Install nvm](https://github.com/nvm-sh/nvm#install--update-script)
```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.8/install.sh | bash
```

[Install Python 2.7](https://github.com/ctch3ng/Installing-Python-2.7-and-pip-on-Ubuntu-24.04-Noble-LTS), needed by node-gyp
```
sudo apt install -y build-essential checkinstall libncurses-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev libreadline-dev libdb-dev
wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz
tar xzf Python-2.7.18.tgz
cd Python-2.7.18
./configure --prefix=/usr/local/python2.7
make
sudo make install
```

#### Clone the repo, switch to the electronless branch and build
```
git clone https://github.com/ESACM/Les-Cahiers-du-Studio.git cahiers
cd cahiers
git checkout electronless
nvm install # installs the node version needed and defined in .nvmrc
nvm install-latest-npm
cd public
npm install
npm run build # builds the dist/build.js file needed for serving the app
cd ..
export PYTHON=/usr/local/python2.7/bin/python2.7 # needed by node-gyp to build the platform-folders package
npm install
npm rebuild sharp
```

#### Run the app

```
cd cahiers
nvm use
npm run node-start
```

To keep it running in the background (ie launching after opening an SSH connexion), you can use screen

```
screen
cd cahiers
nvm use
npm run node-start
```

Use ctrl+a then d to exit the screen session, this keeps the app running but you can quit your SSH session

#### Autostart with systemd

Create the service file

```
sudo nano /etc/systemd/system/cahiers.service
```

Content of the file. Modify for the right user and paths

```
[Unit]
Description=Les Cahiers du Studio
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=<user>
ExecStart=/home/<user>/.nvm/versions/node/v8.9.3/bin/node /home/<user>/cahiers/index.js

[Install]
WantedBy=multi-user.target
```

Test that it's running and it's status

```
sudo systemctl start cahiers.service
sudo systemctl status cahiers.service
```

If everything looks good, enable the service and reboot to check it starts at launch

```
sudo systemctl enable cahiers.service
<reboot>
sudo systemctl status cahiers.service
```


# ToDo

Wrap all that in a Docker container to ease the deployment

# Tweak, fork and debug

### server-side

* Run `npm run debug` to start the server with a better debug in terminal

### client-side

The client-side code is in the `public` folder. To write some new CSS or client-side JS, you need to run the following tasks:

```
npm run install
```

and then, to start the dev tasks:

```
npm run dev
```

To build for production, use

```
npm run build
```

This will create minified JS files in the `public/dist` folder.
