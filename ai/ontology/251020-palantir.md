## What is ontology web? How did Palantir develop ontology web?
It's helpful to first clarify a common point of confusion: the term "ontology web" isn't an official product name. It's most likely a combination of two different concepts:
- The "Semantic Web": A set of standards (like OWL, the Web Ontology Language) for creating a web of data that machines can understand.
- Palantir's "Ontology": A specific, proprietary platform built by Palantir Technologies.

Palantir's platform is inspired by the ideas of the Semantic Web, but it is not the Semantic Web itself.

**Semantic Web** is a vision for the internet, first proposed by Tim Berners-Lee, where web data is structured with formal meaning, 
allowing software agents to "understand" and reason over it.

The key technology that powers **Semantic Web** is the Web Ontology Language (OWL).

## What is an ontology? 
In this context, an ontology is a formal way of describing a domain of knowledge. 
It defines the types of things that exist (e.g., Person, City), the properties they have (e.g., a Person has_name), 
and the relationships between them (e.g., a Person lives_in a City).

### What does OWL do? 
OWL, which is built on standards like XML and RDF (Resource Description Framework), 
provides the language to write these rules down in a file that computers can read. This allows a system to infer new facts. 

For example, if you state that "Paris is_a City" and "Cities are_located_in Countries," a system can infer that "Paris is located in a Country."

## What is Palantir's Ontology?
Palantir's Ontology is the core component of its data platforms (like Foundry and Gotham). 
It's not a public web standard; it's a proprietary tool for private organizations.

Its goal is to solve a massive business problem: large organizations have their data trapped in thousands of different, 
disconnected systems (spreadsheets, databases, logs, etc.). Palantir's Ontology creates a digital twin or a single, 
unified "semantic layer" on top of all that messy data.

It maps the raw data to real-world concepts:
- Objects: The "nouns" of the organization (e.g., an Employee, a Customer, a Factory, an Invoice).
- Properties: The characteristics of those objects (e.g., an Employee object has a name, ID_number, and title).
- Links: The relationships between objects (e.g., an Employee works_at a Factory, a Customer placed_an Order).
- Actions: The "verbs" or things you can do (e.g., you can approve_an Invoice or assign_an Employee to a task).

By doing this, an analyst doesn't need to know how to query 50 different databases. They can just ask a simple question in the Palantir application, 
like "Show me all Factories in Germany and the Employees who work there," and the Ontology figures out where to get the data.

