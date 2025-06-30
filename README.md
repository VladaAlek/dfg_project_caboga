**DFG_Project_Caboga**

Find Person Nodes Queries

The names in the KBL reflect the historical realities of the Adriatic basin in the early 15th century, highlighting the variability in documentation practices of the time. This situation has influenced how the data is stored and interpreted.

  - In many instances, only a first name is recorded and saved in the database as a string (e.g., row 44: Bogoie).
    Query:

MATCH (p:Person {f_name: "Bogoie"})
RETURN p

Result: [First Name String](http://https://github.com/VladaAlek/dfg_project_caboga/blob/main/neo4j_query_table_data_2025-6-30.json "First Name String")
The query returns all person nodes with the specified personal name.
Optionally, for more specific results focused only on the f_name, this query is recommended:

MATCH (p:Person {f_name: "Bogoie"})
WHERE p.l_name IS NULL
RETURN p

   - In other cases, the naming structure parallels the modern system, where individuals are identified by a first name followed by a patronymic — a form that, in our case, is understood as the modern family name. Both entries are saved as strings.
    Query:

MATCH (p:Person)
WHERE p.f_name = "Dobrilo" AND p.l_name = "Antich"
RETURN p

Result: First and last names are strings[First and Last Names Strings](http://https://github.com/VladaAlek/dfg_project_caboga/blob/main/first%20and%20last%20names%20are%20strings.json "First and Last Names Strings")
