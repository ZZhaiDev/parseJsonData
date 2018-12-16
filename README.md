# parseJsonData

## Parsing Json Data in Two Ways - Swift 4

### Json Data:
```
let data = """
    {
        "destination_addresses" : [ "New York, NY, USA" ],
        "origin_addresses" : [ "Washington, DC, USA" ],
        "rows" : [
            {
                "elements" : [
                    {
                        "distance" : {
                            "text" : "225 mi",
                            "value" : 361715
                        },
                        "duration" : {
                            "text" : "3 hours 49 mins",
                            "value" : 13725
                        },
                        "status" : "OK"
                    }
                ]
            }
        ],
        "status" : "OK"
    }
    """.data(using: .utf8)!
```

### First Method: 
```
let json = try? JSONSerialization.jsonObject(with: data, options: [])
        if let dictionary = json as? [String: Any]{
            if let rows = dictionary["rows"] as? [Any]{
                if let firstRows = rows.first as? [String: Any]{
                    if let elements = firstRows["elements"] as? [Any]{
                        if let fisrstElements = elements.first as? [String: Any]{
                            if let duration = fisrstElements["duration"] as? [String: Any]{
                                if let text = duration["text"]{
                                    print(text)
                                }
                                if let value = duration["value"]{
                                    print(value)
                                }
                            }
                        }
                    }
                }
            }
        }
```

### Second Method: Swift4
```
struct Data: Decodable {
    let destination_addresses: [String]?
    let origin_addresses: [String]?
    let rows: [Row]?
}

struct Row: Decodable {
    let elements: [Element]?
}

struct Element: Decodable {
    let duration: Duration?
}

struct Duration: Decodable {
    let text: String?
    let value: Int?
}
do {
    let data = try JSONDecoder().decode(Data.self, from: self.data)
    guard let rows = data.rows else { return }
    guard let row = rows.first else { return }
    guard let elements = row.elements else { return }
    guard let element = elements.first else { return }
    guard let distance = element.duration else { return }
    print(distance.text ?? "", distance.value ?? "")
  } catch let jsonError {
    print(jsonError)
  }

```
