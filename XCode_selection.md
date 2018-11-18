A default XCode selection is performed by the following command (substitue a path to XCode you want to select instead of /Applications/Xcode.app)

```
$sudo xcode-select -s /Applications/Xcode.app
```

An SDK directory can then be selected from a script as

```
`xcode-select -p`/Platforms/MacOSX.platform/Developer/SDKs
```
