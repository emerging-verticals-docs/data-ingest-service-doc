# data-ingest-service

The Data Ingest Service exposes a REST endpoint to allow users to push data to the cloud.  Data is made available via a RabbitMQ exchange. Users can create actors for the data by binding queues to exchanges.

============================================

## Getting Started

## Prerequisities

* data-ingest-core-sdk - https://github.build.ge.com/emerging-verticals/data-ingest-core-sdk

* event-audit-trail-sdk - https://github.build.ge.com/emerging-verticals/event-audit-trail-sdk

* emerging-verticals-commons - https://github.build.ge.com/emerging-verticals/commons

## Installing

The only installation step requirement to run the service is to configure the database.  To setup the database, please run the following file **/src/main/resources/db/migration/V1__init.sql**.  If you are using the default build/deploy steps, you can skip this step because the [Flyway](https://flywaydb.org/) configuration will take care of this for you.

## Cloud Deployment

A manifest has been provided

## APIs

### Ingest Data (HTTPS)
* POST tenants/{tenant_uuid}/ingest
    * Content - multipart/form-data
    * URI path parameters
        * tenant_uuid - uuid of tenant
    * Request Body FORM parameters
         * file - file to be ingested
         * routing-key - subscriber queue

* Sample Response Payload
```
  "payload": {
    "uuid": "ac22ae56-2fa2-47cb-9044-62992c0bba1c",
    "status": "QUEUED",
    "filename": "testfile.txt"
  },
  "uuid": "969e00ad-3ea8-4468-81b3-08b0239c887c",
  "status": 1000,
  "message": "OK",
  "timestamp": 1459470908881
```

### Ingest Data (WSS)
* messages/ingest
    * Content - application/x-www-form-urlencoded
    * headers
        * tenant-uuid - uuid of tenant
        * routing-key - subscriber queue

* Sample Response Payload
```
  "payload": {
    "uuid": "ac22ae56-2fa2-47cb-9044-62992c0bba1c",
    "status": "QUEUED",
    "filename": "testfile.txt"
  },
  "uuid": "969e00ad-3ea8-4468-81b3-08b0239c887c",
  "status": 1000,
  "message": "OK",
  "timestamp": 1459470908881
```

### Get message status
* GET tenants/{tenant_uuid}/status?uuid={uuid}
    * parameters
        * tenant_uuid - uuid of tenant
        * uuid - uuid of the data ingest message(REQUIRED)

* Sample Response Payload
```
  "payload": [
    {
      "uuid": "cfd05523-cb3c-435b-92eb-9cbe2211ae23",
      "ingestDataMessageStatus": "COMPLETE_OK",
      "timestamp": 1475794948458,
      "lastUpdated": 1475794948458,
      "metaData": "{\"uuid\":\"4e6cf345-23ac-4efd-b219-d51adcd3321a\",\"messageUuid\":\"cfd05523-cb3c-435b-92eb-9cbe2211ae23\",\"hash\":null,\"timestamp\":1475794948296,\"status\":1,\"size\":null,\"elapsed\":null,\"source\":\"t2016am\",\"notes\":\"\",\"route\":\"AA\"}"
    }
  ],
  "uuid": "ddc7f50a-6883-45aa-a823-b70e47d2a05c",
  "status": 1000,
  "message": "OK",
  "timestamp": 1459470908881
```

### Update message status
* Put tenants/{tenant_uuid}/status?uuid={uuid}
    * parameters
        * tenant_uuid - uuid of tenant
        * uuid - uuid of the data ingest message(REQUIRED)

* Sample Request
```
{
"timestamp":4407031691763232,
"status":"COMPLETE_OK",
"uuid":"18c119d6-e7d5-4b55-8b85-5727a64e7440",
"tenantUuid":"eaa0dd0a-06b5-4297-b8b2-3af0c68cd808",
"attributes":{"source":"testfile","route":"AA","tenant":"eaa0dd0a-06b5-4297-b8b2-3af0c68cd808"}
} 
```

* Sample Response Payload
```
  "payload": [
    {
      "uuid": "cfd05523-cb3c-435b-92eb-9cbe2211ae23",
      "ingestDataMessageStatus": "COMPLETE_OK",
      "timestamp": 1475794948458,
      "lastUpdated": 1475794948458,
      "metaData": "{\"uuid\":\"4e6cf345-23ac-4efd-b219-d51adcd3321a\",\"messageUuid\":\"cfd05523-cb3c-435b-92eb-9cbe2211ae23\",\"hash\":null,\"timestamp\":1475794948296,\"status\":1,\"size\":null,\"elapsed\":null,\"source\":\"t2016am\",\"notes\":\"\",\"route\":\"AA\"}"
    }
  ],
  "uuid": "ddc7f50a-6883-45aa-a823-b70e47d2a05c",
  "status": 1000,
  "message": "OK",
  "timestamp": 1459470908881
```


### Get Tenant
* GET tenant/{tenant_uuid}
    * parameters
        * tenant_uuid - uuid of tenant to retrieve

* Sample Response Payload
```
   "payload": {
    "id": 15,
    "tenantUuid": "50e825f9-322c-4082-9899-2d00cfd02a5d",
    "serviceInstanceId": "00a70108-f954-4e9e-ad35-4e647b6fb99e",
    "planId": "1aaf5bef-3364-4933-bc94-1142da225b73",
    "timestamp": 1475694785380,
    "trustedIssuers": "https://77f5cc0e-76e3-4c82-ac95-cc8c8b01267b.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token",
    "connectionInformation": "{\"tenantUuid\":\"50e825f9-322c-4082-9899-2d00cfd02a5d\",\"routeInformation\":null,\"catalogueServiceInstanceInformation\":{\"rabbitmq\":{\"serviceInstanceGuid\":\"a7fec8c2-e9e3-49c5-a66f-461e7cafa0bd\",\"serviceBindingGuid\":\"f07854bf-1d04-4f38-bc81-087103b891c1\",\"metadata\":\"{\"username\":\"f07854bf-1d04-4f38-bc81-087103b891c1\",\"password\":\"be24bdd1-e3b5-453b-9b7f-ee3e947e1086\",\"uri\":\"amqp://f07854bf-1d04-4f38-bc81-087103b891c1:be24bdd1-e3b5-453b-9b7f-ee3e947e1086@100.172.62.51:5672/a7fec8c2-e9e3-49c5-a66f-461e7cafa0bd\",\"host\":\"10.72.6.151\",\"port\":5672}\"},\"eats\":{\"serviceInstanceGuid\":\"ba2a9fff-af37-4f48-a18b-ca428117b2ca\",\"serviceBindingGuid\":\"1d4c8ba4-a06f-40f4-85c9-ccf0db793afa\",\"metadata\":\"{\"entity\":{\"service_instance_guid\":\"ba2a9fff-af37-4f48-a18b-ca428117b2ca\",\"service_instance_url\":\"/v2/service_instances/ba2a9fff-af37-4f48-a18b-ca428117b2ca\",\"binding_options\":{},\"volume_mounts\":[],\"app_url\":\"/v2/apps/2b5dab66-c8b1-44d0-b191-9f87b37c7850\",\"gateway_name\":\"\",\"app_guid\":\"2b5dab66-c8b1-44d0-b191-9f87b37c7850\",\"syslog_drain_url\":null,\"gateway_data\":null,\"credentials\":{\"version\":\"1\",\"trusted-issuer-ids\":\"https://e50d9886-e63c-4d9a-8ea2-e4d106cd5107.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token\",\"catalog-uri\":\"https://event-audit-trail.run.aws-usw02-pr.ice.predix.io\",\"zone-oauth-scope\":\"event-audit-trail.zone.8a8e3d49-61cd-4045-ba82-44b707f95383.user\",\"tenant-uuid\":\"8a8e3d49-61cd-4045-ba82-44b707f95383\"}},\"metadata\":{\"guid\":\"1d4c8ba4-a06f-40f4-85c9-ccf0db793afa\",\"updated_at\":null,\"created_at\":\"2016-10-05T19:13:04Z\",\"url\":\"/v2/service_bindings/1d4c8ba4-a06f-40f4-85c9-ccf0db793afa\"}}\"}}}",
    "eventTenantUuid": "595ddf82-0ee1-4f30-ac23-e9854370a4bc"
  },
   "uuid": "cf6321bc-4be8-48bd-8058-71602fc78f63",
   "status": 1000,
   "message": "OK",
   "timestamp": 1466015044971
```

## Response Codes
These are the service-specific responses embedded in service responses

* **1000** - OK
* **1001** - Tenant not found or undefined
* **1002** - Routing key not found or undefined
* **1003** - File paramter is undefined
* **1004** - No message found