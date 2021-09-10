# delta-with-vacuum
Delta 0.6.x with efficient vacuum (parallel deletion support)

### Problem Statement:

Deltalake vacuum jobs are taking to long to finish as underneath file deletion logic was sequential. Known bug for deltalake (v0.6.1) 
Ref: https://github.com/delta-io/delta/issues/395 

### Solution:

Deltalake team has resolved this issue & yet to be released stable version for this. 
Pull Request: https://github.com/delta-io/delta/pull/522
Lot of organizations are using 0.6.x in production & want this to be part of 0.6.x. 
Following are quick steps to generate delta 0.6.1 jar with this patch 


## Steps for Patch Creation:

#### Git clone deltalake repo v0.6.1
```$xslt
git clone --branch v0.6.1 https://github.com/delta-io/delta
```

#### Change Directory
```
cd delta
```

#### Compile, Build & Test Repo
```
build/sbt compile
build/sbt package
build/sbt test
```

#### Create & download patch
```
wget https://github.com/delta-io/delta/pull/522.patch
```

#### Apply Patch
```
git apply --3way 522.patch
```

#### Resolve conflict in config file - DeltaSQLConf.scala - Keep both properties
```
val DELTA_CHECKPOINT_PART_SIZE = ...
val DELTA_VACUUM_PARALLEL_DELETE_ENABLED = ...
```

#### Re-compile, Build, Test:
```
build/sbt compile
build/sbt package
build/sbt test
```

####  Find the new jars
```
<root-dir>/target/scala-2.11/delta-core_2.11-0.6.1.jar
<root-dir>/target/scala-2.12/delta-core_2.12-0.6.1.jar
```

### Pre-built Jars:
##### Scala 2.11
[delta-core_2.11-0.6.1.jar](https://github.com/swapnil-chougule/delta-with-vacuum/raw/main/jar/scala-2.11/delta-core_2.11-0.6.1.jar)

##### Scala 2.12
[delta-core_2.12-0.6.1.jar](https://github.com/swapnil-chougule/delta-with-vacuum/raw/main/jar/scala-2.12/delta-core_2.12-0.6.1.jar)
