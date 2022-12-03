<!-- Output copied to clipboard! -->

<!-----

You have some errors, warnings, or alerts. If you are using reckless mode, turn it off to see inline alerts.
* ERRORs: 0
* WARNINGs: 0
* ALERTS: 1

Conversion time: 1.091 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β33
* Sat Dec 03 2022 08:50:25 GMT-0800 (PST)
* Source doc: Software development proposal
* Tables are currently converted to HTML tables.
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 1.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>



# The Arline Database


    Practical 2 \
 \
Data Models in Database Systems 


### PROFESSOR \
Mara Pudane


### GROUP


    Matheus de Souza | 211ADB031


    Afra Beria Baki | \
Said Naghyiev | 

2022


# QUERIES


### Creating tables and its types


#### Address Type


    To manage addresses of Staffs and Passengers easier to store


```
CREATE TYPE ADRESS_T 
AS OBJECT (
STREET    VARCHAR2(20),
A_NUMBER   NUMBER, 
CITY  VARCHAR2(20),
COUNTRY VARCHAR2(32)
)
```



#### Personal Type

To store personal information with reference of Addresses.


```
CREATE TYPE PERSONAL_INFO_T 
AS OBJECT (
phone_number VARCHAR(32),
email VARCHAR(32),
adress REF ADRESS_T
)
```



#### Staff and Passenger tables

With Personal info type we created Staff and Passengers table to store their details


```
CREATE TABLE STAFFS (ID INTEGER NOT NULL, NAME VARCHAR2(32), ROLE VARCHAR2(32), PERSONAL_INFO DB_201ADB100.PERSONAL_INFO_T, PRIMARY KEY (ID));
```



```
CREATE TABLE PASSENGERS (ID INTEGER NOT NULL, NAME VARCHAR2(32), PERSONAL_INFO DB_201ADB100.PERSONAL_INFO_T, PRIMARY KEY (ID));
```



#### Dimension type with varray


```
CREATE TYPE DIMENSION_T AS VARRAY(3) OF float
```



#### Luggages table with the type of DImension to store it’s size


```
CREATE TABLE LUGGAGES (ID INTEGER NOT NULL, WEIGHT FLOAT(100), PASSENGER INTEGER, FLIGHT INTEGER, STATUS VARCHAR2(32), DIMENSION DB_201ADB100.DIMENSION_T, PRIMARY KEY (ID));
```



#### Route table to store all Navigations


```
CREATE TABLE ROUTE (ID INTEGER NOT NULL, AIRPORT VARCHAR2(32) NOT NULL, DESTINATION VARCHAR2(32) NOT NULL, ROUTE_CODE VARCHAR2(16) NOT NULL, SERVICE_FEE FLOAT(126) NOT NULL, FUEL_COST FLOAT(126) NOT NULL, CONSTRAINT ROUTE_PK PRIMARY KEY (ID), CONSTRAINT SYS_C0051951 UNIQUE (ROUTE_CODE));
```



#### AirCraft


```
CREATE TABLE AIRCRAFTS (ID INTEGER NOT NULL, PLANE_NUMBER VARCHAR2(50) NOT NULL, CAPACITY INTEGER NOT NULL, CONSTRAINT TABLE1_PK PRIMARY KEY (ID));
```



#### Flight_Schedules


```
CREATE TABLE FLIGHT_SCHEDULES (ID INTEGER NOT NULL, FLIGHT_DATE DATE, DEPARTURE DATE, ARRIVAL DATE, AIRCRAFT INTEGER, ROUTE INTEGER, CONSTRAINT FLIGHT_SCHUDULES_PK PRIMARY KEY (ID));
```


Creating Foreign key to address Aircraft and routes


```
ALTER TABLE "FLIGHT_SCHEDULES" ADD CONSTRAINT AIRCRAFT_FK FOREIGN KEY ("AIRCRAFT") REFERENCES "AIRCRAFTS" ("ID");
ALTER TABLE "FLIGHT_SCHEDULES" ADD CONSTRAINT ROUTE_FK FOREIGN KEY ("ROUTE") REFERENCES "ROUTE" ("ID");
```



#### Rest of tables


<table>
  <tr>
  </tr>
</table>



