import sqlite3
from datetime import datetime

# Connect to the SQLite database (or create it)
conn = sqlite3.connect('project_management.db')
cursor = conn.cursor()

# Create tables based on the schema
def create_tables():
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS Projects (
            ProjectID INTEGER PRIMARY KEY,
            ProjectName TEXT,
            Client TEXT,
            Budget REAL,
            StartDate TEXT,
            EndDate TEXT,
            Status TEXT
        )
    ''')
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS CostSummary (
            CostSummaryID INTEGER PRIMARY KEY,
            ProjectID INTEGER,
            EstimatedCost REAL,
            ActualLaborCost REAL,
            ActualMaterialCost REAL,
            PlantCost REAL,
            FuelCost REAL,
            SubcontractorCost REAL,
            OtherCost REAL,
            TotalActualCost REAL,
            Variance REAL,
            LaborCostCode TEXT,
            MaterialCostCode TEXT,
            FOREIGN KEY (ProjectID) REFERENCES Projects(ProjectID)
        )
    ''')

# Insert data into the Projects table
def insert_project(project_name, client, budget, start_date, end_date, status):
    cursor.execute('''
        INSERT INTO Projects (ProjectName, Client, Budget, StartDate, EndDate, Status)
        VALUES (?, ?, ?, ?, ?, ?)
    ''', (project_name, client, budget, start_date, end_date, status))
    conn.commit()

# Insert data into the CostSummary table
def insert_cost_summary(project_id, estimated_cost, actual_labor_cost, actual_material_cost, plant_cost, fuel_cost, subcontractor_cost, other_cost, total_actual_cost, variance, labor_cost_code, material_cost_code):
    cursor.execute('''
        INSERT INTO CostSummary (ProjectID, EstimatedCost, ActualLaborCost, ActualMaterialCost, PlantCost, FuelCost, SubcontractorCost, OtherCost, TotalActualCost, Variance, LaborCostCode, MaterialCostCode)
        VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
    ''', (project_id, estimated_cost, actual_labor_cost, actual_material_cost, plant_cost, fuel_cost, subcontractor_cost, other_cost, total_actual_cost, variance, labor_cost_code, material_cost_code))
    conn.commit()

# Example of querying and displaying projects
def display_projects():
    cursor.execute('SELECT * FROM Projects')
    projects = cursor.fetchall()
    for project in projects:
        print(f'Project: {project[1]}, Status: {project[6]}, Budget: {project[3]}')

# Create tables and insert sample data
create_tables()
insert_project('Example Project', 'ABC Corp', 5000000, '2024-01-01', '2024-12-31', 'Ongoing')
insert_cost_summary(1, 5000000, 1500000, 1200000, 500000, 100000, 200000, 50000, 3550000, 1450000, 'LC001', 'MC001')

# Display all projects
display_projects()

# Close the connection
conn.close()
