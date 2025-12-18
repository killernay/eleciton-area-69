# Thailand Election Areas 2569 (2026)

ข้อมูลเขตเลือกตั้ง ส.ส. แบบแบ่งเขต ประจำปี พ.ศ. 2569 ในรูปแบบ JSON

## ที่มาของข้อมูล

ข้อมูลนี้ถูกสกัดมาจากประกาศคณะกรรมการการเลือกตั้ง (กกต.) ดังนี้:

- ประกาศ กกต. เรื่อง จำนวนสมาชิกสภาผู้แทนราษฎรแบบแบ่งเขตเลือกตั้ง จำนวนเขตเลือกตั้ง และท้องที่ที่ประกอบเป็นเขตเลือกตั้ง
- ประกาศ กกต. เรื่อง จำนวนสมาชิกสภาผู้แทนราษฎรแบบแบ่งเขตเลือกตั้งที่แต่ละจังหวัดจะพึงมี และจำนวนเขตเลือกตั้งแบบแบ่งเขตเลือกตั้งของแต่ละจังหวัด

## ข้อจำกัดและคำเตือน

> **สำคัญ:** ไฟล์ PDF ต้นฉบับจาก กกต. มีปัญหาด้าน encoding ทำให้ไม่สามารถคัดลอกข้อความได้โดยตรง ข้อมูลในไฟล์นี้ถูกสกัดออกมาโดยใช้เทคโนโลยี **OCR (Optical Character Recognition)** ซึ่งอาจมีข้อผิดพลาดในการอ่านตัวอักษร
>
> **ผู้จัดทำไม่รับผิดชอบต่อความผิดพลาดใดๆ ที่อาจเกิดขึ้นจากกระบวนการ OCR** กรุณาตรวจสอบกับเอกสารต้นฉบับจาก กกต. หากต้องการความถูกต้องสมบูรณ์

## โครงสร้างข้อมูล

```json
{
  "version": "2569",
  "type": "mp_constituency",
  "description": "เขตเลือกตั้ง ส.ส. แบบแบ่งเขต พ.ศ. 2569 พร้อมข้อมูลอำเภอและตำบล",
  "total_areas": 400,
  "total_provinces": 77,
  "generated_at": "2024-12-18",
  "provinces": [...]
}
```

### รายละเอียด Fields

| Field | Type | Description |
|-------|------|-------------|
| `version` | string | ปี พ.ศ. ของข้อมูลเขตเลือกตั้ง |
| `type` | string | ประเภทข้อมูล (`mp_constituency` = เขตเลือกตั้ง ส.ส. แบบแบ่งเขต) |
| `description` | string | คำอธิบายชุดข้อมูล |
| `total_areas` | number | จำนวนเขตเลือกตั้งทั้งหมด (400 เขต) |
| `total_provinces` | number | จำนวนจังหวัดทั้งหมด (77 จังหวัด) |
| `generated_at` | string | วันที่สร้างข้อมูล |
| `provinces` | array | รายการจังหวัดทั้งหมด |

### โครงสร้าง Province

