Work in Progress for doing the 11 release
```
# Change this each release
export SAKAI_VERSION=11.0

#This should be static
export SAKAI_SNAPSHOT_VERSION=11-SNAPSHOT

git clone git@github.com:sakaiproject/sakai.git
# Or  git pull --rebase  if you're in an already existing directory
git fetch
git checkout 11.x

# Now that you're in the 11 branch, setup the repo to release!
cd master
#Fix the SNAPSHOT's in the properties first
sed -i -e "s/${SAKAI_SNAPSHOT_VERSION}/${SAKAI_VERSION}/" pom.xml
mvn versions:set -DnewVersion=${SAKAI_VERSION} -DgenerateBackupPoms=false
cd ..

mvn clean install -Ppack-bin -Dmaven.test.skip=true

mvn deploy -Psakai-release -Dmaven.test.skip=true

git commit -a -m "Releasing Sakai ${SAKAI_VERSION}"
git tag -a ${SAKAI_VERSION} -m "Tagging Sakai version ${SAKAI_VERSION}"

cd master
mvn versions:set -DnewVersion=${SAKAI_SNAPSHOT_VERSION} -DgenerateBackupPoms=false
git commit -a -m "Switching Sakai back to ${SAKAI_SNAPSHOT_VERSION}"
```

Now if everythings okay (examine the git log, check sonatype and release the artifacts there at https://oss.sonatype.org/index.html#welcome), push it!

```
git push origin ${SAKAI_VERSION}
git push origin 11.x

```