# What is yarn
Yarn is **Y**et **A**nother **R**ackage **N**anager.
https://yarnpkg.com/

Claimed advantages over npm are speed, predictability, and offline capabilities.

# Yarn basics
Install dependencies from package.json:
`npm install` == `yarn`

Install a package and add to package.json:
`npm install package --save` == `yarn add package`

Install a devDependency to package.json:
`npm install package --save-dev` == `yarn add package --dev`

Remove a dependency from package.json:
`npm uninstall package --save` == `yarn remove package` _(note the --save param)_

Upgrade a package to its latest version:
`npm update --save` == `yarn upgrade`

Install a package globally:
`npm install package -g` == `yarn global add package`

# Cool things yarn can do
## Show why a package was installed
`yarn why package`

```bash
serif@dig:~/dev/learn-something/02-learn-yarn$ yarn why simple-grep
yarn why v0.16.1
[1/4] Why do we have the module "simple-grep"...?
[2/4] Initialising dependency graph...
[3/4] Finding dependency...
[4/4] Calculating file sizes...
info Has been hoisted to "simple-grep"
info This module exists because its specified in "devDependencies".
info Disk size without dependencies: "12kB"
info Disk size with unique dependencies: "12kB"
info Disk size with transitive dependencies: "12kB"
info Amount of shared dependencies: 0
```

## Update the version of the developed package
Shows `yarn` version, the version of the locally developed package from package.json, and asks for a new version number. Automatically updates package.json and commits the change.
`yarn version`

```bash
serif@dig:~/dev/learn-something/02-learn-yarn$ yarn version
yarn version v0.16.1
info Current version: 0.0.1
question New version:
error Invalid semver version
question New version: 0.0.2
info New version: 0.0.2
Done in 5.38s.

serif@dig:~/dev/learn-something/02-learn-yarn$ git log
commit 179394374ede8770f5d35aab0a003327b8f42715
Author: Bodnar Istvan <istvan.bodnar@kinja.com>
Date:   Sun Oct 23 17:26:26 2016 +0200

    v0.0.2
```

# Dependency versions
Semver in npm:
https://docs.npmjs.com/getting-started/semantic-versioning
Checker tool:
https://docs.npmjs.com/misc/semver

Most used version ranges:
 - `~` -- allows patch updates only (1.0.x)
 - `^` -- allows minor and patch updates (1.x.y)
 - `>=` -- use exact version or newer (x.y.z)

Note: public packages has to start with 1.0.0!

## Useful commands when working with versions
Set how versions are stored in package.json after `yarn add`
```bash
npm config set save-prefix='~'
# or
npm config set save-exact true
```

Install specific version:
```bash
npm install backbone@1.3.2
# or
yarn add backbone@1.3.2
```

## Outdated packages
Check outdated package:
```bash
npm outdated
# or
yarn outdated
```

Use `npm-update-outdated` to update everything:
https://www.npmjs.com/package/npm-update-outdated
As a service:
https://greenkeeper.io/

## Dependency types
### devDependencies
Dependencies used only during development -- test runners, linters, formatters, etc.
```
npm install karma --save-dev
# or
yarn add karma --dev
```
### peerDependencies
These won't be installed as dependencies, but rather mark the version of peers that will surely work with the local package.
https://docs.npmjs.com/files/package.json#peerdependencies

### bundledDependencies
Packages that are bundled when publishing the dependency.
// TODO: what does that exactly mean?

### optionalDependencies
These packages will be installed (TODO: dev, production?) -- and actually override versions in the `dependencies` section --, but if for any reason installation fails npm will still proceed.
https://docs.npmjs.com/files/package.json#optionaldependencies

### production dependencies
Install only packages required for production builds
```bash
npm install --production
# or
yarn --production
```

Setting the `NODE_ENV` env var can be used as well
```bash
NODE_ENV=production npm install
```

# Missing from yarn
## git+ssh:// links in dependencies
```json
"devDependencies": {
  "simple-grep": "git+ssh://git@github.com:gawkermedia/simple-grep.git#master"
}
```

Workaround: replace `host:path` with `host/path`
```json
"git+ssh://git@github.com/gawkermedia/simple-grep.git#master"
```
https://github.com/yarnpkg/yarn/issues/513

## xmas
`npm xmas`
:((((((
