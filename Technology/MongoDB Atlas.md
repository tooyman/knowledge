```php
db.collection.createIndex({
   first_name: "text",
   last_name: "text"
})

```
Creating compound text index

```bash
db.collection.find({
   $text: {
      $search: "John smith"
   }
})

```

```python

import hashlib

def generate_hash(field1, field2):
    value = ' '.join(sorted([field1, field2]))
    return hashlib.sha256(value.encode()).hexdigest()

hash1 = generate_hash("John", "smith")
hash2 = generate_hash("smith", "John")

print(hash1)
print(hash2)

```


```scala
import java.security.MessageDigest

def generateHash(field1: String, field2: String): String = {
  val value = List(field1, field2).sorted.mkString(" ")
  val md = MessageDigest.getInstance("SHA-256")
  md.update(value.getBytes)
  md.digest.map("%02x".format(_)).mkString
}

val hash1 = generateHash("John", "smith")
val hash2 = generateHash("smith", "John")

println(hash1)
println(hash2)

```

```json
{  
"ID": "1",  
"First": "John",  
"Last": "Smith"  
}
```

