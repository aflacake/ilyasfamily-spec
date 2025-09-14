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

## Tipe Data Struktural Baru
- Set -> `@set([...])`.
- Map -> `@map([[key, value], ...])`.
- Tuple -> `@tuple([...])`.
- Graph -> `@graph({ "nodes": [...], "edges": [...] })`.

## Ekstensi File
`.ifamily`
