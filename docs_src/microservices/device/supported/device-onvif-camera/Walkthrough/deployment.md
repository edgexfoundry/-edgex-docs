# Deployment
Follow this guide to deploy and run the service.

## Deploy EdgeX and ONVIF Device Camera Microservice

=== "Docker"

      1. Navigate to the EdgeX `compose-builder` directory:
   
         ```bash
         cd edgex-compose/compose-builder/
         ```
      2. Run Edgex with the ONVIF microservice in secure or non-secure mode.

        ##### Non-secure mode

        ```bash
        make run no-secty ds-onvif-camera
        ```
    
        ##### Secure mode 

         <div class="admonition note">
             <p class="admonition-title">Note</p>
             <p>Recommended for secure and production level deployments. Make a note of the Consul ACL token and the JWT token generated
                which are needed to map credentials and execute apis.
             </p>
          </div>

         ```bash
         make run ds-onvif-camera
         make get-consul-acl-token
         make get-token
         ```

        <div class="admonition note">
             <p class="admonition-title">Note</p>
             <p>Secrets such as passwords, certificates, tokens and more in Edgex are stored in a secret store which is implemented using Vault by Hashicorp.
                Vault supports security features by issuing of consul tokens. JWT token is required for the API Gateway which is a trust boundry for Edgex services. 
                It allows for external clients to be verified when issuing REST requests to the microservices. 
                For more info refer [Secure Consul](../../../../../security/Ch-Secure-Consul.md), [API Gateway](../../../../../security/Ch-APIGateway.md) 
                and [Edgex Security](../../../../../security/Ch-Security.md).
            </p>
          </div>

=== "Native"


      <div class="admonition note">
         <p class="admonition-title">Note</p>
         <p>Go version 1.20+ is required to run natively. See <a href="https://go.dev/doc/install">here</a> for more information.</p>
      </div>


      1. Navigate to the EdgeX `compose-builder` directory:

         ```bash
         cd edgex-compose/compose-builder/
         ```

      2. Run EdgeX:

         ```bash
         make run no-secty
         ```

      3. Navigate out of the `edgex-compose` directory to the `device-onvif-camera` directory:

         ```bash
         cd device-onvif-camera
         ```

      4. Run the service
         ```bash
         make run
         ```
         
         <details>
         <summary>[Optional] Run with NATS</summary>
            ```bash
            make run-nats
            ```
         </details>

## Verify Service and Device Profiles

=== "via Command Line"
      1. Check the status of the container:

         ```bash 
         docker ps
         ```

         The status column will indicate if the container is running, and how long it has been up.

         Example Output:

         ```docker
         CONTAINER ID   IMAGE                                                       COMMAND                  CREATED       STATUS          PORTS                                                                                         NAMES
         33f9c5ecb70e   nexus3.edgexfoundry.org:10004/device-onvif-camera:latest    "/device-onvif-camer…"   7 weeks ago   Up 48 minutes   127.0.0.1:59985->59985/tcp                                                                    edgex-device-onvif-camera
         ```

      2. Check whether the device service is added to EdgeX:

         <div class="admonition note">
             <p class="admonition-title">Note</p>
             <p>If running in secure mode all the api executions need the JWT token generated previously. E.g.
                ```bash
                curl --location --request GET 'http://localhost:59881/api/v3/deviceservice/name/device-onvif-camera' \
                --header 'Authorization: Bearer eyJhbGciOiJFUzM4NCIsImtpZCI6ImIzNTY3ZmJjLTlhZTctMjkyNy0xY2IxLWE2NzAzZGQwMWM1ZCJ9.eyJhdWQiOiJlZGdleCIsImV4cCI6MTY4MjcyNDExMCwiaWF0IjoxNjgyNzIwNTEwLCJpc3MiOiIvdjEvaWRlbnRpdHkvb2lkYyIsIm5hbWUiOiJlZGdleHVzZXIiLCJuYW1lc3BhY2UiOiJyb290Iiwic3ViIjoiMTA2NzczMDItMmY0Yi00MjE4LTFhZmUtNzZlOTYwMGJiMmQ5In0.NP0deI0HyQMvdsFwk85N5RwNpgh5lUa507z9Ft2CDT9OEeR8iYOLYmwRLZim3j_BoVSdWxiJf3tmnWo64-mffHoktbFSRooQveakAeoFYuvCXu7tO1-b-QGzzzyWfSjc' \
                --data-raw ''
                ```
            </p>
          </div>

         ```bash
         curl -s http://localhost:59881/api/v3/deviceservice/name/device-onvif-camera | jq .
         ```
         Good response:
         ```json
            {
               "apiVersion": "v3",
               "statusCode": 200,
               "service": {
                  "created": 1657227634593,
                  "modified": 1657291447649,
                  "id": "e1883aa7-f440-447f-ad4d-effa2aeb0ade",
                  "name": "device-onvif-camera",
                  "baseAddress": "http://edgex-device-onvif-camera:59984",
                  "adminState": "UNLOCKED"
               }         
            }
         ```
         Bad response:
         ```json
         {
            "apiVersion": "v3",
            "message": "fail to query device service by name device-onvif-camer",
            "statusCode": 404
         }
         ```


      3. Check whether the device profile is added:

         ```bash
         curl -s http://localhost:59881/api/v3/deviceprofile/name/onvif-camera | jq -r '"profileName: " + '.profile.name' + "\nstatusCode: " + (.statusCode|tostring)'

         ```
         Good response:
         ```bash
         profileName: onvif-camera
         statusCode: 200
         ```
         Bad response:
         ```bash
         profileName: 
         statusCode: 404
         ```

         <div class="admonition note">
            <p class="admonition-title">Note</p>
            <p>`jq -r` is used to reduce the size of the displayed response. The entire device profile with all resources can be seen by removing `-r '"profileName: " + '.profile.name' + "\nstatusCode: " + (.statusCode|tostring)', and replacing it with '.'`</p>
         </div>
      
            

