# The NutriSafe Architecture

### Business Architecture
<figure>
  <img src="./pictures/stakeholder_maps.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;"/>
  <figcaption>
  Fig. X: Stakeholder map for the cheese supply chain (left)

  Fig. Y: Supply chain access to the blockchain (right)
  </figcaption>
</figure>
<p align="justify">
The community uses a stakeholder map shown in Fig. X and the NutriSafe infrastructure as the central element of the design. The community comprises many actors typical for any food supply chain and actors specific for the scenario of soft cheese production, namely dairy and milk truck. <br>
The supply chain depicted in Fig. Y starts with the milk farm. The milk farm hands over the fresh milk to a milk truck, which transports milk to the dairy and takes a sample for quality checks. The dairy processes fresh milk to produce, e.g., soft cheese transported by a logistics service provider to a retailer. The end customer buys the product, the soft cheese, at a grocery store. We assume that all actors use the blockchain to share information concerning production and logistics. A core user story is the creation of traceable product history. The product history is, e.g., providing information to the end-customer or for efficient tracking and tracing in a food safety issue.
</p>

### Architecture Layers

<figure>
  <img src="./pictures/actors_IS_DLT.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
  <figcaption>
  Fig. X: Conceptual architecture of a DLT-based supply chain
  </figcaption>
</figure>

<p align="justify">
The conceptual architecture of a blockchain as infrastructure is depicted in Fig. X Actors have their information systems: ERP Systems, customer relationship management systems (CRM), herd management systems (HMS), and systems used by logistics service providers to manage transportation.
<br>
Note that the depicted supply chain is quite linear. In reality, there are many farmers, logistics service providers, production facilities, wholesalers, and retailers that participate in a supply chain. An analysis of the scenarios resulted in requirements and design decisions.
</p>

<figure align="center">
<img src="./pictures/architecture_interface_layers.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
    <figcaption>
     Fig. Y: The layered architecture to bridge between applications and blockchain network
    </figcaption>
</figure>

Reference to ICBC paper

### APIs

#### REST API
<p align="center">
<img src="./pictures/rest_api_component_model.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
</p>
#### MQTT API
<p align="center">
<img src="./pictures/mqtt_api.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
</p>
Reference to I4CS paper
     

### The Meta Model

### Channel Topology

<img src="./pictures/channel_topology.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 


### Blockchain Operations Framework

#### Blockchain Operations Categories
<p align="center">
<img src="./pictures/blockchain_operations_categories.png"
     width="500"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 
</p>

Reference to DAPPS paper

<img src="./pictures/onboarding_a_new_org.png"
     width="100%"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" /> 

Reference to DAPPS paper

#### Script Environment


