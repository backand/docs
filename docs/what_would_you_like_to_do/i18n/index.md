## Multi language support

Supporting multiple languages often requires supporting multibyte charafcter sets. In order to allow for wider international character sets, you will need to run the folloing MySQL command against your database:

For the following scripts you'll need your schema_name , you can find it here : https://www.backand.com/#/app/<YOUR_APP_NAME>/database

ALTER SCHEMA `<YOUR_SCHEMA_NAME>`  DEFAULT CHARACTER SET utf8 ;

On the table where you want to use multi-languages run :

ALTER TABLE `<YOUR_SCHEMA_NAME>`.`<YOUR_TABLE_NAME>` CONVERT TO CHARACTER SET utf8;

On each VARCHAR column on that table run :

ALTER TABLE `<YOUR_SCHEMA_NAME>`.`<YOUR_TABLE_NAME>` MODIFY COLUMN col VARCHAR(255)
    CHARACTER SET utf8 COLLATE utf8_general_ci;

The final step is to edit your database details. For that you need to go to Settings / Database / Edit Connection
 (https://www.backand.com/#/app/<YOUR_APP_NAME>/database/edit)

 and add '; CharSet=utf8'  to your DB username ( don't forget to re-enter the password).
