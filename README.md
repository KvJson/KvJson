# KvJson
## What KvJson
KvJson aka Key-Value JSON is a simple yet powerful JSON compression library which transforms JSON into Keys and Values to save space. This removes the overhead of Keys appearing for every object.

## Example
Consider this JSON
````json
[
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "phone": "1-770-736-8031 x56442",
    "website": "hildegard.org"
  },
  {
    "id": 2,
    "name": "Ervin Howell",
    "username": "Antonette",
    "email": "Shanna@melissa.tv",
    "phone": "010-692-6593 x09125",
    "website": "anastasia.net",
   },
  {
    "id": 3,
    "name": "Clementine Bauch",
    "username": "Samantha",
    "email": "Nathan@yesenia.net",
    "phone": "1-463-123-4447",
    "website": "ramiro.info",
  },
  {
    "id": 4,
    "name": "Patricia Lebsack",
    "username": "Karianne",
    "email": "Julianne.OConner@kory.org",
    "phone": "493-170-9623 x156",
    "website": "kale.biz",
  },
  {
    "id": 5,
    "name": "Chelsey Dietrich",
    "username": "Kamren",
    "email": "Lucio_Hettinger@annie.ca",
    "phone": "(254)954-1289",
    "website": "demarco.info",
  },
  {
    "id": 6,
    "name": "Mrs. Dennis Schulist",
    "username": "Leopoldo_Corkery",
    "email": "Karley_Dach@jasper.info",
    "phone": "1-477-935-8478 x6430",
    "website": "ola.org",
  },
  {
    "id": 7,
    "name": "Kurtis Weissnat",
    "username": "Elwyn.Skiles",
    "email": "Telly.Hoeger@billy.biz",
   "phone": "210.067.6132",
    "website": "elvis.io",
 },
  {
    "id": 8,
    "name": "Nicholas Runolfsdottir V",
    "username": "Maxime_Nienow",
    "email": "Sherwood@rosamond.me",
    "phone": "586.493.6943 x140",
    "website": "jacynthe.com",
  },
  {
    "id": 9,
    "name": "Glenna Reichert",
    "username": "Delphine",
    "email": "Chaim_McDermott@dana.io",
    "phone": "(775)976-6794 x41206",
    "website": "conrad.com",
  },
  {
    "id": 10,
    "name": "Clementina DuBuque",
    "username": "Moriah.Stanton",
    "email": "Rey.Padberg@karina.biz",
    "phone": "024-648-3804",
    "website": "ambrose.net",
  }
]
````
Now see the keys, `id`, `name`, `username`, `enmail`, `phone`, and `website` appear 10 times unnecessarily.
> KvJson solves this problem by transforming this into following JSON
````json
{
  "Id":[1,2,3,4,5,6,7,8,9,10],
  "Name":["Leanne Graham","Ervin Howell","Clementine Bauch","Patricia Lebsack","Chelsey Dietrich","Mrs. Dennis Schulist","Kurtis Weissnat","Nicholas Runolfsdottir V","Glenna Reichert","Clementina DuBuque"],
  "Username":["Bret","Antonette","Samantha","Karianne","Kamren","Leopoldo_Corkery","Elwyn.Skiles","Maxime_Nienow","Delphine","Moriah.Stanton"],
  "Email":["Sincere@april.biz","Shanna@melissa.tv","Nathan@yesenia.net","Julianne.OConner@kory.org","Lucio_Hettinger@annie.ca","Karley_Dach@jasper.info","Telly.Hoeger@billy.biz","Sherwood@rosamond.me","Chaim_McDermott@dana.io","Rey.Padberg@karina.biz"],
  "Phone":["1-770-736-8031 x56442","010-692-6593 x09125","1-463-123-4447","493-170-9623 x156","(254)954-1289","1-477-935-8478 x6430","210.067.6132","586.493.6943 x140","(775)976-6794 x41206","024-648-3804"],"Website":["hildegard.org","anastasia.net","ramiro.info","kale.biz","demarco.info","ola.org","elvis.io","jacynthe.com","conrad.com","ambrose.net"]
}
````
Order is preserved.

## Benchmarking
When serializing 10000 objects, 42% improvement observed with `KvJson` compared to `System.Text.Json.JsonSerializer`

````csharp
class Program
{
        static void Main(string[] args)
        {                                    
            var people = Enumerable.Range(0, 10000)
                .Select(x => new Person
                {
                    Id = x, Name = Guid.NewGuid().ToString(), DateOfBirth = DateTime.Now
                });
            var lengthWithKvJson = new Serializer<Person>().Serialize(people).Length;
            Console.WriteLine($"Length when serialized with KvJson: {lengthWithKvJson}");
            var lengthWithSystemTextJson = System.Text.Json.JsonSerializer.Serialize(people).Length;
            Console.WriteLine($"Length when serialized with System.Text.Json: {lengthWithSystemTextJson}");
            Console.WriteLine($"Improvement: {100 * lengthWithKvJson / (double)(lengthWithKvJson + lengthWithSystemTextJson)}%");
            Console.ReadKey();

        }
}
class Person
{
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime DateOfBirth { get; set; }

        public override string ToString() => Id + Name + DateOfBirth;
}
````
Output
````
Length when serialized with `KvJson`: 797774
Length when serialized with` System.Text.Json`: 1077834
Improvement: 42.53415425824586%
````

## Limitations
Currently onle simple JSON is supported, i.e. NO NESTED JSON SUPPORT.
