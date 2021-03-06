# fable-pc-app-release
# HOW TO TEST THE AUTO UPDATE FUNCTION OF ANY BUILD

<strong>Even if you have a stable build, you must check that it can perform an update to a higher version </strong>

Once you have a stable and tested build follow these steps to test the auto updater function:
1) Check the latest version number of fable in this release repository. For example, we will consider that the latest one is 1.2.4
2) Now, say we are currently on 1.2.6. So, you go ahead and make the build as described in the section bellow. Nice and by the book. 
3) <strong> The goal is to ensure that the autoupdate code works. If it does not it is critical you fix it before anything else.  </strong> 
So, once 1.2.6 is done installing you go to this local path where Fable is installed 
<pre>C:\Program Files\Fable\resources\app\package.json</pre>
4) <strong>This part is a bit of a hack</strong>: Open package.json and modify 1.2.6 to be smaller than 1.2.4 (or whatever the latest version is). So 1.2.3 or below.
5) Starting your installed Fable will make it think it is at 1.2.3. Consequently, it will attempt to update to 1.2.4 which is the preferred behaviour.

Note: You can monitor the auto update process in the log.log file. Details on that bellow.
Always ask if you are unsure about this.

# HOW TO CREATE A BUILD 64bit

Follow these steps:
1) update package.json with the current version number. Pay attention to the bold type text. Those are the fields that need checking:
<pre>
{
    "name": "Fable",
    "productName": "Fable",
    "version": <strong>"NEW VERSION NUMBER"</strong>,
    "description": "Fable",
    "author": "Shape Robotics ApS",
    "main": "app/main_exe.js",
    "scripts": {
        "postinstall": "install-app-deps",
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "export NODE_ENV=development || SET NODE_ENV=development && electron . --enable-logging",
        "<strong>dist": "build --x64</strong>",
        "pack": "build --dir",
        "build_server": "cd src && pyinstaller server.spec --distpath ../app/app --noconfirm",
        "build_toolboxes": "cd src/pc-app/app/static/xml && python build_toolboxes.py",
        "copy_static": "cpx \"src/pc-app/app/static/**/*.*\" app/app/app/static",
        "copy_drivers": "cpx \"drivers/**/*.*\" app/drivers",
        "copy_sounds": "cpx \"src/pc-app/sounds/*.*\" app/app/sounds",
        "copy_examples": "cpx \"src/pc-app/examples/*.*\" app/app/examples",
        "build_app": "npm run build_toolboxes && npm run build_server && npm run copy_static && npm run copy_drivers && npm run copy_sounds && npm run copy_examples && npm run dist"
    },
    "license": "ISC",
    "dependencies": {
        "electron-log": "1.3.0",
        "electron-updater": "2.16.1",
        "electron-settings": "3.1.4",
        "tree-kill": "1.1.0",
        "request": "2.81.0",
        "request-promise": "4.2.0",
        "async": "2.5.0",
        "aws-sdk": "2.45.0",
        "body-parser": "1.17.1",
        "dynamodb-data-types": "3.0.0",
        "express": "4.15.2",
        "get-port": "3.2.0",
        "getmac": "1.2.1",
        "public-ip": "2.3.3"
    },
    "devDependencies": {
        "electron": "1.8.1",
        "electron-builder": "19.45.4",
        "cpx": "1.5.0"
    },
    "build": {
        "appId": "Fable",
        "asar": false,
        "publish": [
            {
                "provider": "github",
                "owner": "ShapeRobotics",
                "repo": "<strong>fable-pc-app-release</strong>"
            }
        ],
        "mac": {
            "identity": "Shape Robotics ApS (GVCKVYG69A)",
            "category": "developer-tools",
            "icon": "build/icon.icns",
            "target": [
                "zip",
                "dmg"
            ],
            "publish": [
                {
                    "provider": "github",
                    "owner": "ShapeRobotics",
                    "repo": "fable-pc-app-release"
                }
            ]
        },
        "dmg": {
            "title": "${productName}-${version}",
            "icon": "build/icon.icns",
            "background": "build/background.png",
            "contents": [
                {
                    "x": 94,
                    "y": 90
                },
                {
                    "x": 306,
                    "y": 90,
                    "type": "link",
                    "path": "/Applications"
                }
            ]
        },
        "win": {
            "publish": [
                {
                    "provider": "github",
                    "owner": "ShapeRobotics",
                    "repo": "<strong>fable-pc-app-release</strong>"
                }
            ]
        },
        "nsis": {
            "perMachine": true,
            "include": "build/installer.nsh",
            "artifactName": "${productName}-Setup-${version}.${ext}"
        }
    }
}

</pre>
2) add the fields bellow to your SYSTEM ENVIRONMENT VARIABLES. This enables Electron to automatically sign the installer
   <pre> CSC_LINK - add path to .p12 certificate as the value </pre>
   <pre> CSC_KEY_PASSWORD - password for certificate as the value </pre>
    For details please look up:<pre> https://www.electron.build/code-signing </pre>
3) ensure you have a stable build. Test religiously.
4) <pre> npm run build_app </pre>
5) Install the app on your machine and check the install folder:
    <pre> C:/Program Files/Fable/resources/app-update.yml</pre>
   This file contains a field called repo designating where the program looks for updates. It should be <strong> fable-pc-app-release </strong>
6) go to <pre>C:\Users\YOUR_USER_NAME\AppData\Roaming\Fable </pre> and open <strong>log.log</strong> to monitor the auto update process
7) upload your release (.exe and .yml file) to this repository.

Success!

Another way to test is to have the release be available, install the previous version and monitor if the autoupdate goes through. This can be monitored through log.log
For any questions ask your friendly neighborhood devs.

