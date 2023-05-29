# samp-database-migration
This library is created to allow scripters to have database migration process in SA-MP. Good example of database migration tool for Java is Flyway DB, and this library is trying to get closest to Flyway DB, but for PAWN.

## Requirements
- Latest MySQL: https://github.com/pBlueG/SA-MP-MySQL
- File Manager (included in the repository, with minimal changes comparing to orginal one, which is from 2015).

## How To Install ?
- Download MySQL Plugin.
- Include FileManager Plugin (present in this repository).
- Include `database_migration` in your gamemode.

## How To Use ?
- After including `database_migration` in your gamemode, include `MigrateSchemas()` function right after connection, where you will get your database handle that will be parsed as a parameter to the function.
- Practically, first time when you start server after including this library, server will automatically create schemas folder in your scriptfiles folder, with initial migration that will create table which will contain all migrations (including that one).
- After starting server first time, continue writting your migrations in schemas folder in format: `V{version}_{migration_name}.sql` (you can use initial migration from previous step as example).

## Main function
`MigrateSchemas(MySQL:serverDatabase, bool:debugMigration = false)`
- param: `serverDatabase` - Your database handle that will be returned by mysql_connect function.
- param (optional, default: false): `debugMigration` - In case you are unsure about your migrations, you can enable debugging in order to get informations step by step. All informations are printed in console log.

## Example
```pawn
#include <database_migration>

new MySQL: Database;

hook OnGameModeInit()
{
    new MySQLOpt: option_id = mysql_init_options();
    mysql_set_option(option_id, AUTO_RECONNECT, true);
    Database = mysql_connect(MYSQL_HOST, MYSQL_USER, MYSQL_PASS, MYSQL_DATABASE, option_id);
    if(Database == MYSQL_INVALID_HANDLE || mysql_errno(Database) != 0) {
        SendRconCommand("exit");
        return 1;
    }

    MigrateSchemas(Database);
    
    return Y_HOOKS_CONTINUE_RETURN_1;
}
```

## Photos

### Migration schemas database table 
![image](https://github.com/daniscavalic/samp-database-migration/assets/87475152/cd3c8b48-07af-403e-975c-024aa2506812)

### Server log
![image](https://github.com/daniscavalic/samp-database-migration/assets/87475152/4651ff0b-d102-4ca8-9521-65d086c80c4f)

### Schemas folder example
![image](https://github.com/daniscavalic/samp-database-migration/assets/87475152/adc515be-2c16-4b4f-ba93-8de80f987403)

## Credits
- Danis Cavalic - Created 'samp-database-migration' for PAWN.
- pBlueG and team - MySQL for SA-MP.
- JaTochNietDan - File Manager.


