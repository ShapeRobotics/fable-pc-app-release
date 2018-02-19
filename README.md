# fable-pc-app-release HOW TO:
Public release server for our Fable App.

Steps to glory:
1) update package.json with the current version number
2) prepare your SYSTEM ENVIRONMENT VARIABLES for automatic codesigning. 
    Simply add: <pre> CSC_LINK - add path to .p12 certificate </pre>
                <pre> CSC_KEY_PASSWORD - password for certificate </pre>
    For details please look up:<pre> https://www.electron.build/code-signing </pre>
3) ensure you have a stable and tested build. 
4) <pre> npm run build_app </pre>
5) upload your release (.exe and .yml file) to the repository. This one is for 64-bit Windows
6) to ensure your release auto updates open the previous version of Fable after you successfully created the release on github
7) go to <pre>C:\Users\YOUR_USER_NAME\AppData\Roaming\Fable </pre> and open log.log to monitor the auto update process

If the auto-update functions correctly, the pc app will ask you restart the application.
Success!

For any questions ask your friendly neighborhood devs.

