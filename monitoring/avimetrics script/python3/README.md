# Avi Metrics Script

The Avi Metrics script was built with the intent to pull metrics from one or more Avi controllers and send these values to a centralized metrics database(s).
The script supports a number of different endpoints; current support includes AppDynamics, Datadog, Elasticsearch, Graphite, InfluxDB, Logstash, Splunk, and Wavefront.


This repository includes that necessary files to deploy a centralized metrics script
- **Requirements**
    - **python 3.6+**
    - **python3-yaml**
    - **python3-requests**


- **Files**
    - **avimetrics.py**:  This is the script that will pull the values from the Avi Controller API and forward to the metrics enpoints
    - **metrics_endpoints.py**:  This file contains the methods for formatting the data and sending to the defined metrics endpoint(s)
    - **configuration_example.yaml**:  This is an example a <strong> required </strong> configuration.yaml file.  Further details around are found below.
    - **dockerfile**:  This file contains the commands to build a container  








# Installation
The following files are required and must exist within the same directory for successful metric gathering.
- **avimetrics.py**
- **configuration.yaml**
- **metrics_endpoints.py**




# Usage
## avimetrics.py

The metrics script will look in the local directory for the configuration.yaml file.  Using the values from this file the script will pull the relevant data from the Avi Controller API and forward the values to the defined metrics endpoints.

```sh
$ python3 avimetrics.py
```


# Run as a container
To run this script as a container, modify the files as exampled above prior to building.

### Build the container
```sh
$ docker build -t avimetrics .
```
### Start the container
To start the container it is required to specify the configuration via the <strong>EN_CONFIGURATION</strong> environment variable.  

Here is an example for using the contents of a local configuration.yaml as the value for EN_CONFIGURATION when creating the local container.

```sh
$ docker run -d --name avimetrics --restart always --log-opt max-size=1m -e "EN_CONFIGURATION=$(<configuration.yaml)"  avinetworks/avimetrics
```


# configuration.yaml

The configuration.yaml file will include the parameters to define what data values are desired for each Avi Controller cluster.  Within the configuration.yaml file, you will define controller(s) within a list under the key "controllers".  For each controller entry the following values are available:


<table class="documentation-table" cellpadding="0" border="0">
    <tbody><tr>
        <th colspan="4">Parameter</th>
        <th>Choices/<font color="blue">Defaults</font></th>
        <th width="100%">Comments</th>
<tr>
<td colspan="4">
<b>controllers</b>
<div style="font-size: small">
<span style="color: purple">list</span>
</div>
</td>
<td>
</td>
<td>
<div>List of Avi controller clusters and their relevant metrics configurations.</div>
</td>
</tr>
<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>avi_cluster_name</b>
<div style="font-size: small">
<span style="color: purple">string</span>
/ <span style="color: red">required</span>                    
</div>
</td>
<td>
</td>
<td>
<div>Label that Avi controller cluster will be referred as.</div>
</td>
</tr>
<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>avi_controller</b>
<div style="font-size: small">
<span style="color: purple">string</span>
/ <span style="color: red">required</span>                    
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>FQDN or IP address that the script will connect to for API calls</div>
</td>
</tr>

<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>avi_user</b>
<div style="font-size: small">
<span style="color: purple">string</span>
/ <span style="color: red">required</span>                    
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Username that will be used to authenticate to Avi controller cluster</div>
</td>
</tr>

<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>avi_pass</b>
<div style="font-size: small">
<span style="color: purple">string</span>
/ <span style="color: red">required</span>                    
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Password that will be used to authenticate to Avi controller cluster</div>
</td>
</tr>

<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>virtualservice_stats_config</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for defining desired virtualservice metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>virtualservice_metrics</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Retrieve performance metrics for virtualservices</div>
</td>
</tr>


<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>virtualservice_realtime</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Pull realtime metrics for virtualservices if enabled in Avi config</div>
</td>
</tr>



