SMART PARKING SYSTEM
1. Introduction

This project presents a complete Smart Parking System integrating IoT hardware, edge processing, lightweight cloud communication, and a mobile application. The system uses ultrasonic sensors to detect parking slot occupancy, IR sensors for vehicle entry/exit detection, and servo motors for gate control. An ESP32 microcontroller performs local decision-making and communicates with MongoCity (AppBlocky WebDB) to synchronize real-time data with a Flutter-based mobile application used by customers and administrators.

The solution provides automated gate access, live slot monitoring, slot booking with timing, and administrative dashboards for reviewing occupancy trends and revenue.

2. Objectives
Detect real-time occupancy of parking slots
Allow users to view and book slots via a mobile app
Synchronize hardware and app through a cloud database (MongoCity)
Enable automatic gate control using IR sensors
Provide admin dashboard with metrics and booking details

3. System Components
Hardware:
ESP32 Development Board
Ultrasonic Sensors (3) – Slot detection
IR Sensors (2) – Entry & Exit detection
SG90 Servo Motors (2) – Gate control
16×2 I2C LCD – Live status display
Power supply and jumper wires
Software:
Arduino IDE (ESP32 firmware)
Flutter (Cross-platform mobile app)
MongoCity / AppBlocky WebDB (Cloud synchronization)

4. Hardware Pin Mapping
Component	ESP32 Pin
Ultrasonic Sensor 1 TRIG	GPIO 12
Ultrasonic Sensor 1 ECHO	GPIO 27
Ultrasonic Sensor 2 TRIG	GPIO 13
Ultrasonic Sensor 2 ECHO	GPIO 26
Ultrasonic Sensor 3 TRIG	GPIO 14
Ultrasonic Sensor 3 ECHO	GPIO 25
IR Entry Sensor	GPIO 33
IR Exit Sensor	GPIO 32
Servo Entry Gate	GPIO 15
Servo Exit Gate		GPIO 2
LCD (I2C)	SDA/SCL

5. Cloud Integration (MongoCity/AppBlocky WebDB)
The system uses MongoCity as a simple HTTP-based cloud service.
Cloud Endpoints
Fetch Slot Data:
http://mangocity.appblocky.com/webdb/getvalue.php?tag=chp0096
Upload Slot Data:
http://mangocity.appblocky.com/webdb/storeavalue.php?tag=chp0096&value=S1,S2,S3
Data Format
1 = Available  
0 = Occupied  
2 = Booked  
Example:
1,0,2 → Slot1 available, Slot2 occupied, Slot3 booked

6. Flutter Application
User Features
Login
View live parking slot status
Book slots with:
	Vehicle number
	Vehicle type
	Date & time
Automatic fee calculation (₹10/hr)
Booking confirmation and notifications
Admin Features
Dashboard showing:
	Total slots
	Available slots
	Occupied/Booked slots
	Total revenue
	View booking history
	Refresh and logout options

7. System Workflow
ESP32 reads ultrasonic sensors → detects car presence
IR sensors detect entry or exit → triggers gate opening via servo
ESP32 updates LCD with slot status and free slots
Every 3 seconds, ESP32 syncs status to MongoCity
Flutter app fetches latest status and updates UI
User books a slot → App updates MongoCity → ESP32 reads updated status
Admin monitors usage, revenue, and history through dashboard

8. Results
Ultrasonic sensors accurately detected vehicle presence within 10 cm threshold
Gate automation worked smoothly with IR detection
MongoCity successfully synchronized data between hardware and application
Booking, expiry checks, and fee calculations functioned as expected
Admin dashboard correctly displayed metrics and booking history