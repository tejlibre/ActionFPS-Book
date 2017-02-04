Release process

```
$ git checkout master
$ git commit --allow-empty -m "Release 'version_1234'"
$ git tag 'version_1234'
$ git push origin HEAD --tags 
```