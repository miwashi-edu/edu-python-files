# edu-python-files

## Prepare

```bash
cd ~
cd ws
mkdir python_files && cd python_files
```

## Text

```bash
cd ~
cd ws
cd python_files
cat > example.txt << 'EOF'
Hello, World!
This is a sample text file.
EOF
```

```
cd ~
cd ws
cd python_files
echo '#!'"$(which python3)" >  read-text
chmod +x read-text
cat >> read-text << 'EOF'
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)
EOF

echo '#!'"$(which python3)" >  read-text-by-lines
chmod +x read-text-by-lines
cat >> read-text-by-lines << 'EOF'
with open('example.txt', 'r') as file:
    for line in file:
        print(line.strip())
EOF
```

## csv

```bash
cd ~
cd ws
cd python_files
cat > example.csv << 'EOF'
name,age,city
Alice,30,New York
Bob,25,Los Angeles
EOF
```

```bash
cd ~
cd ws
cd python_files
echo '#!'"$(which python3)" >  read-csv
chmod +x read-csv
cat >> read-csv << 'EOF'
import csv
with open('example.csv', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
EOF
```

## xml

```bash
cd ~
cd ws
cd python_files
cat > example.xml << 'EOF'
<users>
  <user>
    <name>Alice</name>
    <age>30</age>
    <city>New York</city>
  </user>
  <user>
    <name>Bob</name>
    <age>25</age>
    <city>Los Angeles</city>
  </user>
</users>
EOF
```

```bash
cd ~
cd ws
cd python_files
echo '#!'"$(which python3)" >  read-xml
chmod +x read-xml
cat >> read-xml << 'EOF'
import xml.etree.ElementTree as ET

tree = ET.parse('example.xml')
root = tree.getroot()

for user in root.findall('user'):
    name = user.find('name').text
    age = user.find('age').text
    city = user.find('city').text
    print(f"Name: {name}, Age: {age}, City: {city}")
EOF
```

## json

```bash
cd ~
cd ws
cd python_files
cat > example.json << 'EOF'
{
  "users": [
    {
      "name": "Alice",
      "age": 30,
      "city": "New York"
    },
    {
      "name": "Bob",
      "age": 25,
      "city": "Los Angeles"
    }
  ]
}
EOF
```


```bash
cd ~
cd ws
cd python_files
echo '#!'"$(which python3)" >  read-json
chmod +x read-json
cat >> read-json << 'EOF'
import json
with open('example.json', 'r') as file:
    data = json.load(file)
    print(data)
EOF
```

## pickle

```bash
cd ~
cd ws
cd python_files
echo '#!'"$(which python3)" >  write-pickle
chmod +x write-pickle
cat >> write-pickle << 'EOF'
import pickle

data = {'name': 'Alice', 'age': 30, 'city': 'New York'}
with open('example.pkl', 'wb') as file:
    pickle.dump(data, file)
EOF
```

```bash
echo '#!'"$(which python3)" >  read-pickle
chmod +x read-pickle
cat >> read-pickle << 'EOF'
import pickle

with open('example.pkl', 'rb') as file:
    data = pickle.load(file)
print(data)
EOF
```

