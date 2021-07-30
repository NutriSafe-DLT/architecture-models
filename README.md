# The NutriSafe Architecture

### Business Architecture

<p align="center">
     <img src="./pictures/stakeholder_maps.png"
     width="75%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
     <br>
      Fig. X: Stakeholder map for the cheese supply chain (left)
     <br>
     Fig. Y: Supply chain access to the blockchain (right)
</p>

<i>
<b>Source:</b> Lamken, D., Wagner, T., Hoiss, T., Seidenfad., K., Hermann, A., Kus, M., & Lechner, U. (n.d.). Design patterns and framework for blockchain integration in supply chains. 2021 IEEE International Conference on Blockchain and Cryptocurrency (ICBC) 
</i><br>

<p align="justify">
The community uses a stakeholder map shown in Fig. X and the NutriSafe infrastructure as the central element of the design. The community comprises many actors typical for any food supply chain and actors specific for the scenario of soft cheese production, namely dairy and milk truck. <br>
The supply chain depicted in Fig. Y starts with the milk farm. The milk farm hands over the fresh milk to a milk truck, which transports milk to the dairy and takes a sample for quality checks. The dairy processes fresh milk to produce, e.g., soft cheese transported by a logistics service provider to a retailer. The end customer buys the product, the soft cheese, at a grocery store. We assume that all actors use the blockchain to share information concerning production and logistics. A core user story is the creation of traceable product history. The product history is, e.g., providing information to the end-customer or for efficient tracking and tracing in a food safety issue.
</p>

### Architecture Layers

<p align="center">
<img src="./pictures/actors_IS_DLT.png"
     width="75%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
     <br>
    Fig. X: Conceptual architecture of a DLT-based supply chain 
</p>

<i>
<b>Source:</b> Hrestic, R., Hermann, A., Hofmeier, M., Hoiß, T., Seidenfad, K., & Lechner, U. (2020). Supply Chains Meet Fintech: Is Tokenization an Option? 3rd International FinTech, InsurTech & Blockchain Forum.
</i><br>

<p align="justify">
<br>
The conceptual architecture of a blockchain as infrastructure is depicted in Fig. X Actors have their information systems: ERP Systems, customer relationship management systems (CRM), herd management systems (HMS), and systems used by logistics service providers to manage transportation.
<br>
Note that the depicted supply chain is quite linear. In reality, there are many farmers, logistics service providers, production facilities, wholesalers, and retailers that participate in a supply chain. An analysis of the scenarios resulted in requirements and design decisions.
</p>

<p align="center">
<img src="./pictures/architecture_interface_layers.png"
     width="75%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
     <br>
     Fig. Y: The layered architecture to bridge between applications and blockchain network

</p>

<i><b>Source:</b> Seidenfad, K., Hoiss, T., & Lechner, U. (2021). A blockchain to bridge business information systems and industrial automation environments in supply chains. In G. Krieger, U.R., Eichler, G., Erfurth, C., Fahrnberger (Ed.), 21st International Conference on Innovations for Community Services. Springer.
</i><br>
<p align="justify">
The technical architecture is structured in four layers, as depicted in Fig. Y. 
<br>
<b>The UI Layer</b> contains the WebApp dashboard, an IOS application designed to enable mobile access and information provision to the blockchain state. The web application and the IOS app use JSON over HTTP to send REST-Calls to the NutriSafe REST API.
<br>
<b>The API Layer</b> builds a layer of abstraction to the underlying system. The REST API and the EDI API provide the connectivity of user faced applications and the blockchain infrastructure. In small and medium-sized enterprises, EDI is a common standard for communication with business partners. REST is the de-facto standard for web-interfaces. We propose an EDI API which enables accessibility by enterprise applications.
<br>
<b>The Persistence Layer</b> with the blockchain ledger and a shared user database. The shared user database enables the authenticity of users or systems for all APIs. The web application provides user management with an interface for adding, deleting, and changing user details and rights to invoke chaincode. 
<br>
<b>The Administration Layer</b> contains the operational support and necessities for configuring and maintaining a Hyperledger Fabric network. The designed scripts for creating update transactions enable a fast way to expand the network. Configuration files are inherent by the blockchain framework and are customized for our scenario.
</p>

