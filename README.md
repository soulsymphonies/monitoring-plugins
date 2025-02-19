# The Linuxfabrik Monitoring Plugins Collection

![image](https://download.linuxfabrik.ch/monitoring-plugins/assets/img/linuxfabrik-monitoring-check-plugins-teaser.png)

This Enterprise Class Check Plugin Collection offers a package of more than 180 Python-based, Nagios-compatible check plugins for Icinga, Naemon, Nagios, OP5, Shinken, Sensu and other monitoring applications. Each plugin is a stand-alone command line tool that provides a specific type of check. Typically, your monitoring software will run these check plugins to determine the current status of hosts and services on your network.

The check plugins run on

* Linux - Tested on RHEL 7+, Fedora 30+, Ubuntu Server 16+, Debian 9+, SLES 15+
* Windows - Tested on Windows 10+ and Windows Server 2019+

All plugins are written in Python and licensed under the [UNLICENSE](https://unlicense.org/), which is a license with no conditions whatsoever that dedicates works to the public domain.

The plugins are fast, reliable and use as few system resources as possible. They uniformly and consistently report the same metrics briefly and precisely on all platforms (for example, always "used" instead of a mixture of "used" and "free"). Automatic detection and Auto-Discovery mechanisms are built-in where possible. Using meaningful default settings, the plugins trigger WARNs and CRITs only where absolutely necessary. In addition they provide information for troubleshooting. We try to avoid dependencies on 3rd party system libraries where possible.


## Support & Sponsoring

The source code is published here without support.

If you need Enterprise Support, [conclude a Service Contract](https://www.linuxfabrik.ch/en/offer/service-and-support/).

If you simply like to support our work, please consider donating and become a sponsor:
* [![GitHubSponsors](https://img.shields.io/github/sponsors/Linuxfabrik?label=GitHub%20Sponsors)](https://github.com/sponsors/Linuxfabrik)
* [![PayPal](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=7AW3VVX62TR4A&source=url)

Do you think more people should know about it? Sharing is caring, so feel free to spread the word. We would really appreciate if you share this on any social media, or link this site on any blog or forum.


## Installation

Have a look at the [INSTALL](https://github.com/Linuxfabrik/monitoring-plugins/blob/main/INSTALL.rst) document for the various options.


## Icons

You can download all check plugin icons from [download.linuxfabrik.ch](https://download.linuxfabrik.ch/monitoring-plugins/icons/icons.tar.gz). For Icinga, put them in `/usr/share/icingaweb2/public/img/icons/`.


## sudoers

On Linux, some check plugins require `sudo`-permissions to run. To do this, we provide a `sudoers` file for your operating system in `monitoring-plugins/assets/sudoers`, for example `CentOS8.sudoers`. You need to place this file in `/etc/sudoers.d/` on the target host.

**Note**

> We are always using the path `/usr/lib64/nagios/plugins/` on all Linux OS, even if `nagios-plugins-all` installs itself to `/usr/lib/nagios/plugins/`. This is because adding a command with `sudo` in Icinga Director, one needs to use the full path of the plugin. See the following [GitHub issue](https://github.com/Icinga/icingaweb2-module-director/issues/2123).


## SELinux

If you are running the plugins on an instance with SELinux that has the `icinga2-selinux` package installed (usually the Icinga server itself), you may need the [Linuxfabrik Monitoring Plugins SELinux Policy Ruleset](https://github.com/Linuxfabrik/monitoring-plugins/blob/main/selinux/linuxfabrik-monitoring-plugins.te) to grant some plugins access to certain resources.

Activate the ruleset like so:

```bash
checkmodule --mls -m --output linuxfabrik-monitoring-plugins.mod linuxfabrik-monitoring-plugins.te
semodule_package --outfile linuxfabrik-monitoring-plugins.pp --module linuxfabrik-monitoring-plugins.mod
semodule --install linuxfabrik-monitoring-plugins.pp
```


## Check Plugin Poster

See some of our check plugins at a glance on an Icinga server:

![image](https://download.linuxfabrik.ch/monitoring-plugins/assets/img/linuxfabrik-monitoring-check-plugins.png)

If you zoom in, for example on *CPU Usage*:

![image](https://download.linuxfabrik.ch/monitoring-plugins/assets/img/linuxfabrik-monitoring-check-plugins-cpu-usage.png)



## Feedback from our Community

A few comments about our monitoring plugins:

> ... I can recommend this family of plugins, they are the highest quality I have seen around. ...

-- [u/exekewtable@reddit](https://www.reddit.com/r/icinga/comments/xcewsg/icinga_python_script_for_qradar_log_source/)

> Ich bin vor kurzem (via Video vom Icinga Camp) über Eure Monitoringplugins gestolpert. Ganz herzlichen Dank dafür, großartige Arbeit!!

-- Christian Lox

> ... many thanks for your great collection of monitoring plugins! I've just found them - clean structure and output, cross-platform, Icinga Directory Basket configurations - loving it and currently migrating step by step most of my checks to use them where possible. 😍

-- [Bernd Bestel](https://github.com/berrnd)

> Nachdem ich beim Versuch, Nagios-Plugins auf VMwares Photon-OS zum laufen zu kriegen, graue Haare gekriegt habe, haben mir eure Plugins zum Ziel verholfen.

-- [MajorTwip](https://twitter.com/MajorTwip)

> A well engineered, regularly updated and maintained collection of plugins. Specially focused on Linux servers/VMs and used at large scale by the company developing it.

-- [straessler](https://exchange.icinga.com/straessler)

> Hello, I stumbled across your collection and am thrilled! Especially the extensive documentary and the Director Baskets are a dream.

-- Stefan Beining


## Human Readable Numbers

Regarding the check plugin output, this is how we convert and append symbols to large numbers in a human-readable format (according to Wikipedia [Names of large numbers](https://en.wikipedia.org/w/index.php?title=Names_of_large_numbers&section=5#Extensions_of_the_standard_dictionary_numbers), and other).

Since the primary hosting platform is Linux, which uses IEC, the plugins display byte sizes in powers of 2 (KiB, MiB, GiB etc.) - otherwise it would be very confusing to have the monitoring plugins said something different than the command line.

| Value             | Symbol   | Origin       | Type              | Description |
| ----------------- | -------- | ------------ | ----------------- | --------------------------------- |
| 1000\^1           | K        |              | Number            | Thousand |
| 1000\^2           | M        | SI Symbol    | Number            | Million ^1^, Million ^2^ |
| 1000\^3           | G        | SI Symbol    | Number            | Milliard ^1^, Billion ^2^ |
| 1000\^4           | T        | SI Symbol    | Number            | Billion ^1^, Trillion ^2^ |
| 1000\^5           | P        | SI Symbol    | Number            | Billiard ^1^, Quadrillion ^2^ |
| 1000\^6           | E        | SI Symbol    | Number            | Trillion ^1^, Quintillion ^2^ |
| 1000\^7           | Z        | SI Symbol    | Number            | Trilliard ^1^, Sextillion ^2^ |
| 1000\^8           | Y        | SI Symbol    | Number            | Quadrillion ^1^, Septillion ^2^ |
| 1024\^0           | B        |              | Bytes             | Bytes |
| 1024\^1           | KiB      | IEC unit     | Bytes             | Kibibytes |
| 1024\^2           | MiB      | IEC unit     | Bytes             | Mebibytes |
| 1024\^3           | GiB      | IEC unit     | Bytes             | Gibibytes |
| 1024\^4           | TiB      | IEC unit     | Bytes             | Tebibytes |
| 1024\^5           | PiB      | IEC unit     | Bytes             | Pebibytes |
| 1024\^6           | EiB      | IEC unit     | Bytes             | Exbibytes |
| 1024\^7           | ZiB      | IEC unit     | Bytes             | Zebibytes |
| 1024\^8           | YiB      | IEC unit     | Bytes             | Yobibytes |
| 1000\^1           | KB       |              | Bytes             | Kilobytes |
| 1000\^2           | MB       |              | Bytes             | Megabytes |
| 1000\^3           | GB       |              | Bytes             | Gigabytes |
| 1000\^4           | TB       |              | Bytes             | Terrabytes |
| 1000\^5           | PB       |              | Bytes             | Petabytes |
| 1000\^6           | EB       |              | Bytes             | Exabytes |
| 1000\^7           | ZB       |              | Bytes             | Zetabytes |
| 1000\^8           | YB       |              | Bytes             | Yottabytes |
| 1000\^1           | Kbps     |              | Bits per Second   | Kilobits |
| 1000\^2           | Mbps     |              | Bits per Second   | Megabits |
| 1000\^3           | Gbps     |              | Bits per Second   | Gigabits |
| 1000\^4           | Tbps     |              | Bits per Second   | Terrabits |
| 1000\^5           | Pbps     |              | Bits per Second   | Petabits |
| 1000\^6           | Ebps     |              | Bits per Second   | Exabits |
| 1000\^7           | Zbps     |              | Bits per Second   | Zetabits |
| 1000\^8           | Ybps     |              | Bits per Second   | Yottabits |
| 1e-12             | ps       |              | Time              | Picoseconds |
| 1e-9              | ns       |              | Time              | Nanoseconds |
| 1e-6              | us       |              | Time              | Microseconds |
| 1e-3              | ms       |              | Time              | Milliseconds |
| 1..59             | s        |              | Time              | Seconds |
| 60                | m        |              | Time              | Minutes |
| 60\*60            | h        |              | Time              | Hours |
| 60\*60\*24        | D        |              | Time              | Days |
| 60\*60\*24\*7     | W        |              | Time              | Weeks |
| 60\*60\*24\*30    | M        |              | Time              | Months |
| 60\*60\*24\*365   | Y        |              | Time              | Years |

* 1: Traditional European (Peletier, long scale)
* 2: US, Canada and modern British (short scale)



## Threshold and Ranges

If a check supports ranges, they can be used as follows:

* Simple value: A range from 0 up to and including the value
* A "Range" is the same as on [nagios-plugins.org](https://nagios-plugins.org/doc/guidelines.html#THRESHOLDFORMAT): *... defined as a start and end point (inclusive) on a numeric scale (possibly negative or positive infinity).*, in the format `start:end`
* Empty value after `:`: positive infinity
* `~`: negative infinity
* `@`: if range starts with `@`, then alert if inside this range (including endpoints)

Examples:

| -w, -c    | OK if result is     | WARN/CRIT if |
| --------- | ------------------- | ------------------- |
| 10        | in (0..10)          | not in (0..10) |
| -10       | in (-10..0)         | not in (-10..0) |
| 10:       | in (10..inf)        | not in (10..inf) |
| :         | in (0..inf)         | not in (0..inf) |
| \~:10     | in (-inf..10)       | not in (-inf..10) |
| 10:20     | in (10..20)         | not in (10..20) |
| \@10:20   | not in (10..20)     | in 10..20 |
| @\~:20    | not in (-inf..20)   | in (-inf..20) |
| @         | not in (0..inf)     | in (0..inf) |



## Command, Parameters and Arguments

Shell commands like `./file-age --filename='/tmp/*'` have two basic parts:

* Command name of the program to run (`./file-age`). May be followed by one or more options, which adjust the behavior of the command or what it will do.
* Options/Parameters normally start with one or two dashes to distinguish them from arguments (parameter `--filename`, value `'/tmp/*'`). They adjust the behavior of the command. Parameters may be short (`-w`) or long (`--warning`). We prefer and often offer only the long version.

Many shell commands may also be followed by one or more arguments, which often indicate a target that the command should operate upon (`useradd linus` for example) . This does not apply to the check-plugins.

To avoid problems when passing *parameter values* that start with a `-`, the command line call must look like this:

* Long parameters: `./file-age --warning=-60:3600` (use `--param=value` instead of `--param value`).
* Short parameters: `./file-age -w-60:3600` (so simply not putting any space nor escaping it in any special way).



## Python 3 vs Python 2

All check plugins are available for Python 3.6+, and most of them also for Python 2.7. The Python 2 check plugins have the suffix "2" (for example `cpu-usage2`), the Python 3 plugins have the suffix "3" (for example `cpu-usage3`).

The Python 3-based check plugins use `#!/usr/bin/env python3`, while the Python 2-based check plugins use `#!/usr/bin/env python2` explicitly.

We stopped maintaining the Python 2-based plugins on 2021-12-31.



## Icinga Director

### For a single Plugin

For each check, we provide an Icinga Director Basket that contains at least the Command definition and a matching Service Template (for example, `check-plugins/cpu-usage/icingaweb2-module-director/cpu-usage.json`). Import this via the WebGUI using Icinga Director > Configuration Baskets > Upload, select the latest entry in the Snapshots tab and restore it.

Alternatively, you can manually configure the plugin as follows.

Create a command for "cpu-usage" in Icinga Director > Commands > Commands:

* Click "+Add", choose Command type: `Plugin Check Command`
* Command name: `cmd-check-cpu-usage`
* Command: `/usr/lib64/nagios/plugins/cpu-usage`
* Timeout: set it according to hints in the check's README (usually `10` seconds)
* Click the "Add" button

Tab "Arguments":

* Run `/usr/lib64/nagios/plugins/cpu-usage --help` to get a list of all arguments.
* Create those you want to be customizable:
    * Argument name `--always-ok`, Value type: String, Condition (set_if): `$cpu_usage_always_ok$`
    * Argument name `--count`, Value type: String, Value: `$cpu_usage_count$`
    * Argument name `--critical`, Value type: String, Value: `` `$cpu_usage_critical$ ``
    * Argument name `--warning`, Value type: String, Value: `` `$cpu_usage_warning$ ``

Tab "Fields":

* Label "CPU Usage: Count", Field name "cpu_usage_count", Mandatory "n"
* Label "CPU Usage: Critical", Field name "cpu_usage_critical", Mandatory "n"
* Label "CPU Usage: Warning", Field name "cpu_usage_warning", Mandatory "n"

Now use this command within a Service Template, a Service Set and/or a Single Service.


### Linuxfabrik's Director Configuration

To use our Icinga Director Configuration including Host Templates, Notifcation Templates and Service Sets, you can generate a single Basket file.

If you are using our [Fork of the Icinga Director](https://github.com/Linuxfabrik/icingaweb2-module-director), you can use the following command:

``` bash
./tools/basket-join
```

If not, generate a Basket without `uuids`:

``` bash
./tools/basket-join
./tools/remove-uuids --input-file icingaweb2-module-director-basket.json --output-file icingaweb2-module-director-basket-no-uuids.json
```

Import the resulting `icingaweb2-module-director-basket.json` via the WebGUI using *Icinga Director > Configuration Baskets > Upload*, select the latest entry in the Snapshots tab and restore it.

If you get the error message `File 'icingaweb2-module-director-basket.json' exeeds the defined ini size.`, you must either load the basket from the command line with `icingacli director basket restore < /tmp/icingaweb2-module-director-basket.json`, or adjust your PHP and/or MariaDB/MySQL settings (as described in [Cant Upload Director Basket](https://github.com/Icinga/icingaweb2-module-director/issues/2458)): 

* PHP: increase `upload_max_filesize` and `post_max_size` (if you use PHP-FPM, don't forget to restart this service)
* MariaDB/MySQL: increase `max_allowed_packet`

If you did not name your master zone `master` during the initial `icinga2 node wizard`, then search and replace `"zone": "master"` with `"zone": "your-master-zone-name"` in the `icingaweb2-module-director-basket.json`.


## Grafana

There are two options to import the Grafana dashboards. You can either import them via the WebGUI or use provisioning.

When importing via the WebGUI simply import the `plugin-name.grafana-external.json` file.

If you want to use provisioning, take a look at [Grafana Provisioning](https://grafana.com/docs/grafana/latest/administration/provisioning/). Beware that you also need to provision the datasources if you want to use provisioning for the dashboards.

If you want to create a custom dashboards that contains a different selection of panels, you can do so using the `tools/grafana-tool` utility.

``` bash
# interactive usage
./tools/grafana-tool assets/grafana/all-panels-external.json
./tools/grafana-tool assets/grafana/all-panels-provisioning.json

# for more options, see
./tools/grafana-tool --help
```


## Reporting Issues

For now, there are two ways:

1.  [Submit an issue](https://github.com/Linuxfabrik/monitoring-plugins/issues/new/choose) (preferred).
2.  [Contact us](https://www.linuxfabrik.ch/en/about-us/contact/) by email or web form and describe your problem.
