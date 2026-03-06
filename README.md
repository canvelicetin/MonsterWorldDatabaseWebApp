Monster Database WebApp
This is a "Dive Into the World of Monsters" Web project based on Blazor Web App, MSSQL Database, and Entity Framework 6. The purpose of this project is to provide a comprehensive database for an RPG (Role-Playing Game) universe. In this website, you can view, edit, and manage various monster species.

In the detailed view, you can see a monster's type, challenge rating, natural habitats, potential loot drops, specific abilities, and inherent traits. You can analyze their combat properties by viewing their resistances and weaknesses against different damage types.

In the Admin interface, you can create, read, update, and delete (CRUD) all elements including Monsters, Abilities, Traits, Loot, and Habitats. You can also manage complex relationships, such as assigning specific drop rates for items in certain habitats or defining which damage types a monster is weak against.

Implementation Steps
1. Database Creation
Create a database in MSSQL Server Management Studio with the query given below.

CREATE DATABASE MonsterDB;
GO
USE MonsterDB;
GO

-- 1. Independent Tables
CREATE TABLE Monster_Type (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Type_Name VARCHAR(50) NOT NULL
);

CREATE TABLE Damage_Type (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Type_Name VARCHAR(50) NOT NULL
);

CREATE TABLE Habitat (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Habitat_Name VARCHAR(50) NOT NULL,
    Region VARCHAR(50)
);

CREATE TABLE Loot (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Loot_Name VARCHAR(50) NOT NULL,
    Rarity VARCHAR(20)
);

CREATE TABLE Trait (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Trait_Name VARCHAR(50) NOT NULL
);

CREATE TABLE Ability (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Name VARCHAR(50) NOT NULL,
    Description VARCHAR(MAX)
);

-- 2. Main Entity Table
CREATE TABLE Monster (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Name VARCHAR(50) NOT NULL,
    Challenge_Rating DECIMAL(3,1) NOT NULL,
    Monster_Type_ID INT NOT NULL,
    FOREIGN KEY (Monster_Type_ID) REFERENCES Monster_Type(ID)
);

-- 3. Relationship Tables (Many-to-Many)
CREATE TABLE Monster_Abilities (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Monster_ID INT NOT NULL,
    Ability_ID INT NOT NULL,
    FOREIGN KEY (Monster_ID) REFERENCES Monster(ID),
    FOREIGN KEY (Ability_ID) REFERENCES Ability(ID)
);

CREATE TABLE Monster_Traits (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Monster_ID INT NOT NULL,
    Trait_ID INT NOT NULL,
    FOREIGN KEY (Monster_ID) REFERENCES Monster(ID),
    FOREIGN KEY (Trait_ID) REFERENCES Trait(ID)
);

CREATE TABLE Monster_Resistances (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Monster_ID INT NOT NULL,
    DamageType_ID INT NOT NULL,
    FOREIGN KEY (Monster_ID) REFERENCES Monster(ID),
    FOREIGN KEY (DamageType_ID) REFERENCES Damage_Type(ID)
);

CREATE TABLE Monster_Weaknesses (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Monster_ID INT NOT NULL,
    DamageType_ID INT NOT NULL,
    FOREIGN KEY (Monster_ID) REFERENCES Monster(ID),
    FOREIGN KEY (DamageType_ID) REFERENCES Damage_Type(ID)
);

CREATE TABLE Monster_Habitat_Loot (
    ID INT PRIMARY KEY IDENTITY(1,1),
    Monster_ID INT NOT NULL,
    Habitat_ID INT NOT NULL,
    Loot_ID INT NOT NULL,
    Drop_Rate DECIMAL(4,2), -- e.g., 0.50 for 50%
    FOREIGN KEY (Monster_ID) REFERENCES Monster(ID),
    FOREIGN KEY (Habitat_ID) REFERENCES Habitat(ID),
    FOREIGN KEY (Loot_ID) REFERENCES Loot(ID)
);

2. Data Population
Open a new query in your MSSQL Server Management Studio and insert the sample data.

3. Setup & Usage
Open the CMPE232.sln file with Visual Studio.
Change your connection string in the appsettings.json file.

4. Important Notes
Do Not Update Model: Do not update the model from the database (.edmx). Manual fixes to .cs files will be lost.

Disclaimer
No real monsters were harmed during the coding of this project.
