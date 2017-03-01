## Multi language support

Supporting multiple languages often requires supporting multi-byte character sets. In order to allow for wider international character sets, you will need to run the following MySQL command against your database:

For the following scripts you'll need your schema_name , you can find it here : https://www.backand.com/#/app/<YOUR_APP_NAME>/database

```sql
ALTER SCHEMA `<YOUR_SCHEMA_NAME>`  DEFAULT CHARACTER SET utf8;
```

On the table where you want to use multi-languages run:

```sql
ALTER TABLE `<YOUR_SCHEMA_NAME>`.`<YOUR_TABLE_NAME>` CONVERT TO CHARACTER SET utf8;
```

On each VARCHAR column on that table run:

```sql
ALTER TABLE `<YOUR_SCHEMA_NAME>`.`<YOUR_TABLE_NAME>` MODIFY COLUMN col VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci;
```

The Last step is to edit the connection details

 1. Go to Setting => Database
 2. Click Get Password and save it to the clipboard
 2. If you don't see the "Edit Connection" button then add "/edit" to the browser url
 3. On the username text box  add ;CharSet=utf8;
 4. Paste the password from the clipboard
 5. Click Save