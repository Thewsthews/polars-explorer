## 1. Managing states across frontend and backend

We need to find out a way for Rust to keep the loaded LazyFrame as a persistent state

## 2. Designing communication protocols between frontend and backend for queries/responses

My plan is to never expose the full dataframe to the frontend; nor would the front end actually carry out any data
operation, including modifying views.
Instead, every data/view operation will be translated as a new query to be added to the current LazyFrame's execution
plan
The user could use a command to execute a query plan, prompting the frontend to send the plan to the backend
Then, the Rust backend will return a response to the frontend.
Every response should be paginated so that at any moment there will be a limited number of records on the display
This would ensure optimal performance, especially for huge datasets.

## Step by step plan

### 1. Reading a csv into a lazyframe

- [x] a. Printing the preview of the lazyframe on Rust backend (11/2/2024)

- [x] b. Print the lazyframe's JSON serialization on the Rust backend (11/3/2024)

- [x] c. Send the JSON to the frontend and display it in a div element (11/3/2024)
    - [x] i. Backend: Use Channel to send serde_json::Value (11/3/2024)
    - [x] ii. Frontend: Receive JSON using receiver.onmessage (11/3/2024)

- [x] d. Populate a data grid element using the received JSON (11/3/2024)
    - [x] i. Parse the incoming JSON (11/3/2024)
    - [x] ii. Display it in a table (11/3/2024)

### 2. App layout design

- [x] a. Fix Data Table's horizontal scrolling (11/4/2024)

- [ ] b. Implement DataFrame selector panel
    - [x] i. Loading a new DataFrame should create a new item (11/4/2024)
    - [x] ii. The user should be able to choose between dataframes (11/5/2024)
    - [ ] iii. The user should be able to rename and delete dataframes

### 3. Paginated query

- [x] a. Implement Paginated Query on the backend (11/4/2024)
- [ ] b. Implement PageInfo Control on the frontend
    - [ ] i. The controls should update using the pageInfo info
    - [ ] ii. Each action made on the controls should initiate a query
    - [ ] iii. The user should be able to change page size

### 4. State Management

- [ ] a. Keep the loaded dataframe in the background state manager
    - [X] i. Implement a HashMap tracking different dataframes with ID (11/4/2024)
    - [ ] ii. When the user removes dataframes on the frontend, the backend should be updated as well
    - [ ] iii. (Future) The results of previous queries should be saved before switching dataframes

- [x] b. Map the displayed dataframes/views with the backend states (11/5/2024)

### 5. Code Quality

- [x] a. Turn Panels into individual components (11/4/2024)

- [x] b. Extract Typing information in Rust and TypeScript (11/4/2024)

- [x] c. Set up separate hooks and services on the frontend to handle different tasks: (11/4/2024)
    - [x] i. DataFrame List and Selection ((Removed)DataFrame.ts)
    - [x] ii. Updating Dataview and PageInfo based on query results ((Removed)DataView.ts)
    - [x] iii. Communicating with the backend (commands.ts)

- [x] d. Extract the communication channels from the main app (11/4/2024)
    - [ ] ~~i. Prevent unnecessary reinitialization of channels~~
    - [x] ii. Correctly recreate communications channels before each exchange (11/5/2024)
- [ ] e. Review spellings and naming conventions to ensure consistency
    - Note: For some reason, it looks like Tauri's commands only accept camel case named arguments

- [ ] f. Implement more robust error handling mechanism

- [x] g. Use Redux to better manage frontend states and reduce prop drilling (11/5/2024)
    - [x] i. Let Redux manage dataframe, dataview and pagination info (11/5/2024)

- [x] h. Modularize Rust backend (11/5/2024)

### 6. Data Exploration

- [ ] a. Ensure that polars can handle ill-formatted dataset
- [ ] b. Allow the app to accept different formats than csv