```json
{
  "name": "กรุงเทพมหานคร",
  "region": "central",
  "total_areas": 33,
  "areas": [...]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | ชื่อจังหวัด |
| `region` | string | ภาค (`central`, `north`, `northeast`, `south`, `east`, `west`) |
| `total_areas` | number | จำนวนเขตเลือกตั้งในจังหวัด |
| `areas` | array | รายการเขตเลือกตั้ง |

### โครงสร้าง Area (เขตเลือกตั้ง)

```json
{
  "area": 1,
  "name": "กรุงเทพมหานคร เขต 1",
  "districts": [...]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `area` | number | หมายเลขเขตเลือกตั้ง |
| `name` | string | ชื่อเขตเลือกตั้ง |
| `districts` | array | รายการอำเภอ/เขต ที่อยู่ในเขตเลือกตั้งนี้ |

### โครงสร้าง District (อำเภอ/เขต)

```json
{
  "name": "เขตพระนคร",
  "subdistricts": ["พระบรมมหาราชวัง", "วังบูรพาภิรมย์", ...]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | ชื่ออำเภอ (ต่างจังหวัด) หรือ ชื่อเขต (กทม.) |
| `subdistricts` | array | รายชื่อตำบล (ต่างจังหวัด) หรือ แขวง (กทม.) |

## การใช้งาน

### JavaScript / Node.js

```javascript
// อ่านไฟล์
const data = require('./election-areas-2569.json');

// หาเขตเลือกตั้งของจังหวัด
const bangkok = data.provinces.find(p => p.name === 'กรุงเทพมหานคร');
console.log(`กรุงเทพมหานครมี ${bangkok.total_areas} เขตเลือกตั้ง`);

// หาว่าตำบล/แขวง อยู่เขตเลือกตั้งไหน
function findElectionArea(provinceName, subdistrictName) {
  const province = data.provinces.find(p => p.name === provinceName);
  if (!province) return null;

  for (const area of province.areas) {
    for (const district of area.districts) {
      if (district.subdistricts.includes(subdistrictName)) {
        return {
          province: province.name,
          area: area.area,
          areaName: area.name,
          district: district.name
        };
      }
    }
  }
  return null;
}

console.log(findElectionArea('กรุงเทพมหานคร', 'สีลม'));
// Output: { province: 'กรุงเทพมหานคร', area: 1, areaName: 'กรุงเทพมหานคร เขต 1', district: 'เขตบางรัก' }
```

### Python

```python
import json

# อ่านไฟล์
with open('election-areas-2569.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

# หาเขตเลือกตั้งของจังหวัด
bangkok = next((p for p in data['provinces'] if p['name'] == 'กรุงเทพมหานคร'), None)
print(f"กรุงเทพมหานครมี {bangkok['total_areas']} เขตเลือกตั้ง")

# หาว่าตำบล/แขวง อยู่เขตเลือกตั้งไหน
def find_election_area(province_name, subdistrict_name):
    province = next((p for p in data['provinces'] if p['name'] == province_name), None)
    if not province:
        return None

    for area in province['areas']:
        for district in area['districts']:
            if subdistrict_name in district['subdistricts']:
                return {
                    'province': province['name'],
                    'area': area['area'],
                    'area_name': area['name'],
                    'district': district['name']
                }
    return None

print(find_election_area('กรุงเทพมหานคร', 'สีลม'))
# Output: {'province': 'กรุงเทพมหานคร', 'area': 1, 'area_name': 'กรุงเทพมหานคร เขต 1', 'district': 'เขตบางรัก'}
```

### cURL / Fetch (ถ้า host บน GitHub)

```bash
curl -s https://raw.githubusercontent.com/<username>/<repo>/main/election-areas-2569.json | jq '.provinces[0]'
```

```javascript
fetch('https://raw.githubusercontent.com/<username>/<repo>/main/election-areas-2569.json')
  .then(res => res.json())
  .then(data => console.log(data.total_areas));
```

## สถิติข้อมูล

- **จำนวนจังหวัด:** 77 จังหวัด
- **จำนวนเขตเลือกตั้งทั้งหมด:** 400 เขต
- **ปีที่ใช้:** พ.ศ. 2569

## License

ข้อมูลนี้เป็นข้อมูลสาธารณะจากคณะกรรมการการเลือกตั้ง (กกต.) เผยแพร่เพื่อประโยชน์สาธารณะ

## ข้อมูลที่ตรวจพบว่าอาจขาดหาย

จากการตรวจสอบเบื้องต้นกับฐานข้อมูลตำบลครั้งก่อน พบว่ามี **16 ตำบลที่ไม่ปรากฏในเขตเลือกตั้งใดเลย** ซึ่งอาจเกิดจากข้อผิดพลาดในกระบวนการ OCR หรือการตีความข้อมูล:

| จังหวัด | อำเภอ | ตำบล |
|---------|-------|------|
| นครพนม | เมืองนครพนม | ในเมือง |
| นครพนม | เมืองนครพนม | หนองแสง |
| นครศรีธรรมราช | เมืองนครศรีธรรมราช | คลัง |
| นครศรีธรรมราช | เมืองนครศรีธรรมราช | ท่าวัง |
| นครศรีธรรมราช | เมืองนครศรีธรรมราช | ในเมือง |
| นครศรีธรรมราช | ร่อนพิบูลย์ | ควนเกย |
| ปทุมธานี | คลองหลวง | คลองสอง |
| ปทุมธานี | คลองหลวง | คลองหนึ่ง |
| ปทุมธานี | หนองเสือ | นพรัตน์ |
| ปทุมธานี | หนองเสือ | บึงกาสาม |
| ปทุมธานี | หนองเสือ | บึงบา |
| ปทุมธานี | หนองเสือ | ศาลาครุ |
| ปทุมธานี | หนองเสือ | หนองสามวัง |
| ร้อยเอ็ด | ธวัชบุรี | มะอึ |
| สุรินทร์ | ศีขรภูมิ | กุดหวาย |
| สุรินทร์ | ศีขรภูมิ | ผักไหม |

> หากท่านพบข้อมูลที่ขาดหายหรือผิดพลาด สามารถแจ้งหรือส่ง Pull Request เพื่อแก้ไขได้

## Disclaimer

ข้อมูลนี้จัดทำขึ้นโดยการใช้ OCR สกัดจากไฟล์ PDF ของ กกต. ซึ่งมีปัญหาด้าน encoding ทำให้ไม่สามารถคัดลอกข้อความได้โดยตรง

**ผู้จัดทำไม่รับประกันความถูกต้องสมบูรณ์ของข้อมูล และไม่รับผิดชอบต่อความเสียหายใดๆ ที่อาจเกิดขึ้นจากการใช้ข้อมูลนี้** หากต้องการข้อมูลที่ถูกต้องสมบูรณ์ กรุณาตรวจสอบกับเอกสารต้นฉบับจากคณะกรรมการการเลือกตั้ง

---

Made with OCR from ECT Thailand PDFs
