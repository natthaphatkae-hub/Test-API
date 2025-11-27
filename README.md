# Tax Calculator API

REST API สำหรับคำนวณภาษีเงินได้บุคคลธรรมดา

# เทคโนโลยีที่ใช้
Javascript
Node js. + Express
ทดสอบโดยใช้ Postman
# วิธีการติดตั้ง
npm init -y
npm install express
# วิธีการรัน
```bash
node index.js
```

API จะรันที่ `http://localhost:3000`

# API Endpoints
# POST /tax/calculations

# คำนวณภาษีพื้นฐาน

**Request Body:**
```json
{
  "totalIncome": 750000,
  "wht": 0,
  "allowances": [
    {
      "allowanceType": "donation",
      "amount": 0
    }
  ]
}
```

**Response:**
```json
{
  "tax": 63500
}
```

# คำนวณภาษีพร้อมรายละเอียดแต่ละขั้น
**Request Body:**
```json
{
  "totalIncome": 750000,
  "wht": 0,
  "allowances": [
    {
      "allowanceType": "donation",
      "amount": 0
    }
  ]
}
```

**Response:**
```json
{
    "tax": 63500,
    "taxLevel": [
        {
            "level": "0-150,000",
            "tax": 0
        },
        {
            "level": "150,001-500,000",
            "tax": 35000
        },
        {
            "level": "500,001-1,000,000",
            "tax": 28500
        },
        {
            "level": "1,000,001-2,000,000",
            "tax": 0
        },
        {
            "level": "2,000,001 ขึ้นไป",
            "tax": 0
        }
    ]
}
```

# คำนวณภาษีพร้อม WHT
**Request Body:**
```json
{
  "totalIncome": 600000,
  "wht": 15000,
  "allowances": [
    {
      "allowanceType": "donation",
      "amount": 0
    }
  ]
}
```

**Response:**
```json
{
  "tax": 26000
}
```
# รองรับค่าลดหย่อน (Bonus)
**Request Body:**
```json
{
  "totalIncome": 850000,
  "wht": 0,
  "allowances": [
    {
      "allowanceType": "donation",
      "amount": 150000
    }
  ]
}
```
**Response:**
```json
{
  "tax": 56000
}
```
# Test Cases

# Test Case 1: ไม่ต้องเสียภาษี
**Request Body:**
```json
{
  "totalIncome": 60000,
  "wht": 0,
  "allowances": []
}
```
**Response:**
```json
{
  "tax": 0
}
```
# Test Case 2: คำนวณภาษีขั้นเดียว
**Request Body:**
```json
{
  "totalIncome": 350000,
  "wht": 0,
  "allowances": []
}
```
**Response:**
```json
{
  "tax": 14000
}
```
# Test Case 3: คำนวณภาษีหลายขั้น
**Request Body:**
```json
{
  "totalIncome": 1200000,
  "wht": 0,
  "allowances": []
}
```
**Response:**
```json
{
  "tax": 144000
}
```
# Test Case 4: มี WHT
**Request Body:**
```json
{
  "totalIncome": 450000,
  "wht": 8000,
  "allowances": []
}
```
**Response:**
```json
{
  "tax": 15000
}
```
# Test Case 5: มีค่าลดหย่อน
**Request Body:**
```json
{
  "totalIncome": 700000,
  "wht": 0,
  "allowances": [
    {
      "allowanceType": "donation",
      "amount": 120000
    }
  ]
}
```
**Response:**
```json
{
  "tax": 43000
}
```
# Test Case 6: Validation - totalIncome ติดลบ
**Request Body:**
```json
{
  "totalIncome": -250000,
  "wht": 0,
  "allowances": []
}
```
**Response:**
```json
{
  "error": "totalIncome must be a positive number"
}
```
# Test Case 7: Validation - wht มากกว่า totalIncome
**Request Body:**
```json
{
  "totalIncome": 300000,
  "wht": 400000,
  "allowances": []
}
```
**Response:**
```json
{
  "error": "wht cannot be greater than totalIncome"
}
```

# ตัวอย่างการใช้งาน


# Test Case 1: ไม่ต้องเสียภาษี
```bash
curl -X POST http://localhost:5000/tax/calculations \
  -H "Content-Type: application/json" \
  -d '{
  "totalIncome": 60000,
  "wht": 0,
  "allowances": []
}
```
# Test Case 2: คำนวณภาษีขั้นเดียว
```bash
curl -X POST http://localhost:5000/tax/calculations \
  -H "Content-Type: application/json" \
  -d '{
  "totalIncome": 350000,
  "wht": 0,
  "allowances": []
}
```
# Test Case 3: คำนวณภาษีหลายขั้น
```bash
curl -X POST http://localhost:5000/tax/calculations \
  -H "Content-Type: application/json" \
  -d '{
  "totalIncome": 1200000,
  "wht": 0,
  "allowances": []
}
```
# Test Case 4: มี WHT
```bash
curl -X POST http://localhost:5000/tax/calculations \
  -H "Content-Type: application/json" \
  -d '{
  "totalIncome": 450000,
  "wht": 8000,
  "allowances": []
}
```
# Test Case 5: มีค่าลดหย่อน
```bash
curl -X POST http://localhost:5000/tax/calculations \
  -H "Content-Type: application/json" \
  -d '{
  "totalIncome": 700000,
  "wht": 0,
  "allowances": [
    {
      "allowanceType": "donation",
      "amount": 120000
    }
  ]
}
```
# Test Case 6: Validation - totalIncome ติดลบ
```bash
curl -X POST http://localhost:5000/tax/calculations \
  -H "Content-Type: application/json" \
  -d '{
  "totalIncome": -250000,
  "wht": 0,
  "allowances": []
}
```
# Test Case 7: Validation - wht มากกว่า totalIncome
```bash
curl -X POST http://localhost:5000/tax/calculations \
  -H "Content-Type: application/json" \
  -d '{
  "totalIncome": 300000,
  "wht": 400000,
  "allowances": []
}
```

# การทดสอบ

# Test Case 1: ไม่ต้องเสียภาษี
```bash
{
    "tax": 0
}
```
# Test Case 2: คำนวณภาษีขั้นเดียว
```bash
{
    "tax": 14000
}
```
# Test Case 3: คำนวณภาษีหลายขั้น
```bash
{
    "tax": 138000
}
```
# Test Case 4: มี WHT
```bash
{
    "tax": 16000
}
```
# Test Case 5: มีค่าลดหย่อน
```bash
{
    "tax": 41000
}
```
# Test Case 6: Validation - totalIncome ติดลบ
```bash
{
    "error": "Invalid totalIncome"
}
```
# Test Case 7: Validation - wht มากกว่า totalIncome
```bash
{
    "tax": 0
}
```
.
