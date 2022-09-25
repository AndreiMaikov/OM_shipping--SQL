# Online Marketplace Shipping Schema Project
The schema mentioned in the title was created as part of a major project aimed at building from scratch some online marketplace (I’ll be referring to this particular online marketplace as just **OM**). 

The OM system architecture includes a 3NF database (MySQL). Not only does this DB hold all the information about OM's customers and their orders as well as sellers and their products, but it is also meant to facilitate OM shipping operations. 

The following diagram features the DB’s conceptual data model:

![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Full_condensed.png)

(click on the image to view it full size).

Shipping is planned and fulfilled by OM’s employees using vehicles owned or leased by the company. It is required that the shipping process is organized in what the stakeholders called **waves** 

![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Shipping.svg)