```
CREATE TYPE MAINTANACE_T
AS OBJECT (
PROBLEM_NAME VARCHAR(32),
PROBLEM_SOURCE VARCHAR(32)
);

CREATE TABLE MAINTANACE (ID INTEGER NOT NULL, START_DATE DATE, END_DATE DATE, AIRCRAFT INTEGER, DETAILS DB_201ADB100.MAINTANACE_T, PRIMARY KEY (ID));

CREATE TABLE TRANSACTIONS (ID INTEGER NOT NULL, BOOKINGDATE DATE, PAYMENT FLOAT(126), PASSENGER INTEGER, FLIGHT INTEGER, PRIMARY KEY (ID));

CREATE TABLE TRANSACTIONS (ID INTEGER NOT NULL, BOOKINGDATE DATE, PAYMENT FLOAT(126), PASSENGER INTEGER, FLIGHT INTEGER, PRIMARY KEY (ID));

CREATE TABLE FLIGHT_SCHEDULES (ID INTEGER NOT NULL, FLIGHT_DATE DATE, DEPARTURE DATE, ARRIVAL DATE, AIRCRAFT INTEGER, ROUTE INTEGER, CONSTRAINT FLIGHT_SCHUDULES_PK PRIMARY KEY (ID));
```


References


```
ALTER TABLE "LUGGAGES" ADD CONSTRAINT LUGGAGES_FLIGHT_FK FOREIGN KEY ("FLIGHT") REFERENCES "FLIGHT_SCHEDULES" ("ID");
ALTER TABLE "LUGGAGES" ADD CONSTRAINT LUGGAGES_PASSENGER_FK FOREIGN KEY ("PASSENGER") REFERENCES "PASSENGERS" ("ID");
ALTER TABLE "LUGGAGES" ADD CHECK (STATUS IN ('DONE', 'LOST', 'IN_FLIGHT', 'FOUND'));
ALTER TABLE "MAINTANACE" ADD CONSTRAINT MAINTANACE_AIRCRAFT_FK FOREIGN KEY ("AIRCRAFT") REFERENCES "AIRCRAFTS" ("ID");
ALTER TABLE "TRANSACTIONS" ADD CONSTRAINT FLIGHT_FK FOREIGN KEY ("FLIGHT") REFERENCES "FLIGHT_SCHEDULES" ("ID");
ALTER TABLE "TRANSACTIONS" ADD CONSTRAINT PASSENGER_FK FOREIGN KEY ("PASSENGER") REFERENCES "PASSENGERS" ("ID");
ALTER TABLE "WORK" ADD CONSTRAINT EMPLOYEE_FK1 FOREIGN KEY ("EMPLOYEE") REFERENCES "STAFFS" ("ID");
ALTER TABLE "WORK" ADD CONSTRAINT FLIGHT_FK2 FOREIGN KEY ("FLIGHT") REFERENCES "FLIGHT_SCHEDULES" ("ID");
```



### Creating data into tables

Inserting aircrafts data


```
INSERT INTO AIRCRAFTS (ID, PLANE_NUMBER, CAPACITY) VALUES (0, 'Airbus 320', 180);
```


Inserting routes data


```
INSERT INTO ROUTE (ID, AIRPORT, DESTINATION, ROUTE_CODE, SERVICE_FEE, FUEL_COST) VALUES (0, 'RIX', 'AFG', '1119', 620, 10000);
```


Inserting Staff information as procedure, to use types hierarchy and make data types easier to manage


```
DECLARE
    adr_ref REF ADRESS_T;
BEGIN
        SELECT REF(A) INTO adr_ref
        FROM ADRESSES_TABLE A
        WHERE A.street = 'Ikbal';
       
        INSERT INTO "STAFFS" VALUES (2, 'John','Janitor',PERSONAL_INFO_T('5641332', 'abcdg@gmail.com', adr_ref));       
END;
```


Inserting Passenger information, same logic from Staff


```
DECLARE
    adr_ref REF ADRESS_T;
BEGIN
        SELECT REF(A) INTO adr_ref
        FROM ADRESSES_TABLE A
        WHERE A.street = 'BRAZIL';
 
        INSERT INTO "Passengers" VALUES (8, 'Muhammed ibn Saddar', PERSONAL_INFO_T('21324134', 'sadsff@gmail.com', adr_ref));
 
END;
```


Inserting baggages value with dimension type containing height, width and length


```
INSERT INTO  "LUGGAGES" ("ID", "WEIGHT", "PASSENGER", "FLIGHT", "STATUS", "DIMENSION") VALUES (2, 20, 3, 3, 'IN_FLIGHT', Dimension_t(30,45,15));
```


Inserting maintenance with maintenance type containing reason and source


