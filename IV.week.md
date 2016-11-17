
# new tags

simple as:

```
s1: xcode7.3.1
s2: xcode8.1
s3: xcode8.1
s4: xcode8.0
s5: xcode8.1
```
---

# blade

https://github.com/jondot/blade

```bash
$ cat Bladefile 
```

```ruby
blades:
  - source: assets/iTunesArtwork@2x.png
    mount: Gistr/Images.xcassets/AppIcon.appiconset
    contents: true
```

```bash
$ blade --verbose
```

---

# asdf package manager (ruby)

https://github.com/asdf-vm/asdf
https://github.com/asdf-vm/asdf-ruby

```bash
$ asdf local ruby 2.2.3
```

---

# fastlane storage problem

> $ vim fastlane/.env

```bash
GYM_OUTPUT_DIRECTORY="./"
GYM_BUILD_PATH="./"
GYM_DERIVED_DATA_PATH="./"
GYM_BUILDLOG_PATH="./"
GYM_ARCHIVE_PATH="./"
```

---

# enterprice deploy

- requier fastlane update
- new env value

```bash
SIGH_PROFILE_ENTERPRISE=true
```

---

# gitlab-ci.yml

```yml
before_script:
  - export LANG=en_US.UTF-8
  - xcrun swift -version
  - xcodebuild -version
  
  # download and load envs
  - python tools/download.py --token ${AUTO_CLOSE_TOKEN} --key-version ${token}
  - source artifacts/.env-ci
  
  # install submodules
  - git submodule update --init --recursive

  # install bundler
  - gem install bundler

  # install gems, update pods repo
  - bundle install --quiet
  - bundle exec pod repo update --silent
```

---

# private keychain

```bash
KEYCHAIN_NAME="ci.keychain"
KEYCHAIN_PASSWORD="password"
```

```ruby
private_lane :beta do |options|
	...
	create_keychain(unlock: true, timeout: false)

	import_certificate(
	    keychain_password: ENV["KEYCHAIN_PASSWORD"], 
	    certificate_path: options[:certificate_path], 
	    certificate_password: options[:certificate_password], 
	    log_output:true
	)
	...
end

after_all do |lane|
	...
	delete_keychain
end

error do |lane, exception|
	...
	delete_keychain
end

```

---

#GitLab

follow changes at:
http://git.appunite.com/appunite/signing-tuts-ios

---

# protip

In Xcode 8, place the cursor above a method or function & press "⌥ + ⌘ + /" to auto-generate a doc comment.

source: https://twitter.com/felix_schwarz/status/774166330161233920