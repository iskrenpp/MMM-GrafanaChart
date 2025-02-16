# Magic Mirror Module: MMM-GrafanaChart
This [MagicMirror²](http://robstechlog.com/2017/06/25/building-a-big-magicmirror-with-metal-frame-the-summary/) module allows you to display a chart generated by [grafana](https://grafana.com/).

<b>Important Note:</b> This module requires a running grafana installation. To install Grafana, follow the official [installation instructions](http://docs.grafana.org/installation/).

<b>[This blogpost](http://www.robstechlog.com/2017/06/30/personal-weather-chart-module/) describes how to install and use grafana and build a weatherchart.</b><br>
![example of a grafana weather chart](https://github.com/SvenSommer/MMM-GrafanaChart/blob/master/weather_explained.gif?raw=true)

## Installation of the module

In your terminal, go to your MagicMirror's Module folder:
````
cd ~/MagicMirror/modules
````

Clone this repository:
````
git clone https://github.com/SvenSommer/MMM-GrafanaChart
````

Configure the module in your `config.js` file.

## Configuration
<b> Configuration of the Grafana </b>

Change the grafana.ini to have the following settings
````ini
[auth.anonymous]

# enable anonymous access
enabled = true

# specify role for unauthenticated users
org_role = Viewer

[security]
# https://grafana.com/docs/grafana/latest/installation/configuration/#allow-embedding
allow_embedding = true
````
<b> Configuration of the MagicMirror config-file </b>

To use this module, you have to specify where your grafana installation is hosted and which chart you'd like to display.


Add the module to the modules array in the `config/config.js` file:
````javascript
modules: [
	{
		module: 'MMM-GrafanaChart',
		position: 'top_right',   // This can be any of the regions.
		config: {
		                configVersion: "v1", // this is optional as default is v1 e.g. original config settings. Uncomment and set to "v2" for new simpler config for url with only one complete url string
				width: "100%", // Optional. Default: 100%
				height: "100%", // Optional. Default: 100%
				scrolling: "yes", // Optional. Default: no
				refreshInterval: 900, //Optional. Default: 900 = 1/4 hour
				chartTag: "custom chart name", // Optional. Used for log message in Console when using more than one module in config.js
				
				// below config is required for configVersion = "v2"
				url: "https://localhost:5000/....", // see below on how to get the URL from Grafana
				// end of required config for configVersion = "v2"
				
				// below config is required for configVersion = "v1"
				version: "6", // Only add this line if you are using Grafana verison 6 or greater
				id: "as8fA8na", // Only Mandartory if you are using Grafana verison 6 or greater found after /d/ in the url
				host: "grafana_host", //Mandatory. See url when displaying within grafana
				port: 3000, // Mandatory.
				dashboardname: "weatherforecast", // Mandatory.
				orgId: 1, // Mandatory.
				panelId: 2, // Mandatory.
				from: "now-1d", // use any of grafanas time-range-controls
				to: "now", // 
				// end of required config for configVersion = "v1"
		}
	},
]
````

### ORIGINAL CONFIGURATION WITH PARSED URL PARTS ( configVersion = "v1" ):

Everything needed is extractable from the <code>url</code> when you're viewing your chart using grafana in your browser.

<b>Grafana version 6.x</b>

![url provides needed information](https://github.com/SvenSommer/MMM-GrafanaChart/blob/master/grafana_version_6_explanations_image.png?raw=true)

<b>Grafana version 5 or older</b>

![url provides needed information](https://github.com/SvenSommer/MMM-GrafanaChart/blob/master/config_url.png?raw=true)

### NEW CONFIGURATION WITH PARSED URL PARTS ( configVersion = "v2" ):

Grafana has a nice dialog that allows you to generate the embed URL you need.

For this make sure that your current view of the graph represents the time range and autorefresh interval you want on your mirror.

Then click the graph's header, go to "Share", then into the "Embed" tab.

Deselect the "Current time range" option, which would turn your "last three hours" query into the actual last three hours in absolute time.

Afterwards you can copy the URL from the `iframe`'s `src` attribute. Open it in a tab to check that everything looks and behaves as you would like.

If everything is as you like, you can copy the URL and paste it in the `url` config option.

Here is the process in a short animated form:
![How to get the URL](EmbedURL_from_Grafana.gif)

## Optional configuration options

The following properties can be configured:


<table width="100%">
	<!-- why, markdown... -->
	<thead>
		<tr>
			<th>Option</th>
			<th width="100%">Description</th>
		</tr>
	<thead>
	<tbody>
		<tr>
			<td><code>width</code></td>
			<td>Width of the displayed chart. <code>'150 px'</code> or <code>'50 %'</code> are valid options.	<br><b>Default value:<code>"100%"</code></b></td>
		</tr>
		<tr>
			<td><code>height</code></td>
			<td>Height of the displayed chart. <code>'150 px'</code> or <code>'50 %'</code> are valid options.	<br><b>Default value:<code>"100%"</code></b></td>
		</tr>
		<tr>
			<td><code>refreshInterval</code></td>
			<td>Update interval of the diagram in seconds.
				<br><b>Default value:</b> <code>900</code>  = 15 \* 60 (four times every hour)
			</td>
		</tr>
	</tbody>
</table>
