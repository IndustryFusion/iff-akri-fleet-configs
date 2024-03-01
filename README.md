## Akri Deployment for MQTT & OPC-UA on single Gateway

These deployments must be made on every gateway cluster under 'devices' namespace using Rancher Fleet CD.

**Folder - akri:**

The akri folder contains fleet configuration to deploy Akri helm with MQTT and OPC-UA discovery handler with default controller disabled. Akri is responsible for discovering MQTT and OPC-UA instances in the node network.

**Folder - iff:**

IFF Akri controller deployment, role binding and Akri Configurations for both MQTT and OPC-UA instance discovery is located in this folder. Using values.yaml file, the Akri Configurations for MQTT and OPC-UA can be configured to detect right machines/devices using IP address of OPC-UA server and MQTT broker URL and single main MQTT topic of the machine.

**Usecase:**

IFF prepares the single node smartbox as a platform by deploying the above mentioned Akri components and ships them to users for connecting them to a predefined machine IP address for OPC-UA and with predefined MQTT Broker URL and a topic.

User connects the smartbox to the LAN to which his machine is also connected and switches it on (Akri configurations already knows this IP addresses and other information). The Akri DH scans the network and chooses the right servers. Once the detection is successful, Akri automatically creates the device instances.

Once the instances are created, the IFF Akri controller searches for gateway configurations in the IFF Factory Manager 5.0 endpoint for new digital twin asset creations. If it finds assets, it creates the data service deployments for the digital twin asset.

**Deployment:**

The helm charts in this project are desgined to be deployed using Rancher's Fleet plugin (GitOps based Continous Delivery plugin). In the Rancher Management Server, navigate to 'Continous Delivery' option from the left navigation bar. Add a new Git Repo, mention name of the repository and branch that contains this project, deploy it to the desired cluster in Rancher.

Note: The charts can also be used to deploy on Kubernetes using Helm CLI with minor modification.

**Troubleshooting/Known Issues**

1. All IP addresses in the subnet must be listed with possible ports for OPC-UA, which is a huge list of combinations. The Local Discovery Server (LDS) proposal from the OPC foundation might help to aggregate all the OPC-UA servers in one place. (Reaserch and Testing - In Progress) [https://profiles.opcfoundation.org/profile/1634]
2. Only one topic of MQTT must be noted in the configuration for one machine. Multiple will cause creation of multiple unwanted Akri instances.
3. Some machines have their OPC UA server port at 4840 (standard), and some at 62548 (custom).
4. The OPC server's application name value is not unique across locations and clients. It might be a localized value. (Akri must be restructured to use applicationUri instead - SUSE has taken note)

