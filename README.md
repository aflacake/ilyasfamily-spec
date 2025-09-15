# Spesifikasi IlyasFamily
Versi: 0.3

## Tujuan
IlyasFamily adalah format pertukaran dan penyimpanan data universal berbasis tiga tipe data utama, tambahan tipe data struktural baru, dan format lebih matematis.

Data IlyasFamily dapat diserialisasi dalam tiga notasi: JSON-like, S-expression, dan Tree. Ketiganya ekuivalen secara semantik.

## 2. Universal Tipe
### 2.1 Atom
Nilai dasar yang tidak dapat dipecah lagi.\
Contoh tipe atom
- Angka -> `42` dan `3.14`.
- String -> `"halo dunia"`.
- Boolean -> `true` dan `false`.
- Null/none -> `null`.

#### Atom Tambahan
- `date` -> `@date("YYYY-MM-DD")`.
- `datetime` -> `@datetime("YYYY-MM-DDTHH:MM:SSZ")`.
- `binary` -> `@binary("<Base64 encoded data>")`.
- `uuid` -> `@uuid("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx")`.

### 2.2 List
Koleksi berurutan. Ditulis dengan tanda `[...]`.
- Bisa kosong -> `[]`.
- Bisa berisi atom, record, atau list lain -> `[1, 2, [3, 4], {"a": 5}]`\
  `[1, 2, 3]`, `["a", "b", {"x": 1}]`, `[1, 2, 3, "empat"]`

### 2.3 Record
Koleksi pasangan kunci -> nilai. Ditulis dengan `{...}`.\
Kunci umumnya adalah string.
- Kuncinya biasanya string (bisa diperluas ke tipe lain).
- Nilai bisa atom, list, atau record lain.\
  `{ "nama": "Budi", "umur": 21 }`.

Contoh:
```ifamily
{
  "nama": "Budi",
  "umur": 21,
  "hobi": ["coding", "menulis"]
}
```

### Sintaks
- String menggunakan tanda kutip ganda `"..."`.
- Angka ditulis dalam format desimal.
- Boolean: `true` dan `false`.
- Null: `null`.

## 3. Tipe Data Struktural Baru
### 3.1 Set -> `@set([...])`.
Koleksi unik tanpa urutan.
```ifamily
@set([1, 2, 3, 4])
```

### 3.2 Map -> `@map([[key, value], ...])`.
Mirip record tapi kunci bisa tipe apapun (bukan string).
```ifamily
@map([
  [1, "satu"],
  [@uuid("550e8400-e29b-414d-a71-446655440000"), "unik"]
])
```

### 3.3 Tuple -> `@tuple([...])`.
- Mirip list tapi panjangnya tetap (fixed length) atau bermakna "data berpasangan".
- Bisa dipakai untuk koordinat, pasangan nilai, dll.
```ifamily
@tuple([1, 2])
```

### 3.4 Graph -> `@graph({ "nodes": [...], "edges": [...] })`.\
Struktur lebih kompleks: ada `nodes` dan `edges`.\
Struktur simpuldan sisi.

- Directed vs Undirected\
  Tambahkan flag directed: `true` atau `false`.
  ```ifamily
  @graph({
   "directed": true,
   "nodes": ["A", "B", "C"],
   "edges": [["A", "B"], ["B", "C"]]
  })
  ```
- Weighted Edges\
  Edge bisa berupa record dengan `source`, `target`, dan `weight`.
  ```ifamily
  @graph({
    "nodes": ["A", "B", "C"],
    "edges": [
      {"from": "A", "to": "B", "weight": 5},
      {"from": "B", "to": "C", "weight": 3}
    ]
  })
  ```
- Node Properties\
Node bisa berupa record juga, bukan sekadar string.
```ifamily
  @graph({
  "nodes": [
    {"id": "A", "label": "Start"},
    {"id": "B", "label": "End"}
  ],
  "edges": [
    {"from": "A", "to": "B", "type": "transition"}
  ]
})
```

## 4. Format Matematis (Alternatif Serialisasi)
### 4.1 S-Ekspression
Asal dari lisp (1950-an) bentuk representasi universal yang sangat sederhana.\
List rekursif (semua list).\
`(add 1 2)`
  
Dalam konteks IlyasFamily, S-Expresssion bisa dipakai untuk representasi ringkas:
```ifamily
(graph
 (nodes "A" "B" "C")
 (edges ("A" "B") ("B" "C")))
```

### 4.2 Tree/Node-Based Format
Intinya semua data dianggap pohon (tree).
Pohon dengan label dan anak.\
```
<person>
<name>Budi</name>
</person>
```
  
Representasi eksplisit IlyasFamily style:
```ifamily
@node("Person", {
  "Name": "Budi",
  "Age": 21,
  "Address": @node("Address", {
    "city": "Bandung",
    "code": 40123
  })
})
```

## Contoh Dokumen
### 5.1 Universal
```ifamily
{
  "nama": "Budi",
  "umur": 21,
  "lahir": @date("2000-01-01"),
  "aktif": true,
  "nilai": [90, 85, 88],
  "id": @uuid("550e8400-e29b-41d4-a716-446655440000"),
  "hobi": ["coding", "menulis"],
  "alamat": {"kota": "Bandung", "kode": 40123}
}
```

### 5.2 Struktural Baru
```ifamily
{
  "angka_unik": @set([1, 2, 2, 3]),
  "kamus": @map([
    ["x", 10],
    ["y", 20]
  ]),
  "koordinat": @tuple([-6.2, 106.8]),
  "graf": @graph({
    "directed": false,
    "nodes": ["A", "B", "C"],
    "edges": [["A", "B"], ["B", "C"]]
  })
}
```

### 5.3 Format Matematis
- S-Expression
  ```ifamily
  (person 
    (nama "Nazwa") 
    (umur 21) 
    (hobi ("coding" "menulis")))
  ```
- Tree/Node
  ```ifamily
  @node("Person", {
    "Name": "Budi",
    "Age": 21,
    "Address": @node("Address", {
      "city": "Bandung",
      "code": 40123
    })
  })
  ```

## Ekstensi File
`.ifamily`
