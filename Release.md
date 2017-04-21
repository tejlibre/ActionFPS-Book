Release process

```
$ git checkout master
$ git commit --allow-empty -m "Release 'version_1234'"
$ git tag 'version_1234'
$ git push origin HEAD --tags 
```

Wait a while (for build to complete),
go to https://github.com/ActionFPS/ActionFPS-Game/releases
and select the release you're interested in. 

Then you can 'publish' the release into a full one.