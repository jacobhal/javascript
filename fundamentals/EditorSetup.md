# Section 5: Editor Setup

## Snippets
Setup your own VSCode snippets: Preferences -> User Snippets -> New Global Snippets File.
For instance, you can write console.log() easily.

## Prettier
You should download the extension "Prettier - Code Formatter" in VSCode which enforces consistent styling and code formatting.

How to set it up:
1. Preferences -> Settings -> Search "default formatter" -> Select esbnp.prettier-vscode
2. (Optional) Preferences -> Settings -> Search "formatonsave" -> Check the format on save checkbox
3. Add a .prettierrc file (with optional .json or .yml extension where YAML is default). Another way is to add the Prettier configuration to an .editorconfig file which is commonly used for C# etc.
4. Configure the .prettierrc file to your liking (documentation: https://prettier.io/docs/en/options.html).

> If you are using linting (e.g. ESLint) you will have to decide how you want it to work with Prettier. You can for instance let Prettier do the formatting and the linter do the linting.

## Node.js
Install Node.js in order to setup a dev environment which allows us to run a development server on a port and use hot-reloading. Installing Node.js automatically installs npm as well.

You have lots of node packages to choose from in order to setup a dev server:
* A simple one is the "live-server" package. Run the "live-server" command in your project folder to start a server there.
* If you are using react, you can always use the "create-react-app" package and go from there.

## Debugging
Go to the "Sources" tab in Chrome and set breakpoints.
Use the "Scope" section to see current variables.

Shortcuts:
* F8 or Command+\: Pause or resume script execution
* F10 or Command+': Step over
* F11 or Command+;	: Step into
* Shift + F11 or Command+Shift+;: Step out of