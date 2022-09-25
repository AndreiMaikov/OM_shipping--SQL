# Online Marketplace Shipping Schema Project
The schema mentioned in the title was created as part of a major project aimed at building from scratch some online marketplace (I’ll be referring to this particular online marketplace as just **OM**). 

The OM system architecture includes a 3NF database (MySQL). Not only does this DB hold all the information about OM's customers and their orders as well as sellers and their products, but it is also meant to facilitate OM shipping operations. 
The following diagram features the DB’s conceptual data model:

![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Full_condensed.png)

(click on the image to view it full size).

It is possible that someone involved in OM business processes can have several different roles. For example, a person can act in one transaction as a buyer and in other &mdash; as a seller, or the same employee can do order picking and deliver it as a driver. This results in many-to-many relationships between **users** (those involved in the OM business processes) and **the roles** available in such processes (that are Customer, Vendor, Administrator, Picker and Driver). To resolve these many-to-many relationships in compliance with the third normal form requirements, a three-table structure users&ndash;roles&ndash;user_roles is utilized. Each row of the latter table corresponds to a user and one of the user’s roles.

## OM shipping process organization principles and their implementation  in the DB

All OM’s shipping operations are both planned and carried out by the company’s employees; the vehicles used can be owned or leased by the company.

It is required that the shipping process is organized in what the stakeholders calls **waves**. A wave is an established by OM time interval and a set of shipping assignments that are to be started and finished within this in interval. Several waves might be created within one day.


![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Shipping.svg)

![\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}](https://latex.codecogs.com/svg.latex?x%3D%5Cfrac%7B-b%5Cpm%5Csqrt%7Bb%5E2-4ac%7D%7D%7B2a%7D)
