---
editable: false
sourcePath: en/_api-ref/mdb/greenplum/api-ref/Cluster/update.md
---

# Method update
Updates the specified Greenplum® cluster.
 

 
## HTTP request {#https-request}
```
PATCH https://mdb.api.cloud.yandex.net/managed-greenplum/v1/clusters/{clusterId}
```
 
## Path parameters {#path_params}
 
Parameter | Description
--- | ---
clusterId | Required. ID of the Greenplum® Cluster resource to update. To get the Greenplum® cluster ID, use a [list](/docs/managed-greenplum/api-ref/Cluster/list) request.  The maximum string length in characters is 50.
 
## Body parameters {#body_params}
 
```json 
{
  "updateMask": "string",
  "description": "string",
  "labels": "object",
  "name": "string",
  "config": {
    "version": "string",
    "backupWindowStart": {
      "hours": "integer",
      "minutes": "integer",
      "seconds": "integer",
      "nanos": "integer"
    },
    "access": {
      "dataLens": true,
      "webSql": true,
      "dataTransfer": true
    },
    "zoneId": "string",
    "subnetId": "string",
    "assignPublicIp": true
  },
  "masterConfig": {
    "resources": {
      "resourcePresetId": "string",
      "diskSize": "string",
      "diskTypeId": "string"
    }
  },
  "segmentConfig": {
    "resources": {
      "resourcePresetId": "string",
      "diskSize": "string",
      "diskTypeId": "string"
    }
  },
  "maintenanceWindow": {

    // `maintenanceWindow` includes only one of the fields `anytime`, `weeklyMaintenanceWindow`
    "anytime": {},
    "weeklyMaintenanceWindow": {
      "day": "string",
      "hour": "string"
    },
    // end of the list of possible fields`maintenanceWindow`

  },
  "securityGroupIds": [
    "string"
  ],
  "deletionProtection": true
}
```

 
Field | Description
--- | ---
updateMask | **string**<br><p>Field mask that specifies which fields of the Greenplum® Cluster resource should be updated.</p> <p>A comma-separated names off ALL fields to be updated. Оnly the specified fields will be changed. The others will be left untouched. If the field is specified in ``updateMask`` and no value for that field was sent in the request, the field's value will be reset to the default. The default value for most fields is null or 0.</p> <p>If ``updateMask`` is not sent in the request, all fields' values will be updated. Fields specified in the request will be updated to provided values. The rest of the fields will be reset to the default.</p> 
description | **string**<br><p>New description of the Greenplum® cluster.</p> <p>The maximum string length in characters is 256.</p> 
labels | **object**<br><p>Custom labels for the Greenplum® cluster as ``key:value`` pairs. Maximum 64 per resource. For example, &quot;project&quot;:&quot;mvp&quot; or &quot;source&quot;:&quot;dictionary&quot;.</p> <p>The new set of labels will completely replace the old ones. To add a label, request the current set with the <a href="/docs/managed-greenplum/api-ref/Cluster/get">get</a> method, then send an <a href="/docs/managed-greenplum/api-ref/Cluster/update">update</a> request with the new label added to the set.</p> <p>No more than 64 per resource. The maximum string length in characters for each key is 63. Each key must match the regular expression ``[a-z][-_0-9a-z]*``. The maximum string length in characters for each value is 63. Each value must match the regular expression ``[-_0-9a-z]*``.</p> 
name | **string**<br><p>New name for the cluster.</p> <p>The maximum string length in characters is 63. Value must match the regular expression ``[a-zA-Z0-9_-]*``.</p> 
config | **object**<br><p>Greenplum® cluster configuration.</p> <p>Greenplum® cluster configuration.</p> 
config.<br>version | **string**<br><p>Version of the Greenplum® server software.</p> 
config.<br>backupWindowStart | **object**<br><p>Time to start the daily backup, in the UTC timezone.</p> <p>Represents a time of day. The date and time zone are either not significant or are specified elsewhere. An API may choose to allow leap seconds. Related types are <a href="https://github.com/googleapis/googleapis/blob/master/google/type/date.proto">google.type.Date</a> and <a href="https://github.com/protocolbuffers/protobuf/blob/master/src/google/protobuf/timestamp.proto">google.protobuf.Timestamp</a>.</p> 
config.<br>backupWindowStart.<br>hours | **integer** (int32)<br><p>Hours of day in 24 hour format. Should be from 0 to 23. An API may choose to allow the value &quot;24:00:00&quot; for scenarios like business closing time.</p> 
config.<br>backupWindowStart.<br>minutes | **integer** (int32)<br><p>Minutes of hour of day. Must be from 0 to 59.</p> 
config.<br>backupWindowStart.<br>seconds | **integer** (int32)<br><p>Seconds of minutes of the time. Must normally be from 0 to 59. An API may allow the value 60 if it allows leap-seconds.</p> 
config.<br>backupWindowStart.<br>nanos | **integer** (int32)<br><p>Fractions of seconds in nanoseconds. Must be from 0 to 999,999,999.</p> 
config.<br>access | **object**<br><p>Access policy for external services.</p> <p>Greenplum® cluster access options.</p> 
config.<br>access.<br>dataLens | **boolean** (boolean)<br><p>Allows data export from the cluster to Yandex DataLens.</p> 
config.<br>access.<br>webSql | **boolean** (boolean)<br><p>Allows SQL queries to the cluster databases from the Yandex Cloud management console.</p> 
config.<br>access.<br>dataTransfer | **boolean** (boolean)<br><p>Allow access for DataTransfer.</p> 
config.<br>zoneId | **string**<br><p>ID of the availability zone the cluster belongs to. To get a list of available zones, use the <a href="/docs/compute/api-ref/Zone/list">list</a> request.</p> <p>The maximum string length in characters is 50.</p> 
config.<br>subnetId | **string**<br><p>ID of the subnet the cluster belongs to. This subnet should be a part of the cloud network the cluster belongs to (see <a href="/docs/managed-greenplum/api-ref/Cluster#representation">Cluster.networkId</a>).</p> <p>The maximum string length in characters is 50.</p> 
config.<br>assignPublicIp | **boolean** (boolean)<br><p>Whether or not the cluster has a public IP address.</p> <p>After the cluster has been created, this setting cannot be changed.</p> 
masterConfig | **object**<br><p>Configuration of the Greenplum® master subcluster.</p> <p>Configuration of the master subcluster.</p> 
masterConfig.<br>resources | **object**<br><p>Resources allocated to Greenplum® master subcluster hosts.</p> <p>A list of computational resources allocated to a host.</p> 
masterConfig.<br>resources.<br>resourcePresetId | **string**<br><p>ID of the preset for computational resources allocated to a host. Available presets are listed in the <a href="/docs/managed-greenplum/concepts/instance-types">documentation</a>.</p> 
masterConfig.<br>resources.<br>diskSize | **string** (int64)<br><p>Volume of the storage used by the host, in bytes.</p> 
masterConfig.<br>resources.<br>diskTypeId | **string**<br><p>Type of the storage used by the host: ``network-hdd``, ``network-ssd`` or ``local-ssd``.</p> 
segmentConfig | **object**<br><p>Configuration of the Greenplum® segment subcluster.</p> <p>Configuration of the segment subcluster.</p> 
segmentConfig.<br>resources | **object**<br><p>Resources allocated to Greenplum® segment subcluster hosts.</p> <p>A list of computational resources allocated to a host.</p> 
segmentConfig.<br>resources.<br>resourcePresetId | **string**<br><p>ID of the preset for computational resources allocated to a host. Available presets are listed in the <a href="/docs/managed-greenplum/concepts/instance-types">documentation</a>.</p> 
segmentConfig.<br>resources.<br>diskSize | **string** (int64)<br><p>Volume of the storage used by the host, in bytes.</p> 
segmentConfig.<br>resources.<br>diskTypeId | **string**<br><p>Type of the storage used by the host: ``network-hdd``, ``network-ssd`` or ``local-ssd``.</p> 
maintenanceWindow | **object**<br><p>Window of maintenance operations.</p> <p>A Greenplum® cluster maintenance window. Should be defined by either one of the two options.</p> 
maintenanceWindow.<br>anytime | **object**<br>An any-time maintenance window. <br>`maintenanceWindow` includes only one of the fields `anytime`, `weeklyMaintenanceWindow`<br><br><p>An any-time maintenance window.</p> 
maintenanceWindow.<br>weeklyMaintenanceWindow | **object**<br>A weekly maintenance window. <br>`maintenanceWindow` includes only one of the fields `anytime`, `weeklyMaintenanceWindow`<br><br><p>A weekly maintenance window.</p> 
maintenanceWindow.<br>weeklyMaintenanceWindow.<br>day | **string**<br><p>Day of the week.</p> 
maintenanceWindow.<br>weeklyMaintenanceWindow.<br>hour | **string** (int64)<br><p>Hour of the day in the UTC timezone.</p> <p>Acceptable values are 1 to 24, inclusive.</p> 
securityGroupIds[] | **string**<br><p>User security groups.</p> 
deletionProtection | **boolean** (boolean)<br><p>Whether or not cluster is protected from being deleted.</p> 
 
