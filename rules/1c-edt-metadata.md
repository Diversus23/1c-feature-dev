## 1C EDT Metadata Rules

Rules for working with 1C metadata files in EDT format (`.mdo`, `.form`, and other XML files 1C:EDT).

### UUIDs in Metadata Objects

When creating or adding **any** metadata elements to `.mdo` files — **always generate cryptographically random UUID v4 values**.

This applies to all XML elements containing a `uuid=` attribute:
- `<attributes uuid="...">` — object attributes
- `<tabularSections uuid="...">` — tabular sections
- `<forms uuid="...">` — forms
- `<commands uuid="...">` — commands
- `<templates uuid="...">` — templates
- Any other elements with `uuid=`

This also applies to `typeId` and `valueTypeId` inside `<producedTypes>`.

**Never** use:
- Template/placeholder UUIDs like `a1b2c3d4-...`, `00000000-...`, `11111111-...`
- Identical UUIDs for different elements
- Sequential or predictable UUIDs

```xml
<!-- WRONG: placeholder UUID -->
<attributes uuid="a1b2c3d4-e5f6-7890-abcd-ef1234567890">

<!-- CORRECT: random UUID v4 -->
<attributes uuid="e8faf55f-f1ec-4203-af79-cf9aa977b457">
```

### UUID Generation

To generate a random UUID v4, use PowerShell:

```powershell
[guid]::NewGuid().ToString()
```

Or generate multiple at once:

```powershell
1..5 | ForEach-Object { [guid]::NewGuid().ToString() }
```

### Attribute Structure

When adding a new attribute to an `.mdo` file, follow the standard structure:

```xml
<attributes uuid="<random-uuid>">
  <name>AttributeName</name>
  <synonym>
    <key>ru</key>
    <value>Attribute display name</value>
  </synonym>
  <type>
    <types>DataType</types>
  </type>
  <minValue xsi:type="core:UndefinedValue"/>
  <maxValue xsi:type="core:UndefinedValue"/>
  <fillValue xsi:type="core:UndefinedValue"/>
  <fullTextSearch>Use</fullTextSearch>
  <dataHistory>Use</dataHistory>
</attributes>
```

### Element Ordering

When adding new elements — insert them **after** existing elements of the same kind (attributes after attributes, tabularSections after tabularSections, etc.), preserving the section order within the `.mdo` file.
