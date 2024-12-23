<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador de Prova</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .question { margin-bottom: 20px; }
        .correct { color: green; font-weight: bold; }
        .wrong { color: red; font-weight: bold; }
        .feedback { margin-top: 5px; font-size: 0.9em; }
        .result { font-size: 1.2em; font-weight: bold; margin-top: 20px; }
    </style>
</head>
<body>
    <h1>Simulador de Prova</h1>
    <div id="questions-container">
        <!-- As perguntas e respostas serão inseridas dinamicamente aqui -->
    </div>
    <button type="button" onclick="submitQuiz()">Submeter</button>
    <div id="result-container"></div>

    <script>
        const questions = [
            { id: "q1", question: "What can you do with a multi action? (2 respostas corretas)", options: [
                { value: "A", text: "A. Run allocation processes" },
                { value: "B", text: "B. Approve data" },
                { value: "C", text: "C. Run allocation data actions" },
                { value: "D", text: "D. Import transaction data" }
            ], correct: ["C", "D"], correctText: ["C. Run allocation processes", "D. Import transaction data"] },

            { id: "q2", question: "You are creating a data action to copy data from one year to the next. In the parameter for the source year, which default setting must you change?", options: [
                { value: "A", text: "A. Granularity" },
                { value: "B", text: "B. Level" },
                { value: "C", text: "C. Hierarchy" },
                { value: "D", text: "D. Cardinality" }
            ], correct: ["D"], correctText: ["D. Cardinality"] },

            { id: "q3", question: "You input new data for a private version in a story. What must you do to ensure the new data is added to the model?", options: [
                { value: "A", text: "A. Wait to update" },
                { value: "B", text: "B. Wait to publish" },
                { value: "C", text: "C. Save to model" },
                { value: "D", text: "D. Send to model" }
            ], correct: ["B"], correctText: ["B. Wait to publish"] },

            { id: "q4", question: "You have a dimension with members for product groups and products. Each product group has associated products. You want to plan by product group without disaggregating into the products. How can you do this?", options: [
                { value: "A", text: "A. Disable allocations" },
                { value: "B", text: "B. Use two properties" },
                { value: "C", text: "C. Disable distribution" },
                { value: "D", text: "D. Use two hierarchies" }
            ], correct: ["D"], correctText: ["D. Use two properties"] },

            { id: "q5", question: "How can you improve the performance of advanced data actions? (Note: There are 3 correct answers to this question)", options: [
                { value: "A", text: "A. Use fewer MEMBERSET statements" },
                { value: "B", text: "B. Use fewer aggregation dimension functions" },
                { value: "C", text: "C. Use fewer data functions" },
                { value: "D", text: "D. Use fewer FOREACH functions" },
                { value: "E", text: "E. Use fewer IF statements" }
            ], correct: ["C", "D", "E"], correctText: ["C. Use fewer data functions", "D. Use fewer FOREACH functions", "E. Use fewer IF statements"] },

            { id: "q6", question: "How can you determine node relationships in a value driver tree? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Use a calculated member" },
                { value: "B", text: "B. Use a story calculated measure" },
                { value: "C", text: "C. Use a model converted measure" },
                { value: "D", text: "D. Use a dimension hierarchy" }
            ], correct: ["A", "D"], correctText: ["A. Use a calculated member", "D. Use a dimension hierarchy"] },

            { id: "q7", question: "You want to total several income and expense accounts using the account type property. What configuration option in the advanced formula must you use?", options: [
                { value: "A", text: "A. Signflip" },
                { value: "B", text: "B. Unbooke" },
                { value: "C", text: "C. Variable" },
                { value: "D", text: "D. Append" }
            ], correct: ["A"], correctText: ["A. Signflip"] },

            { id: "q8", question: "What account types use the average rate type? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. LEQ" },
                { value: "B", text: "B. AST" },
                { value: "C", text: "C. EXP" },
                { value: "D", text: "D. INC" }
            ], correct: ["C", "D"], correctText: ["C. EXP", "D. INC"] },

            { id: "q9", question: "You are creating an allocation step to distribute expenses from the HR cost center to your operating cost centers. Which dimension setting controls how much is distributed to each operating cost center?", options: [
                { value: "A", text: "A. Redistribute" },
                { value: "B", text: "B. Reference" },
                { value: "C", text: "C. Distribute" },
                { value: "D", text: "D. Driver" }
            ], correct: ["D"], correctText: ["D. Driver"] },

            { id: "q10", question: "Where can you change a data lock status? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Version management" },
                { value: "B", text: "B. Data action" },
                { value: "C", text: "C. Calendar task" },
                { value: "D", text: "D. Multi action" }
            ], correct: ["C", "D"], correctText: ["C. Calendar task", "D. Multi action"] },

            { id: "q11", question: "You are creating a new public version. Which categories can you use? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Forecast" },
                { value: "B", text: "B. Budget" },
                { value: "C", text: "C. Predictive" },
                { value: "D", text: "D. Actual" }
            ], correct: ["B", "D"], correctText: ["B. Budget", "D. Actual"] },

            { id: "q12", question: "Where can you create a blank planning version?", options: [
                { value: "A", text: "A. In the version dimension" },
                { value: "B", text: "B. In the planning model" },
                { value: "C", text: "C. In version management" },
                { value: "D", text: "D. In a data cell" }
            ], correct: ["C"], correctText: ["C. In version management"] },
{ id: "q13", question: "What type of predictive scenario can write back to a planning model?", options: [
                { value: "A", text: "A. Time series forecast" },
                { value: "B", text: "B. Classification" },
                { value: "C", text: "C. Regression" },
                { value: "D", text: "D. Value driver tree" }
            ], correct: ["A"], correctText: ["A. Time series forecast"] },

            { id: "q14", question: "You are entering values for several expense accounts in a data table. Which data entry mode must you use to process the data with a delay defined in System Administration?", options: [
                { value: "A", text: "A. Fluid" },
                { value: "B", text: "B. Single" },
                { value: "C", text: "C. New" },
                { value: "D", text: "D. Mass" }
            ], correct: ["A"], correctText: ["A. Fluid"] },

            { id: "q15", question: "You are creating a script for an advanced data action. Which character designates a virtual variable member?", options: [
                { value: "A", text: "A. %" },
                { value: "B", text: "B. /" },
                { value: "C", text: "C. #" },
                { value: "D", text: "D. ." }
            ], correct: ["B"], correctText: ["B. /"] },

            { id: "q16", question: "What can you use input controls for in a story? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Selecting an alternate data source" },
                { value: "B", text: "B. Changing dimensions or measures displayed in a table" },
                { value: "C", text: "C. Filtering data on a page" },
                { value: "D", text: "D. Implementing row-level and column-level security in a table" }
            ], correct: ["B", "C"], correctText: ["B. Changing dimensions or measures displayed in a table", "C. Filtering data on a page"] },

            { id: "q17", question: "What can you use to perform cell-based calculations in a story?", options: [
                { value: "A", text: "A. Table formulas" },
                { value: "B", text: "B. Calculated measures" },
                { value: "C", text: "C. Dimension formulas" },
                { value: "D", text: "D. Restricted measures" }
            ], correct: ["A"], correctText: ["A. Table formulas"] },

            { id: "q18", question: "Which features are available in the Optimized Design Experience? (Note: There are 3 correct answers to this question)", options: [
                { value: "A", text: "A. Linked widgets diagram" },
                { value: "B", text: "B. Grid pages" },
                { value: "C", text: "C. Undo button" },
                { value: "D", text: "D. Composites" },
                { value: "E", text: "E. Explorer" }
            ], correct: ["A", "C", "D"], correctText: ["A. Linked widgets diagram", "C. Undo button", "D. Composites"] },

            { id: "q19", question: "You are creating a styling rule for a table with a hierarchy of Country, Region, and City. You want to apply the styling rule only to countries. Which level option must you use?", options: [
                { value: "A", text: "A. Self" },
                { value: "B", text: "B. Self and Descendants" },
                { value: "C", text: "C. Self and Siblings" },
                { value: "D", text: "D. Self and Children" }
            ], correct: ["A"], correctText: ["A. Self"] },

            { id: "q20", question: "What is required to use version management in a story?", options: [
                { value: "A", text: "A. Analytic model" },
                { value: "B", text: "B. Classic mode" },
                { value: "C", text: "C. Optimized mode" },
                { value: "D", text: "D. Planning model" }
            ], correct: ["D"], correctText: ["D. Planning model"] },

            { id: "q21", question: "For which activities must you enable Advanced Mode in story design? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Add JavaScript to a button" },
                { value: "B", text: "B. Add a radio button" },
                { value: "C", text: "C. Add a widget" },
                { value: "D", text: "D. Add a layer to a geo map" }
            ], correct: ["A", "B"], correctText: ["A. Add JavaScript to a button", "B. Add a radio button"] },

            { id: "q22", question: "When scrolling down in a long table, how can you retain column headers? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Enable Keeping member names visible" },
                { value: "B", text: "B. Freeze Dimension Header Column" },
                { value: "C", text: "C. Enable Auto-Size And Page Table Vertically" },
                { value: "D", text: "D. Freeze Dimension Header Row" }
            ], correct: ["B", "D"], correctText: ["B. Freeze Dimension Header Column", "D. Freeze Dimension Header Row"] },

            { id: "q23", question: "You want to display differences between measures in a chart. What can you use?", options: [
                { value: "A", text: "A. Reference line" },
                { value: "B", text: "B. Threshold" },
                { value: "C", text: "C. Variance" },
                { value: "D", text: "D. Restricted measure" }
            ], correct: ["C"], correctText: ["C. Variance"] },

            { id: "q24", question: "You are creating a styling rule for a table. What is the context?", options: [
                { value: "A", text: "A. The most granular level in the table" },
                { value: "B", text: "B. The highest level in the table" },
                { value: "C", text: "C. The table header" },
                { value: "D", text: "D. The location of the cursor" }
            ], correct: ["D"], correctText: ["D. The location of the cursor"] },

            { id: "q25", question: "To which story elements can you apply conditional formatting? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Composite" },
                { value: "B", text: "B. Chart" },
                { value: "C", text: "C. Table" },
                { value: "D", text: "D. Lane" }
            ], correct: ["B", "C"], correctText: ["B. Chart", "C. Table"] },

            { id: "q26", question: "Which programming language is used for scripting in an SAP Analytics Cloud story?", options: [
                { value: "A", text: "A. JavaScript" },
                { value: "B", text: "B. Python" },
                { value: "C", text: "C. Wrangling Expression Language" },
                { value: "D", text: "D. ABAP" }
            ], correct: ["A"], correctText: ["A. JavaScript"] },

            { id: "q27", question: "You are designing a new story. You want the size and position of widgets to adjust dynamically for viewing on different devices and screen sizes. Which page type must you use?", options: [
                { value: "A", text: "A. Composite" },
                { value: "B", text: "B. Responsive" },
                { value: "C", text: "C. Grid" },
                { value: "D", text: "D. Canvas" }
            ], correct: ["B"], correctText: ["B. Responsive"] },

            { id: "q28", question: "Which calculation types include dynamic date options? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Dimension to Measure" },
                { value: "B", text: "B. Restricted Measure" },
                { value: "C", text: "C. Difference From" },
                { value: "D", text: "D. Aggregation" }
            ], correct: ["B", "C"], correctText: ["B. Restricted Measure", "C. Difference From"] },

            { id: "q29", question: "You want to use an input control to filter data appearing in a story. At what level is the filter applied?", options: [
                { value: "A", text: "A. Calculation" },
                { value: "B", text: "B. Page" },
                { value: "C", text: "C. Story" },
                { value: "D", text: "D. Component" }
            ], correct: ["B"], correctText: ["B. Page"] },

            { id: "q30", question: "You have a story in My Files. You want your colleague to review and comment on the story. What must you do?", options: [
                { value: "A", text: "A. Create a Review task for the story" },
                { value: "B", text: "B. Add a Comment widget to the story" },
                { value: "C", text: "C. Include the story in a Discussion" },
                { value: "D", text: "D. Share with View access" }
            ], correct: ["B"], correctText: ["B. Add a Comment widget to the story"] },

            { id: "q31", question: "You have a column chart in a story. You notice some of the labels are missing until you mouse over the data point. How can you ensure that the labels are always visible?", options: [
                { value: "A", text: "A. Set the input control on the chart dimension to ALL" },
                { value: "B", text: "B. Increase the font size of the axis labels" },
                { value: "C", text: "C. Increase the overall size of the chart widget on the page" },
                { value: "D", text: "D. Select the Avoid Data Label Overlap checkbox" }
            ], correct: ["D"], correctText: ["D. Select the Avoid Data Label Overlap checkbox"] },

            { id: "q32", question: "Your story takes a long time to open. What could cause this? (Note: There are 3 correct answers to this question)", options: [
                { value: "A", text: "A. Complex formatting" },
                { value: "B", text: "B. Many hyperlinks" },
                { value: "C", text: "C. Large calculation scope" },
                { value: "D", text: "D. Many data sources" },
                { value: "E", text: "E. Apply chart filters" }
            ], correct: ["A", "C", "D"], correctText: ["A. Complex formatting", "C. Large calculation scope", "D. Many data sources"] },

            { id: "q33", question: "How can you limit the refresh time of a story?", options: [
                { value: "A", text: "A. Use canvas pages" },
                { value: "B", text: "B. Collapse the hierarchy" },
                { value: "C", text: "C. Create calculated measures" },
                { value: "D", text: "D. Implement a value driver tree" }
            ], correct: ["B"], correctText: ["B. Collapse the hierarchy"] },

            { id: "q34", question: "What can you do to reduce the data refresh time when opening a story? (Note: There are 3 correct answers to this question)", options: [
                { value: "A", text: "A. Avoid tables with more than 500 rows and 60 columns" },
                { value: "B", text: "B. Load widgets in the background" },
                { value: "C", text: "C. Avoid prompts" },
                { value: "D", text: "D. Use responsive pages" },
                { value: "E", text: "E. Use conditional formatting" }
            ], correct: ["A", "B", "E"], correctText: ["A. Avoid tables with more than 500 rows and 60 columns", "B. Load widgets in the background", "E. Use conditional formatting"] },

            { id: "q35", question: "How can you help a user enter data faster in a planning story? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Set Size Limits for Planning Performance in the model." },
                { value: "B", text: "B. Select fluid data entry mode in the story." },
                { value: "C", text: "C. Enable Optimize Recommended Planning Area in the model." },
                { value: "D", text: "D. Enable unbooked data in the story." }
            ], correct: ["B", "C"], correctText: ["B. Select fluid data entry mode in the story.", "C. Enable Optimize Recommended Planning Area in the model."] },

            { id: "q36", question: "What are the available connection types in SAP Analytics Cloud? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Import" },
                { value: "B", text: "B. Live" },
                { value: "C", text: "C. On-premise" },
                { value: "D", text: "D. Cloud" }
            ], correct: ["A", "B"], correctText: ["A. Import", "B. Live"] },

            { id: "q37", question: "You need to delete characters from a column in a dataset. What can you use? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Calculation editor" },
                { value: "B", text: "B. Transform bar" },
                { value: "C", text: "C. Formula bar" },
                { value: "D", text: "D. Custom expression editor" }
            ], correct: ["B", "D"], correctText: ["B. Transform bar", "D. Custom expression editor"] },

            { id: "q38", question: "You are using a live connection for a model. Where can you define data security?", options: [
                { value: "A", text: "A. Data access control" },
                { value: "B", text: "B. SAP Analytics Cloud model" },
                { value: "C", text: "C. SAP Analytics Cloud role" },
                { value: "D", text: "D. Source system" }
            ], correct: ["D"], correctText: ["D. Source system"] },

            { id: "q39", question: "You are using a live connection for a model. Where is the data stored?", options: [
                { value: "A", text: "A. Embedded dataset" },
                { value: "B", text: "B. SAP Analytics Cloud model" },
                { value: "C", text: "C. Source system" },
                { value: "D", text: "D. Public dataset" }
            ], correct: ["C"], correctText: ["C. Source system"] },

            { id: "q40", question: "You import data into a dataset. One of the columns imported is Year, and SAP Analytics Cloud interprets it as a measure. How can you ensure that it is treated as a calendar year?", options: [
                { value: "A", text: "A. Includes the Year measure in a level-based time hierarchy in the dataset." },
                { value: "B", text: "B. Add the month as a suffix to the Year measure." },
                { value: "C", text: "C. Change the Year measure to a dimension in the dataset." },
                { value: "D", text: "D. Insert a character into the Year measure using the transform bar." }
            ], correct: ["C"], correctText: ["C. Change the Year measure to a dimension in the dataset."] },

 { id: "q41", question: "What must you use to transform data in a dataset using if/then/else logic?", options: [
                { value: "A", text: "A. Custom expression editor" },
                { value: "B", text: "B. Calculations editor" },
                { value: "C", text: "C. Formula bar" },
                { value: "D", text: "D. Transform bar" }
            ], correct: ["A"], correctText: ["A. Custom expression editor"] },

            { id: "q42", question: "Your embedded dataset in SAP Analytics Cloud has columns for Country, Region, City, and Customer Name. You want to aggregate measures for these columns as a single column. What can you do?", options: [
                { value: "A", text: "A. Convert the embedded dataset to a model." },
                { value: "B", text: "B. Create a parent-child hierarchy in the dataset." },
                { value: "C", text: "C. Create a group that includes the dimensions." },
                { value: "D", text: "D. Create a level-based hierarchy in the dataset." }
            ], correct: ["D"], correctText: ["D. Create a level-based hierarchy in the dataset."] },

            { id: "q43", question: "What can you use to organize dimensions into logical categories in a live model?", options: [
                { value: "A", text: "A. Parent-child hierarchy" },
                { value: "B", text: "B. Value driver tree" },
                { value: "C", text: "C. Groups" },
                { value: "D", text: "D. Level-based hierarchy" }
            ], correct: ["A"], correctText: ["A. Parent-child hierarchy"] },

            { id: "q44", question: "What source system can you connect to with a live connection?", options: [
                { value: "A", text: "A. SAP Business ByDesign Analytics" },
                { value: "B", text: "B. SAP Datasphere" },
                { value: "C", text: "C. SAP SuccessFactors" },
                { value: "D", text: "D. SAP ERP" }
            ], correct: ["B"], correctText: ["B. SAP Datasphere"] },

            { id: "q45", question: "You have a story based on an import model. The transaction data in the model's data source changes. How can you update the data in the model? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Refresh the import job" },
                { value: "B", text: "B. Schedule the import" },
                { value: "C", text: "C. Allow model import" },
                { value: "D", text: "D. Refresh the story" }
            ], correct: ["B", "D"], correctText: ["B. Schedule the import", "D. Refresh the story"] },

            { id: "q46", question: "You have a live data model with two dimensions: Firstname and Lastname. Users want a single dimension in the data model that displays the dimensions as Lastname, Firstname. What must you do?", options: [
                { value: "A", text: "A. Create the combined data in the source system data model." },
                { value: "B", text: "B. Create a calculated dimension in the story." },
                { value: "C", text: "C. Create a dimension in the live data model." },
                { value: "D", text: "D. Group the Firstname and Lastname in the data model." }
            ], correct: ["B"], correctText: ["B. Create a calculated dimension in the story."] },

            { id: "q47", question: "In a planning data model, what dimensions are included by default? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Organization" },
                { value: "B", text: "B. Date" },
                { value: "C", text: "C. Version" },
                { value: "D", text: "D. Entity" }
            ], correct: ["B", "C"], correctText: ["B. Date", "C. Version"] },

            { id: "q48", question: "The SAP Analytics Cloud (SAC) modeler has removed the first three characters from an SAP Analytics Cloud public dimension imported from a source system. What is impacted by this change?", options: [
                { value: "A", text: "A. Source system" },
                { value: "B", text: "B. Embedded datasets" },
                { value: "C", text: "C. Public datasets" },
                { value: "D", text: "D. Stories" }
            ], correct: ["D"], correctText: ["D. Stories"] },

            { id: "q49", question: "Which dimension type can you use like a measure?", options: [
                { value: "A", text: "A. Account" },
                { value: "B", text: "B. Date" },
                { value: "C", text: "C. Entity" },
                { value: "D", text: "D. Organization" }
            ], correct: ["A"], correctText: ["A. Account"] },

            { id: "q50", question: "In which types of data source can you concatenate data? (Note: There are 3 correct answers to this question)", options: [
                { value: "A", text: "A. Embedded dataset" },
                { value: "B", text: "B. Data analyzer insight" },
                { value: "C", text: "C. Live data model" },
                { value: "D", text: "D. Standalone dataset" },
                { value: "E", text: "E. Imported data model" }
            ], correct: ["A", "D", "E"], correctText: ["A. Embedded dataset", "D. Standalone dataset", "E. Imported data model"] },

            { id: "q51", question: "In a data model, what can you use to further describe a dimension?", options: [
                { value: "A", text: "A. Variable" },
                { value: "B", text: "B. Property" },
                { value: "C", text: "C. Data action" },
                { value: "D", text: "D. Measure" }
            ], correct: ["B"], correctText: ["B. Property"] },

            { id: "q52", question: "What features are supported by data analyzer? (Note: There are 3 correct answers to this question)", options: [
                { value: "A", text: "A. Conditional formatting" },
                { value: "B", text: "B. Calculated measures" },
                { value: "C", text: "C. Linked dimensions" },
                { value: "D", text: "D. Charts" },
                { value: "E", text: "E. Input controls" }
            ], correct: ["A", "B", "D"], correctText: ["A. Conditional formatting", "B. Calculated measures", "D. Charts"] },

            { id: "q53", question: "You have a dataset that extracts data from an SAP Business Warehouse (SAP BW) system. The data in the SAP BW system changes. How can you update the dataset?", options: [
                { value: "A", text: "A. You must refresh the story that uses the dataset." },
                { value: "B", text: "B. You must manually reimport the data." },
                { value: "C", text: "C. You can schedule the dataset to update on a regular basis." },
                { value: "D", text: "D. You must create a new dataset." }
            ], correct: ["C"], correctText: ["C. You can schedule the dataset to update on a regular basis."] },

            { id: "q54", question: "Which of the following data sources can you use in SAP Analytics Cloud data analyzer? (Note: There are 3 correct answers to this question)", options: [
                { value: "A", text: "A. SAP Analytics Cloud analytic model" },
                { value: "B", text: "B. SAP HANA view" },
                { value: "C", text: "C. SAP Datasphere model" },
                { value: "D", text: "D. SAP Analytics Cloud public dataset" },
                { value: "E", text: "E. SAP BusinessObjects Universe" }
            ], correct: ["A", "B", "C"], correctText: ["A. SAP Analytics Cloud analytic model", "B. SAP HANA view", "C. SAP Datasphere model"] },

            { id: "q55", question: "You want to save your data analyzer result. What is it saved as?", options: [
                { value: "A", text: "A. Model" },
                { value: "B", text: "B. Insight" },
                { value: "C", text: "C. Dataset" },
                { value: "D", text: "D. Story" }
            ], correct: ["B"], correctText: ["B. Insight"] },

            { id: "q56", question: "Which automatically created dimension type can you delete from an analytic data model?", options: [
                { value: "A", text: "A. Date" },
                { value: "B", text: "B. Generic" },
                { value: "C", text: "C. Version" },
                { value: "D", text: "D. Organization" }
            ], correct: ["B"], correctText: ["B. Generic"] },

            { id: "q57", question: "Your users need to analyze data in a story. What kinds of data models can you create? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Standalone" },
                { value: "B", text: "B. Planning" },
                { value: "C", text: "C. Embedded" },
                { value: "D", text: "D. Analytic" }
            ], correct: ["B", "D"], correctText: ["B. Planning", "D. Analytic"] },

            { id: "q58", question: "Which SAP Analytics Cloud feature uses natural language processing?", options: [
                { value: "A", text: "A. Smart insight" },
                { value: "B", text: "B. Data analyzer" },
                { value: "C", text: "C. Digital boardroom" },
                { value: "D", text: "D. Just Ask feature" }
            ], correct: ["A"], correctText: ["A. Smart insight"] },

            { id: "q59", question: "What must a data model contain in SAP Analytics Cloud? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Measures" },
                { value: "B", text: "B. Hierarchies" },
                { value: "C", text: "C. Dimensions" },
                { value: "D", text: "D. Calculations" }
            ], correct: ["A", "C"], correctText: ["A. Measures", "C. Dimensions"] },

            { id: "q60", question: "Which tasks can you perform in data analyzer? (Note: There are 2 correct answers to this question)", options: [
                { value: "A", text: "A. Input data" },
                { value: "B", text: "B. Drill down on data" },
                { value: "C", text: "C. Create cross-calculation" },
                { value: "D", text: "D. Filter data" }
            ], correct: ["B", "D"], correctText: ["B. Drill down on data", "D. Filter data"] },
            // Continue adicionando as perguntas conforme necessário...
        ];

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        function renderQuestions() {
            const container = document.getElementById("questions-container");
            container.innerHTML = ""; // Limpa o container
            shuffleArray(questions); // Embaralha as perguntas

            questions.forEach(question => {
                const questionDiv = document.createElement("div");
                questionDiv.classList.add("question");
                questionDiv.id = `${question.id}-container`;

                const questionText = document.createElement("p");
                questionText.textContent = question.question;
                questionDiv.appendChild(questionText);

                // Embaralha as opções
                const shuffledOptions = [...question.options];
                shuffleArray(shuffledOptions);

                shuffledOptions.forEach(option => {
                    const label = document.createElement("label");
                    const input = document.createElement("input");
                    input.type = question.correct.length > 1 ? "checkbox" : "radio";
                    input.name = question.id;
                    input.value = option.value;

                    label.appendChild(input);
                    label.appendChild(document.createTextNode(option.text));
                    questionDiv.appendChild(label);
                    questionDiv.appendChild(document.createElement("br"));
                });

                container.appendChild(questionDiv);
            });
        }

        function submitQuiz() {
            document.querySelectorAll('.feedback').forEach(el => el.remove()); // Limpar feedback anterior
            const resultContainer = document.getElementById("result-container");
            resultContainer.innerHTML = ""; // Limpar resultado anterior

            let totalQuestions = questions.length;
            let correctAnswers = 0;

            questions.forEach(question => {
                const questionContainer = document.getElementById(`${question.id}-container`);
                const selectedAnswers = Array.from(document.querySelectorAll(`input[name="${question.id}"]:checked`))
                    .map(input => input.value);

                const feedback = document.createElement("div");
                feedback.className = "feedback";

                if (JSON.stringify(selectedAnswers.sort()) === JSON.stringify(question.correct.sort())) {
                    feedback.textContent = "Correto!";
                    feedback.classList.add("correct");
                    correctAnswers++;
                } else {
                    feedback.innerHTML = `Errado! Respostas corretas:<br>${question.correctText.join("<br>")}`;
                    feedback.classList.add("wrong");
                }

                questionContainer.appendChild(feedback);
            });

            const percentage = ((correctAnswers / totalQuestions) * 100).toFixed(2);
            const result = document.createElement("div");
            result.className = "result";
            result.textContent = `Você acertou ${correctAnswers} de ${totalQuestions} (${percentage}%)`;
            resultContainer.appendChild(result);
        }

        // Renderiza as perguntas ao carregar a página
        renderQuestions();
    </script>
</body>
</html>

