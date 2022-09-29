
# Online Marketplace Shipping Schema Project


The schema mentioned in the title was created as part of a major project aimed at building from scratch some online marketplace (I’ll be referring to this particular online marketplace as just **OM**). 

## On the entire OM database

The OM system architecture includes a 3NF database (MySQL). Not only does this DB store all the information about OM's customers and their orders as well as sellers and their products, but it is used for planning shipping operations. 
The following diagram shows the DB’s conceptual data model:

![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM.svg)

Tables in the diagram are marked accordingly to the type of information they hold – whether it is related to customers, products, etc. The Common group includes a three-table structure **users&ndash;user_roles&ndash;roles**, which is worth elaborating on here.

It is possible that someone involved in OM business processes can have several different roles. For example, a person can act in one transaction as a buyer and in other &mdash; as a seller, or the same employee can pick an order and deliver it as a driver. 

This results in many-to-many relationships between **users** (those involved in the OM business processes) and **the roles** available in such processes (that are Customer, Vendor, Administrator, Picker and Driver). To resolve these many-to-many relationships in compliance with the third normal form requirements, the structure users&ndash;user_roles&ndash;roles is utilized. Each row of the latter table corresponds to a user and one of the user’s roles.

The structure of the Shipping table group and the reasons why this structure was chosen are discussed below.

## OM shipping process features and their implementation  in the schema

The Shipping tables were designed to suit the following characteristics of the OM shipping process:

- All OM’s shipping operations are both planned and carried out by the company’s employees; the vehicles used for shipping are owned or leased by the company.
- Order pickers and drivers (and, if there are any, employees who do both jobs) must be provided with shipment assignments specifying the orders they need to assemble/deliver, the related locations and  completion times, etc.
- It is required that the shipping process is organized in so-called **waves**. A wave consists of a time interval established by the OM management and a set of shipping assignments that are to be started and finished during this interval. Several waves can be planned for one day (e.g., 10&nbsp;am&nbsp;&ndash;&nbsp;2&nbsp;pm and 3&nbsp;pm&nbsp;&ndash;&nbsp;7&nbsp;pm).

Wave planning can be done in two stages: 
    **(A**)&nbsp;determining which employees and vehicles are available for the wave, and
    **(B)**&nbsp;assigning vehicles to drivers, dispatching  orders to pickers and drivers, and preparing shipment assignments for them.
    
    

### Stage A. Determining employee and vehicle availability. OM Shipping schema

These tasks define the structure of the Shipping section [of the OM database]. The Shipping tables can be designed within a smaller schema &mdash; **OM Shipping schema**&nbsp;&mdash; which includes only them and the user–user_roles–roles group of the Common section; in other words,
<p align = "center">
   OM Shipping schema =  Shipping tables from the OM schema + users&ndash;user_roles&ndash;roles tables.
</p>

The following diagram shoes the OM Shipping schema along with the Stage A and B data flows in the OM system (for the code, see
<a href="https://github.com/AndreiMaikov/MVM_Shipping--SQL/tree/main/src/OM_Shipping_schema.sql">OM_Shipping_schema.sql</a>).
To integrate the Shipping tables developed this way into the entire OM schema, one only needs to add them to the rest of the tables: no references between the Shipping tables and the tables in Customers, Vendors, Products, and Orders sections are required.



<a name = "diagram OM_Shipping>
![ ](https://github.com/AndreiMaikov/MVM_Shipping--SQL/blob/main/images/OM_Shipping.svg)
	   </a>

The five tables used for determining employee and vehicle availability are:
- wave_timings;
- staff_regular_availability;
- blocked_periods;
- vehicles;
- vehicles_out_of_service.

After the availability is calculated (see below), the results are placed into another two tables:
- wave_available_staff
- wave_available_vehicles

#### The functions of the tables

The **wave_timings** table simply contains beginning and ending times of each wave.

The **staff_regular_availability** table stores each employee’s individual weekly schedule (which can be different for different employees).  The following is supported:
- the availability of the employee can vary from day to day during the week;
- several time intervals of availability within a day are possible for one employee;
- the employee can be temporarily unavailable for a given interval (indicated by the Boolean flag in the type column).

The **blocked_periods** table contains information regarding planned periods when each user will be unavailable, 
	such as vacations, leaves for medical reasons, 
	holidays specific to religious or cultural traditions, etc.
	Each user may have several such blocked periods.
	
The **vehicles** table stores comprehensive information on the vehicles the company uses for shipping. This information is mostly used in Stage B or for administrative purposes; in stage A, the table is only used as a list of the delivery vehicles owned or leased by the company, with their lease terms if applicable.

For each vehicle, the **vehicles_not_in_service** table provides beginning and ending times of the planned periods when the vehicle cannot be used for shipping due to any reason – e.g., being in repairs or employed for another service. 

Within each wave and for employee, the table **wave_available_staff** lists all the time intervals that the employee is available for (during the entire interval). The **wave_available_vehicles** table provides the same information about the vehicles.

#### Constraints

A number of constraints are added to the Shipping tables.

- UNIQUE and CHECK constraints are used to prevent some possible data entry mistakes (such as associating one user id with more than one picker ids, a period’s beginning time being later than its ending time). 

- ON DELETE CASCADE and ON UPDATE CASCADE constraints are included to avoid errors caused by row deletion or updating when foreign keys are involved. 

For details about these constraints, please see the code and comments in
<a href="https://github.com/AndreiMaikov/MVM_Shipping--SQL/tree/main/src/OM_Shipping_schema.sql">OM_Shipping_schema.sql</a>).

#### Calculating availability intervals for a wave

Speaking mathematically, the problem of determining an employee’s availability for a given wave is essentially is a problem of finding the intersection between two sets: the wave’s time interval and the union of all the time intervals when the employee is available (considering the employee’s regular availability and blocked periods). To solve this problem, one have to do some manipulations with inequalities that define time intervals involved. The same is true of determining vehicle availability.

Such calculations can be performed either on the database level using stored procedures or, alternatively, on the application level of the system (see the diagram).

Each of these options has its advantages and drawbacks. Choosing between them needs weighing such factors as the convenience of implementation and maintenance as well as the effect on the performance of the specific system in question.


![\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}](https://latex.codecogs.com/svg.latex?x%3D%5Cfrac%7B-b%5Cpm%5Csqrt%7Bb%5E2-4ac%7D%7D%7B2a%7D)

This sentence uses `$` delimiters to show math inline: $\sqrt{3x-1}+(1+x)^2$

$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$
