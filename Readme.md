# Online Marketplace Shipping Schema Project
The schema mentioned in the title was created as part of a major project aimed at building from scratch some online marketplace (I’ll be referring to this particular online marketplace as just **OM**). 

The OM system architecture includes a 3NF database (MySQL). Not only does this DB hold all the information about OM's customers and their orders as well as sellers and their products, but it is also meant to facilitate OM shipping operations. 
The following diagram shows the DB’s final conceptual data model:

![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Full_condensed.png)

(click on the image to view it full size).

Tables in the diagram are marked accordingly to the type of information they hold – whether it is related to customers, products, etc. The Common group includes a three-table structure users–roles–user_roles, which is worth elaborating on here.

It is possible that someone involved in OM business processes can have several different roles. For example, a person can act in one transaction as a buyer and in other &mdash; as a seller, or the same employee can pick an order and deliver it as a driver. 

This results in many-to-many relationships between **users** (those involved in the OM business processes) and **the roles** available in such processes (that are Customer, Vendor, Administrator, Picker and Driver). To resolve these many-to-many relationships in compliance with the third normal form requirements, the structure users&ndash;roles&ndash;user_roles is utilized. Each row of the latter table corresponds to a user and one of the user’s roles.

## OM shipping process features and their implementation in the schema

The DB was designed to suit the following characteristics of the OM shipping process:

- All OM’s shipping operations are both planned and carried out by the company’s employees; the vehicles used for shipping are owned or leased by the company.
- Order pickers and drivers (and, if there are any, employees who do both jobs) must be provided with shipping assignments specifying the orders they need to assemble/deliver, the related locations and  completion times, as well instructions regarding packing orders and passing them from pickers to drivers
- It is required that the shipping process is organized in what so-called **waves**. A wave consists of a time interval established by the OM management and a set of shipping assignments that are to be started and finished during this interval. Several waves can be planned for one day.


![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Shipping.svg)

![\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}](https://latex.codecogs.com/svg.latex?x%3D%5Cfrac%7B-b%5Cpm%5Csqrt%7Bb%5E2-4ac%7D%7D%7B2a%7D)

This sentence uses `$` delimiters to show math inline: $\sqrt{3x-1}+(1+x)^2$

$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$
