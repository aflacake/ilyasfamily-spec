# Spesifikasi IlyasFamily
Versi: 0.2

## Tujuan
IlyasFamily adalah format pertukaran dan penyimpanan data universal berbasis tiga tipe data utama, tambahan tipe data struktural baru, dan format lebih matematis.

## Tipe Data
- Atom\
  Contoh tipe atom
  - Angka -> `42` dan `3.14`.
  - String -> `"halo dunia"`.
  - Boolean -> `true` dan `false`.
  - Null/none -> `null`.

  ### Atom Tambahan
  - `date` -> `@date("YYYY-MM-DD")`.
  - `datetime` -> `@datetime("YYYY-MM-DDTHH:MM:SSZ")`.
  - `binary` -> `@binary("<Base64 encoded data>")`.
  - `uuid` -> `@uuid("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx")`.
- List\
  Koleksi berurutan. Ditulis dengan tanda `[...]`.
  - Bisa kosong -> `[]`.
  - Bisa berisi atom, record, atau list lain -> `[1, 2, [3, 4], {"a": 5}]`\
  `[1, 2, 3]`, `["a", "b", {"x": 1}]`
- Record\
  Koleksi pasangan kunci -> nilai. Ditulis dengan `{...}`.
  - Kuncinya biasanya string (bisa diperluas ke tipe lain).
  - Nilai bisa atom, list, atau record lain.\
  `{ "nama": "Nazwa", "umur": 21 }`.

## Tipe Data Struktural Baru
- Set -> `@set([...])`.\
  Koleksi unik tanpa urutan.
- Map -> `@map([[key, value], ...])`.\
  Mirip record tapi kunci bisa tipe apapun (bukan string).
- Tuple -> `@tuple([...])`.
  - Mirip list tapi panjangnya tetap (fixed length) atau bermakna "data berpasangan".
  - Bisa dipakai untuk koordinat, pasangan nilai, dll.
- Graph -> `@graph({ "nodes": [...], "edges": [...] })`.\
  Struktur lebih kompleks: ada `nodes` dan `edges`.

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

## Sintaks
- String menggunakan tanda kutip ganda `"..."`.
- Angka ditulis dalam format desimal.
- Boolean: `true` dan `false`.
- Null: `null`.

## Contoh Dokumen
```ifamily
{
  "nama": "Budi",
  "umur": 21,
  "hobi": ["coding", "menulis"],
  "alamat": {"kota": "Bandung", "kode": 40123}
}
```

## Ekstensi File
`.ifamily`
