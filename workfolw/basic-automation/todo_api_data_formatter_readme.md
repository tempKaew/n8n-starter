# Todo API Data Formatter Workflow

## 📋 ภาพรวม

Workflow นี้เป็นตัวอย่างการดึงข้อมูล Todo จาก JSONPlaceholder API และจัดรูปแบบข้อมูลใหม่ให้อยู่ในรูปแบบที่ต้องการ เหมาะสำหรับการเรียนรู้การใช้งาน HTTP Request และ Data Transformation ใน N8N

## 🎯 วัตถุประสงค์

- ดึงข้อมูล Todo item จาก REST API
- แปลงข้อมูลให้อยู่ในรูปแบบที่กำหนด
- เปลี่ยนชื่อ field และปรับโครงสร้างข้อมูล

## 🔧 โครงสร้าง Workflow

![Todo API Data Formatter Flow](/images/todo_api_data_formatter-flow.jpg)

### Node ที่ใช้งาน:

1. **Manual Trigger** - เริ่มต้น workflow ด้วยการคลิก
2. **HTTP Request** - ดึงข้อมูลจาก API
3. **Edit Fields** - จัดรูปแบบข้อมูลใหม่

## ⚙️ การตั้งค่าแต่ละ Node

### 1. Manual Trigger
```
Type: Manual Trigger
Description: เริ่มต้นการทำงานเมื่อคลิก 'Execute workflow'
```

### 2. HTTP Request
```
Method: GET
URL: https://jsonplaceholder.typicode.com/todos/1
Headers: Default
Authentication: None
Response Format: JSON
```

**ข้อมูลที่ได้รับ:**
```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

### 3. Edit Fields
```javascript
{
  "todoId": {{ $json.id }},
  "todoTitle": "{{ $json.title }}",
  "todoStatus": {{ $json.completed }}
}
```

**ผลลัพธ์ที่ได้:**
```json
{
  "todoId": 1,
  "todoTitle": "delectus aut autem",
  "todoStatus": false
}
```

## 🚀 วิธีการใช้งาน

### การ Import Workflow

1. ดาวน์โหลดไฟล์ `todo-api-data-formatter.json`
2. เข้าไปที่ N8N Web Interface
3. คลิก **"+"** → **"Import from file"**
4. เลือกไฟล์ที่ดาวน์โหลด
5. คลิก **"Import"**

### การทดสอบ Workflow

1. คลิกที่ปุ่ม **"Execute workflow"**
2. ตรวจสอบผลลัพธ์ที่แต่ละ Node
3. ดูข้อมูลที่ได้รับจาก HTTP Request
4. ตรวจสอบข้อมูลที่ถูกแปลงใน Edit Fields

## 🔍 การทำงานแบบละเอียด

### Step 1: เริ่มต้น Workflow
- คลิก "Execute workflow" เพื่อเริ่มการทำงาน
- Manual Trigger จะส่งสัญญาณไปยัง Node ต่อไป

### Step 2: ดึงข้อมูลจาก API
- HTTP Request Node จะเรียก GET API ไปยัง JSONPlaceholder
- ได้รับข้อมูล Todo item ID = 1
- ข้อมูลจะถูกส่งต่อไปยัง Edit Fields Node

### Step 3: แปลงข้อมูล
- Edit Fields Node จะจับข้อมูลจาก HTTP Request
- แปลง `id` → `todoId`
- แปลง `title` → `todoTitle` 
- แปลง `completed` → `todoStatus`
- ส่งออกข้อมูลในรูปแบบใหม่

## 📊 ตัวอย่างการใช้งานจริง

Workflow นี้สามารถปรับใช้กับกรณีต่างๆ เช่น:

- **Data Migration** - แปลงข้อมูลจากระบบเก่าไปใหม่
- **API Integration** - เชื่อมต่อกับ API ภายนอก
- **Data Normalization** - จัดรูปแบบข้อมูลให้เป็นมาตรฐาน
- **Reporting** - เตรียมข้อมูลสำหรับรายงาน

## 🛠️ การปรับแต่ง

### เปลี่ยน Todo ID
แก้ไข URL ใน HTTP Request Node:
```
https://jsonplaceholder.typicode.com/todos/2
https://jsonplaceholder.typicode.com/todos/3
```

### เพิ่ม Field อื่นๆ
ใน Edit Fields Node สามารถเพิ่ม field ได้:
```javascript
{
  "todoId": {{ $json.id }},
  "todoTitle": "{{ $json.title }}",
  "todoStatus": {{ $json.completed }},
  "userId": {{ $json.userId }},
  "createdAt": "{{ new Date().toISOString() }}"
}
```

### ดึง Todo หลายรายการ
เปลี่ยน URL เป็น:
```
https://jsonplaceholder.typicode.com/todos
```

## 🔧 Troubleshooting

### ปัญหาที่อาจพบ

**1. HTTP Request ไม่ทำงาน**
- ตรวจสอบการเชื่อมต่ออินเทอร์เน็ต
- ลอง Manual Test ใน HTTP Request Node

**2. Edit Fields แสดงข้อผิดพลาด**
- ตรวจสอบ syntax ใน expression
- ใช้ `{{ $json.fieldname }}` แทน `$json.fieldname`

**3. ไม่มีข้อมูลส่งออก**
- ตรวจสอบว่า HTTP Request ได้รับข้อมูลแล้ว
- ดู raw data ใน execution log

## 📚 แหล่งข้อมูลเพิ่มเติม

- [JSONPlaceholder API Documentation](https://jsonplaceholder.typicode.com/)
- [N8N HTTP Request Node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/)
- [N8N Edit Fields Node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.set/)
- [N8N Expressions](https://docs.n8n.io/code-examples/expressions/)
