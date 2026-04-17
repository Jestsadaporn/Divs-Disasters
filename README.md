# Divs-Disasters
# Data dictionary และ ER Diagram เพื่ออธิบายโครงสร้างข้อมูล

### ตาราง Visitor 
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|VId|serial|unique identifier of Visitor|primary key|
|VUserName|varchar(60)|Visitor Username|not null, unique|
|VPassword|varchar(60)|Visitor Password|not null|

#### คำอธิบายเพิ่มเติม : 
> มีหน้าที่ :เก็บข้อมูลผู้ใช้งาน ระบุตัวตนผู้ใช้ <br>
> ความสัมพันธ์กับตารางอื่น : CartToOut ระบุชื่อของคนซื้อตั๋ว
 gun pru

--------------------------

### ตาราง Ride
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|RideID|serial|unique identifier of Ride|primary key|
|RideName|varchar(255)|Name of Ride|not null|
|RideDisct|varchar(2000)|Description of Ride||
|Rideshortdisct|varchar(80)|brief description of ride||
|RideImagePath|varcahr(100)|Absolut path of Ride Image||

##### คำอธิบายเพิ่มเติม
> มีหน้าที่ : เก็บข้อมูลเครื่องเล่น รูปภาพ คำอธิบายเครื่องเล่น <br>
> ความสัมพันธ์ : คิดราคาของเครื่องเล่นในตาราง RideTicketPrice กับเก็บข้อมูลตั๋วที่จะเพิ่มในตาราง RideCart

-------------------------

### ตาราง RideCart
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|RideCartID|serial|unique identifier of RideCart|primary key|
|CartID|integer|CartID from CartToOut table (foreign key)|[ref: > CartToOut.CartID]|
|RideID|integer|RideID from Ride table|[ref: > Ride.RideID]|
|seniorticketamount|integer | quantity of senior ticket| default(0) |
|adultticketamount |integer | quantity of adult ticket| default(0)|
|childrenticketamount|integer | quantity of children ticket| default(0) |
|VisitDate|date|date to visit the park|default: curren date|
|Isfastpass|boolean| is User use Fastpass | default : false|

##### คำอธิบายเพิ่มเติม
> มีหน้าที่ : ตะกร้าเก็บตั๋วได้ชนิดเดียว เครื่องเล่นเดียว ต่อ 1 Rows และบอกว่าผู้ใช้กดใช้งาน Fastpass หรือไม่ <br>
> ความสัมพันธ์ :  Ticket เอากำหนด UID ของตั๋วแต่ละอัน กับ CartToout โยนข้อมูลเข้าไปใน CartToOut
-------------------------

### ตาราง CartToOut
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|CartID|serial|unique identifier of CartToOut|primary key|
|VId|integer|VId from Visitor table Visitor(foreign key)|[ref: > Visitor.VId]|
|BookDate|timestamp|time Visitor make Booking|[default: "null"]|
|isBookedStatus|boolean |Status of Book|[default: false]|
|TotalPrice|decimal(10,2)|Price of all Ticket that booked|default 0|

##### คำอธิบายเพิ่มเติม
> มีหน้าที่ : เป็นใบเสร็จ และเป็นตะกร้าของของ RideCart ที่ยังไม่ Booked <br>
> ความสัมพันธ์ : แสดงความเป็นเจ้าของมาจาก Visitor และ RideCart 

-------------------------

### ตาราง RideTicketPrice
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|TicketType|enum TicketType {"Children","Adult","Senior","FastpassPlus"}|Type of the Ticket|primary key|
|TicketPrice|decimal(10,2)|Price of the ticket|default: 100|

##### คำอธิบายเพิ่มเติม
> มีหน้าที่ : เก็บข้อมูล ราคาของเครื่องเล่น ที่เอาไว้สำหรับคำนวณราคา <br>
> ความสัมพันธ์ : กับ Ride บอกราคา 

-------------------------

### ตาราง Ticket
|Attribute|Datatype|Description|Constraints|
|---|---|---|---|
|TicketUID|uuid|unique identifier of each Ticket|primary key default gen_random_uuid()|
|RideCartID|integer|Id of RideCart ref from RideCartTable|ref: > RideCart.RideCartID|
|IsUse|boolean|status that tell is used yet|default: false|
|tickettype|tikettype| type of this ticket||

##### คำอธิบายเพิ่มเติม
> มีหน้าที่ : เก็บ UID ของตั๋วแต่ละอัน และบอกว่าตั๋วถูกใช้งานรึยัง <br>
> ความสัมพันธ์ : กับ RideCart สร้าง UID ให้กับตั๋วที่ถูกจองไป