<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>virtualservice_runtime</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Pull runtime data for virtualservices</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>virtualservice_metrics_list</b>
<div style="font-size: small">
<span style="color: purple">list</span>                 
</div>
</td>
</div>
</td>
<td><strong>Default:</strong> Script contains a list of default virtualservice metric.  List provided further within this document
</td>
<td>
<div>List of virtualservice metrics to pull from API</div>
</td>
</tr>


<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>serviceengine_stats_config</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for defining desired serviceengine metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>serviceengine_metrics</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Retrieve performance metrics for serviceengines</div>
</td>
</tr>


<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>serviceengine_realtime</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Pull realtime metrics for serviceengines if enabled in Avi config</div>
</td>
</tr>



<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>serviceengine_runtime</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Pull runtime data for serviceengines</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>serviceengine_metrics_list</b>
<div style="font-size: small">
<span style="color: purple">list</span>                 
</div>
</td>
</div>
</td>
<td><strong>Default:</strong> Script contains a list of default serviceengine metric.  List provided further within this document
</td>
<td>
<div>List of serviceengine metrics to pull from API</div>
</td>
</tr>

<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>pool_stats_config</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for defining desired pool metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>pool_metrics</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Retrieve performance metrics for pools</div>
</td>
</tr>


<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>pool_realtime</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Pull realtime metrics for pools if enabled in Avi config</div>
</td>
</tr>



<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>pool_runtime</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Pull runtime data for pools</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>pool_metrics_list</b>
<div style="font-size: small">
<span style="color: purple">list</span>                 
</div>
</td>
</div>
</td>
<td><strong>Default:</strong> Script contains a list of default pool metric.  List provided further within this document
</td>
<td>
<div>List of pool metrics to pull from API</div>
</td>
</tr>

<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>controller_stats_config</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for defining desired controller metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>controller_metrics</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Retrieve performance metrics for controllers</div>
</td>
</tr>


<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>controller_realtime</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
</td>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Pull realtime metrics for controllers if enabled in Avi config</div>
</td>
</tr>


<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>controller_metrics_list</b>
<div style="font-size: small">
<span style="color: purple">list</span>                 
</div>
</td>
</div>
</td>
<td><strong>Default:</strong> Script contains a list of default controller metric.  List provided further within this document
</td>
<td>
<div>List of controller metrics to pull from API</div>
</td>
</tr>


<tr>
<td class="elbow-placeholder"></td>
<td colspan="3">
<b>metrics_endpoint_confiig</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for defining desired endpoints for the metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>appdynamics_http</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending data to appdynamics http endpoint listener</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li>
    <li>False</li>
</td>
<td>
<div>Enable sending metrics to appdynamics http endpoint listener</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>IP or FQDN for the Appdynamics HTTP endpoint listener</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server_port</b>
<div style="font-size: small">
<span style="color: purple">integer</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Listening port for the Appdynamics HTTP endpoint listener</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>datadog</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending metrics to datadog</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li></b>
    <li>False</li>
<td>
<div>Enable sending metrics to datadog</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>api_url</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>API url for sending data to datadog. EX:  app.datadoghq.com/api/v1/series?api_key=</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>api_key</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>API key used for authentication to datadog api</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>elasticsearch</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending metrics to elasticsearch</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li></b>
    <li>False</li>
</td>
<td>
<div>Enable sending metrics to elasticsearch</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>IP or FQDN for sending data to elasticsearch</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server_port</b>
<div style="font-size: small">
<span style="color: purple">integer</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Listening port for elasticsearch </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>protocol</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td><li>HTTP</li></b>
    <li>HTTPS</li>
</td>
<td>
<div>Protocol used for elasticsearch; HTTP / HTTPS </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>index</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Elasticsearch index used for Avi metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>timestamp</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Elasticsearch fieldname used for time filer.  Ex:  @timestamp</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>auth-enabled</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td><li>True</li></b>
    <li>False</li>
