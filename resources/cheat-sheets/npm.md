---
layout: page
title: NPM
---

[Back to Cheat Sheets](/resources/cheat-sheets/)

## Common Commands

```shell
# Update Node Package Manager
npm install -g npm

# Initialize an NPM managed project, creating packages.json
npm init

# Install Package, and add to packages.json as dependency
npm install package-name --save

# Uninstall Package
npm uninstall package-name --save

# List installed packages
# Also points out installed packages that are not dependencies as "extraneous".
npm ls

# Remove Installed Packages that are not dependencies
npm prune

# List outdated packages
npm outdated

# Install Package as production dependency
npm install --save-prod gulp

# Install Package as development dependency
npm install --save-dev gulp

# Install Package Globally (used for CLI tools)
npm install -g gulp

# Detect Global Packages that need updating
npm outdated -g --depth=0

# Publish / Unpublish Module to NPM Registry
npm publish
npm unpublish

# Register NPM User Account from Command Line
# You can view this user account on the web by inserting your username into this URL:
# https://www.npmjs.com/~your-username
npm adduser

# Display NPM Configuration
npm config ls

# Increment the Version of your Package
npm version patch
npm version major
npm version minor
npm version prerelease
npm version preminor
npm version premajor
```

## Node Version Manager (NVM)

```shell
# list remote versions
nvm ls-remote

# list locally installed versions
nvm list
```

## Creating a Node Module

Using npm init you can completely setup a packages.json file that defines your module.

In the "main" Javascript file specified, index.js, you can define properties on the "export" object and these properties will be accessible from your package.

```javascript
# index.js
exports.printMsg = function() {
  console.log("This is a message from the demo package");
}
```

Javascript using module dependency

```javascript
var demo = require('npm-demo-pkg');
demo.printMsg()# // outputs "This is a message from the demo package" to console
```

## Publishing Node Modules

The name and version are the only fields required in your packages.json file.

You have to have a registered user account in the NPM registry. You can accomplish this using npm adduser.

Once this is completed, you simply use npm publish to push your new package up to the NPM registry. To update your package you will need to first update the version in packages.json, then use npm publish. You can also use the npm version command to increment the version.

## Semantic Versioning

Projects should start out released as version 1.0.0. Anything before 1.0.0, such as version 0.2.1 would be considered a pre-release (i.e. Alpha, Beta, etc). After the first stable release, the following should apply:

Bug fixes and other minor changes: Patch release, increment the last number, e.g. 1.0.1
New features which don't break existing features: Minor release, increment the middle number, e.g. 1.1.0
Changes which break backwards compatibility: Major release, increment the first number, e.g. 2.0.0

When you are specifying the version of a package that you want in your packages.json file, as a dependency, you can use the following formats to specify that you only want patch update, no minor or major updates.

Update Latest Patch Versions Only

- 1.4
- ~1.4.2
- 1.4.x

Update Latest Minor Versions Only

- 1
- 1.x
- ^1.4.0

Update to Latest Major Version

- \* (asterisk)
- x

## Scoped Packages

A scoped package has a name that begins with your username like so:

```javascript
{
  "name": "@username/project-name"
}
```

You can initialize your package using the --scoped argument like so:

```shell
npm init --scope=username
```

If you create scoped packages all the time, you can configure NPM to do this by default in your ~/.npmrc file.

```shell
npm config set scope username
```

Scoped packages are private by default. NPMjs.com requires that you be a paid member to host your own private packages with them. Public scoped packages are free however without requiring a membership. You can publish a package, and set it as public for all future publishes, using this command:

```shell
npm publish --access=public
```

To use a scoped package, you simply include the username before the package name like so:

```shell
npm install @username/project-name --save
```

```javascript
// in your project require the package like so
var projectName = require("@username/project-name");
```
