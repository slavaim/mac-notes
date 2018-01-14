To inspect a signature and entitlemenst for an application execute ```codesign``` command as shown (in this case for /Applications/TextEdit.app)

```
$ codesign -dvvv --entitlements - /Applications/TextEdit.app
Executable=/Applications/TextEdit.app/Contents/MacOS/TextEdit
Identifier=com.apple.TextEdit
Format=app bundle with Mach-O thin (x86_64)
CodeDirectory v=20100 size=1475 flags=0x0(none) hashes=39+5 location=embedded
Platform identifier=4
Hash type=sha256 size=32
CandidateCDHash sha256=8f7a256755f3741aeff52dc7d16e8f859f780b38
Hash choices=sha256
CDHash=8f7a256755f3741aeff52dc7d16e8f859f780b38
Signature size=4485
Authority=Software Signing
Authority=Apple Code Signing Certification Authority
Authority=Apple Root CA
Info.plist entries=32
TeamIdentifier=not set
Sealed Resources version=2 rules=13 files=431
Internal requirements count=1 size=68
??qq~<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.apple.application-identifier</key>
	<string>com.apple.TextEdit</string>
	<key>com.apple.developer.ubiquity-container-identifiers</key>
	<array>
		<string>com.apple.TextEdit</string>
	</array>
	<key>com.apple.security.app-sandbox</key>
	<true/>
	<key>com.apple.security.files.user-selected.executable</key>
	<true/>
	<key>com.apple.security.files.user-selected.read-write</key>
	<true/>
	<key>com.apple.security.print</key>
	<true/>
</dict>
</plist>
```
