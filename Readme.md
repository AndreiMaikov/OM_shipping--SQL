# Online Marketplace Shipping Schema Project
The schema mentioned in the title was created as part of a major project aimed at building from scratch some online marketplace (I’ll be referring to this particular online marketplace as just **OM**). 

The OM system architecture includes a 3NF database (MySQL). Not only does this DB hold all the information about OM's customers and their orders as well as sellers and their products, but it is also meant to facilitate OM shipping operations. 
The following diagram features the DB’s conceptual data model:

![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Full_condensed.png)

(click on the image to view it full size).

It is possible that someone involved in OM business processes can have several different roles. For example, a person can act in one transaction as a buyer and in other &mdash; as a seller, or the same employee can do order picking and deliver it as a driver. This results in many-to-many relationships between **users** (those involved in the OM business processes) and **the roles** available in such processes (that are Customer, Vendor, Administrator, Picker and Driver). To resolve these many-to-many relationships in compliance with the third normal form requirements, a three-table structure users&ndash;roles&ndash;user_roles is employed. Each row of the latter table corresponds to a user and one of the user’s roles.

## OM shipping process requirements and their implementation  in the DB

Shipping is planned and fulfilled by OM’s employees using vehicles owned or leased by the company. It is required that the shipping process is organized in what the stakeholders called **waves** 

![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Shipping.svg)