## Response {#responses}
**HTTP Code: 200 - OK**

```json 
{
  "id": "string",
  "description": "string",
  "createdAt": "string",
  "createdBy": "string",
  "modifiedAt": "string",
  "done": true,
  "metadata": "object",

  //  includes only one of the fields `error`, `response`
  "error": {
    "code": "integer",
    "message": "string",
    "details": [
      "object"
    ]
  },
  "response": "object",
  // end of the list of possible fields

}
```
An Operation resource. For more information, see [Operation](/docs/api-design-guide/concepts/operation).
 
Field | Description
--- | ---
id | **string**<br><p>ID of the operation.</p> 
description | **string**<br><p>Description of the operation. 0-256 characters long.</p> 
createdAt | **string** (date-time)<br><p>Creation timestamp.</p> <p>String in <a href="https://www.ietf.org/rfc/rfc3339.txt">RFC3339</a> text format.</p> 
createdBy | **string**<br><p>ID of the user or service account who initiated the operation.</p> 
modifiedAt | **string** (date-time)<br><p>The time when the Operation resource was last modified.</p> <p>String in <a href="https://www.ietf.org/rfc/rfc3339.txt">RFC3339</a> text format.</p> 
done | **boolean** (boolean)<br><p>If the value is ``false``, it means the operation is still in progress. If ``true``, the operation is completed, and either ``error`` or ``response`` is available.</p> 
metadata | **object**<br><p>Service-specific metadata associated with the operation. It typically contains the ID of the target resource that the operation is performed on. Any method that returns a long-running operation should document the metadata type, if any.</p> 
error | **object**<br>The error result of the operation in case of failure or cancellation. <br> includes only one of the fields `error`, `response`<br><br><p>The error result of the operation in case of failure or cancellation.</p> 
error.<br>code | **integer** (int32)<br><p>Error code. An enum value of <a href="https://github.com/googleapis/googleapis/blob/master/google/rpc/code.proto">google.rpc.Code</a>.</p> 
error.<br>message | **string**<br><p>An error message.</p> 
error.<br>details[] | **object**<br><p>A list of messages that carry the error details.</p> 
response | **object** <br> includes only one of the fields `error`, `response`<br><br><p>The normal response of the operation in case of success. If the original method returns no data on success, such as Delete, the response is <a href="https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#empty">google.protobuf.Empty</a>. If the original method is the standard Create/Update, the response should be the target resource of the operation. Any method that returns a long-running operation should document the response type, if any.</p> 