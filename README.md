# Fontforge docker image
Repository to build and publish docker image containing fontforge cli.

## How to build

```
./gradlew docker -i
```

## Push docker image

```
./gradlew dockerTagsPush -i
```

##### Note: alpine image is not built by this command as it's still experimental.