</td>
<td>
<div>Define if elasticsearch has authentication enabled</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>username</b>
<div style="font-size: small">
<span style="color: purple">string</span>                 
</div>
</td>
</div>
<td>
</td>
<td>
<div>If auth-enabled is True, username used for authentication to elasticsearch</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>password</b>
<div style="font-size: small">
<span style="color: purple">string</span>                 
</div>
</td>
</div>
<td>
</td>
<td>
<div>If auth-enabled is True, password provided for authentication to elasticsearch</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>graphite</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending metrics to graphite</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li></b>
    <li>False</li>
</td>
<td>
<div>Enable sending metrics to graphite</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>IP or FQDN for sending data to graphite</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server_port</b>
<div style="font-size: small">
<span style="color: purple">integer</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Listening port for graphite </div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>influxdb</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending metrics to influxdb</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li></b>
    <li>False</li>
</td>
<td>
<div>Enable sending metrics to influxdb</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>IP or FQDN for sending data to influxdb</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server_port</b>
<div style="font-size: small">
<span style="color: purple">integer</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Listening port for influxdb </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>protocol</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td><li>HTTP</li></b>
    <li>HTTPS</li>
</td>
<td>
<div>Protocol used for Influxdb; HTTP / HTTPS </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>db</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Influxdb DB used for Avi metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>metric_prefix</b>
<div style="font-size: small">
<span style="color: purple">string</span>                  
</div>
</td>
</div>
<td>
</td>
<td>
<div>If desired, prefix to apply to metric names</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>auth-enabled</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>                 
</div>
</td>
</div>
<td><li>True</li></b>
    <strong><li>False&nbsp;←</li></strong>
</td>
<td>
<div>Define if Influxdb is setup for authentication</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>username</b>
<div style="font-size: small">
<span style="color: purple">string</span>                 
</div>
</td>
</div>
<td>
</td>
<td>
<div>If auth-enabled is True, username used for authentication to influxdb</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>password</b>
<div style="font-size: small">
<span style="color: purple">string</span>                 
</div>
</td>
</div>
<td>
</td>
<td>
<div>If auth-enabled is True, password provided for authentication to influxdb</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>logstash</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending metrics to logstash</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li></b>
    <li>False</li>
</td>
<td>
<div>Enable sending metrics to logstash</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>IP or FQDN for sending data to logstash</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server_port</b>
<div style="font-size: small">
<span style="color: purple">integer</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Listening port for logstash </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>protocol</b>
<div style="font-size: small">
<span style="color: purple">string</span>                
</div>
</td>
</div>
<td><strong><li>UDP&nbsp;←</li></strong>
    <li>TCP</li>
</td>
<td>
<div>Protocol used for logstash; UDP / TCP </div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>splunk</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending metrics to Splunk HEC</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li></b>
    <li>False</li>
</td>
<td>
<div>Enable sending metrics to Splunk</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>server</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>IP or FQDN for sending data to Splunk</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>hec_port</b>
<div style="font-size: small">
<span style="color: purple">integer</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Listening port for Splunk HEC </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>hec_protocol</b>
<div style="font-size: small">
<span style="color: purple">string</span>      
/ <span style="color: red">required</span>             
</div>
</td>
</div>
<td><li>HTTP</li>
    <li>HTTPS</li>
</td>
<td>
<div>Protocol used for Splunk HEC; HTTP / HTTPS </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>hec_token</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>HEC Token used for sending data to Splunk</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>index_type</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td><li>event</li>
    <li>metric</li>
</td>
<td>
<div>Type of index configured for receiving Avi metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>index</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>Index name setup for receiving Avi metrics</div>
</td>
</tr>

<tr>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="2">
<b>wavefront</b>
<div style="font-size: small">
<span style="color: purple">dictionary</span>                 
</div>
</td>
</div>
</td>
<td>
</td>
<td>
<div>Parameters for sending metrics to Wavefront</div>
</td>
</tr>


<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>enable</b>
<div style="font-size: small">
<span style="color: purple">boolean</span>  
/ <span style="color: red">required</span>                   
</div>
</td>

