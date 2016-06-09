# Chocolatey - Machine Package Manager for Windows

## Let's get Chocolatey!
Easy Install!
Open administrative PowerShell (Ensure Get-ExecutionPolicy is at least RemoteSigned).
```bash
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned
```
```bash
PS C:\>iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
```
## Chocolatey basics

### Installing programs Chocolatey style
Once you’ve got Chocolatey up and running, it’s time to start installing programs. Open administrative PowerShell. If you wanted to install VLC you’d type:
```bash
PS C:\>choco install vlc
# or
PS C:\>cinst vlc
```
You can also search for packages right on the command line:
```bash
PS C:\>choco search [keyword]
```
### Multiple installs
There are two ways to install multiple programs in one sitting with Chocolatey. The first is to type multiple arguments into the command line. If you wanted to install VLC, GIMP, and Firefox you’d type:
```bash
PS C:\>cinst vlc gimp Firefox
```
For much larger batches of programs, however, you’re better off creating an XML document with a .config file extension and formatting it like so:
```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
    <package id="UTorrent" />
    <package id="notepadplusplus" />
    <package id="IrfanView" />
</packages>
```
### Outdated packages
Returns a list of outdated packages.
```bash
PS C:\>choco outdated
```
### Updating and uninstalling
Updating installed programs via Chocolatey is simple too. Type choco upgrade [program name] or cup [program name] into an administrative command. To update DosBox, for example, type:
```bash
PS C:\>choco upgrade dosbox
# or
PS C:\>cup dosbox
```
If your package is using an alternative source other than the main Chocolatey package feed, you can type:
```bash
PS C:\>cup [package name] –source [source URL]
```
Updating one package at a time in itself is not that awesome. What would be better is to update them all with one command. So type:
```bash
PS C:\>cup all
```
Uninstalling a package is a little different. Going back to our example, you'd type the following to uninstall DosBox:
```bash
PS C:\>choco uninstall dosbox
```
## Create Your Own Chocolatey Packages
Creating a package is super simple. Here’s what you need:

* Nuspec (nuget specification file for a .nupkg file)
* Executable(s) and/or a PowerShell script file named chocolateyinstall.ps1

It’s really that easy. You fill out the [informational aspects of your package](https://github.com/chocolatey/choco/wiki/CreatePackages) in the nuspec. If you have an executable you want to include in the package, you just drop that into the package and you are done. Chocolatey will find that executable and create a batch file that is on the path referencing that executable.
If you need to download a zip, executable, MSI, or something else, then you create a chocolateyInstall.ps1 file where the power of PowerShell and Chocolatey comes in. PowerShell gives you a scripting environment with the full power of the .NET framework. You can set ACLs, download files, etc. With PowerShell you can do anything you can do in .NET. When it comes to Chocolatey, you are automating instructions for installing and/or configuring an application. The PS1 file is just a set of instructions on how to download and "install" an application/tool on a machine. There are built-in helpers that really help with reducing the amount of work that you need to do. Take a look at the following:
```bash
PS C:\>Install-ChocolateyPackage 'notepadplusplus' 'exe' '/S' 'http://download.tuxfamily.org/notepadplus/5.9/npp.5.9.Installer.exe'
```
