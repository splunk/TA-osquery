TA-osquery
==========

![osquery-logo](static/appIcon_2x.png) 

A Splunk technology add-on for osquery

*Join the conversation at* [![Slack Status](https://img.shields.io/badge/slack-@splunk/security-yellow.svg?logo=slack)](https://splunk-usergroups.slack.com/signup)

| branch | build status |
| ---    | ---          |
| master | [![master status](https://circleci.com/gh/splunk/TA-osquery/tree/master.svg?style=svg)](https://circleci.com/gh/splunk/TA-osquery/tree/master)

- Original Author: Jose Hernandez
- Current maintainers:
- Sourcetype: osquery:results, osquery:snapshots, osquery:INFO, osquery:WARNING, osquery:ERROR
- Has index-time ops: false



# Features
* Parses and extracts fields for the following logs: 
	* `osqueryd.INFO, osqueryd.WARNING, osqueryd.ERROR`
	* `osqueryd.results.log` (from process_events and file_events tables)
	* `osqueryd.snapshots.log`
* Provides Datamodel Mapping for:
	* **Alerts** Data Model based on alerts from packs
	* **Changes** Data Model based on custom `splunk` query pack
	* **Endpoint** Data Model based on custom `splunk` query pack
* Does _correct_ time extraction
* Currently documented and tested for macOS only, although parsing logic can be reused for \*nix & Windows if desired.

# Deploying TA
1. Drop the TA on the Search Head(s), Indexer(s), Heavy Forwarder (if required, or if using AWS), and Universal Forwarder (if required).

# Setting up osquery for local logging
This deployment process has been documented and tested based on Mac OS Mojave. It is important that you thoroughly test this on your environment before rolling out to production.
Ref: https://osquery.readthedocs.io

```
#!/bin/bash
sudo spctl --master-disable
sudo installer -pkg osquery.pkg -target /
sudo mkdir /var/log/osquery
sudo cp splunk.conf /var/osquery/packs
sudo cp osquery.conf /var/osquery/osquery.conf
sudo mv /etc/security/audit_control /etc/security/audit_control.bak
sudo echo "#
# $P4: //depot/projects/trustedbsd/openbsm/etc/audit_control#8 $
#
dir:/var/audit
flags:ex,pc,ap,aa,lo,ad
minfree:5
naflags:no
policy:cnt,argv,arge
filesz:2M
expire-after:10M
superuser-set-sflags-mask:has_authenticated,has_console_access
superuser-clear-sflags-mask:has_authenticated,has_console_access
member-set-sflags-mask:
member-clear-sflags-mask:has_authenticated" > /etc/security/audit_control
sudo spctl --master-enable
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.auditd.plist
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.auditd.plist
sudo /usr/local/bin/osqueryctl start
```

# Logging to AWS (preferred)
**osquery config**

This is the preferred collection method as it removes the need for a Universal Forwarder on each endpoint and ensures remote log connection can be achieved whether or not the endpoint is connected to the corporate network. Splunk Cloud is great way of logging from endpoints but would still require a Universal Forwarder to be rolled out.

The install process is the same as above, apart from the osquery.conf needs to be modified to log to AWS. This can be done by following the osquery doc https://osquery.readthedocs.io/en/stable/deployment/aws-logging/

**splunk config**
1. Install and configure [Splunk Add-On For Amazon Web Services](https://splunkbase.splunk.com/app/1876/) on your Heavy Forwarder to pull your osquery events from your S3 bucket.


# To Do's
* Test Changes Data model mapping for FIM events in the results log
* Document a 'Logging locally' method, whereby log rotation is discussed, and running Splunk UF as root to read the log file.