```
INSERT INTO MAINTANACE (ID, START_DATE, END_DATE, AIRCRAFT, DETAILS) VALUES (1, TIMESTAMP '2021-10-08 05:40:00', TIMESTAMP '2021-10-08 06:40:00', 1, MAINTANACE_T('Motor crash', 'Motor'));
```



### TRIGGER

If you make some changes on flight schedules they will be stored in onupdateschedule table to show which flights delayed or modified or canceled

For example if you go to database and delete flight it will be stored as canceled

Or if you modify date it will add to the table as delayed and calculate delyay


```
CREATE TRIGGER ONUPDATESCHEDULE
  AFTER INSERT OR UPDATE OR DELETE
  on "DB_201ADB100"."FLIGHT_SCHEDULES"
  FOR EACH ROW
  DECLARE
   flight   DELAYED_FLIGHTS;
begin
        IF DELETING THEN
                flight := DELAYED_FLIGHTS('Canceled', :OLD.FLIGHT_DATE, null, :OLD.ID);
        END IF;

        IF UPDATING  THEN 
                flight := DELAYED_FLIGHTS('Delayed', :OLD.FLIGHT_DATE, :NEW.FLIGHT_DATE, :OLD.ID);
        END IF;
        IF flight IS NOT NULL THEN
                INSERT INTO "DB_201ADB100".DELAYED_FLIGHTS_TABLE
                VALUES flight;
        END IF;
end;
```





### Map and Order methods

Route type later we added map function to calculate price of service


```
CREATE OR REPLACE TYPE "DB_201ADB100"."ROUTE_TYPE" 
AS OBJECT (
fuel_price float,
service_fee float,
destination VARCHAR(32),
AIRPORT VARCHAR(32),
MAP MEMBER FUNCTION cost_of_flight RETURN FLOAT
)

CREATE OR REPLACE TYPE BODY "DB_201ADB100"."ROUTE_TYPE" AS 
  MAP MEMBER FUNCTION cost_of_flight RETURN FLOAT IS
  BEGIN
     RETURN service_fee * fuel_price;
  END cost_of_flight;
END;

DECLARE
  r ROUTE_TYPE;
 
BEGIN
  r :=NEW ROUTE_TYPE(10,5, null,null
  DBMS_OUTPUT.PUT_LINE('COST:' || r.cost_of_flight());
END;
```



### Retrieving data \
Getting Staff information according to the flight schedule. DEREF usage.


```
SELECT S.NAME, F.FLIGHT_DATE, DEREF(S.PERSONAL_INFO.ADRESS) FROM STAFFS S, "WORK" W, FLIGHT_SCHEDULES F
WHERE W.EMPLOYEE = S.ID AND W.FLIGHT = F.ID;
```


Result:



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


Getting Passengers information for flights. DEREF usage. \



```
SELECT P.NAME, F.FLIGHT_DATE, DEREF(P.PERSONAL_INFO.ADRESS) FROM PASSENGERS P, "TRANSACTIONS" T, FLIGHT_SCHEDULES F
WHERE T.PASSENGER = P.ID AND T.FLIGHT = F.ID;
```


Result:

Getting information of luggages for passengers on a flight. DEREF usage. \



```
SELECT P.NAME, L.STATUS, DEREF(P.PERSONAL_INFO.ADRESS) FROM PASSENGERS P, LUGGAGES L
WHERE L.PASSENGER = P.ID
```


Result: \
 \
 \
Getting values from the address table. VALUE usage. \



```
SELECT VALUE(A) FROM ADRESSES_TABLE A;
```


` \
`Getting dimension values of luggages. TABLE usage.


```
SELECT D.* FROM LUGGAGES L, TABLE(L.DIMENSION) D WHERE L.ID = 1;
```



#### MaterializedView


```
CREATE MATERIALIZED VIEW FLIGHT_DETAILS
BUILD IMMEDIATE
REFRESH FORCE
ON COMMIT
ENABLE QUERY REWRITE
AS
SELECT f.id, f.flight_date, f.departure, f.arrival, a.plane_number, r.airport, r.destination, (r.fuel_cost + r.service_fee) as "cost"
from flight_schedules f, aircrafts a, route r
where f.aircraft = a.id and
f.route = r.id;



   DECLARE
	adr_ref REF ADRESS_T;
BEGIN
        SELECT REF(A) INTO adr_ref
        FROM ADRESSES_TABLE A
        WHERE A.country = 'Italy';
        
        INSERT INTO "PASSENGERS" VALUES (1, 'Math Somthing',PERSONAL_INFO_T('1231323', 'somthisadng@gmail.com', adr_ref));
       
END;
```



# Group report \
Link to the sql code \
….