</div>
<td><li>True</li></b>
   <li>False</li>
</td>
<td>
<div>Enable sending metrics to Wavefront</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>instance</b>
<div style="font-size: small">
<span style="color: purple">string</span>  
/ <span style="color: red">required</span>                   
</div>
</td>
</div>
<td>
</td>
<td>
<div>IP or FQDN for Wavefront</div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>api_key</b>
<div style="font-size: small">
<span style="color: purple">string</span>                  
</div>
</td>
</div>
<td>
</td>
<td>
<div>If using Wavefront direct ingestion, API used for authentication </div>
</td>
</tr>

<tr>
<td>
</td>
<td>
</td>
<td class="elbow-placeholder"></td>
<td colspan="1">
<b>proxy_port</b>
<div style="font-size: small">
<span style="color: purple">integer</span>                
</div>
</td>
</div>
<td><strong>Default:</strong> 2878
</td>
<td>
<div>If using Wavefront proxy, listening port for proxy</div>
</td>
</tr>


</tbody>
</table>



# metrics_endpoint_config examples

## appdynamics_http

Define the AppDynamics Standalone Machine Agent IP address and the port that the HTTP listener is listening on

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
     appdynamics_http:
         enable: True
         server: 169.254.0.1
         server_port: 8293     
```



## datadog

Define the URL and API key used to send metrics into Datadog

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
         datadog:
             enable: True
             api_url: app.datadoghq.com/api/v1/series?api_key=
             api_key: abcdefghijgklmnopqrstuvwxyz12345  
```



## elasticsearch

Define the values for sending metrics to Elasticsearch via the document API.

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
     elasticsearch:
         enable: True
         server: 169.254.0.1
         server_port: 9200
         protocol : https
         index: avimetrics
         timestamp: "@timestamp"
         auth-enabled: True
         username: admin
         password: password
```



## graphite

Define the grahite server host name/ip and the tcp port carbon cache is listening on

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
         graphite:
             enable: True
             server: 169.254.0.1
             server_port: 2003
```



## influxdb

Define the values for sending metrics to InfluxDB via HTTP API.  The script will send values using the InfluxDB's Line Protocol format.

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
     influxdb:
         enable: True
         server: 169.254.0.1
         server_port: 8086
         protocol: https
         db: avi
         metric_prefix: ""
         auth-enabled: False
         username: admin
         password: password
```



## logstash

Define the values for sending metrics to a Logstash endpoint.  The script will send values in a format that is expecting the configured logstash codec to be json_lines.

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
     logstash:
         enable: True
         server: 169.254.0.1
         server_port: 517
         protocol: udp
    

```



## splunk

Define the values for sending values to Splunk HTTP Endpoint Collector.

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
        splunk:
            enable: True
            server: 169.254.0.1
            hec_protocol: https
            hec_port: 8088
            hec_token: abcdefgh-ijkl-mnop-qrst-uvwxyz123456
            index_type: event
            index: avi:metrics
```



## wavefront

Define the values for sending values to Wavefront, either direct ingestion or through a wavefront proxy.

EXAMPLE:

```sh
controllers:
   - avi_cluster_name: demo_controller
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     metrics_endpoint_config:
     wavefront:
         enable: True
         instance: xxxxxxxx.wavefront.com
         #_comment:  If using direct ingestion specify an api_key, if no key then wavefront proxy will be used
         api_key: 12a345bc-de6f-789a-0bcd-ef1234a5bcd6
         #_comment:  If using proxy specify the listening port, if not defined defaults to 2878
         #proxy_port: 2878
