---
uid: TroubleshootTheAdapter
---

# Troubleshooting

<<<<<<< HEAD
PI adapters provide features for troubleshooting issues related to connectivity, data flow, and configuration. Resources include adapter logs and the Wireshark troubleshooting tool . If you are still unable to resolve issues or need additional guidance, contact AVEVA PI Support through the [AVEVA Customer Portal](https://my.osisoft.com/).
=======
AVEVA adapters provide features for troubleshooting issues related to connectivity, data flow, and configuration. Resources include adapter logs and the Wireshark troubleshooting tool . If you are still unable to resolve issues or need additional guidance, contact AVEVA PI Support through the [AVEVA Customer Portal](https://my.osisoft.com/).
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241

**Note:** Make sure to also check the troubleshooting information specific to your adapter in this user guide.

## Logs

Messages from the System and OmfEgress logs provide information on the status of the adapter. For example, they show if a connection from the adapter to an egress endpoint exists.

Perform the following steps to view the System and OmfEgress logs:

1. Navigate to the logs directory: 
    Windows: `%ProgramData%\OSIsoft\Adapters\<AdapterName>\Logs` 
    Linux: `/usr/share/OSIsoft/Adapters/<AdapterName>/Logs`.  
    **Example:**  A successful connection to a PI Web API egress endpoint displays the following message in the OmfEgress log:

    ```json
    2020-11-02 11:08:51.870 -06:00 [Information] Data will be sent to the following OMF endpoint: 
    Id: <omfegress id>
    Endpoint: <pi web api URL>   (note: the pi web api default port is 443)
    ValidateEndpointCertificate: <true or false>
    ```

2. Optional: Change the log level of the adapter to receive more information and context. For more information, see [Logging configuration](xref:LoggingConfiguration).

### ASP .NET Core platform log

The ASP .NET Core platform log provides information from the Kestrel web server that hosts the application. The log could contain information that the adapter is overloaded with incoming data. Perform the following steps to spread the load among multiple adapters:

1. Decrease the scan frequency.
2. Lower the amount of data selection items.

## Wireshark

Wireshark is a protocol-specific troubleshooting tool that supports all current adapter protocols. Perform the following steps if you want to use Wireshark to capture traffic from the data source to the adapter or from the adapter to the OMF destination.

1. Download [Wireshark](https://www.wireshark.org/download.html).
2. Familiarize yourself with the tool and read the [Wireshark user guide](https://www.wireshark.org/docs/wsug_html_chunked/).

## Health and diagnostics egress to PI Web API

The adapter sends health and diagnostics data to PI Web API; in some cases, conflicts may occur that are due to changes or perceived changes in PI Web API. For example, a `409 - Conflict` error message displays if you upgrade your adapter version and the PI points do not match in  the upgraded version. However, data is continued to be sent as long as containers are created, so buffering only starts if no containers are created.

To resolve the conflict, perform the following steps:

1. Stop the adapter.
2. Delete the `Health` folder inside of the `Buffers` folder.
3. Stop PI Web API.
4. Delete the relevant adapter created AF structure.
5. Delete the associated health and diagnostics PI points on any or all PI Data Archives created by PI Web API.
6. Start PI Web API.
7. Start the adapter.

## Adapter connection to egress endpoint

<<<<<<< HEAD
Certain egress health information in both PI Web API and ADH show if an adapter connection to an egress endpoint exists. To verify an active connection, perform one of the following procedures:
=======
Certain egress health information in both PI Web API and AVEVA Data Hub show if an adapter connection to an egress endpoint exists. To verify an active connection, perform one of the following procedures:
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241

### PI Web API connection

Perform the following steps to determine if a connection to the PI Web API endpoint exists:

1. Open PI Web API.
2. Select the OmfEgress component of your adapter, for example *GVAdapterUbuntu.\<AdapterName\>.OmfEgress*.
3. Make sure that the following PI points have been created for your egress endpoint:
    - **DeviceStatus**
    - **NextHealthMessageExpected**
    - **IORate**

<<<<<<< HEAD
### ADH connection

Perform the following steps to determine if a connection to the ADH endpoint exists:

1. Open ADH.
=======
### AVEVA Data Hub connection

Perform the following steps to determine if a connection to the AVEVA Data Hub endpoint exists:

1. Open AVEVA Data Hub.
>>>>>>> 61fa1904844108ba597ef7bf338ec2f9a8d4c241
2. Click **Sequential Data Store** > **Streams**.
3. Makes sure that  the following streams have been created for your egress endpoint:
    - **DeviceStatus**
    - **NextHealthMessageExpected**
    - **IORate**

### TCP connection

Perform the following steps to see all established TCP sessions in Linux:

1. Open a terminal.
2. Type the following command: `ss  -o state established -t -p`
3. Press Enter.

## Egress debug logging

Perform the following steps to enable debugging and to troubleshoot issues between the AVEVA Adapter and the egress destination:

1. Set the appropriate time value for the **DebugExpiration** property in the egress configuration.
   **Note:** To disable debugging, set the **DebugExpiration** property to `null`.
2. Navigate to the debugging folder to review the logs.

**Note:** We recommend enabling the egress debugging feature for a limited time to avoid running out of disk space.

Date and time strings should use the following formats:

UTC: `yyyy-mm-ddThh:mm:ssZ`

Local: `yyyy-mm-ddThh:mm:ss`

If the time is not specified, it will default to the start of the day (e.g., `2025-06-19` will default to `2025-06-19T00:00:00`)

### Debugging folder or file structure

Because the overall number and size of each request or response pair captured by debugging can be quite large, the debugging information is stored in a separate folder. Debugging folders and files are created in the logs folder as follows:

Windows: `%programdata%\OSIsoft\Adapters\{adapterType}\Logs\EgressDebugLogs\{endpointType}\{egressId}\{omfType}\{Ticks}-{Guid}-{Req/Res}.txt`

Linux: `/usr/share/OSIsoft/Adapters/{adapterType}/Logs/EgressDebugLogs/{endpointType}/{egressId}/{omfType}/{Ticks}-{Guid}-{Req/Res}.txt`

The specific elements of the file structure are defined in the following table.

| Element    | Represents                       |
|------------|----------------------------------|
| **adapterType** | The type of the adapter: OpcUa, Modbus, MQTT, and so on. |
| **endpointType** | The type of egress endpoint: Data or Health. |
| **egressId** | The Id of egress endpoint specified in configuration. |
| **omfType**  | The OMF message type: Type, Container, or Data. |
| **Ticks**    | The time in milliseconds (tick count) for UTC DateTime when the determined message was written to disk. |
| **Guid**     | The unique GUID for each request or response pair. |
| **Req/Res**  | Whether the message was HTTP request or response. |
