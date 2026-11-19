# Varist antivirus and hybrid detection engine plugin - Development

### Configuration

Set your license keys as environment variables to automatically populate the config.ini when starting the devcontainer
```
export HALON_VARIST_LICENSE_KEY_HDE=
export HALON_VARIST_LICENSE_KEY_AV=
```

Download the binaries from https://download.varist.com using your license keys.

Place both zipped folders in the root of the project. When starting the devcontainer the content should automatically be extracted and license keys filled in the respective `config.ini` file.

