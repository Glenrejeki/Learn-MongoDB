//Database
use school
db.createCollection("students")
scholl> db.dropDatabase()
{ ok: 1, dropped: 'scholl' }

// Insert

school> db.students.insertOne({name:"Glen", age:30, gpa:4.0})
{
  acknowledged: true,
  insertedId: ObjectId('68c3d6054b95b3834b149f48')
}
school> db.students.find()
[
  {
    _id: ObjectId('68c3d6054b95b3834b149f48'),
    name: 'Glen',
    age: 30,
    gpa: 4
  }
]
school> 

]
school> db.students.insertMany([{name:"Dosen A", age:50, ipk:3.2}, {}, {}])
