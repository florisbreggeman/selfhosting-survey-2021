# Data from the Self-hosting survey 2021.

This repository contains complete data and analysis from the self-hosting survey 2021.
Context of the data, methodology for collecting and normalising the data, an overview of the data, and an analysis can be found in paper.pdf
This should be sufficient to answer most questions; if you want to do your own analysis, the data can be found in data.sql.

All data in this repository has been normalised; raw data cannot be published, as it contains potentially privacy sensitive information.
Normalisation has been implemented by reassigning the text value of the answer to a certain question to the text value of another answer; see the `normalisation_logs` folder or appendix B of paper.pdf for details.
Two responses have been removed from the dataset entirely; these can be found unprocessed in `normalisation_logs/deleted_entries.log`

# File information

## paper.pdf

This file is a full-length research paper about the survey.
Note that this paper has not been peer reviewed.
All data in this paper comes from the same source as data.sql, and all graphs and tables have been generated from this data.

## data.sql

This is a sql dump of the normalised data.
It contains instruction to populate an empty database with the survey data, and includes the create table statements.
The source of this dump is a MariaDB database; using this file with other DBMS software may not be successful.

The main table in the database is the `responses` table.
The `id` column of this table is the main key of the whole dataset; one `responses.id` can always be linked to one response.

Each question has been given a shorthand name. These shorthand names are:

 - `hardware`: "What kind of hardware do you use?",
 - `os`: "What operating system does your server use?",
 - `containers`: "Do you use containers?",
 - `container_system`: "Which container systems do you use?",
 - `container_manager`: "Which container manager do you use,
 - `program`: "Do you also program?",
 - `program_time`: "On average, how many days a week do you sit down to program?",
 - `program_host`: "How much of your software do you host yourself?",
 - `vc`: "Which version control systems do you host?",
 - `cicd`: "Which CI/CD systems do you host?",
 - `users`: "How many users do you have (including yourself)",
 - `user_type` : "Who are your additional users?",
 - `user_access`: "Do any of your users have additional access rights?",
 - `user_access_quantity`: "What sort of additional access rights do your users have?",
 - `services`: "What end-user services do you host?",
 - `technologies`: "What backend technologies power your services",
 - `auth_service`: "Do you have a centralised authentication service?",
 - `auth_mechanims`: "Which mechanisms does your authentication service use?",
 - `reason`: "Why do you self-host?"

How the answer to any question has been stored in the database depends on the type of question:

 - Results for questions that have a boolean or integer answer are stored directly in the `responses` table, as are the values for the open text field. The column is the same as the shorthand name of the question.
 - If a question has a single possible text answer, the values for these text answers are stored in a value table. This value table is named after the shorthand name of the question. It has two columns; an integer called `id` and the text value called `name`. The `responses` table will also have a column named after the shorthand name of the question, which references the text table's `id` column.
 - If a question allows for multiple answers, there is also a text table as describe above. Additionally, there is a link table; this table is called after the shorthand name with `_link` appended. It has two columns: `response`, which references `responses.id` ; and `entity`, which references `id` column of the text table. This way, a response is linked to any number of answers.

The following is important to note when dealing with the data:

 - A respondent did not need to answer any question. As such, any  field in the `responses` table can be `NULL`.
 - Similarly, for a multiple select question, a `_link` table will not contain a value for every `response`.
 - The `services` and `technologies` questions are considered multiple select, as they have been normalised into this structure.

## survey.pdf

A copy of the survey converted into PDF.
It was originally generated via Qualtrics' export to .docx tool, and then converted into PDF.
It can also be found as appendix A of paper.pdf

## normalisation\_logs

The normalisation logs in text format.
Normalisation has been performed by assigning all references to one value to another value.
In the logs, each value is formatted as id:"value".
Note that all values have been converted to lowercase before being inserted into the database.




