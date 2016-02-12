# Unity3D with GIT - .gitignore for bitbucket and github

Unity has own VCS (version control system) called Asset Server. It is, however not free. No worries, you can always go with GIT and Github or BitBucket (free up to 5 users)

If you decide to go with GIT here are few steps to follow:

* Create your project
* Enable “Meta files” Mode (Edit > Project Settings > Editor)
* Initiate git with  git init (run command from the top level of your Unity3d project folder)
* Create .gitignore file (also top level of your Unity3d project folder)
* Paste below below settings into your .gitignore file

```
# ---------------[ Unity generated ]------------------ #
Temp/
Obj/
UnityGenerated/
Library/

# ----[ Visual Studio / MonoDevelop generated ]------- #

ExportedObj/
*.svd
*.userprefs
*.csproj
*.pidb
*.suo
*.sln
*.user
*.unityproj
*.booproj

# -------------[ OS generated ]------------------------ #
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
Icon?
ehthumbs.db
Thumbs.db
```
* Create your repository on BitBucket or Github and add it as remote to your project

**Note:**  
If you on mac or windows it might make sense to updated version control server with your public key so you are not prompted password every single time



Here you go. In just few steps you can take your Unity3d development to the next level.


You might want to also check [official unity documentation](
http://docs.unity3d.com/Manual/ExternalVersionControlSystemSupport.html)