=== "via EdgeX UI"

      <div class="admonition note">
      <p class="admonition-title">Note</p>
      <p>Secure mode login to Edgex UI requires the JWT token generated in the above step</p>
      </div>

      <details>
      <summary><strong>Entering the JWT token</strong></summary>
         ![](../images/EdgeXJWTLogin.png)
      </details>
   

      1. Visit http://localhost:4000 to go to the dashboard for EdgeX Console GUI:

         ![EdgeXConsoleDashboard](../images/EdgeXDashboard.png)
         <p align="left">
            <i>Figure 1: EdgeX Console Dashboard</i>
         </p>

      2. To see **Device Services**, **Devices**, or **Device Profiles**, click on their respective tab:

         ![EdgeXConsoleDeviceServices](../images/EdgeXDeviceServices.png)
         <p align="left">
            <i>Figure 2: EdgeX Console Device Service List</i>
         </p>

         ![EdgeXConsoleDeviceList](../images/EdgeXDeviceList.png)
         <p align="left">
            <i>Figure 3: EdgeX Console Device List</i>
         </p>

         ![EdgeXConsoleDeviceProfileList](../images/EdgeXDeviceProfiles.png)
         <p align="left">
            <i>Figure 4: EdgeX Console Device Profile List</i>
         </p>
## Manage Devices
Follow these instructions to update devices.


### Curl Commands

#### Add Device

!!! Warning
    Be careful when storing any potentially important information in cleartext on files in your computer. This includes information such as your camera IP and MAC addresses.

1. Edit the information to appropriately match the camera. The fields `Address`, `MACAddress` and `Port` should match that of the camera:

    <div class="admonition note">
             <p class="admonition-title">Note</p>
             <p>If running in secure mode all the api executions need the JWT token generated previously.
            </p>
    </div>

      ```bash
      curl -X POST -H 'Content-Type: application/json'  \
      http://localhost:59881/api/v3/device \
      -d '[
               {
                  "apiVersion": "v3",
                  "device": {
                     "name":"Camera001",
                     "serviceName": "device-onvif-camera",
                     "profileName": "onvif-camera",
                     "description": "My test camera",
                     "adminState": "UNLOCKED",
                     "operatingState": "UP",
                     "protocols": {
                        "Onvif": {
                           "Address": "10.0.0.0",
                           "Port": "10000",
                           "MACAddress": "aa:bb:cc:11:22:33",
                           "FriendlyName":"Default Camera"
                        },
                        "CustomMetadata": {
                           "Location":"Front door"
                        }
                     }
                  }
               }
      ]'
      ```

      Example Output: 
      ```bash
      [{"apiVersion":"v3","statusCode":201,"id":"fb5fb7f2-768b-4298-a916-d4779523c6b5"}]
      ```

