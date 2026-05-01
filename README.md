# UI Specification Document: User Management Screen

## 1. Overview
This document outlines the user interface specifications, component behaviors, and data requirements for the "User Management" screen. This interface allows administrators to view the list of existing users, filter/sort them, create new users, and edit existing user details.

### Visual Reference
<img width="800" height="370" alt="c2f1cb7e-5022-433a-93a2-1ac0b6ec1015" src="https://github.com/user-attachments/assets/b3916c7d-4895-4ce4-bf35-841a4b8df42e" />

## 2. Initial State (On Page Load)
*   **Data Grid (Left Panel):** Populates with the list of existing users fetched from the backend.
*   **Hide Disabled User Checkbox:** Checked by default. The data grid should initially filter out any users where `Enabled = false`.
*   **User Details Form (Right Panel):** Set to "New User" mode. All input fields should be empty, and the `Enabled` checkbox should be unchecked (or set to a default system state).
*   **Save User Button:** Visible but disabled until mandatory fields in the form are filled out correctly.

## 3. Layout & Structure
The screen is divided into three main sections:
1.  **Top Action Bar:** Contains global actions and filters.
2.  **Left Panel (Data Grid):** A table displaying the user directory.
3.  **Right Panel (Form):** A data entry form for adding or editing a user.

---

## 4. UI Components & Behaviors

### 4.1. Top Action Bar
*   **`+ New User` Button:**
    *   **Type:** Primary Button.
    *   **Behavior:** When clicked, clears all fields in the Right Panel form, resets validation errors, deselects any highlighted row in the Data Grid, and changes the form title to "New User".
*   **`Hide Disabled User` Checkbox:**
    *   **Type:** Checkbox with a label.
    *   **Behavior:** 
        *   **Checked (Default):** The Data Grid hides rows where the "Enabled" status is `false`.
        *   **Unchecked:** The Data Grid displays all users, including disabled ones.
*   **`Save User` Button:**
    *   **Type:** Primary/Action Button (Right-aligned).
    *   **Behavior:** Triggers form validation on the Right Panel. If validation passes, it submits the payload (Create or Update) to the API. On success, refreshes the Data Grid and displays a success notification.

### 4.2. Left Panel: Data Grid (User List)
*   **Component Type:** Data Table / Grid View.
*   **Columns:**
    *   `ID`: Numeric identifier.
    *   `User Name`: String.
    *   `Email`: String.
    *   `Enabled`: Boolean (`true` / `false`).
*   **Column Features:** Every column header includes **Sort** (Up/Down arrows) and **Filter** (Funnel icon) functionalities.
*   **Row Interaction:** 
    *   Clicking a row highlights it (indicating selection).
    *   Triggers an event to populate the Right Panel form with the selected user's data.
    *   Changes the Right Panel title from "New User" to "Edit User".

### 4.3. Right Panel: User Details Form
*   **Section Title:** Dynamic Text ("New User" or "Edit User" depending on the context).
*   **Fields:**
    *   **`Username`:** Text Input. Alphanumeric. (Mandatory)
    *   **`Display Name`:** Text Input. (Optional)
    *   **`Phone`:** Text Input. Accepts numeric characters and standard phone formatting symbols (`+`, `-`).
    *   **`Email`:** Text Input. Must include front-end email format validation. (Mandatory)
    *   **`User Roles`:** Dropdown / Multi-select menu. 
        *   **Placeholder:** "Select user roles..."
        *   **Available Options:** `Guest`, `Admin`, `SuperAdmin`.
    *   **`Enabled`:** Checkbox. Determines if the user account is active.

## 5. Validation & Error Handling
1.  **Required Fields:** If the user attempts to click "Save User" without filling in mandatory fields, highlight the empty fields with a red border and display inline error messages.
2.  **Email Validation:** Ensure the `Email` field contains a valid email string (`*@*.*`) before enabling submission.
3.  **API Feedback:** If the backend returns an error (e.g., "Username already exists"), display a distinct error toast or alert message at the top of the form.
