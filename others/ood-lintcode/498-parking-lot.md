# 498 · Parking Lot

## Question

Description

Design a parking lot.

see CC150 OO Design for details.

1. `n` levels, each level has `m` rows of spots and each row has `k` spots.So each level has `m` x `k` spots.
2. The parking lot can park motorcycles, cars and buses
3. The parking lot has motorcycle spots, compact spots, and large spots
4. Each row, motorcycle spots id is in range `[0,k/4)(0 is included, k/4 is not included)`, compact spots id is in range `[k/4,k/4*3)(k/4*3 is not included)` and large spots id is in range `[k/4*3,k)(k is not included)`.
5. A motorcycle can park in any spot
6. A car park in single compact spot or large spot
7. A bus can park in five large spots that are consecutive and within same row. it can not park in small spots

Example

**Example 1**

```text
Input:
level=1
num_rows=1
spots_per_row=11
parkVehicle("Motorcycle_1")
parkVehicle("Car_1")
parkVehicle("Car_2")
parkVehicle("Car_3")
parkVehicle("Car_4")
parkVehicle("Car_5")
parkVehicle("Bus_1")
unParkVehicle("Car_5")
parkVehicle("Bus_1")

Output:
true
true
true
true
true
true
false
true

Explanation: 
Parking Lot：
Motorcycle: 0 1
Car:        2 3 4 5
Bus:        6 7 8 9 10
When "Car_5" first got to the parking lot, there is no place for it in compact spots. 
The "Car_5" has to park in Bus spot 6. So "Bus_1" cannot park until "Car_5" left.
```

**Example 2**

```text
Input:
level=1
num_rows=1
spots_per_row=14
parkVehicle("Motorcycle_1")
parkVehicle("Motorcycle_2")
parkVehicle("Motorcycle_3")
parkVehicle("Car_1")
parkVehicle("Car_2")
parkVehicle("Car_3")
parkVehicle("Motorcycle_4")
parkVehicle("Car_4")
parkVehicle("Car_5")
parkVehicle("Car_6")
parkVehicle("Car_7")
parkVehicle("Bus_1")
unParkVehicle("Car_1")
unParkVehicle("Motorcycle_4")
unParkVehicle("Car_3")
unParkVehicle("Car_6")
parkVehicle("Bus_1")
unParkVehicle("Car_7")
parkVehicle("Bus_1")

Output:
true
true
true
true
true
true
true
true
true
true
true
false
false
true
```

## Solution

```cpp
// enum type for Vehicle
enum class VehicleSize {
    Motorcycle,
    Compact,
    Large
};

class Vehicle {
public:
    virtual VehicleSize getSize() = 0;
    virtual int getSpotNum() = 0;
};

class Bus: public Vehicle {
public:
    VehicleSize getSize() {
        return VehicleSize::Large;
    }
    int getSpotNum() {
        return 5;
    }
};

class Car: public Vehicle {
public:
    VehicleSize getSize() {
        return VehicleSize::Compact;
    }
    int getSpotNum() {
        return 1;
    }
};

class Motorcycle: public Vehicle {
public:
    VehicleSize getSize() {
        return VehicleSize::Motorcycle;
    }
    int getSpotNum() {
        return 1;
    }
};

class Level {
public:
    Level(int num_rows, int spots_per_row) {
        spots.resize(num_rows, vector<Vehicle*>(spots_per_row, NULL));
        this->num_rows = num_rows;
        this->spots_per_row = spots_per_row;
    }

    bool parkVehicle(Vehicle* vehicle) {
        for (int row = 0; row < num_rows; ++row) {
            int start = 0;
            if (vehicle->getSize() == VehicleSize::Compact) {
                start = spots_per_row / 4;
            } else if (vehicle->getSize() == VehicleSize::Large) {
                start = spots_per_row / 4 * 3;
            }
            for (int i = start; i < spots_per_row - vehicle->getSpotNum() + 1; ++i) {
                bool can_park = true;
                for (int j = i; j < i + vehicle->getSpotNum(); ++j) {
                    if (spots[row][j] != NULL) {
                        can_park = false;
                        break;
                    }
                }
                if (can_park) {
                    vehicle_to_row_start_spot[vehicle] = {row, i};
                    for (int j = i; j < i + vehicle->getSpotNum(); ++j) {
                        spots[row][j] = vehicle;
                    } 
                    return true;
                }
            }
        }
        return false;
    }

    void unParkVehicle(Vehicle* vehicle) {
        int row = vehicle_to_row_start_spot[vehicle].first, start = vehicle_to_row_start_spot[vehicle].second;
        for (int i = start; i < start + vehicle->getSpotNum(); ++i) {
            if (spots[row][i] == vehicle) {
                spots[row][i] = NULL;
            }
        }
    }

private:
    vector<vector<Vehicle*>> spots;
    unordered_map<Vehicle*, pair<int, int>> vehicle_to_row_start_spot;
    int num_rows, spots_per_row;
};

class ParkingLot {
public:
    // @param n number of leves
    // @param num_rows  each level has num_rows rows of spots
    // @param spots_per_row each row has spots_per_row spots
    ParkingLot(int n, int num_rows, int spots_per_row) {
        for (int i = 0; i < n; ++i) {
            Level* level = new Level(num_rows, spots_per_row);
            levels.push_back(level);
        }
    }

    // Park the vehicle in a spot (or multiple spots)
    // Return false if failed
    bool parkVehicle(Vehicle* vehicle) {
        for (int i = 0; i < levels.size(); ++i) {
            if (levels[i]->parkVehicle(vehicle)) {
                vehicle_to_level[vehicle] = levels[i];
                return true;
            }
        }
        return false;
        
    }

    // unPark the vehicle
    void unParkVehicle(Vehicle* vehicle) {
        Level *level = vehicle_to_level[vehicle];
        if (level) {
            level->unParkVehicle(vehicle);
        }
    }

private:
    vector<Level*> levels;
    unordered_map<Vehicle*, Level*> vehicle_to_level;
};
```

