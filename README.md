### LAB 4 REPORT

---

## 1. `list_students.jsp`

1.  **Client Request:** User navigates to `list_students.jsp`.
2.  **Server Execution:** Tomcat executes the JSP file.
3.  **Setup & Connection:** The scriptlet imports `java.sql.*`, loads the JDBC driver, and connects to the `student_management` database.
4.  **Database Query:** A `Statement` executes `SELECT * FROM students ORDER BY id DESC`.
5.  **Database Response:** MySQL returns a `ResultSet` (`rs`) with the student data.
6.  **HTML Generation:** A `while (rs.next())` loop iterates the `ResultSet`. Each row's data is injected into an HTML table row (`<tr>`). Edit/Delete links with the student `id` are also generated.
7.  **Resource Cleanup:** The `finally` block closes the `ResultSet`, `Statement`, and `Connection`.
8.  **Client Response:** Tomcat sends the complete HTML page with the student table to the browser.

   <img width="1300" height="411" alt="image" src="https://github.com/user-attachments/assets/6f2f68e9-cff8-49fe-8e13-a77875113185" />

---

## 2. `add_student.jsp` --> `process_add.jsp`

1.  **Client Request:** User visits `add_student.jsp`, which shows an empty HTML form. The form `POST`s to `process_add.jsp`.
2.  **Client Action:** User fills the form and clicks "Save." The browser sends a `POST` request with the form data to `process_add.jsp`.
3.  **Server Execution:** `process_add.jsp` retrieves form data using `request.getParameter()`.
4.  **Validation:** The script checks for missing required fields. If invalid, it redirects back to `add_student.jsp` with an error.
5.  **Database Insert:** If valid, it connects to MySQL. A `PreparedStatement` with `?` placeholders is used for the `INSERT` query. `pstmt.setString()` binds user data to prevent SQL injection, and `pstmt.executeUpdate()` runs the command.
6.  **Server Redirect (PRG):**
    * On success, it redirects (`response.sendRedirect`) to `list_students.jsp?message=...`.
    * On error (like a duplicate code), it redirects back to `add_student.jsp?error=...`.
7.  **Client Response:** The browser receives the redirect and makes a new `GET` request to `list_students.jsp`, showing the updated list.
    <img width="1297" height="620" alt="image" src="https://github.com/user-attachments/assets/54388381-f19d-4b4d-a777-ba66fc6d6a1d" />
  
   <img width="1300" height="530" alt="image" src="https://github.com/user-attachments/assets/50da4508-f143-4d75-a498-6d3076a6f90e" />

---

## 3. `edit_student.jsp` --> `process_edit.jsp`

1.  **Client Request:** User clicks "Edit," sending a `GET` request to `edit_student.jsp?id=X`.
2.  **Server Execution (Fetch):** `edit_student.jsp` gets the `id` from the URL. It runs a `SELECT * FROM students WHERE id = ?` query to fetch the student's current details.
3.  **HTML Generation:** The retrieved data is used to pre-fill the `value` attributes of the form inputs. A hidden field (`<input type="hidden" name="id" ...>`) stores the `id` for submission.
4.  **Client Action:** User modifies data and `POST`s the form to `process_edit.jsp`.
5.  **Server Execution (Update):** `process_edit.jsp` retrieves all form data, including the hidden `id`. It prepares and executes an `UPDATE ... WHERE id = ?` query.
6.  **Server Redirect (PRG):**
    * On success, redirects to `list_students.jsp?message=...`.
    * On failure, redirects back to `edit_student.jsp?id=X&error=...`.
7.  **Client Response:** The client reloads the main list page, showing the updated data.

    <img width="1297" height="617" alt="image" src="https://github.com/user-attachments/assets/ffd720a7-823e-47f7-b578-602740dc7d33" />
    <img width="1306" height="541" alt="image" src="https://github.com/user-attachments/assets/6005b2d6-d178-44fa-88e1-e70ad7a850ac" />

---

## 4. `delete_student.jsp`

1.  **Client Request:** User clicks "Delete." A JavaScript `confirm()` dialog appears. If the user clicks **OK**, the browser sends a `GET` request to `delete_student.jsp?id=X`.
2.  **Server Execution:** `delete_student.jsp` retrieves the `id` from the URL and connects to the database.
3.  **Database Delete:** It prepares and executes a `DELETE FROM students WHERE id = ?` query. The `WHERE` clause is critical to ensure only the correct record is deleted.
4.  **Server Redirect:**
    * On success (`rowsAffected > 0`), redirects to `list_students.jsp?message=...`.
    * On failure (ID not found or `SQLException`), redirects to `list_students.jsp?error=...`.
5.  **Client Response:** The client loads the list page, showing the final data.

    <img width="440" height="116" alt="image" src="https://github.com/user-attachments/assets/0cda1eba-4f94-4dae-9d36-10dbea086af9" />
    <img width="1300" height="486" alt="image" src="https://github.com/user-attachments/assets/9e1e6f88-4c41-4727-b920-5d0ee6bb33d1" />
