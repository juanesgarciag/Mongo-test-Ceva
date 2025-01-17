# Mongo-test-Ceva

## Exercises solution
1.
  ```
  const searchFragment = 'example';
  const sixMonthsAgo = new Date();
  sixMonthsAgo.setMonth(sixMonthsAgo.getMonth() - 6);
  
  db.collection('users').find({
    $and: [
      {
        $or: [
          { email: searchFragment }, // Buscar por email exacto
          { first_name: { $regex: `^${searchFragment}`, $options: 'i' } },
          { last_name: { $regex: `^${searchFragment}`, $options: 'i' } } 
        ]
      },
      { last_connection_date: { $gte: sixMonthsAgo } } // Usuarios conectados en los últimos 6 meses
    ]
  });
  ```

2.
  ```
    db.collection('users').aggregate([
      { 
        $unwind: "$roles" // Descompone el array de roles para procesarlos individualmente
      },
      { 
        $group: { 
          _id: "$roles", // Agrupa por cada rol
          users: { $push: "$email" } // Crea un array con los correos electrónicos de los usuarios
        } 
      }
    ]);

  ```

3.
  ```
    //Case 1
    db.collection('users').updateOne(
      { _id: ObjectId("5cd96d3ed5d3e20029627d4a") },
      { $set: { last_connection_date: new Date() } }
    );

    //Case 2
    db.collection('users').updateOne(
      { _id: ObjectId("5cd96d3ed5d3e20029627d4a") },
      { $addToSet: { roles: "admin" } }
    );

    //Case 3
    db.collection('users').updateOne(
      { 
        _id: ObjectId("5cd96d3ed5d3e20029627d4a"),
        "addresses.zip": 75001
      },
      { 
        $set: { "addresses.$.city": "Paris 1" }
      }
    );
  ```