### APIs

#### REST API

<p align="center">
<img src="./pictures/rest_api_component_model.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
  
  <br>
    Fig. Y: The component model of the REST API
</p>

<p align="justify">
The REST API provides the interface for all web applications to the blockchain. Note that the first design iteration of NutriSafe utilizes a REST API for each organization.
<br>
The RESTful interface (cf. Fig. X) provides a set of functions to enable the interaction with the NutriSafe Hyperledger Fabric network. To authenticate the transaction proposals to the blockchain network, the user's organization's certificate and the corresponding key have to be accessible for the REST API. Custom clients transfer username and password to receive a JSON Web Token (JWT) per session on the REST API. Since the REST API hosts its own user database, user management is also part of its feature set. Customizable whitelists define the function calls per user and are adjustable to chaincode updates.
</p>

#### MQTT API

<p align="justify">
A setup with mechatronic components and an MES is depicted in Fig. Z. The MES is utilizing the REST API. The mechatronic components such as PLC and sensors are accessing our MQTT API, which consists of an MQTT broker and an MQTT-to-BC-Gateway. To increase interoperability, we propose all MQTT devices to be compliant with the Sparkplug B specification. <br>
Since MQTT is offering a large flexibility for topic naming and payload encoding, this flexibility becomes a limitation when different organizations want to access dis- tributed data of a less known OT-environment. Sparkplug is a standardization project by the Eclipse Foundation and a recent approach to reach more interoperability in the IoT, by making MQTT data more reasonable. It contains specifications for semantic topic naming, session states and payload management.
</p>
<p align="center">
<img src="./pictures/mqtt_api.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
</p>
<i><b>Source:</b> Seidenfad, K., Hoiss, T., & Lechner, U. (2021). A blockchain to bridge business information systems and industrial automation environments in supply chains. In G. Krieger, U.R., Eichler, G., Erfurth, C., Fahrnberger (Ed.), 21st International Conference on Innovations for Community Services. Springer.
</i><br>

<p align="justify">
The concept shown in Fig. Z utilizes two generic structures (cf. Fig. 6 and Fig. 13) as follows: The MES is aware of the machines' running jobs and knows the Sparkplug compliant namespace of each involved device. Each time a new job is finished, the MES adds the namespace of the active machine to the product-lot as a new attribute. Since the new lot is timestamped and contains a unique namespace, we can now query the ledger of the production machine precisely by the tuple of topic-domain and timestamp. Each device has its own ledger, and each ledger can be concatenated from top to down. I.e., a machine has its own ledger, which is referring to the ledgers of its subcomponents.
Our solution benefits from the combination of the generic structures of Sparkplug and the NutriSafe Meta Model. The flexible data model of NutriSafe (cf. Fig. 6) enables to address the full bandwidth of products in the food industry. Annotating predecessor and successor to each product-lot empowers, e.g., authorities to make a forward and backward tracing on the supply chain data. Within specific tracing cases, it is necessary to query data about the OT-environment which was involved in the production process. Here Sparkplug comes into play. The semantic granularity ranges from production lines, over single machines and down to components such as actuators or sensors. Furthermore, industrial environments are shaped by patchworks of modern and also historically grown legacy infrastructures. Here our current data model faces limitations because it does not offer a means to integrate these infrastructural data.
</p>
     

### The Meta Definition
<p align="justify">
The meta definition represents a data set of diverse product specifications. It enables to extend a product model in a running blockchain, whereas the REST API offers an interface and identity handling for client applications that intend to connect to the Hyperledger Fabric network. 
The meta definition allows to manage a large and diverse number of different product representations in the blockchain. The meta definition is saved as one key-value pair on the ledger with the key predefined as METADEF. The meta definition contains a unitList, describing possible unit metrics for products, an attributeToDataTypeMap, which maps datatypes to attributes and a productNameToAttributesMap, which connects a list of attributes to a product type.  
</p>

