# Data dictionary และ ER Diagram เพื่ออธิบายโครงสร้างข้อมูล
### ตาราง Visitor 
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|VId|serial|unique identifier of Visitor|primary key|
|VUserName|varchar(60)|Visitor Username|not null|
|VPassword|varchar(60)|Visitor Password|not null|

##### คำอธิบายเพิ่มเติม
--------------------------

### ตาราง Ride
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|RideID|serial|unique identifier of Ride|primary key|
|RideName|varchar(255)|Name of Ride|not null|
|RideDisct|varchar(255)|Description of Ride|not null|

##### คำอธิบายเพิ่มเติม
-------------------------

### ตาราง RideCart
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|RideCartID|serial|unique identifier of RideCart|primary key|
|CartID|integer|CartID from CartToOut table (foreign key)|[ref: > CartToOut.CartID]|
|RideID|integer|RideID from Ride table|[ref: > Ride.RideID]|
|TicketType|enum TicketType {"Children","Adult","Senior","Fastpass"}|TicketType for price for each of Visitor|[default: "Adult"]|
|TicketAmount|int|Amount of each TicketType|[check: `TicketAmount > 0`, default: 1]|

##### คำอธิบายเพิ่มเติม
-------------------------

### ตาราง CartToOut
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|CartID|serial|unique identifier of CartToOut|primary key|
|VId|integer|VId from CartToOut table Visitor(foreign key)|[ref: > Visitor.VId]|
|BookDate|timestamp|time Visitor make Booking|[default: "Current Time"]|
|Status|enum BookStatus {"Booked","NotBookYet","Used/Expire"}|Status about Visitor mankeing Book yet |[default: "NotBookYet"]|
|VisitDate|date|Date visitor will be come|[default: "Tomorrow"|
|TotalPrice|decimal|Price of all|[default: 5]|

##### คำอธิบายเพิ่มเติม
-------------------------

### ตาราง RideTicketPrice
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|ChildrenPrice|decimal(10,2)|Price for Children|[default: 100]|
|AdultPrice|decimal(10,2)|Price for Adult|[default: 100]|
|SeniorPrice|decimal(10,2)|Price for Senior|[default: 100]|
|FastPassPrice|decimal(10,2)|price of Fastpass|[default: 100]|
|RideID|int|in case each Ride differece price|[ref: - Ride.RideID]|

##### คำอธิบายเพิ่มเติม
-------------------------
