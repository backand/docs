## Multi language support

Supporting multiple languages often requires supporting multibyte charafcter sets. In order to allow for wider international character sets, you will need to run the folloing MySQL command against your database:

On the table where you want to use multi-languages run :

ALTER TABLE database.table CONVERT TO CHARACTER SET utf8;

On each VARCHAR column on that table run :

ALTER TABLE database.table MODIFY COLUMN col VARCHAR(255)
    CHARACTER SET utf8 COLLATE utf8_general_ci;

 The Last step is to edit the connection details

 1. Go to Setting => Database
 2. Click Get Password and save it to the clipboard
 2. If you don't see the "Edit Connection" button then add "/edit" to the browser url
 3. On the username text box  add ;CharSet=utf8;
 4. Paste the password from the clipboard
 5. Click Save