# Enterprise-Architect-SQL-Injection v.16.0.1605(Build: 1605) - 32 bit

**Timeline:**       
**Vulnerability reported to vendor:**  07.11.2022           
**New fixed build 1625:**    02.03.2023                      
**Disclosure:** 04.08.2023           

**Affected Products:**            
Enterprise-Architect v.16.0.1605(Build: 1605) - 32 bit             

**Proof of Concept**       
Additional SQL queries can be injected into _Find_ field within _Select Classifier_ functionality.       
Below are the steps required to recreate the vulnerability:             
<img src="/Location.jpeg">          
Press the _Search_**(1)** button then chose Browse for _Diagram_**(2)**:           
In the newly opened windows pick _Search_**(1)** functionality and in the _Find_**(2)** form paste the following payload:         
```
‘union select null,password,null,null,Userlogin,null,null,null,null from t_secuser;--
```
<img src="/PoC.jpeg">

In the search _results_**(3)**, all users of the application and the password for the admin account were returned.

As the databases structure differs, simpler payload can be used:
```
‘union select null,@@version,null,null,null,null,null,null,null;--
```
Search results will return the version of the database used.

**Additional Info**                       
According to vendor, passwords were stored in plaintext within "t_secuser" table in older versions of EA.
From EA 11 onward passwords are stored in t_xref as hashes using SHA hashing algorithm.

Personally, I think that the occurence of plaintext passwords in version 16.0.1605 must be caused by an upgrade from a older version.
Surely worth checking if you own an instance of Enterprise Architect or if you are testing one of these.
