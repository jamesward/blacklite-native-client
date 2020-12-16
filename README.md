# Blacklite Reader

## Build JVM App
```
./gradlew install
export PATH=$PATH:build/install/blacklite-reader/bin
```

## Or Build the GraalVM Native Image
```
./gradlew nativeImage
export PATH=$PATH:build/graal
```

## Date Processing

```
blacklite-reader \
  --after="2020-11-03 19:22:09" \
  --before="2020-11-03 19:22:11" \
  --timezone=PST \
  /tmp/blacklite/archive.2020-11-03-07-22.669.db
```

## Still dies with exception

Anything with a "where clause" dies.

```
blacklite-reader -c --where="limit > 10000" /tmp/blacklite/archive.db
```

## Working with Binary Content

Extract data using the `binary` flag and redirect to a file.

```bash
blacklite-reader --binary /tmp/blacklite/archive.db > zarchive.zst
zstd -d zarchive.zst 
```

## Dev Info

Generate Native Image Configs
```
JAVA_HOME=~/.gradle/caches/com.palantir.graal/20.3.0/11/graalvm-ce-java11-20.3.0 \
JAVA_OPTS=-agentlib:native-image-agent=config-output-dir=src/main/resources/META-INF/native-image \
build/install/blacklite-reader/bin/blacklite-reader /tmp/blacklite/archive.db
```
