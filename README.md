# OpenAI REST Collector IO
----
## About this Pack

This pack is built as a complete SOURCE + DESTINATION solution (identified by the IO suffix). Data collection and delivery happen entirely within the Pack's context - you can choose how data arrives at a DESTINATION:
*  *Send to Worker Group Routes* (the default): data is sent to the top-level Worker Group Routes.
*  *Default Destination*: data is sent to the Worker Group's [Default Destination](https://docs.cribl.io/stream/destinations-default/). 
*  *In-Pack Destination*: data is sent to one or more Destinations configured within the Pack.

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

* Every bundled Source within this pack adds a hidden field: `__packsource`. This field allows for simplified routing based on the Pack source.
* This pack is configured by default to use the Destination *Send to Worker Group Routes*. You *must* add either a Worker Group Route or rely on the Default Destination.
* To explicitly use the Worker Group's *Default Destination*, change the Pack's Routes to *default:default*. The Pack will then route the data to the destination currently set as the Default on the Worker Group.

### Configure the Collectors

*Note on Collector naming: Collectors ending in `_1h` collect 24 one hour "bucket widths" of data once/day. If you want to collect other widths, clone the collector and modify the bucket_width, limit, and schedule. See documentation links above for options.*

* Obtain an [OpenAI API Key](https://platform.openai.com/settings/organization/api-keys) from your Administrator.
* Set the `openai_api_token` variable to this value.
* Perform a Commit/Deploy (otherwise Preview may throw errors).
* Perform a Run > Preview to verify that each Collector works correctly.
* Schedule data ingestion by clicking the **Schedule** button under the Actions column of the Collector and toggle the **Enabled** button to enable data flow. Collectors requiring State Tracking have it enabled by default. Modify the cron schedule if required:
  * The default for Audit Log data is to run every 5 minutes. 
  * The default for all other Collectors is to run once daily just past midnight.

### Configure your Destination/Update Pack Routes
To ensure proper data routing, you must make a choice: retain the current setting to use the Default Destination defined by your Worker Group, or define a new Destination directly inside this pack and adjust the pack's route accordingly.

### Commit and Deploy
Once everything is configured, perform a Commit & Deploy to enable data collection (if changes have been made).

## Pack Configurable Items 
The following are the in-Pack configurable items - review/update them as needed. 

### Lookups
The `cribl_openai_audit_log_types.csv` is used to configure OpenAI audit log `type`'s that should be dropped (drop functionality is controlled by a variable - see below):
* `type`: OpenAI [audit log type](https://platform.openai.com/docs/api-reference/audit-logs/object)
* `action`: "drop" to drop this `type`

### Variables

The Pack has the following variables:
* `openai_api_token`: Your OpenAI API token
* `cribl_openai_enable_audit_log_drop`: Set to `true` to enable audit log reduction.
* `cribl_openai_drop_empty_usage_detail_events`: Set to `true` to drop usage detail events that have no results. 
* `openai_splunk_default_index`: Default index for the Splunk output - defaults to `openai`.

## Upgrades

Upgrading certain Cribl Packs using the same Pack ID can have unintended consequences. See [Upgrading an Existing Pack](https://docs.cribl.io/stream/packs#upgrading) for details.

## Release Notes
### Version 1.1.1
* Updated Route Destinations to "Send to Worker Group Routes". See above for details.

### Version 1.1.0
- Collectors are now configured via variables
- Audit log Collector is pre-scheduled

### Version 1.0.0
- Initial release

## Contributing to the Pack

To contribute to the Pack, please connect with us on [Cribl Community Slack](https://cribl-community.slack.com/). You can suggest new features or offer to collaborate.

## License
This Pack uses the following license: [Apache 2.0](https://github.com/criblio/appscope/blob/master/LICENSE).