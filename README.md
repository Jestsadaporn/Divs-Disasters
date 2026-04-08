# Divs-Disasters
# Data dictionary และ ER Diagram เพื่ออธิบายโครงสร้างข้อมูล
### ตาราง Visitor 
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|VId|serial|unique identifier of Visitor|primary key|
|VUserName|varchar(60)|Visitor Username|not null, unique|
|VPassword|varchar(60)|Visitor Password|not null|

##### คำอธิบายเพิ่มเติม
--------------------------

### ตาราง Ride
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|RideID|serial|unique identifier of Ride|primary key|
|RideName|varchar(255)|Name of Ride|not null|
|RideDisct|varchar(n)|Description of Ride||
|RideImagePath|varcahr(100)|Absolut path of Ride Image||

##### คำอธิบายเพิ่มเติม
-------------------------

### ตาราง RideCart
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|RideCartID|serial|unique identifier of RideCart|primary key|
|CartID|integer|CartID from CartToOut table (foreign key)|[ref: > CartToOut.CartID]|
|RideID|integer|RideID from Ride table|[ref: > Ride.RideID]|
|TicketType|enum TicketType {"Children","Adult","Senior","Fastpass"}|TicketType for price for each of Visitor|[default: "Adult"]|
|TicketAmount|int|Amount of each TicketType|[check: `TicketAmount > 0`, default: 1]
|VisitDate|date|date to visit the park|default: Tomorrow|

##### คำอธิบายเพิ่มเติม
-------------------------

### ตาราง CartToOut
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|CartID|serial|unique identifier of CartToOut|primary key|
|VId|integer|VId from Visitor table Visitor(foreign key)|[ref: > Visitor.VId]|
|BookDate|timestamp|time Visitor make Booking|[default: "Current Time"]|
|Status|enum BookStatus {"Booked","NotBookYet","Used/Expire"}|Status about Visitor making Book yet |[default: "NotBookYet"]|
|TotalPrice|decimal(10,2)|Price of all Ticket that booked||

##### คำอธิบายเพิ่มเติม
-------------------------

### ตาราง RideTicketPrice
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|TicketType|enum TicketType {"Children","Adult","Senior","Fastpass"}|Type of the Ticket|not null, composite key|
|TicketPrice|decimal(10,2)|Price of the ticket|default: 100|
|RideID|int|in case each Ride differece price|[ref: - Ride.RideID, composite key]|

##### คำอธิบายเพิ่มเติม
-------------------------

### ตาราง Ticket
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|TicketUID|uid|unique identifier of each Ticket|primary key|
|TicketOwner|int|Owner of this Ticket|ref: - Visitor.VId|
|RideCartID|integer|Id of RideCart ref from RideCartTable|ref: > RideCart.RideCartID|
|IsUse|boolean|status that tell is used yet|default: false|