```





# Example configuration.yaml

```sh
controllers:
   - avi_cluster_name: demo_controller1
     avi_controller: 169.254.0.1
     avi_user: admin
     avi_pass: password
     virtualservice_stats_config:
         virtualservice_metrics: True
         virtualservice_realtime: True
         virtualservice_runtime: True
     serviceengine_stats_config:
         serviceengine_metrics: True
         serviceengine_runtime: False
         serviceengine_realtime: True
     pool_stats_config:
         pool_metrics: True
         pool_runtime: True
         pool_realtime: True
     controller_stats_config:
         controller_metrics: True
         controller_runtime: True
         controller_metrics_list:
            - controller_stats.avg_cpu_usage
            - controller_stats.avg_disk_usage
            - controller_stats.avg_mem_usage
     metrics_endpoint_config:
         influxdb:
             enable: True
             server: 169.254.0.10
             server_port: 8086
             protocol: http
             db: avi
             metric_prefix: ""
             auth-enabled: False
             username: admin
             password: password
         wavefront:
             enable: True
             instance: xxxxxxxx.wavefront.com
             api_key: 12a345bc-de6f-789a-0bcd-ef1234a5bcd6

   - avi_cluster_name: demo_controller2
     avi_controller: 169.254.0.2
     avi_user: admin
     avi_pass: password
     virtualservice_stats_config:
         virtualservice_metrics: True
         virtualservice_realtime: False
         virtualservice_runtime: False
     controller_stats_config:
         controller_metrics: True
         controller_runtime: True
     metrics_endpoint_config:
         wavefront:
             enable: True
             instance: xxxxxxxx.wavefront.com
             proxy_port: 2878