## How Did Palantir Develop Its Ontology?
Palantir didn't invent the concept of an ontology, but they were pioneers in applying it at a massive scale for data integration.
1. Origins (Early 2000s): Palantir was founded in 2003 by Peter Thiel, Alex Karp, and others. 
A core idea came from Thiel's experience at PayPal, where they built sophisticated systems to detect complex financial fraud by linking seemingly unrelated transactions and accounts.
2. Early Customers (CIA): Palantir's first major customer was the U.S. intelligence community, 
particularly through In-Q-Tel (the CIA's venture capital firm). These organizations had the ultimate data-silo problem: 
vast amounts of intelligence data (reports, signals, financial records) in different formats and systems that couldn't be cross-referenced.
3. The "Intelligence Augmentation" Problem: Palantir's goal was not to build an "artificial intelligence" to replace analysts. 
It was to build an "intelligence augmentation" (IA) platform. The Ontology was developed as the human-centric way 
to allow analysts to model their world and then connect all their disparate data to that model.
4. Evolution to Foundry: This core concept, proven in government and intelligence, was later generalized for the commercial world 
in their Foundry platform. A manufacturing company has the same type of problem as an intelligence agency: data from supply chains, 
HR systems, factory sensors, and sales are all separate. The Palantir Ontology allows them to build a digital twin of their entire business.

## Simple Code Example
The "code" for these two concepts looks very different. The Semantic Web (OWL) uses a formal XML-based syntax, while Palantir's Ontology is primarily defined using configuration files (like YAML) within its platform.

### Example 1: "Ontology Web" (OWL/RDF)
This is a simple OWL file, written in RDF/XML syntax. It defines two classes, Person and City, and a relationship (livesIn) that links a Person to a City.

```xml
<?xml version="1.0"?>
<rdf:RDF xmlns="http://www.example.com/ontology#"
     xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
     xmlns:owl="http://www.w3.org/2002/07/owl#"
     xmlns:rdfs="http://www.w3.org/2000/01/rdf-schema#">

  <owl:Ontology rdf:about="http://www.example.com/ontology"/>

  <owl:Class rdf:about="#Person">
    <rdfs:subClassOf rdf:resource="http://www.w3.org/2002/07/owl#Thing"/>
  </owl:Class>

  <owl:Class rdf:about="#City"/>

  <owl:ObjectProperty rdf:about="#livesIn">
    <rdfs:domain rdf:resource="#Person"/>  <rdfs:range rdf:resource="#City"/>   </owl:ObjectProperty>
  
  <owl:DatatypeProperty rdf:about="#hasName">
      <rdfs:domain rdf:resource="#Person"/>
      <rdfs:range rdf:resource="http://www.w3.org/2001/XMLSchema#string"/>
  </owl:DatatypeProperty>

</rdf:RDF>
```

# Write a complete, sample ontology code that can run on Palantir's platform such as Foundry or Gotham.
It's important to clarify that a Palantir ontology isn't a single, standalone script that you "run" like a Python or HTML file. Instead, it's a set of configuration files (typically YAML) that define the structure of your data model within the Palantir Foundry platform.

These configuration files are managed in a code repository inside Foundry. When you commit changes, the platform reads them and updates the ontology automatically.

I will provide a complete and realistic set of configuration files for a simple airline ontology. This example defines Flight, Airport, and Passenger objects, the links between them, and an Action to delay a flight.

Here are the configuration files that would define a simple airline ontology in Palantir Foundry.

## Object Type Definitions

```yaml
# This file defines the "nouns" of our world: Flights, Airports, and Passengers.
# Each object type is mapped to a backing dataset within the Foundry platform.

# ========== Airport Object ==========
Airport:
  rid: "ri.ontology.main.object-type.airport" # Unique Resource Identifier for this object type
  displayName: "Airport"
  pluralName: "Airports"
  primaryKey: "airport_code" # The unique key for an airport, like "JFK" or "SFO"
  properties:
    airport_code:
      displayName: "Airport Code (IATA)"
      type: "string"
    airport_name:
      displayName: "Airport Name"
      type: "string"
    city:
      displayName: "City"
      type: "string"
    country:
      displayName: "Country"
      type: "string"
  # This section links the abstract object to a concrete dataset in Foundry
  backingDataSource:
    type: "dataset"
    datasetRid: "ri.foundry.main.dataset.airport_data"

# ========== Flight Object ==========
Flight:
  rid: "ri.ontology.main.object-type.flight"
  displayName: "Flight"
  pluralName: "Flights"
  primaryKey: "flight_number"
  properties:
    flight_number:
      displayName: "Flight Number"
      type: "string"
    status:
      displayName: "Status" # e.g., "On Time", "Delayed", "Cancelled"
      type: "string"
    departure_time:
      displayName: "Departure Time"
      type: "timestamp"
    arrival_time:
      displayName: "Arrival Time"
      type: "timestamp"
    # These properties will be used as foreign keys to create links
    origin_airport_code:
      displayName: "Origin Airport Code"
      type: "string"
    destination_airport_code:
      displayName: "Destination Airport Code"
      type: "string"
  backingDataSource:
    type: "dataset"
    datasetRid: "ri.foundry.main.dataset.flight_schedule"

# ========== Passenger Object ==========
Passenger:
  rid: "ri.ontology.main.object-type.passenger"
  displayName: "Passenger"
  pluralName: "Passengers"
  primaryKey: "passenger_id"
  properties:
    passenger_id:
      displayName: "Passenger ID"
      type: "string"
    full_name:
      displayName: "Full Name"
      type: "string"
    seat_number:
      displayName: "Seat Number"
      type: "string"
    # Foreign key for the flight this passenger is on
    on_flight_number:
      displayName: "Flight Number"
      type: "string"
  backingDataSource:
    type: "dataset"
    datasetRid: "ri.foundry.main.dataset.passenger_manifest"
```

## Link Type Definitions
```yaml
# This file defines the relationships between our objects.
# Links connect objects based on the primary and foreign keys defined in object_types.yml.

# ========== Flight <-> Airport Links ==========
FlightDepartsFromAirport:
  rid: "ri.ontology.main.link-type.flight-departs-from-airport"
  displayName: "Departs From"
  cardinality: "MANY_TO_ONE" # Many flights can depart from one airport
  from:
    objectType: "Flight"
    foreignKey: "origin_airport_code" # The column in the flight dataset
  to:
    objectType: "Airport"
    primaryKey: "airport_code" # The column in the airport dataset

FlightArrivesAtAirport:
  rid: "ri.ontology.main.link-type.flight-arrives-at-airport"
  displayName: "Arrives At"
  cardinality: "MANY_TO_ONE" # Many flights can arrive at one airport
  from:
    objectType: "Flight"
    foreignKey: "destination_airport_code"
  to:
    objectType: "Airport"
    primaryKey: "airport_code"

# ========== Passenger <-> Flight Link ==========
PassengerIsOnFlight:
  rid: "ri.ontology.main.link-type.passenger-is-on-flight"
  displayName: "Is On Flight"
  cardinality: "MANY_TO_ONE" # Many passengers can be on one flight
  from:
    objectType: "Passenger"
    foreignKey: "on_flight_number"
  to:
    objectType: "Flight"
    primaryKey: "flight_number"
```

## Action Type Definitions
```yaml
# This file defines the "verbs" of our world—the actions users can take on objects.
# This action allows a user to delay a flight, which will update its properties.

DelayFlight:
  rid: "ri.ontology.main.action-type.delay-flight"
  displayName: "Delay Flight"
  # This action operates on a single Flight object
  appliesTo:
    objectType: "Flight"
    cardinality: "ONE"
  
  # Defines the input form a user will see when they run the action
  parameters:
    new_departure_time:
      displayName: "New Departure Time"
      type: "timestamp"
      required: true
    delay_reason:
      displayName: "Reason for Delay"
      type: "string"
      required: true
  
  # Defines what happens when the action is submitted
  logic:
    # This action modifies the properties of the Flight object
    type: "MODIFY_OBJECT"
    modifications:
      # Set the 'departure_time' property to the value from the 'new_departure_time' parameter
      - type: "SET_PROPERTY"
        property: "departure_time"
        value:
          parameter: "new_departure_time"
      # Set the 'status' property to a hardcoded string value
      - type: "SET_PROPERTY"
        property: "status"
        value: "Delayed"
```

These files provide a complete, albeit simple, definition for an airline ontology. 
In a real Palantir environment, you would commit these files to a code repository. 
The platform would then parse them to create the interactive, searchable model for users, who could then explore flights, 
see which passengers are on them, and take actions like delaying a flight through a user-friendly interface.

# How does Palantir platform use these configuration files to actually delay a flight? Show execution process step by step.

Here is a step-by-step breakdown of how the Palantir platform uses your configuration files to execute the "Delay Flight" action. 
The core principle is that the YAML files are not a script that runs, but a declarative definition that tells the platform's services 
how to build user interfaces and process data modifications.

## The Execution Process: From User Click to Data Update
Here’s the journey of the action, from the operator's screen to the underlying dataset.

### Step 1: The User Interface is Generated
- An airline operations manager is looking at a dashboard in a Foundry application. 
They click on a specific flight, "UA-456", which opens its Object View.
- Because the user is viewing a Flight object, the platform checks the ontology for any actions that apply to flights. 
It sees the DelayFlight action in ontology/action_types.yml and reads the appliesTo section:
```yaml
appliesTo:
  objectType: "Flight"
  cardinality: "ONE"
```
- Based on this, the platform dynamically generates a "Delay Flight" button or menu option on the user's screen for flight UA-456.

### Step 2: The User Triggers the Action
- The user clicks the "Delay Flight" button.
- The platform now reads the parameters section of the DelayFlight action to build an input form on the fly.
```yaml
parameters:
  new_departure_time:
    displayName: "New Departure Time"
    type: "timestamp"
    required: true
  delay_reason:
    displayName: "Reason for Delay"
    type: "string"
    required: true
```
- The user is presented with a pop-up form containing a timestamp picker for "New Departure Time"
  and a text box for "Reason for Delay". Both fields are marked as required.

### Step 3: User Submits the Form
- The user selects a new time (e.g., "2025-10-20 18:30:00") and types "Late inbound aircraft" as the reason.
- They click "Submit".

### Step 4: The Platform Executes the Logic (Backend)
- The platform's Action Service receives the request, which includes:
  - The target object: The Flight object for "UA-456".
  - The action type: DelayFlight.
  - The parameters: The new time and the reason string.
- The service looks up the logic section for the DelayFlight action.
```yaml
logic:
  type: "MODIFY_OBJECT"
  modifications:
    - type: "SET_PROPERTY"
      property: "departure_time"
      value:
        parameter: "new_departure_time"
    - type: "SET_PROPERTY"
      property: "status"
      value: "Delayed"
```
- It processes the modifications in order:
  1. It prepares to set the departure_time property of the UA-456 object to the value from the new_departure_time parameter ("2025-10-20 18:30:00").
  2. It prepares to set the status property of the UA-456 object to the hardcoded string "Delayed"
 
### Step 5: Data Write-Back to the Source Dataset
- This is the critical final step. The ontology is a "semantic layer," not the data's permanent home. The platform needs to make the change in the original dataset.
- The platform knows from ontology/object_types.yml that the Flight object is backed by the dataset with the ID ri.foundry.main.dataset.flight_schedule.
- The platform's write-back service initiates a transaction against this dataset. It finds the row where the flight_number column (the primary key) is "UA-456".
- It then updates that specific row: the value in the departure_time column is changed, and the value in the status column is changed to "Delayed".

### Step 6: Closing the Loop
- Once the write-back transaction is successfully committed, the platform's data pipelines detect the change.
- The Flight object for UA-456 is updated in the Ontology.
- The user's application view automatically refreshes, now showing the flight's status as "Delayed" with the new departure time.
- Crucially, a complete, immutable audit trail is saved, recording that this specific user delayed this flight at this time for this reason.

# Simulation
Now, Write a function named delay_flight to which flight_number, new_time, and delay_reason will be given as arguments of the function.
This function will get called when a user clicks a button to execute the "Delay Flight" action. Assume that user interface for the action already exists.

As part of platform logic, this function should follow all the steps of "The Execution Process: From User Click to Data Update" that you described in the last answer.
To do that, this function should refer to all the configuration files such as ontology/object_types.yml, ontology/link_types.yml, ontology/action_types.yml.

Also, you need to create sample files Doc1, Doc2, Doc3, each linking to backingDataSource of Airport, Flight, and Passenger respectively.
Don't forget "Data Write-Back to the Source Dataset" during the execution process. 

This is an excellent practice. It moves from theory to a practical simulation.
Since the Palantir platform itself isn't something we can run code on directly, I will simulate its logic using Python. This script will:
- Create all the YAML configuration files and sample data files (as CSVs) that you requested.
- Define the delay_flight function.
- Execute the function, which will read the YAML files to understand the "platform logic" and perform the data write-back by modifying the correct CSV file.

This example clearly separates the Configuration (the YAML files) from the Data (the CSV files) and the Execution Logic (the Python function).

```python
# Requires: PyYAML (install with: pip install PyYAML)

import yaml
import csv
import os

# --- Configuration & Data Setup ---

# Define the content for all our files
YAML_CONFIGS = {
    "ontology/object_types.yml": """
Airport:
  rid: "ri.ontology.main.object-type.airport"
  displayName: "Airport"
  primaryKey: "airport_code"
  properties:
    airport_code: { type: "string" }
    airport_name: { type: "string" }
  backingDataSource:
    type: "dataset"
    datasetRid: "ri.foundry.main.dataset.airport_data"

Flight:
  rid: "ri.ontology.main.object-type.flight"
  displayName: "Flight"
  primaryKey: "flight_number"
  properties:
    flight_number: { type: "string" }
    status: { type: "string" }
    departure_time: { type: "timestamp" }
    origin_airport_code: { type: "string" }
    destination_airport_code: { type: "string" }
  backingDataSource:
    type: "dataset"
    datasetRid: "ri.foundry.main.dataset.flight_schedule"

Passenger:
  rid: "ri.ontology.main.object-type.passenger"
  displayName: "Passenger"
  primaryKey: "passenger_id"
  properties:
    passenger_id: { type: "string" }
    full_name: { type: "string" }
    on_flight_number: { type: "string" }
  backingDataSource:
    type: "dataset"
    datasetRid: "ri.foundry.main.dataset.passenger_manifest"
""",
    "ontology/link_types.yml": """
FlightDepartsFromAirport:
  rid: "ri.ontology.main.link-type.flight-departs-from-airport"
  displayName: "Departs From"
  cardinality: "MANY_TO_ONE"
  from: { objectType: "Flight", foreignKey: "origin_airport_code" }
  to: { objectType: "Airport", primaryKey: "airport_code" }

PassengerIsOnFlight:
  rid: "ri.ontology.main.link-type.passenger-is-on-flight"
  displayName: "Is On Flight"
  cardinality: "MANY_TO_ONE"
  from: { objectType: "Passenger", foreignKey: "on_flight_number" }
  to: { objectType: "Flight", primaryKey: "flight_number" }
""",
    "ontology/action_types.yml": """
DelayFlight:
  rid: "ri.ontology.main.action-type.delay-flight"
  displayName: "Delay Flight"
  appliesTo:
    objectType: "Flight"
    cardinality: "ONE"
  parameters:
    new_departure_time:
      displayName: "New Departure Time"
      type: "timestamp"
      required: true
    delay_reason:
      displayName: "Reason for Delay"
      type: "string"
      required: true
  logic:
    type: "MODIFY_OBJECT"
    modifications:
      - type: "SET_PROPERTY"
        property: "departure_time"
        value: { parameter: "new_departure_time" }
      - type: "SET_PROPERTY"
        property: "status"
        value: "Delayed"
"""
}

# CSV data for our "backing datasets"
CSV_DATA = {
    "data/Doc1_airports.csv": """airport_code,airport_name,city,country
JFK,John F. Kennedy International,New York,USA
SFO,San Francisco International,San Francisco,USA
LHR,London Heathrow,London,UK
""",
    "data/Doc2_flights.csv": """flight_number,status,departure_time,origin_airport_code,destination_airport_code
UA-456,On Time,2025-10-20T17:00:00Z,JFK,LHR
AA-100,On Time,2025-10-20T18:00:00Z,SFO,JFK
BA-212,On Time,2025-10-20T19:00:00Z,LHR,SFO
""",
    "data/Doc3_passengers.csv": """passenger_id,full_name,seat_number,on_flight_number
P1001,Alice Smith,12A,UA-456
P1002,Bob Johnson,12B,UA-456
P1003,Charlie Brown,22C,AA-100
"""
}

# A mapping of the abstract dataset RIDs to the actual data files
DATA_FILE_MAP = {
    "ri.foundry.main.dataset.airport_data": "data/Doc1_airports.csv",
    "ri.foundry.main.dataset.flight_schedule": "data/Doc2_flights.csv",
    "ri.foundry.main.dataset.passenger_manifest": "data/Doc3_passengers.csv"
}

def setup_simulation_files():
    """Creates all the directories and files needed for the simulation."""
    print("Setting up simulation environment...")
    os.makedirs("ontology", exist_ok=True)
    os.makedirs("data", exist_ok=True)

    for path, content in YAML_CONFIGS.items():
        with open(path, "w") as f:
            f.write(content)
    
    for path, content in CSV_DATA.items():
        with open(path, "w")as f:
            f.write(content)
    print("All config and data files created.\n")

def load_yaml_file(path):
    """Helper function to load a YAML file."""
    with open(path, 'r') as f:
        return yaml.safe_load(f)

# --- Core Platform Logic Simulation ---

def delay_flight(flight_number, new_time, delay_reason):
    """
    Simulates the Palantir platform executing the 'DelayFlight' action.
    
    This function mimics the "Execution Process" by reading the YAML configs
    to determine *what* to do and *how* to do it, then performs a write-back
    to the correct backing data file.
    """
    
    print(f"--- Action 'DelayFlight' triggered for: {flight_number} ---")
    
    # The platform would already have these configs loaded, but we load them here.
    object_configs = load_yaml_file("ontology/object_types.yml")
    action_configs = load_yaml_file("ontology/action_types.yml")
    # link_configs are not needed for this action, but the platform would know about them.
    # link_configs = load_yaml_file("ontology/link_types.yml") 
    
    # === Step 1, 2, 3: UI Generation & Submission ===
    # This is simulated by the call to this function.
    # The user has already seen the form (generated from 'parameters')
    # and submitted it with the arguments: flight_number, new_time, delay_reason.
    print(f"Step 1-3: UI generated, user submitted form with:")
    print(f"  - flight_number: {flight_number}")
    print(f"  - new_departure_time: {new_time}")
    print(f"  - delay_reason: {delay_reason}\n")
    
    # === Step 4: Platform Executes the Logic ===
    print("Step 4: Platform executes logic...")
    action_name = "DelayFlight"
    action_config = action_configs[action_name]
    
    target_object_type = action_config['appliesTo']['objectType'] # "Flight"
    logic = action_config['logic']
    
    print(f"-> Found action '{action_config['displayName']}'")
    print(f"-> Logic is '{logic['type']}' on object '{target_object_type}'\n")

    # === Step 5: Data Write-Back to the Source Dataset ===
    print("Step 5: Performing data write-back...")
    
    # 1. Find the target object's config
    target_object_config = object_configs[target_object_type]
    
    # 2. Find its backing dataset and primary key
    dataset_rid = target_object_config['backingDataSource']['datasetRid']
    primary_key = target_object_config['primaryKey'] # "flight_number"
    
    # 3. Map the abstract dataset RID to the physical file
    data_file_path = DATA_FILE_MAP.get(dataset_rid)
    if not data_file_path:
        print(f"!! FAILED: No data file mapping found for RID: {dataset_rid}")
        return

    print(f"-> Modifying file: {data_file_path} (identified from RID {dataset_rid})")
    
    # 4. Read the file, find the row, and apply modifications
    all_rows = []
    header = []
    was_updated = False
    
    try:
        with open(data_file_path, 'r', newline='') as f:
            reader = csv.DictReader(f)
            header = reader.fieldnames
            
            for row in reader:
                # Check if this is the row we need to modify
                if row[primary_key] == flight_number:
                    was_updated = True
                    print(f"-> Found matching row for {primary_key}='{flight_number}'")
                    
                    # Process the modifications from the action's logic
                    for mod in logic['modifications']:
                        if mod['type'] == 'SET_PROPERTY':
                            prop = mod['property']
                            value = mod['value']
                            
                            # Determine the new value
                            new_value = None
                            if 'parameter' in value:
                                # Value comes from a user parameter
                                if value['parameter'] == 'new_departure_time':
                                    new_value = new_time
                                elif value['parameter'] == 'delay_reason':
                                    new_value = delay_reason
                            elif isinstance(value, str):
                                # Value is hardcoded in the config
                                new_value = value
                            
                            print(f"--> Applying logic: SET {prop} = {new_value}")
                            row[prop] = new_value # Apply the change to the row
                
                all_rows.append(row)

        # 5. Write the modified data back to the CSV file
        if was_updated:
            with open(data_file_path, 'w', newline='') as f:
                writer = csv.DictWriter(f, fieldnames=header)
                writer.writeheader()
                writer.writerows(all_rows)
            print(f"-> Write-back to {data_file_path} successful.\n")
        else:
            print(f"!! FAILED: No row found with {primary_key} = '{flight_number}'\n")

    except Exception as e:
        print(f"!! FAILED during file operation: {e}\n")

    # === Step 6: Closing the Loop ===
    print("Step 6: Action complete.")
    print(f"-> The platform will now re-index {data_file_path} to update the 'Flight' object view.")
    print("--- End of Action ---")


# --- Main execution block to run the simulation ---
if __name__ == "__main__":
    # 1. Create all the config and data files
    setup_simulation_files()
    
    # 2. Show the "BEFORE" state of the data
    print("--- Initial State of data/Doc2_flights.csv ---")
    # with open(CSV_DATA["data/Doc2_flights.csv".split("\n")[0]], 'r') as f:
    with open("data/Doc2_flights.csv", 'r') as f:
        print(f.read())
    print("="*50 + "\n")
    
    # 3. Execute the action as if a user clicked the button
    delay_flight(
        flight_number="UA-456", 
        new_time="2025-10-20T18:30:00Z", 
        delay_reason="Late inbound aircraft"
    )
    
    print("\n" + "="*50)
    
    # 4. Show the "AFTER" state of the data to prove it worked
    print("\n--- Final State of data/Doc2_flights.csv (after write-back) ---")
    # with open(CSV_DATA["data/Doc2_flights.csv".split("\n")[0]], 'r') as f:
    with open("data/Doc2_flights.csv", 'r') as f:
        print(f.read())
```