2. Map credentials using the `map-credentials.sh` script.  

    <div class="admonition note">
             <p class="admonition-title">Note</p>
             <p>If running in secure mode Consul ACL and the JWT token generated previously are needed for mapping credentials.
            </p>
    </div>

      a. Run `bin/map-credentials.sh`

      b. Select `(Create New)`
            ![](../images/creds-pick.png)  
      c. Enter the Secret Name to associate with these credentials  
            ![](../images/creds-name.png)   
      d. Enter the username  
            ![](../images/creds-username.png)  
      e.  Enter the password  
            ![](../images/creds-password.png)  
      f. Choose the Authentication Mode  
            ![](../images/creds-method.png)  
      g. Assign one or more MAC Addresses to the credential group  
            ![](../images/creds-mac.png)  
      h. Learn more about updating credentials [here](../supplementary-info/utility-scripts.md)  

      Successful:

      ```bash 
      Dependencies Check: Success
            Consul Check: ...
                        curl -X GET http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera?keys=true
      Response [200]      Success
      curl -X GET http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/AppCustom/CredentialsMap?keys=true
      Response [200] 
      Secret Name: a
      curl -X GET http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/AppCustom/CredentialsMap/a?raw=true
      Response [404] 
      Failed! curl returned a status code of '404'
      Setting InsecureSecret: a/SecretName
      curl --data "<redacted>" -X PUT http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/Writable/InsecureSecrets/a/SecretName
      Response [200] true


      Setting InsecureSecret: a/SecretData/username
      curl --data "<redacted>" -X PUT http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/Writable/InsecureSecrets/a/SecretData/username
      Response [200] true


      Setting InsecureSecret: a/SecretData/password
      curl --data "<redacted>" -X PUT http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/Writable/InsecureSecrets/a/SecretData/password
      Response [200] true


      Setting InsecureSecret: a/SecretData/mode
      curl --data "usern<redacted>metoken" -X PUT http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/Writable/InsecureSecrets/a/SecretData/mode
      Response [200] true


      Setting Credentials Map: a = ''
      curl -X PUT http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/AppCustom/CredentialsMap/a
      Response [200] true



      Secret Name: a
      curl -X GET http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/AppCustom/CredentialsMap/a?raw=true
      Response [200] 
      Setting Credentials Map: a = '11:22:33:44:55:66'
      curl --data "11:22:33:44:55:66" -X PUT http://localhost:8500/v1/kv/edgex/v3/device-onvif-camera/AppCustom/CredentialsMap/a
      Response [200] true
      ``` 

3. Verify device(s) have been succesfully added to core-metadata.

      ```bash
      curl -s http://localhost:59881/api/v3/device/all | jq -r '"deviceName: " + '.devices[].name''
      ```

      Example Output: 
      ```bash
      deviceName: Camera001
      deviceName: device-onvif-camera
     ```

      <div class="admonition note">
         <p class="admonition-title">Note</p>
         <p>`jq -r` is used to reduce the size of the displayed response. The entire device with all information can be seen by removing `-r '"deviceName: " + '.devices[].name'', and replacing it with '.'`</p>
      </div>

#### Update Device

   There are multiple commands that can update aspects of the camera entry in meta-data. Refer to the [Swagger documentation](./general-usage.md) for Core Metadata for more information. For editing specific fields, see the [General Usage](./general-usage.md) tab.

#### Delete Device

   ```bash
   curl -X 'DELETE' \
   'http://localhost:59881/api/v3/device/name/<device name>' \
   -H 'accept: application/json' 
   ```

## Shutting Down
To stop all EdgeX services (containers), execute the `make down` command. This will stop all services but not the images and volumes, which still exist.

1. Navigate to the `edgex-compose/compose-builder` directory.
1. Run this command
   ```bash
   make down
   ```
1. To shut down and delete all volumes, run this command
   ```bash
   make clean
   ```

## Next Steps

[Learn how to use the device service>](./general-usage.md){: .md-button}


# License

[Apache-2.0](https://github.com/edgexfoundry-holding/device-onvif-camera/blob/main/LICENSE)
