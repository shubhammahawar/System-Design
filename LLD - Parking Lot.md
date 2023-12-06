
**Design a Parking Lot**

A parking lot or car park is a dedicated cleared area that is intended for parking vehicles. In most countries where cars are a major mode of transportation, parking lots are a feature of every city and suburban area. Shopping malls, sports stadiums, megachurches, and similar venues often feature parking lots over large areas.

A parking lot is an area where cars can be parked for a certain amount of time. A parking lot can have multiple floors with each floor having a different number of slots and each slot being suitable for different types of vehicles.

**System Requirements**

**Functional Requirements**

1. The parking lot should have multiple entry and exit points.
2. The parking lot should have multiple floors where customers can park their cars.
3. Customers can collect a parking ticket from the entry points and can pay the parking fee at the exit points on their way out.
4. Customers can pay the tickets at the automated exit panel or to the parking attendant.
5. Customers can pay via both cash and credit cards.
6. Customers should also be able to pay the parking fee at the customer’s info portal on each floor. If the customer has paid at the info portal, they don’t have to pay at the exit.
7. The system should not allow more vehicles than the maximum capacity of the parking lot. If the parking is full, the system should be able to show a message at the entrance panel and on the parking display board on the ground floor.
8. Each parking floor will have many parking spots. The system should support multiple types of parking spots such as Compact, Large, Handicapped, Motorcycle, etc.
9. The Parking lot should have some parking spots specified for electric cars. These spots should have an electric panel through which customers can pay and charge their vehicles.
10. The system should support parking for different types of vehicles like car, truck, van, motorcycle, etc.
11. Each parking floor should have a display board showing any free parking spot for each spot type.
12. The system should support a per-hour parking fee model. For example, customers have to pay $4 for the first hour, $3.5 for the second and third hours, and $2.5 for all the remaining hours.
13. Given a vehicle, it finds the first available slot, books it, creates a ticket, parks the vehicle, and finally returns the ticket.
14. Unparks a vehicle given the ticket id.
15. Displays the number of free slots per floor for a specific vehicle type.
16. Displays all the free slots per floor for a specific vehicle type.
17. Displays all the occupied slots per floor for a specific vehicle type.

18. Details about the Parking Slots:
    Each type of slot can park a specific type of vehicle.
    No other vehicle should be allowed by the system.

    Finding the first available slot should be based on:

        The slot should be of the same type as the vehicle.
        The slot should be on the lowest possible floor in the parking lot.
        The slot should have the lowest possible slot number on the floor.
        Numbered serially from 1 to n for each floor where n is the number of parking slots on that floor.

19. Details about the Parking Lot Floors:


    Numbered serially from 1 to n where n is the number of floors.
    Might contain one or more parking lot slots of different types.
    We will assume that the first slot on each floor will be for a truck, the next 2 for bikes, and all the other slots for cars.


20. Details about the Tickets:


    The ticket id would be of the following format:
     <parking_lot_id>_<floor_no>_<slot_no>
    Example: PR1234_2_5 (denotes 5th slot of 2nd floor of parking lot PR1234)
    We can assume that there will only be 1 parking lot. The ID of that parking lot is PR1234.


**Non-Functional Requirements**

1. Scale the system for 5k parking slots for a parking lot.
2. The system should be able to handle 10k parkings per day.
3. The system should have the capacity to hold ticketing information for, say, 10 years.


**Use Case Diagram**

Here are the main Actors in our system:

**Admin**: Mainly responsible for adding and modifying parking floors, parking spots, entrance, and exit panels, adding/removing parking attendants, etc.

**Customer**: All customers can get a parking ticket and pay for it.

**Parking Attendant**: Parking attendants can do all the activities on the customer’s behalf, and can take cash for ticket payment.

**System**: To display messages on different info panels, as well as assigning and removing a vehicle from a parking spot.

**Here are the top use cases for Parking Lot:**

