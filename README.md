# Varist antivirus and hybrid detection engine plugin

## Installation

Follow the [instructions](https://docs.halon.io/manual/comp_install.html#installation) in our manual to add our package repository and then run the below command.

### Ubuntu

```
apt-get install halon-extras-varist
```

### RHEL

```
yum install halon-extras-varist
```

##  Example

### varist_hde(fp [, options])
Scan a File pointer (fp) with the Varist Hybrid Detection Engine.

#### Params

- fp `File` - the mail file
- options `array` - options array

The following options are available in the **options** array
- flat `boolean` - If set to true, the JSON report structure is flat. Defaults to false.
- pw `string` - Password to decrypt zip and 7zip files.
- address `string` - Address of the server where the Anti-Malware Service is running.
- port `number` - TCP port of the Anti-Malware Service server
- timeout `number`- Timeout in seconds. The default is 5 seconds.
- filename `string` - Set the name of the file.


#### Returns

Ann associative array with a `report` property object that contains various information coming from the engine. Such as `Rating`, `ScanResult`, `FileName`, etc. An `error` property is set if an error occurs.


```
import { varist_hde } from "varist";

$file = $mail->toFile();
$res = varist_hde($file, [
    "address" => "http://hybrid-analyzer",
    "port" => 4660,
]);

if ($res["report"]["Rating"] > 75)
    Reject();
```

### varist_av(fp [, options])
Scan a File pointer (fp) with the Varist Anti-Malware Service.

#### Params

- fp `File` - the mail file
- options `array` - options array

The following options are available in the **options** array

- address `string` - Address of the server where the Anti-Malware Service is running.
- port `number` - TCP port of the Anti-Malware Service server
- timeout `number`- Timeout in seconds. The default is 5 seconds.
- filename `string` - Set the name of the file.


#### Returns

An associative array, with a `result` proptery containing a string value representing the result of the scanning. For example `CLEAN`, `INFECTED` or `ERROR`. `detections` a property containing some more information on what was found when scanning the input. An `error` property is set if any error occurs containing the error message.
```
import { varist_av } from "varist";

$file = $mail->toFile();
$res = varist_av($file, [
    "address" => "http://scanserver",
    "port" => 4660,
]);

if ($res["result"] != "CLEAN") 
    Reject();
```


### varist_hde_risk_level(rating [, mediumThreshold, [ highThreshold]])

Get a risk level based on the report rating.

#### Params
- rating `number` - The rating to get a risk level for.
- mediumThreshold `number` - Custom medium risk level threshold. Defaults to 40
- highThreshold `number` - Custom high risk level threshold. Defaults to 75


#### Returns
A risk level as a string value. Possible levels are `clean`, `malicious`, `low`, `medium`, `high`.

```
import { varist_hde_risk_level, varist_hde } from "varist";

$file = $mail->toFile();
$res = varist_hde($file, [
    "address" => "http://hybrid-analyzer",
    "port" => 4660,
]);

$rating = $res["report"]["Rating"];

$level = varist_hde_risk_level($rating);

if ($level != "clean")
    Reject();
```