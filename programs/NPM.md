# NPM

[NPM](https://www.npmjs.com/) (sometimes called *Node Package Manager*, although that meaning has been deprecated?)  
Installs NodeJS packages via command line and maintains a list of the installed packages. Packages can be divided in the **dependencies** list (needed to run) and **devDependencies** (needed to build, not installed in the final packaged version for end-users)

#### Initialize the package list (package.json)
```npm init```

#### Install a package locally and update the list
```npm install <package> --save```
Files will be installed to node_modules

#### Install a package locally and save it to the devDependencies
```npm install <package> --save-dev```

#### Install a package globally
```npm install -g <package>```

#### Check outdated global packages
```npm outdated -g --depth=0```

#### Update global packages
```npm update -g```

#### Install packages in package.json
```npm install```

#### Update packages [optionally save them in package.json]
```npm update [--save]```

#### Uninstall package
```npm uninstall <package>```
(options are similar to install)

#### Uninstall packages not in package.json
```npm prune```

#### Check outdated packages
```npm outdated```

## Semver (Semantic Versioning)
- `~1.2.3` = `>=1.2.3 <1.3.0`
- `~1.2` = `>=1.2.0 <1.3.0` = `1.2.x`
- `~1` = `>=1.0.0 <2.0.0` = `1.x`
- `*` = latest version (dangerous)
- `^1.2.3` = `>=1.2.3 <2.0.0`
- `^0.2.3` = `>=0.2.3 <0.3.0`