**Add/Remove/Edit parking floor**: To add, remove or modify a parking floor from the system. Each floor can have its own display board to show free parking spots.

**Add/Remove/Edit parking spot**: To add, remove or modify a parking spot on a parking floor.

**Add/Remove a parking attendant**: To add or remove a parking attendant from the system.

**Take ticket**: To provide customers with a new parking ticket when entering the parking lot.

**Scan ticket**: To scan a ticket to find out the total charge.

**Credit card payment**: To pay the ticket fee with credit card.

**Cash payment**: To pay the parking ticket through cash.

**Add/Modify parking rate**: To allow admin to add or modify the hourly parking rate.


**Here is the use case diagram of our Parking Lot:**


![parkingLot-usecaseDiagram](https://github.com/shubhammahawar/System-Design/assets/22192051/58dc9941-9e0c-4a16-9828-09ee35d8eb1f)


**Class Diagram:**

Here are the main classes of our Parking Lot System:

1. **ParkingLot**: The central part of the organization for which this software has been designed. It has attributes like ‘Name’ to distinguish it from any other parking lots and ‘Address’ to define its location.
2. **ParkingFloor**: The parking lot will have many parking floors.
3. **ParkingSpot**: Each parking floor will have many parking spots. Our system will support different parking spots 1) Handicapped, 2) Compact, 3) Large, 4) Motorcycle, and 5) Electric.
4. **Account**: We will have two types of accounts in the system: one for an Admin, and the other for a parking attendant.
5. **Parking ticket**: This class will encapsulate a parking ticket. Customers will take a ticket when they enter the parking lot.
6. **Vehicle**: Vehicles will be parked in the parking spots. Our system will support different types of vehicles 1) Car, 2) Truck, 3) Electric, 4) Van and 5) Motorcycle.
7. **EntrancePanel and ExitPanel**: EntrancePanel will print tickets, and ExitPanel will facilitate payment of the ticket fee.
8. **Payment**: This class will be responsible for making payments. The system will support credit card and cash transactions.
9. **ParkingRate**: This class will keep track of the hourly parking rates. It will specify a dollar amount for each hour. For example, for a two hour parking ticket, this class will define the cost for the first and the second hour.
10. **ParkingDisplayBoard**: Each parking floor will have a display board to show available parking spots for each spot type. This class will be responsible for displaying the latest availability of free parking spots to the customers.
11. **ParkingAttendantPortal**: This class will encapsulate all the operations that an attendant can perform, like scanning tickets and processing payments.
12. **CustomerInfoPortal**: This class will encapsulate the info portal that customers use to pay for the parking ticket. Once paid, the info portal will update the ticket to keep track of the payment.
13. **ElectricPanel**: Customers will use the electric panels to pay and charge their electric vehicles.



