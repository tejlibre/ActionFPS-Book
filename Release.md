TODO: describe release process. very easy : generate windows installer, generate linux client & server.
then execute the script that uploads the whole thing to github with the API key. its all ready, just document it.
ez

## Update Version

* Edit source/src/cube.h
* Edit source/vcpp/buildEnv/ac.nsi
* Edit actionfps_release.bat
* Edit actionfps.sh

## Build Packages

Requirements : NSIS, http://nsis.sourceforge.net/mediawiki/images/8/8a/GetVersion.zip, \Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe

* Run build scripts in source/vcpp/buildEnv/ on windows
* Upload windows_client.exe to ??? on a linux machine
* Run source/release/package_assaultcube.sh github_user github_apikey on this linux machine


