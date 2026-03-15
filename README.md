# Stranger Things Mini-Puzzle

Skills used: Game design, Visualization

Tableau Public link: [https://public.tableau.com/app/profile/sagi.sagi/viz/StrangerThingsMiniPuzzlefinal/Room1](https://public.tableau.com/app/profile/sagi.sagi/viz/StrangerThingsMiniPuzzlefinal/Room1)

## PROJECT OVERVIEW

**Designed and built interactive dashboards simulating a hidden-object minipuzzle inspired by Stranger Things, using a non-game engine: Tableau Desktop**

## DASHBOARD

[View pdf files here](Dashboard)

## EXECUTION

***Skills used: Set Action, Conditional calculated field, Parameter**

[Detail execution in Notion here](https://www.notion.so/Stranger-Things-Mini-Puzzle-2a76780f5cc1816ebcd5ee58d7e1b41c?source=copy_link)

### **Assets / theme**

- 2 background images
- Stranger Things font
- Object → Custom shapes

Create schema - mock dataset

## GATE 1 / DASHBOARD 1 – FIND HIDDEN OBJECTS

### **Gameplay / game mechanism**

- User interacts with dashboard to find **10 hidden objects (demogorgons)**
- Click onto the right object → it should disappear, others remain no change → **Score / Count** will be accumulatively added
- Win condition: **Score = 10 (all objects are found) + Panel Congratulations**
- Enter “Gate 2” by clicking onto the **“portal”** (sheet/object)

### Action logic

click object → mark found (simulated via set & action) → update score (by calculated field) → dashboard navigation

### **Workflow**

1. Assign custom shapes as demogorgons

<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%204.png" />

- Drag object_id & object_name → **Marks - Detail & Shape**, define & adjust x_pos & y_pos to be aligned with the background
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%205.png" />

*note: 

MUST USE manual fixed x_pos & y_pos (Manual axis range)

for smoother UX when clicking object → duplicate the sheet, the 2nd sheet has 0% opacity

2. **Set Action:**
- Create **2 empty Sets from the field: Object_id**: Set Selected_Object_R1 (record user clicks) and Set Found_Object_R1 (record true objects clicked)
- In Dashboard, create an **Action - Change set values: Hunt Demo** - Run action on Select - Assign only latest value to Set Selected_Object_R1
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%206.png" />

- Create another **Action - Change set values: Found Demo** - run Action on Select - Add all values to Set Found_Object_R1
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%207.png" />

*important: Actions need to target the **right Source Sheets**!

3. For the **“disappear effect”**
- Create a **calculated field named Show_Object** → when an object is chosen / added into Set Found_Object_R1, it would be “hide”
- Dashboard only shows “not hidden” objects

*important: **Filter “Show_Object”** should be **applied only in this worksheet**

<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%208.png" />

4. Score / Counting found objects
- Create a calculated field named Found_Count → use **FIXED: COUNTD()** for cumulative count (not affected by filters) → drag into sheet Counter
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%209.png" />

5. Win condition
- Create a calculated field named Is_Win_Room1 → set **win condition: Found_Count = 10** & **only show sheet Panel_R1 when Is_Win_Room1 = TRUE**
- Panel_R1 and Portal sheets are created as Text / Shape
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2010.png" />
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2011.png" />
  
6. Dashboard navigation
- Create Action **GoToSheet** → Run action on Select sheet “Portal” / “Panel_R1” → users will be navigated to Dashboard “Gate 2”

<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2012.png" />

## GATE 2 – PAIRING OBJECT + HIDDEN MESSAGE

### Gameplay / game mechanism

- User interacts with objects/sheets to select Pairing Objects, 12 zones/pillars correspond to 12 Pairs
- Each pillar has:

       - 1 fixed object

       - 5 random objects (including 1 right object/answer)

       - 1 text input field (parameter)

- Win condition: 12 right answers (right objects + right text input) = **All_Solved (parameter): TRUE** & User click **“Confirm” button**
- Hidden message: record 12 first-letter of 12 right answers
- Show only in “win” status: hidden message + “Thank you” text

### Action logic

Click object → Text input → Pair check (via Parameter) → Confirm all 12 true pairs (via Set Action & Parameter) → Hide all objects & Show hidden message

### Workflow

1. Assign custom shapes as objects (12 sheets - 12 palettes with shapes)

<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2013.png" />

- Drag Object_id → Marks - Detail & Shape, define x_pos & y_pos by calculated field following **field “slot_position”**
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2014.png" />

*note: set the position/coordinates for fixed object by [Object_role] = "base”, adjust object size by calculated field

2. Text input field
- Create 12 parameters corresponding to 12 pillars, type **STRING**
- Create a sheet named **Input_Field** containing all 12 parameters + empty text → show corresponding Parameter to each pillar on Dashboard
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2015.png" />

3. **Check True Pair - Set Action:**
- Create 12 **Action - Change parameter: Pillar Check** - Run action on Select - Target each pillar’s parameter - **Source Field: Object name**

→ Text parameter would be automatically assigned value as corresponding object name
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2016.png" />

- Create 2 calculated fields: **IsPillarSolved** to check if each answer / parameter is true & **AllSolvedCount** to check if users get all 12 right answers (parameter)
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2017.png" />
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2018.png" />

4. Win condition
- Create a parameter **Param_All_Solved** to check if all answers are right, set **initial value = FALSE**
- Create a sheet (simulated a button) “Confirm”: drag **AllSolvedCount and Param_All_Solved → Marks - Detail**
- Create an **Action - Change Parameter: Final Confirm** - Run action on Select sheet “Confirm” - Target **Param_All_Solved** and **Source Field = AllSolvedCount**

→ when user clicks 12 right objects / 12 right input (object name) changed in parameters ⇒ AllSolvedCount = TRUE
→ when user click “Confirm” button ⇒ **true win state: change Param_All_Solve → TRUE**

<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2019.png" />

5. Hidden message
- Create 2 calculated fields with opposite conditions:
- **Hide all objects** (all 12 sheets) after user reach “win state” → use it as **Filter** in object sheets and Input_Field 
- **Show_Hidden_Message** when win condition is met → only use in sheet “Hidden message”
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2020.png" />
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2021.png" />

- Trigger all parameters to disappear using **calculated field and Control visibility using value** of parameter “Show_Input_Control”
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2022.png" />
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2023.png" />

- In sheet “Hidden message”, drag fields Letter_Order and Syllable (first-letter) → **Marks - Detail & Text,** define letters coordinates by calculated fields
<img width="432" alt="image" src="https://github.com/sagisagi95/Tableau-Interactive-Dashboard-MiniPuzzle/blob/main/image/image%2024.png" />

- Finally, “thank you” text only appears within Hidden message: use Text item in Dashboard & Control visibility using value of **Param_All_Solved**
