TA-osquery
==========

![osquery-logo](static/appIcon_2x.png) 
An Splunk technology add-on for osquery

*Join the conversation at* [![Slack Status](https://https://img.shields.io/badge/slack-@splunk/security-yellow.svg?logo=slack)](https://splunk-usergroups.slack.com/signup)

| branch | build status |
| ---    | ---          |
| master | [![master status](https://circleci.com/gh/splunk/TA-osquery/tree/master.svg?style=svg&circle-token=3235b111e69550b365975bc1612b496ba4dcb556)](https://circleci.com/gh/splunk/TA-osquery/tree/master)

- Original Author: Jose Hernandez
- Current maintainers:
- Sourcetype: osquery:results, osquery:snapshots, osquery:INFO, osquery:WARNING, osquery:ERROR
- Has index-time ops: false

# Features
* Parses and extracts fields for the following logs: 
	* `osqueryd.INFO, osqueryd.WARNING, osqueryd.ERROR`
	* `osqueryd.results.log`
	* `osqueryd.snapshots.log`
* Provides Datamodel Mapping for:
	* **Alerts** Data Model base on alerts from packs
	* **Changes** Data Model base on FIM events from packs
	* **Endpoint** Data Model base on Splunks Query Pack (todo)
* Does _correct_ time extraction 

# Deploying
1. Remember to drop the TA in your indexers as well as your forwarders
2. Do not forget to remove the example `default/inputs.conf`
3. Add the Splunk query pack to your osquery agent (todo)

# To Do's
* Test Changes Data model mapping for FIM events in the results log
* Create a splunk query pack
* Populate Endpoint DM with the results of the splunk query pack

