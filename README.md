# OpenAI REST Collector IO
----
## About this Pack

This pack is built as a complete SOURCE + DESTINATION solution (identified by the IO suffix). Data collection and delivery happen entirely within the pack's context, eliminating the need to connect it to globally defined Sources and Destinations. 

This Pack is designed to collect, process, and output OpenAI log data via the [OpenAI REST API](https://platform.openai.com/docs/api-reference/introduction). It currently supports the following endpoints:
* [Audit Logs](https://platform.openai.com/docs/api-reference/audit-logs)
* [Costs](https://platform.openai.com/docs/api-reference/usage/costs)
* [Users](https://platform.openai.com/docs/api-reference/users/list)
* [Projects](https://platform.openai.com/docs/api-reference/projects/list)
* [Completions Usage Details](https://platform.openai.com/docs/api-reference/usage/completions)
* [Embeddings Usage Details](https://platform.openai.com/docs/api-reference/usage/embeddings)
* [Moderations Usage Details](https://platform.openai.com/docs/api-reference/usage/moderations)
* [Images Usage Details](https://platform.openai.com/docs/api-reference/usage/images)
* [Audio Speeches Usage Details](https://platform.openai.com/docs/api-reference/usage/audio_speeches)
* [Audio Transcriptions Usage Details](https://platform.openai.com/docs/api-reference/usage/audio_transcriptions)

The Pack includes (optional) Splunk output processing that maps OpenAI data to the following sourcetypes by default:
* Audit Logs: `sourcetype=openai:audit`
* Costs: `sourcetype=openai:costs`
* Users: `sourcetype=openai:users`
* Projects: `sourcetype=openai:projects`
* All Usage Details data: `sourcetype=openai:bucket_data`



## Deployment

* This pack is configured by default to use the Worker Group's *Default Destination*.
* To use the *Default Destination*: No changes are required. The pack will route the data to the destination currently set as the Default on the Worker Group.
* To use a different Destination: You must update the pack's routes to specify your desired Destination.
* For immediate functionality without requiring Pack route filter expression modifications, every bundled Source within this pack adds a hidden field: `__packsource`. This field allows for seamless routing based on the Pack source.

### Configure the Collectors

* Obtain an [OpenAI API Key](https://platform.openai.com/settings/organization/api-keys) from your Administrator.
* For each Collector, update the Collect Header named `Authorization` to include your API Key. It must be a Javascript string in the format `'Bearer <your_api_token'`.
* Perform a Run > Preview to verify that each Collector works correctly.
* Schedule data ingestion by clicking the **Schedule** button under the Actions column of the Collector and toggle the **Enabled** button to enable data flow. Collectors requiring State Tracking have it enabled by default. Modify the cron schedule if required:
  * The default for Audit Log data is to run every 5 minutes. 
  * The default for all other Collectors is to run once daily just past midnight.

### Configure your Destination/Update Pack Routes
To ensure proper data routing, you must make a choice: retain the current setting to use the Default Destination defined by your Worker Group, or define a new Destination directly inside this pack and adjust the pack's route accordingly.

This Pack includes a Splunk HEC Output. To configure it:

* Create a [HEC token in Splunk](https://help.splunk.com/en/splunk-enterprise/get-started/get-data-in/10.0/get-data-with-http-event-collector/set-up-and-use-http-event-collector-in-splunk-web).
* Populate the **Splunk HEC Endpoints** parameter with the Splunk HEC URL.
* Populate the **HEC Auth token** with the token created in Splunk

### Commit and Deploy
Once everything is configured, perform a Commit & Deploy to enable data collection.

## Pack Configurable Items 
The following are the in-Pack configurable items - review/update them as needed. 

### Lookups
The `cribl_openai_audit_log_types.csv` is used to configure OpenAI audit log `type`'s that should be dropped (drop functionality is controlled by a variable - see below):
* `type`: OpenAI [audit log type](https://platform.openai.com/docs/api-reference/audit-logs/object)
* `action`: "drop" to drop this `type`

### Variables

The Pack has the following variables:
* `cribl_openai_enable_audit_log_drop`: Set to `true` to enable audit log reduction.
* `cribl_openai_drop_empty_usage_detail_events`: Set to `true` to drop usage detail events that have no results. 
* `openai_splunk_default_index`: Default index for the Splunk output - defaults to `openai`.

## Upgrades

Upgrading certain Cribl Packs using the same Pack ID can have unintended consequences. See [Upgrading an Existing Pack](https://docs.cribl.io/stream/packs#upgrading) for details.

## Release Notes


### Version 1.0.0
Initial release

## Contributing to the Pack

To contribute to the Pack, please connect with us on [Cribl Community Slack](https://cribl-community.slack.com/). You can suggest new features or offer to collaborate.

## License
This Pack uses the following license: [Apache 2.0](https://github.com/criblio/appscope/blob/master/LICENSE).