![parkingLot-classDiagram](https://github.com/shubhammahawar/System-Design/assets/22192051/3de4b95e-702b-4bce-ab3d-c95694d9a12e)


![parkingLot-umlConvention](https://github.com/shubhammahawar/System-Design/assets/22192051/6702e822-ddc0-4132-807c-1794e8de3bdf)



**Activity Diagram**


Customer paying for parking ticket: Any customer can perform this activity. Here are the set of steps:

![parkingLot-ActivityDiagram](https://github.com/shubhammahawar/System-Design/assets/22192051/a4df534f-1ce8-4098-a91f-bb41f7863cd2)


**Database Schema Design**

![parkingLotSchemaDesign](https://github.com/shubhammahawar/System-Design/assets/22192051/3d8106b8-5f58-416d-b96b-93b27a1a8b5a)


**Other DB Schema**:

![parkingLot-otherDbSchema](https://github.com/shubhammahawar/System-Design/assets/22192051/af19991d-2e18-484d-9607-cce1bbd0ccd4)


![parkingLot-otherDBSchema-2](https://github.com/shubhammahawar/System-Design/assets/22192051/7b63a846-18d7-4f89-8850-822864c7f1eb)



**Other Class Diagram**

![parkingLot-otherClassDiagram](https://github.com/shubhammahawar/System-Design/assets/22192051/88f52456-12b5-42ad-9c47-4c56c6c09d1f)


--------------------------------------------------------------------------------------------------------------------------------

### 3. Application Layers:

#### a. Presentation Layer
- Handles user interaction and input.
- Initiates requests to the Application Layer.

#### b. Application Layer
- Implements business logic.
- Communicates with the Data Access Layer.

#### c. Data Access Layer
- Manages interactions with the database.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 4. Endpoints:

#### a. /checkAvailability
- GET: Check the availability of parking spaces.

#### b. /parkVehicle
- POST: Park a vehicle in the parking lot.

#### c. /removeVehicle
- POST: Remove a vehicle from the parking lot.

#### d. /getTicketDetails
- GET: Retrieve details of a parking ticket.


-----------------------------------------------------------------------------------------------------------------------------------------

### 5. Queries for Various Use Cases:

#### a. Check Availability
```sql
SELECT Capacity, CurrentOccupancy FROM ParkingLot WHERE LotID = :lotID;
```

#### b. Park Vehicle
```sql
INSERT INTO Vehicle (LicensePlate, Type) VALUES (:licensePlate, :type);
INSERT INTO Ticket (EntryTime, VehicleID) VALUES (CURRENT_TIMESTAMP, LAST_INSERT_ID());
```

#### c. Remove Vehicle
```sql
UPDATE Ticket SET ExitTime = CURRENT_TIMESTAMP WHERE VehicleID = :vehicleID AND ExitTime IS NULL;
```

#### d. Get Ticket Details
```sql
SELECT * FROM Ticket WHERE TicketID = :ticketID;
```


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


### 6. Associations between Different Tables:

- ParkingLot and Vehicle: One-to-Many (A parking lot can have many vehicles).
- Vehicle and Ticket: One-to-One (Each vehicle has one corresponding ticket).


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 7. Database:

Use a relational database like MySQL or PostgreSQL for data consistency and ease of querying.



----------------------------------------------------------------------------------------------------------------------------------------

### 8. Cover Various Corner Cases:

#### a. Full Parking Lot
Handle scenarios where the parking lot is full, and no more vehicles can be parked.

#### b. Invalid Tickets
Implement checks to ensure that only valid tickets are used for vehicle removal.

#### c. Fee Calculation
Handle edge cases related to fee calculation, such as discounts, lost tickets, or long-duration stays.

#### d. Concurrent Transactions
Implement mechanisms to handle concurrent transactions to avoid race conditions.

#### e. Security
Ensure data security by implementing proper authentication and authorization mechanisms.

#### f. Data Integrity
Use foreign key constraints to maintain data integrity in the database.

This is a basic outline, and the actual implementation details may vary based on specific requirements and technologies used.


---------------------------------------------------------------------------------------------------------------------------------------

**9. Solid Principles :**

https://workat.tech/machine-coding/tutorial/solid-design-principles-8yu7bjegrxs5

--------------------------------------------------------------------------------------------------------------------------------------

10. **Low Level Code**


Enums and Constants: Here are the required enums, data types, and constants:

public enum VehicleType {
    CAR(1), TRUCK(2), ELECTRIC(3), VAN(4), MOTORBIKE(5);

    private final int value;

    VehicleType(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}

public enum ParkingSpotType {
    HANDICAPPED(1), COMPACT(2), LARGE(3), MOTORBIKE(4), ELECTRIC(5);

    private final int value;

    ParkingSpotType(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}

public enum AccountStatus {
    ACTIVE(1), BLOCKED(2), BANNED(3), COMPROMISED(4), ARCHIVED(5), UNKNOWN(6);

    private final int value;

    AccountStatus(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}

public enum ParkingTicketStatus {
    ACTIVE(1), PAID(2), LOST(3);

    private final int value;

    ParkingTicketStatus(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}

public class Address {
    private String streetAddress;
    private String city;
    private String state;
    private String zipCode;
    private String country;

    public Address(String street, String city, String state, String zipCode, String country) {
        this.streetAddress = street;
        this.city = city;
        this.state = state;
        this.zipCode = zipCode;
        this.country = country;
    }
}

public class Person {
    private String name;
    private Address address;
    private String email;
    private String phone;

    public Person(String name, Address address, String email, String phone) {
        this.name = name;
        this.address = address;
        this.email = email;
        this.phone = phone;
    }
}

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

// Account, Admin, and ParkingAttendant: These classes represent various people that interact with our system:

import static com.example.constants.AccountStatus;

public class Account {
    private String userName;
    private String password;
    private Person person;
    private AccountStatus status;

    public Account(String userName, String password, Person person, AccountStatus status) {
        this.userName = userName;
        this.password = password;
        this.person = person;
        this.status = status;
    }

    public void resetPassword() {
        // Implementation for resetting password
    }
}




public class Admin extends Account {
    public Admin(String userName, String password, Person person, AccountStatus status) {
        super(userName, password, person, status);
    }

    public void addParkingFloor(String floor) {
        // Implementation for adding parking floor
    }

    public void addParkingSpot(String floorName, Spot spot) {
        // Implementation for adding parking spot
    }

    public void addParkingDisplayBoard(String floorName, DisplayBoard displayBoard) {
        // Implementation for adding parking display board
    }

    public void addCustomerInfoPanel(String floorName, InfoPanel infoPanel) {
        // Implementation for adding customer info panel
    }

    public void addEntrancePanel(EntrancePanel entrancePanel) {
        // Implementation for adding entrance panel
    }

    public void addExitPanel(ExitPanel exitPanel) {
        // Implementation for adding exit panel
    }
}

public class ParkingAttendant extends Account {
    public ParkingAttendant(String userName, String password, Person person, AccountStatus status) {
        super(userName, password, person, status);
    }

    public void processTicket(String ticketNumber) {
        // Implementation for processing ticket
    }
}


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

ParkingSpot: Here is the definition of ParkingSpot and all of its children classes:


import com.example.constants.ParkingSpotType;

public abstract class ParkingSpot {
    private int number;
    private boolean isFree;
    private Vehicle vehicle;
    private ParkingSpotType parkingSpotType;

    public ParkingSpot(int number, ParkingSpotType parkingSpotType) {
        this.number = number;
        this.isFree = true;
        this.vehicle = null;
        this.parkingSpotType = parkingSpotType;
    }

    public boolean isFree() {
        return isFree;
    }

    public void assignVehicle(Vehicle vehicle) {
        this.vehicle = vehicle;
        this.isFree = false;
    }

    public void removeVehicle() {
        this.vehicle = null;
        this.isFree = true;
    }
}

public class HandicappedSpot extends ParkingSpot {
    public HandicappedSpot(int number) {
        super(number, ParkingSpotType.HANDICAPPED);
    }
}

public class CompactSpot extends ParkingSpot {
    public CompactSpot(int number) {
        super(number, ParkingSpotType.COMPACT);
    }
}

public class LargeSpot extends ParkingSpot {
    public LargeSpot(int number) {
        super(number, ParkingSpotType.LARGE);
    }
}

public class MotorbikeSpot extends ParkingSpot {
    public MotorbikeSpot(int number) {
        super(number, ParkingSpotType.MOTORBIKE);
    }
}

public class ElectricSpot extends ParkingSpot {
    public ElectricSpot(int number) {
        super(number, ParkingSpotType.ELECTRIC);
    }
}


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

ParkingFloor: This class encapsulates a parking floor:

import java.util.HashMap;
import java.util.Map;

public class ParkingFloor {
    private String name;
    private Map<Integer, ParkingSpot> handicappedSpots;
    private Map<Integer, ParkingSpot> compactSpots;
    private Map<Integer, ParkingSpot> largeSpots;
    private Map<Integer, ParkingSpot> motorbikeSpots;
    private Map<Integer, ParkingSpot> electricSpots;
    private Map<Integer, InfoPortal> infoPortals;
    private Map<String, Integer> freeHandicappedSpotCount;
    private Map<String, Integer> freeCompactSpotCount;
    private Map<String, Integer> freeLargeSpotCount;
    private Map<String, Integer> freeMotorbikeSpotCount;
    private Map<String, Integer> freeElectricSpotCount;
    private ParkingDisplayBoard displayBoard;

    public ParkingFloor(String name) {
        this.name = name;
        this.handicappedSpots = new HashMap<>();
        this.compactSpots = new HashMap<>();
        this.largeSpots = new HashMap<>();
        this.motorbikeSpots = new HashMap<>();
        this.electricSpots = new HashMap<>();
        this.infoPortals = new HashMap<>();
        this.freeHandicappedSpotCount = new HashMap<>();
        this.freeCompactSpotCount = new HashMap<>();
        this.freeLargeSpotCount = new HashMap<>();
        this.freeMotorbikeSpotCount = new HashMap<>();
        this.freeElectricSpotCount = new HashMap<>();
        this.displayBoard = new ParkingDisplayBoard();
    }

    public void addParkingSpot(ParkingSpot spot) {
        switch (spot.getType()) {
            case HANDICAPPED:
                handicappedSpots.put(spot.getNumber(), spot);
                break;
            case COMPACT:
                compactSpots.put(spot.getNumber(), spot);
                break;
            case LARGE:
                largeSpots.put(spot.getNumber(), spot);
                break;
            case MOTORBIKE:
                motorbikeSpots.put(spot.getNumber(), spot);
                break;
            case ELECTRIC:
                electricSpots.put(spot.getNumber(), spot);
                break;
            default:
                System.out.println("Wrong parking spot type");
        }
    }

    public void assignVehicleToSpot(Vehicle vehicle, ParkingSpot spot) {
        spot.assignVehicle(vehicle);
        switch (spot.getType()) {
            case HANDICAPPED:
                updateDisplayBoardForHandicapped(spot);
                break;
            case COMPACT:
                updateDisplayBoardForCompact(spot);
                break;
            case LARGE:
                updateDisplayBoardForLarge(spot);
                break;
            case MOTORBIKE:
                updateDisplayBoardForMotorbike(spot);
                break;
            case ELECTRIC:
                updateDisplayBoardForElectric(spot);
                break;
            default:
                System.out.println("Wrong parking spot type!");
        }
    }

    private void updateDisplayBoardForHandicapped(ParkingSpot spot) {
        if (displayBoard.getHandicappedFreeSpot().getNumber() == spot.getNumber()) {
            for (int key : handicappedSpots.keySet()) {
                if (handicappedSpots.get(key).isFree()) {
                    displayBoard.setHandicappedFreeSpot(handicappedSpots.get(key));
                }
            }
            displayBoard.showEmptySpotNumber();
        }
    }

    private void updateDisplayBoardForCompact(ParkingSpot spot) {
        if (displayBoard.getCompactFreeSpot().getNumber() == spot.getNumber()) {
            for (int key : compactSpots.keySet()) {
                if (compactSpots.get(key).isFree()) {
                    displayBoard.setCompactFreeSpot(compactSpots.get(key));
                }
            }
            displayBoard.showEmptySpotNumber();
        }
    }

    public void freeSpot(ParkingSpot spot) {
        spot.removeVehicle();
        switch (spot.getType()) {
            case HANDICAPPED:
                freeHandicappedSpotCount.put("free_spot", freeHandicappedSpotCount.get("free_spot") + 1);
                break;
            case COMPACT:
                freeCompactSpotCount.put("free_spot", freeCompactSpotCount.get("free_spot") + 1);
                break;
            case LARGE:
                freeLargeSpotCount.put("free_spot", freeLargeSpotCount.get("free_spot") + 1);
                break;
            case MOTORBIKE:
                freeMotorbikeSpotCount.put("free_spot", freeMotorbikeSpotCount.get("free_spot") + 1);
                break;
            case ELECTRIC:
                freeElectricSpotCount.put("free_spot", freeElectricSpotCount.get("free_spot") + 1);
                break;
            default:
                System.out.println("Wrong parking spot type!");
        }
    }
}


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

ParkingDisplayBoard: This class encapsulates a parking display board:

public class ParkingDisplayBoard {
    private int id;
    private ParkingSpot handicappedFreeSpot;
    private ParkingSpot compactFreeSpot;
    private ParkingSpot largeFreeSpot;
    private ParkingSpot motorbikeFreeSpot;
    private ParkingSpot electricFreeSpot;

    public ParkingDisplayBoard(int id) {
        this.id = id;
    }

    public void showEmptySpotNumber() {
        StringBuilder message = new StringBuilder();

        if (handicappedFreeSpot.isFree()) {
            message.append("Free Handicapped: ").append(handicappedFreeSpot.getNumber());
        } else {
            message.append("Handicapped is full");
        }
        message.append("\n");

        if (compactFreeSpot.isFree()) {
            message.append("Free Compact: ").append(compactFreeSpot.getNumber());
        } else {
            message.append("Compact is full");
        }
        message.append("\n");

        if (largeFreeSpot.isFree()) {
            message.append("Free Large: ").append(largeFreeSpot.getNumber());
        } else {
            message.append("Large is full");
        }
        message.append("\n");

        if (motorbikeFreeSpot.isFree()) {
            message.append("Free Motorbike: ").append(motorbikeFreeSpot.getNumber());
        } else {
            message.append("Motorbike is full");
        }
        message.append("\n");

        if (electricFreeSpot.isFree()) {
            message.append("Free Electric: ").append(electricFreeSpot.getNumber());
        } else {
            message.append("Electric is full");
        }

        System.out.println(message.toString());
    }

    public void setHandicappedFreeSpot(ParkingSpot handicappedFreeSpot) {
        this.handicappedFreeSpot = handicappedFreeSpot;
    }

    public void setCompactFreeSpot(ParkingSpot compactFreeSpot) {
        this.compactFreeSpot = compactFreeSpot;
    }

    public void setLargeFreeSpot(ParkingSpot largeFreeSpot) {
        this.largeFreeSpot = largeFreeSpot;
    }

    public void setMotorbikeFreeSpot(ParkingSpot motorbikeFreeSpot) {
        this.motorbikeFreeSpot = motorbikeFreeSpot;
    }

    public void setElectricFreeSpot(ParkingSpot electricFreeSpot) {
        this.electricFreeSpot = electricFreeSpot;
    }
}


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

ParkingLot: Our system will have only one object of this class. This can be enforced by using the Singleton pattern. In software engineering, 
the singleton pattern is a software design pattern that restricts the instantiation of a class to only one object.


import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ParkingLot {
    private static ParkingLot instance = null;

    private static class OnlyOne {
        private String name;
        private String address;
        private ParkingRate parkingRate;

        private int compactSpotCount;
        private int largeSpotCount;
        private int motorbikeSpotCount;
        private int electricSpotCount;
        private int maxCompactCount;
        private int maxLargeCount;
        private int maxMotorbikeCount;
        private int maxElectricCount;

        private Map<Integer, EntrancePanel> entrancePanels;
        private Map<Integer, ExitPanel> exitPanels;
        private Map<Integer, ParkingFloor> parkingFloors;
        private Map<Integer, ParkingTicket> activeTickets;

        private Lock lock;

        public OnlyOne(String name, String address) {
            this.name = name;
            this.address = address;
            this.parkingRate = new ParkingRate();

            this.compactSpotCount = 0;
            this.largeSpotCount = 0;
            this.motorbikeSpotCount = 0;
            this.electricSpotCount = 0;
            this.maxCompactCount = 0;
            this.maxLargeCount = 0;
            this.maxMotorbikeCount = 0;
            this.maxElectricCount = 0;

            this.entrancePanels = new HashMap<>();
            this.exitPanels = new HashMap<>();
            this.parkingFloors = new HashMap<>();
            this.activeTickets = new HashMap<>();

            this.lock = new ReentrantLock();
        }
    }

    private ParkingLot() {
        // private constructor to prevent instantiation
    }

    public static ParkingLot getInstance(String name, String address) {
        if (instance == null) {
            instance = new ParkingLot();
            instance.initialize(name, address);
        } else {
            instance.updateDetails(name, address);
        }
        return instance;
    }

    private void initialize(String name, String address) {
        OnlyOne onlyOne = new OnlyOne(name, address);
        instance = onlyOne;
    }

    private void updateDetails(String name, String address) {
        instance.OnlyOne.name = name;
        instance.OnlyOne.address = address;
    }

    public ParkingTicket getNewParkingTicket(Vehicle vehicle) throws Exception {
        if (isFull(vehicle.getType())) {
            throw new Exception("Parking full!");
        }

        instance.lock.lock();
        ParkingTicket ticket = new ParkingTicket();
        vehicle.assignTicket(ticket);
        ticket.saveInDB();
        instance.incrementSpotCount(vehicle.getType());
        instance.activeTickets.put(ticket.getTicketNumber(), ticket);
        instance.lock.unlock();
        return ticket;
    }

    private boolean isFull(VehicleType type) {
        if (type == VehicleType.Truck || type == VehicleType.Van) {
            return instance.largeSpotCount >= instance.maxLargeCount;
        }

        if (type == VehicleType.Motorbike) {
            return instance.motorbikeSpotCount >= instance.maxMotorbikeCount;
        }

        if (type == VehicleType.Car) {
            return (instance.compactSpotCount + instance.largeSpotCount) >= (instance.maxCompactCount + instance.maxLargeCount);
        }

        return (instance.compactSpotCount + instance.largeSpotCount + instance.electricSpotCount) >= (instance.maxCompactCount + instance.maxLargeCount
                                                                                                      + instance.maxElectricCount);
    }

    private void incrementSpotCount(VehicleType type) {
        int largeSpotCount = 0;
        int motorbikeSpotCount = 0;
        int compactSpotCount = 0;
        int electricSpotCount = 0;

        if (type == VehicleType.Truck || type == VehicleType.Van) {
            largeSpotCount += 1;
        } else if (type == VehicleType.Motorbike) {
            motorbikeSpotCount += 1;
        } else if (type == VehicleType.Car) {
            if (instance.compactSpotCount < instance.maxCompactCount) {
                compactSpotCount += 1;
            } else {
                largeSpotCount += 1;
            }
        } else {  // Electric car
            if (instance.electricSpotCount < instance.maxElectricCount) {
                electricSpotCount += 1;
            } else if (instance.compactSpotCount < instance.maxCompactCount) {
                compactSpotCount += 1;
            } else {
                largeSpotCount += 1;
            }
        }
    }

    public boolean isFull() {
        for (Integer key : instance.parkingFloors.keySet()) {
            if (!instance.parkingFloors.get(key).isFull()) {
                return false;
            }
        }
        return true;
    }

    public void addParkingFloor(ParkingFloor floor) {
        // store in database
    }

    public void addEntrancePanel(EntrancePanel entrancePanel) {
        // store in database
    }

    public void addExitPanel(ExitPanel exitPanel) {
        // store in database
    }
}


