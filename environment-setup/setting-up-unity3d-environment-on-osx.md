# Setting Up Unity3d Environment on OSX

## Running Unity3d with Microsoft Visual Studio on OSX

It doesn't seem to be possible to use MS Visual Studio effectively especially with debugging if Unity is running on OSX based system and VS is running on
a virtual machine.

My advise is to installing Unity3d on both OSX system and a Windows virtual machine along with MS Visual Studio installed on virtual machine only.

When properly configured Windows based Unity can be used for debugging,
general development with Visual Studio and non OSX/IOS deployments. 
OSX based Unity can be used for IOS deployment. 

Both should be able to open project from the same location but it is also possible to have more than one repository clones that are kept in sync via
file source version control system. 

## Prequisits

### Unity3d
[Download](http://unity3d.com/unity/download) and install Unity3d on OSX.

***Note:*** You will need both Windows and OSX version of the installer.

### Parallers
[Download](https://desktop.parallels.com/#/licenses), install
and activate Parallels Desktop.

### Install Windows

 - Select Windows OS ISO (should be found automaticaly if you have one).
 - *Don't* put serial number, select advanced installation options.
 - Actiave Windows

### Windows setup

- Install Parallel Tools from Parallels Desktop menu
`Virtual Machine > Install Parallel Tool`
- Adjust `Video` settings to *Scaled Resolution* from
`Virtual Machine > Configure`
- Make `home folder` available for Windows in sharing options
- Restart Virtual Machine

### Microsoft Visual Studio and Visual Studio Tools setup

- Install [Microsoft Visual Studio](http://www.visualstudio.com/) and install 
- Install [Unity Visual Studio Tools](http://unityvs.com/) and install


Navigate to `Unity > Preferences > External Tools` and point
`External Script Editor` to `Microsoft Visual Studio 2013` location