<p align="center">
<img src="./pictures/meta_definition.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
  
  <br>
    Fig. Y: The meta definition
</p>

<i>
<b>Source:</b> Lamken, D., Wagner, T., Hoiss, T., Seidenfad., K., Hermann, A., Kus, M., & Lechner, U. (n.d.). Design patterns and framework for blockchain integration in supply chains. 2021 IEEE International Conference on Blockchain and Cryptocurrency (ICBC) 
</i><br>


### Channel Topology
<p align="center">
<img src="./pictures/channel_topology.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
</p>

<i><b>Source:</b> Seidenfad, K., Hoiss, T., & Lechner, U. (2021). A blockchain to bridge business information systems and industrial automation environments in supply chains. In G. Krieger, U.R., Eichler, G., Erfurth, C., Fahrnberger (Ed.), 21st International Conference on Innovations for Community Services. Springer.<i><br>


### Blockchain Operations Framework

#### Blockchain Operations Categories
<p align="center">
<img src="./pictures/blockchain_operations_categories.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
</p>

<i><b>Source:</b> Hoiss, T., Seidenfad, K. & Lechner, U. (2021). Blockchain Service Operations – A Structured Approach to Operate a Blockchain Solution. To be appear in IEEE DAPPS 2021
</i><br>

<img src="./pictures/onboarding_a_new_org.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 

<i><b>Source:</b> Hoiss, T., Seidenfad, K. & Lechner, U. (2021). Blockchain Service Operations – A Structured Approach to Operate a Blockchain Solution. To be appear in IEEE DAPPS 2021
</i><br>

### Script Environment

The Hyperledger Fabric framework provides a set of scripts for basic network management operations by default. A toolchain for the generation, configuration, and administration of fabric-based networks is realized using the Hyperledger Fabric framework's client software. However, most of the administrative operations require long sequences of commands executed in a strict order. Scripting those operations is fundamental for efficient network deployment and operation. Therefor, we elaborated a compact sheet for documenting these scripts and all its environmental dependencies and implications.

#### The Documentation Model

<img src="./pictures/empty_script.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />
<p align="justify">

<i><b>Source:</b> Seidenfad, K., Hoiss, T., & Lechner, U. (2021). A blockchain to bridge business information systems and industrial automation environments in supply chains. In G. Krieger, U.R., Eichler, G., Erfurth, C., Fahrnberger (Ed.), 21st International Conference on Innovations for Community Services. Springer.<i> <br>

The documentation model consists of the following fields. <br>
<b>Name:</b> The name of the script file.
<br>
<b>Goal:</b> The desired goal by starting the script.
<br>
<b>Skript Parameter:</b> The parameters inside the script and the default value.
<br>
<b>Environmental Prerequisites:</b> Prerequisites, such as locally running processes or the presence of specific network entities.
<br>
<b>Required Files:</b> The presence of required files, such as certs, in the filesystem.
<br>
<b>Output File and/or Result State:</b> The result of the script operation, can be a output file oder the change of a state for a specific network entity.
</p>

#### create_crypto_peer_organisation.sh

<img src="./pictures/create_crypto_peer_organisation.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### initialize_ordering_service.sh

<img src="./pictures/initialize_ordering_service.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### start_cli.sh

<img src="./pictures/start_cli.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### create_consortium.sh

<img src="./pictures/create_consortium.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### org_join_consortium.sh

<img src="./pictures/org_join_consortium.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### create_peer.sh

<img src="./pictures/create_peer.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### create_channel.sh

<img src="./pictures/create_channel.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### peer_channel_join.sh

<img src="./pictures/peer_channel_join.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### anchor_peer_update.sh

<img src="./pictures/anchor_peer_update.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### org_join_channel.sh

<img src="./pictures/org_join_channel.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />

#### start_prometheus.sh

<img src="./pictures/start_prometheus.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />