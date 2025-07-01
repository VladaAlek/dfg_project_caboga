# DFG_Project_Caboga****DFG_Project_Caboga

Find Person Nodes Queries [[Main Folder: Find Person Nodes Queries](https://github.com/VladaAlek/dfg_project_caboga/tree/main/Find%20Person%20Nodes%20Queries)] [[Data Set Used](https://github.com/VladaAlek/dfg_project_caboga/blob/main/CSV_Files/01_07_25_person_template.csv)]

The names in the KBL reflect the historical realities of the Adriatic basin in the early 15th century, highlighting the variability in documentation practices of the time. This situation has influenced how the data is stored and interpreted.

  - In many instances, only a first name is recorded and saved in the database as a string (e.g., row 44: Bogoie).
**Query:**

MATCH (p:Person {f_name: "Bogoie"})
RETURN p

**Result:** [First Name String](https://github.com/VladaAlek/dfg_project_caboga/blob/main/Find%20Person%20Nodes%20Queries/First%20Name%20String.json)
The query returns all person nodes with the specified personal name.
Optionally, for more specific results focused only on the f_name, this query is recommended:

MATCH (p:Person {f_name: "Bogoie"})
WHERE p.l_name IS NULL
RETURN p

   - In other cases, the naming structure parallels the modern system, where individuals are identified by a first name followed by a patronymic — a form that, in our case, is understood as the modern family name. Both entries are saved as strings.
**Query:**

MATCH (p:Person)
WHERE p.f_name = "Dobrilo" AND p.l_name = "Antich"
RETURN p

**Result:** [First and Last Names Strings](https://github.com/VladaAlek/dfg_project_caboga/blob/main/Find%20Person%20Nodes%20Queries/first%20and%20last%20names%20are%20strings.json "First and Last Names Strings")

- Cases where the first name is recorded with variations (e.g., 'p24' Antonio; Antoine) are very common. In such cases, the query converts the semicolon-separated string into a list to extract the first name. The f_name is then matched with the s_name variable.
**Query:**
WITH "Antonio" AS f_name, "di Bon" AS s_name
MATCH (p:Person)
WHERE f_name IN split(p.f_name, "; ") and p.l_name = s_name
RETURN p
**Result:** [first name is semicolon string](https://github.com/VladaAlek/dfg_project_caboga/blob/main/Find%20Person%20Nodes%20Queries/first%20name%20is%20semicolon%20string.json)
  
The same approach is valid when only the *l_name values* are stored as a string separated by semicolons (p35: Bernardo - Gaschigl; di Gaschigl; Gaschigli).
**Query:**
  
WITH "Bernardo" AS f_name, "Gaschigl" AS s_name
MATCH (p:Person)
WHERE s_name IN split(p.l_name, "; ") and p.f_name = f_name
RETURN p
**Result:** [last name is semicolon string](https://github.com/VladaAlek/dfg_project_caboga/blob/main/Find%20Person%20Nodes%20Queries/last%20name%20is%20semicolon%20string.json "last name is semicolon string")

Logically, the same concept applies when all elements forming a person's identity are stored as semicolon-separated strings (e.g., p12: Antich; Antoie; Antoni; Antonio and Çalapia; Çalepia; Çelepia; Galapia).

**IMPORTANT NOTION**

This solution allows users to navigate through multiple name combinations, as demonstrated in the record from page 142 of the Main Book (p. 352 of the printed eddition) and in the query bellow. However, the drawback of this approach is that users must *consult the index* beforehand to select the appropriate Cypher query.

**Query:**

WITH "Antoie" AS f_name, "Çalapia" AS s_name
MATCH (p:Person)
WHERE s_name IN split(p.l_name, "; ") and f_name IN split(p.f_name, "; ")
RETURN p

**Result:** [f_name_l_name_semicolon_separated strings](https://github.com/VladaAlek/dfg_project_caboga/blob/main/Find%20Person%20Nodes%20Queries/f_name_l_name_semicolon_separated%20strings.json)

However, members of noble families had proper family names. The query below retrieves all members of a given noble house.

**Query:**

WITH "di Mençe" AS s_name
MATCH (p:Person)
WHERE s_name IN split(p.l_name, "; ")
RETURN p

**Result:** [proper_family_names](https://github.com/VladaAlek/dfg_project_caboga/blob/main/Find%20Person%20Nodes%20Queries/proper_family_names.json)

By focusing on the l_name attribute, specific family members can be traced.

**Query:**
WITH "di Mençe" AS f_name, "di Ratcho" AS p_name
MATCH (p:Person)
WHERE f_name IN split(p.l_name, "; ") and p_name IN split(p.l_name, "; ")
RETURN p

**Result:** [individual nobleman](https://github.com/VladaAlek/dfg_project_caboga/blob/main/Find%20Person%20Nodes%20Queries/individual%20nobleman.json)