```





# Default Metrics Being Collected


### Controller Cluster Metrics

- Cluster node roles
- Network pool Usage
- License Usage %
- Days until license(s) expire
- Current Avi Vantage version
- Cluster Node Performance Metrics
    - controller_stats.avg_cpu_usage
    - controller_stats.avg_disk_usage
    - controller_stats.avg_mem_usage


### Controller Cluster Runtime Metrics

- Controller Cluster states
- Subnet IP pool usage
- License usage
- License expiration
- Controller software version



### Service Engine Metrics:

- Statistics for each Service Engine
    - se_if.avg_bandwidth
    - se_stats.avg_connection_mem_usage
    - se_stats.avg_connections
    - se_stats.avg_connections_dropped
    - se_stats.avg_cpu_usage
    - se_stats.avg_disk1_usage
    - se_stats.avg_mem_usage
    - se_stats.avg_dynamic_mem_usage
    - se_stats.avg_persistent_table_usage
    - se_stats.avg_rx_bandwidth
    - se_if.avg_rx_bytes
    - se_if.avg_rx_pkts
    - se_if.avg_rx_pkts_dropped_non_vs
    - se_if.avg_tx_pkts
    - se_if.avg_tx_bytes
    - se_stats.avg_ssl_session_cache_usage
    - se_if.avg_connection_table_usage
    - se_stats.max_se_bandwidth
    - se_stats.avg_eth0_bandwidth
    - se_stats.pct_syn_cache_usage
    - se_stats.avg_packet_buffer_usage
    - se_stats.avg_packet_buffer_header_usage
    - se_stats.avg_packet_buffer_large_usage
    - se_stats.avg_packet_buffer_small_usage
    - healthscore.health_score_value



### Service Engine Runtime Metrics:

- Virtual Server count per Service Engine
- Service Engine count
- Service Engine / Controller missed heartbeats
- Service Engine connected state
- Service Engine Virtual Service hosted used capacity
- Service Engine software version





### Virtual Service Stats

- Statistics for each Virtual Service; total and broken down per Service Engine
    - l4_client.apdexc
    - l4_client.avg_bandwidth
    - l4_client.avg_application_dos_attacks
    - l4_client.avg_complete_conns
    - l4_client.avg_connections_dropped
    - l4_client.avg_new_established_conns
    - l4_client.avg_policy_drops
    - l4_client.avg_rx_pkts
    - l4_client.avg_tx_pkts
    - l4_client.avg_rx_bytes
    - l4_client.avg_tx_bytes
    - l4_client.max_open_conns
    - l4_client.avg_lossy_connections
    - l7_client.avg_complete_responses
    - l7_client.avg_client_data_transfer_time
    - l7_client.avg_client_txn_latency
    - l7_client.sum_application_response_time
    - l7_client.avg_resp_4xx_avi_errors
    - l7_client.avg_resp_5xx_avi_errors
    - l7_client.avg_resp_2xx
    - l7_client.avg_resp_4xx
    - l7_client.avg_resp_5xx
    - l4_client.avg_total_rtt
    - l7_client.avg_page_load_time
    - l7_client.apdexr
    - l7_client.avg_ssl_handshakes_new
    - l7_client.avg_ssl_connections
    - l7_client.sum_get_reqs
    - l7_client.sum_post_reqs
    - l7_client.sum_other_reqs
    - l7_client.avg_frustrated_responses
    - l7_client.avg_waf_attacks
    - l7_client.pct_waf_attacks
    - l7_client.sum_total_responses
    - l7_client.avg_waf_rejected
    - l7_client.avg_waf_evaluated
    - l7_client.avg_waf_matched
    - l7_client.avg_waf_disabled
    - l7_client.pct_waf_disabled
    - l7_client.avg_http_headers_count
    - l7_client.avg_http_headers_bytes
    - l7_client.pct_get_reqs
    - l7_client.pct_post_reqs
    - l7_client.avg_http_params_count
    - l7_client.avg_uri_length
    - l7_client.avg_post_bytes
    - dns_client.avg_complete_queries
    - dns_client.avg_domain_lookup_failures
    - dns_client.avg_tcp_queries
    - dns_client.avg_udp_queries
    - dns_client.avg_udp_passthrough_resp_time
    - dns_client.avg_unsupported_queries
    - dns_client.pct_errored_queries
    - dns_client.avg_domain_lookup_failures
    - dns_client.avg_avi_errors
    - dns_server.avg_complete_queries
    - dns_server.avg_errored_queries
    - dns_server.avg_tcp_queries
    - dns_server.avg_udp_queries
    - l4_server.avg_rx_pkts
    - l4_server.avg_tx_pkts
    - l4_server.avg_rx_bytes
    - l4_server.avg_tx_bytes
    - l4_server.avg_bandwidth
    - l7_server.avg_complete_responses
    - l4_server.avg_new_established_conns
    - l4_server.avg_pool_open_conns
    - l4_server.avg_pool_complete_conns
    - l4_server.avg_open_conns
    - l4_server.max_open_conns
    - l4_server.avg_errored_connections
    - l4_server.apdexc
    - l4_server.avg_total_rtt
    - l7_server.avg_resp_latency
    - l7_server.apdexr
    - l7_server.avg_application_response_time
    - l7_server.pct_response_errors
    - l7_server.avg_frustrated_responses
    - l7_server.avg_total_requests
    - healthscore.health_score_value


### Virtual Service Runtime Stats

- Virtual Service operational status (Up/Down)
- Service Engines Virtual Service hosted on
- Service Engine designated Primary that Virtual Service is hosted on










### Pool Server Metrics

- Statistics for each invidual Pool Server
    - l4_server.avg_rx_pkts
    - l4_server.avg_tx_pkts
    - l4_server.avg_rx_bytes
    - l4_server.avg_tx_bytes
    - l4_server.avg_bandwidth
    - l7_server.avg_complete_responses
    - l4_server.avg_new_established_conns
    - l4_server.avg_pool_open_conns
    - l4_server.avg_pool_complete_conns
    - l4_server.avg_open_conns
    - l4_server.max_open_conns
    - l4_server.avg_errored_connections
    - l4_server.apdexc
    - l4_server.avg_total_rtt
    - l7_server.avg_resp_latency
    - l7_server.apdexr
    - l7_server.avg_application_response_time
    - l7_server.pct_response_errors
    - l7_server.avg_frustrated_responses
    - l7_server.avg_total_requests
    - healthscore.health_score_value


### Pool Server Runtime Metrics
- Pool member(s) operational status (Up/Down)