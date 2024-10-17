---
title: "The singleton pattern in Dart"
date: 2024-03-04
tags:
  - dart
categories:
  - programming
author_profile: true
draft: false
comments: false
---

The singleton software design pattern is used in Object-Oriented programming languages to ensure that an object can be instantiated only once. The purpose is the efficiency of the code and of the memory. Having a single instance of an object allows to avoid re-implementation of features, multiple executions of the same operations, and duplication of resources.

In the case of the Dart language, the singleton is defined as

```dart
class Singleton {
    static final Singleton _instance = Singleton._constructor();

    factory Singleton() => _instance;

    Singleton._constructor();
}
```

A single static instance is built through a constructor that has been declared private (note the underscore at the beginning of the symbol name). Such constructor therefore cannot be accessed from outside the class itself. The concreate instance il then delivered by the factory.

If the singleton is not supposed to life for the entire execution, it is advisable to ensure that garbage collection is disposing of it. This is achieved through implemententing the `dispose()` call to deallocate all memory.

```dart
@override
void dispose() {
    myAllocatedResource.dispose(); // Dispose of allocated resources.
    super.dispose();
}
```

## Common use cases

### Database instance

In many apps a simple database is used for data persistence. It is generally convenient to define a database helper that provides all the database related services maintaining a single connection to the database. This can be achieved through a singleton. A minimal example is the following.

```dart
class DBHelper {
    // Data containing the settings
    Database? _database;
    String? _path;
    
    static final DBHelper _instance = DBHelper._constructor();
    factory DBHelper() => _instance;
    DBHelper._constructor();

    // Getter of the database connection
    Future<Database> get db async {
        if (_database != null) {
            return _database!;
        }

        // if _database is null we instantiate it
        _database = await _initDB();
        return _database!;
    }

    Future<Database> _initDB() async {
        final String path = join(await getDatabasesPath(), "database.db");
        _path = path;
        return await openDatabase(path);
    }
}
```

### Settings

Settings should usually remain consistent throughout the app. A singleton class is perfect for the job

```dart
class Settings {
    // Data containing the settings
    String user;
    String userMail;
    Colors backgroundColor;

    static final Settings _instance = Settings._constructor();
    factory Settings() => _instance;
    Settings._constructor() {
        // Code to be run at construction.
        // This code will be executed exactly one time.
    };
}
```
