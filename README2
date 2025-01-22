import mysql.connector
from datetime import datetime

db_config = {
    "host": "localhost",
    "user": "your_username",
    "password": "your_password",
    "database": "FlightsDB"
}

connection = mysql.connector.connect(**db_config)
cursor = connection.cursor()


airport_insert_query = """
INSERT INTO Airports (iata_code, icao_code, name, city, country)
VALUES (%s, %s, %s, %s, %s)
ON DUPLICATE KEY UPDATE
    name=VALUES(name), city=VALUES(city), country=VALUES(country);
"""

airport_data = (
    "SKG",       # IATA code
    "LGTS",      # ICAO code
    "Thessaloniki Airport",  # Name
    "Thessaloniki",          # City
    "Greece"                 # Country
)

cursor.execute(airport_insert_query, airport_data)
connection.commit()
airport_id = cursor.lastrowid

# Insert data into Flights table
flight_insert_query = """
INSERT INTO Flights (arrival_time, airport_id, airline_name, flight_number)
VALUES (%s, %s, %s, %s);
"""

# Example flight data to be stored (from the DataFrame flights_df)
for _, row in flights_df.iterrows():
    cursor.execute(flight_insert_query, (
        datetime.strptime(row["arrival_time"], "%Y-%m-%dT%H:%M:%S"),
        airport_id,
        row["airline_name"],
        row["flight_number"]
    ))

connection.commit()

# Close the connection
cursor.close()
connection.close()

print("Data stored successfully in MySQL!")
