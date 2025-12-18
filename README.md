# 📱 คู่มือสร้าง Student Management App แบบครบถ้วน
## เหมาะสำหรับพิมพ์และปฏิบัติตาม

---

# 🎯 ภาพรวม

แอพพลิเคชันจัดการข้อมูลนักเรียน ประกอบด้วย:
- **1 ไฟล์หลัก:** App.js
- **3 Components:** Header, StudentCard, SummaryBox  
- **4 Stylesheets:** AppStyles, HeaderStyles, StudentCardStyles, SummaryBoxStyles

**รวมทั้งหมด 8 ไฟล์**

---

# 📋 ขั้นตอนที่ 1: ติดตั้งและสร้างโปรเจค

## 1.1 ติดตั้ง Expo CLI

เปิด Terminal หรือ Command Prompt พิมพ์:

```bash
npm i -g expo
```

กด Enter และรอจนติดตั้งเสร็จ

---

## 1.2 สร้างโปรเจค

```bash
npx create-expo-app --template blank student-app
```

กด Enter และรอจนสร้างเสร็จ (อาจใช้เวลา 2-3 นาที)

---

## 1.3 เข้าโฟลเดอร์โปรเจค

```bash
cd student-app
```

---

# 📝 ขั้นตอนที่ 2: สร้างและเขียนโค้ดทุกไฟล์

---

## ไฟล์ที่ 1/8: App.js

**ตำแหน่ง:** `student-app/App.js`

**คำแนะนำ:** 
- เปิดไฟล์ App.js ที่มีอยู่แล้ว
- **ลบโค้ดเดิมทั้งหมด**
- พิมพ์โค้ดด้านล่างนี้แทน

```javascript
import React, { useState } from 'react';
import { View, ScrollView } from 'react-native';
import { StatusBar } from 'expo-status-bar';
import Header from './components/Header';
import StudentCard from './components/StudentCard';
import SummaryBox from './components/SummaryBox';
import styles from './styles/AppStyles';

export default function App() {
  // ข้อมูลนักเรียน
  const [students, setStudents] = useState([
    { id: 1, name: 'สมชาย ใจดี', grade: 'ม.3', gpa: 3.75, status: 'active' },
    { id: 2, name: 'สมหญิง รักเรียน', grade: 'ม.3', gpa: 3.95, status: 'active' },
    { id: 3, name: 'วิชัย ขยัน', grade: 'ม.3', gpa: 3.50, status: 'inactive' },
    { id: 4, name: 'ประภา เก่ง', grade: 'ม.3', gpa: 4.00, status: 'active' },
  ]);

  // ฟังก์ชันสลับสถานะนักเรียน
  const toggleStatus = (id) => {
    setStudents(students.map(student => 
      student.id === id 
        ? { ...student, status: student.status === 'active' ? 'inactive' : 'active' }
        : student
    ));
  };

  // คำนวณสถิติ
  const activeStudents = students.filter(s => s.status === 'active').length;
  const totalStudents = students.length;
  const averageGPA = (
    students
      .filter(s => s.status === 'active')
      .reduce((sum, s) => sum + s.gpa, 0) / activeStudents
  ).toFixed(2);

  return (
    <View style={styles.container}>
      <StatusBar style="light" />
      
      <Header title="ระบบจัดการนักเรียน" />
      
      <SummaryBox 
        activeStudents={activeStudents}
        totalStudents={totalStudents}
        averageGPA={averageGPA}
      />
      
      <ScrollView style={styles.scrollView}>
        {students.map(student => (
          <StudentCard 
            key={student.id}
            student={student}
            onToggle={() => toggleStatus(student.id)}
          />
        ))}
      </ScrollView>
    </View>
  );
}
```

**✅ บันทึกไฟล์ (Ctrl+S หรือ Cmd+S)**

---

## ไฟล์ที่ 2/8: Header.js

**ตำแหน่ง:** `student-app/components/Header.js`

**คำแนะนำ:** 
- สร้างไฟล์ใหม่ชื่อ `Header.js` ในโฟลเดอร์ `components`
- พิมพ์โค้ดด้านล่างนี้

```javascript
import React from 'react';
import { View, Text } from 'react-native';
import styles from '../styles/HeaderStyles';

const Header = ({ title }) => {
  return (
    <View style={styles.header}>
      <Text style={styles.headerText}>{title}</Text>
    </View>
  );
};

export default Header;
```

**✅ บันทึกไฟล์**

---

## ไฟล์ที่ 3/8: StudentCard.js

**ตำแหน่ง:** `student-app/components/StudentCard.js`

**คำแนะนำ:** 
- สร้างไฟล์ใหม่ชื่อ `StudentCard.js` ในโฟลเดอร์ `components`
- พิมพ์โค้ดด้านล่างนี้

```javascript
import React from 'react';
import { View, Text, TouchableOpacity } from 'react-native';
import styles from '../styles/StudentCardStyles';

const StudentCard = ({ student, onToggle }) => {
  const isActive = student.status === 'active';
  
  return (
    <View style={[
      styles.card, 
      !isActive && styles.cardInactive
    ]}>
      <View style={styles.infoContainer}>
        <Text style={[
          styles.name,
          !isActive && styles.textInactive
        ]}>
          {student.name}
        </Text>
        <Text style={[
          styles.details,
          !isActive && styles.textInactive
        ]}>
          ชั้น: {student.grade} | GPA: {student.gpa.toFixed(2)}
        </Text>
      </View>
      
      <TouchableOpacity 
        style={[
          styles.statusButton,
          isActive ? styles.statusButtonActive : styles.statusButtonInactive
        ]}
        onPress={onToggle}
      >
        <Text style={styles.statusButtonText}>
          {isActive ? 'เรียนอยู่' : 'หยุดเรียน'}
        </Text>
      </TouchableOpacity>
    </View>
  );
};

export default StudentCard;
```

**✅ บันทึกไฟล์**

---

## ไฟล์ที่ 4/8: SummaryBox.js

**ตำแหน่ง:** `student-app/components/SummaryBox.js`

**คำแนะนำ:** 
- สร้างไฟล์ใหม่ชื่อ `SummaryBox.js` ในโฟลเดอร์ `components`
- พิมพ์โค้ดด้านล่างนี้

```javascript
import React from 'react';
import { View, Text } from 'react-native';
import styles from '../styles/SummaryBoxStyles';

const SummaryBox = ({ activeStudents, totalStudents, averageGPA }) => {
  return (
    <View style={styles.summaryContainer}>
      <Text style={styles.summaryTitle}>สรุปข้อมูล</Text>
      
      <View style={styles.statsRow}>
        <View style={styles.statBox}>
          <Text style={styles.statNumber}>{activeStudents}</Text>
          <Text style={styles.statLabel}>กำลังเรียน</Text>
        </View>
        
        <View style={styles.statBox}>
          <Text style={styles.statNumber}>{totalStudents}</Text>
          <Text style={styles.statLabel}>ทั้งหมด</Text>
        </View>
        
        <View style={styles.statBox}>
          <Text style={styles.statNumber}>{averageGPA}</Text>
          <Text style={styles.statLabel}>GPA เฉลี่ย</Text>
        </View>
      </View>
    </View>
  );
};

export default SummaryBox;
```

**✅ บันทึกไฟล์**

---

## ไฟล์ที่ 5/8: AppStyles.js

**ตำแหน่ง:** `student-app/styles/AppStyles.js`

**คำแนะนำ:** 
- สร้างไฟล์ใหม่ชื่อ `AppStyles.js` ในโฟลเดอร์ `styles`
- พิมพ์โค้ดด้านล่างนี้

```javascript
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  scrollView: {
    flex: 1,
    paddingHorizontal: 16,
    paddingBottom: 20,
  },
});

export default styles;
```

**✅ บันทึกไฟล์**

---

## ไฟล์ที่ 6/8: HeaderStyles.js

**ตำแหน่ง:** `student-app/styles/HeaderStyles.js`

**คำแนะนำ:** 
- สร้างไฟล์ใหม่ชื่อ `HeaderStyles.js` ในโฟลเดอร์ `styles`
- พิมพ์โค้ดด้านล่างนี้

```javascript
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  header: {
    backgroundColor: '#2196F3',
    paddingTop: 50,
    paddingBottom: 20,
    paddingHorizontal: 16,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5,
  },
  headerText: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#ffffff',
    textAlign: 'center',
  },
});

export default styles;
```

**✅ บันทึกไฟล์**

---

## ไฟล์ที่ 7/8: StudentCardStyles.js

**ตำแหน่ง:** `student-app/styles/StudentCardStyles.js`

**คำแนะนำ:** 
- สร้างไฟล์ใหม่ชื่อ `StudentCardStyles.js` ในโฟลเดอร์ `styles`
- พิมพ์โค้ดด้านล่างนี้

```javascript
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  card: {
    backgroundColor: '#ffffff',
    borderRadius: 12,
    padding: 16,
    marginTop: 12,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  cardInactive: {
    backgroundColor: '#f0f0f0',
    opacity: 0.7,
  },
  infoContainer: {
    flex: 1,
  },
  name: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 4,
  },
  details: {
    fontSize: 14,
    color: '#666',
  },
  textInactive: {
    color: '#999',
  },
  statusButton: {
    paddingHorizontal: 16,
    paddingVertical: 8,
    borderRadius: 20,
    minWidth: 90,
    alignItems: 'center',
  },
  statusButtonActive: {
    backgroundColor: '#4CAF50',
  },
  statusButtonInactive: {
    backgroundColor: '#FF5722',
  },
  statusButtonText: {
    color: '#ffffff',
    fontSize: 14,
    fontWeight: '600',
  },
});

export default styles;
```

**✅ บันทึกไฟล์**

---

## ไฟล์ที่ 8/8: SummaryBoxStyles.js

**ตำแหน่ง:** `student-app/styles/SummaryBoxStyles.js`

**คำแนะนำ:** 
- สร้างไฟล์ใหม่ชื่อ `SummaryBoxStyles.js` ในโฟลเดอร์ `styles`
- พิมพ์โค้ดด้านล่างนี้

```javascript
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  summaryContainer: {
    backgroundColor: '#ffffff',
    margin: 16,
    marginBottom: 8,
    borderRadius: 12,
    padding: 16,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  summaryTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 12,
  },
  statsRow: {
    flexDirection: 'row',
    justifyContent: 'space-around',
  },
  statBox: {
    alignItems: 'center',
    flex: 1,
  },
  statNumber: {
    fontSize: 28,
    fontWeight: 'bold',
    color: '#2196F3',
    marginBottom: 4,
  },
  statLabel: {
    fontSize: 12,
    color: '#666',
    textAlign: 'center',
  },
});

export default styles;
```

**✅ บันทึกไฟล์**

---

# ✅ ขั้นตอนที่ 3: ตรวจสอบโครงสร้างไฟล์

ตรวจสอบว่าโครงสร้างโฟลเดอร์ของคุณเป็นแบบนี้:

```
student-app/
│
├── App.js                          ✅
│
├── components/
│   ├── Header.js                   ✅
│   ├── StudentCard.js              ✅
│   └── SummaryBox.js               ✅
│
├── styles/
│   ├── AppStyles.js                ✅
│   ├── HeaderStyles.js             ✅
│   ├── StudentCardStyles.js        ✅
│   └── SummaryBoxStyles.js         ✅
│
└── (ไฟล์อื่นๆ ที่ Expo สร้างให้)
```

**ต้องมีครบ 8 ไฟล์**

---

# 🚀 ขั้นตอนที่ 4: รันแอพ

## 4.1 เปิด Terminal ในโฟลเดอร์ student-app

ตรวจสอบว่าคุณอยู่ในโฟลเดอร์ `student-app`:

```bash
pwd
# ต้องแสดง: .../student-app
```

---

## 4.2 รันแอพ

พิมพ์คำสั่ง:

```bash
npm start
```

หรือ

```bash
npx expo start
```

---

## 4.3 รอจนแอพโหลดเสร็จ

คุณจะเห็น:
- QR Code บนหน้าจอ Terminal
- หรือเปิด Browser ที่ http://localhost:19006

---

# 📱 ขั้นตอนที่ 5: ทดสอบบนมือถือ

## 5.1 ติดตั้งแอพ Expo Go

**iOS:**
- เปิด App Store
- ค้นหา "Expo Go"
- กด "ดาวน์โหลด"

**Android:**
- เปิด Google Play Store
- ค้นหา "Expo Go"
- กด "ติดตั้ง"

---

## 5.2 สแกน QR Code

**iOS:**
- เปิดกล้อง
- ชี้ไปที่ QR Code
- กดแจ้งเตือนที่ปรากฏ

**Android:**
- เปิดแอพ Expo Go
- กด "Scan QR Code"
- สแกน QR Code จากหน้าจอ Terminal

---

## 5.3 รอแอพโหลด

- รอ 10-30 วินาที
- แอพจะปรากฏบนมือถือ

---

# 🎨 ผลลัพธ์ที่ได้

แอพจะแสดง:

1. **Header (สีน้ำเงิน)**
   - ข้อความ "ระบบจัดการนักเรียน"

2. **Summary Box (พื้นขาว)**
   - จำนวนนักเรียนกำลังเรียน: 3
   - จำนวนนักเรียนทั้งหมด: 4
   - GPA เฉลี่ย: 3.73

3. **รายการนักเรียน (4 การ์ด)**
   - สมชาย ใจดี - [เรียนอยู่] (ปุ่มเขียว)
   - สมหญิง รักเรียน - [เรียนอยู่] (ปุ่มเขียว)
   - วิชัย ขยัน - [หยุดเรียน] (ปุ่มแดง)
   - ประภา เก่ง - [เรียนอยู่] (ปุ่มเขียว)

---

# 🎯 ทดสอบการทำงาน

## ทดสอบ 1: กดปุ่มสถานะ

1. กดปุ่ม "เรียนอยู่" ของนักเรียนคนใดก็ได้
2. ปุ่มจะเปลี่ยนเป็น "หยุดเรียน" (สีแดง)
3. การ์ดจะเป็นสีเทาและโปร่งใส
4. GPA เฉลี่ยจะอัพเดทอัตโนมัติ

## ทดสอบ 2: กดปุ่มสถานะอีกครั้ง

1. กดปุ่ม "หยุดเรียน"
2. ปุ่มจะเปลี่ยนกลับเป็น "เรียนอยู่" (สีเขียว)
3. การ์ดกลับเป็นสีขาว
4. GPA เฉลี่ยจะอัพเดทอัตโนมัติ

---

# 🐛 การแก้ปัญหา

## ปัญหา 1: แอพไม่โหลด

**วิธีแก้:**
```bash
npx expo start -c
```

---

## ปัญหา 2: Cannot find module

**สาเหตุ:** พิมพ์ชื่อไฟล์ผิด หรือไฟล์ไม่อยู่ในตำแหน่งที่ถูกต้อง

**ตรวจสอบ:**
1. ชื่อไฟล์ต้องตรงทุกตัวอักษร (case-sensitive)
2. ไฟล์ต้องอยู่ในโฟลเดอร์ที่ถูกต้อง
3. ตรวจสอบ import path ใน App.js

---

## ปัญหา 3: Syntax Error

**สาเหตุ:** พิมพ์โค้ดผิด

**ตรวจสอบ:**
1. วงเล็บ `{ }` ต้องครบคู่
2. วงเล็บ `( )` ต้องครบคู่
3. ไม่ลืม `;` และ `,`
4. เครื่องหมาย `'` และ `"` ต้องครบคู่

---

## ปัญหา 4: QR Code ไม่ปรากฏ

**วิธีแก้:**
```bash
# หยุดแอพ (กด Ctrl+C)
# รันใหม่
npm start
```

---

## ปัญหา 5: Port already in use

**วิธีแก้:**
```bash
npx expo start --port 19001
```

---

# 📋 Checklist สุดท้าย

ก่อนรันแอพ ตรวจสอบ:

- [ ] ติดตั้ง Expo แล้ว (`npm i -g expo`)
- [ ] สร้างโปรเจค (`npx create-expo-app --template blank student-app`)
- [ ] อยู่ในโฟลเดอร์ student-app (`cd student-app`)
- [ ] สร้างโฟลเดอร์ components และ styles แล้ว
- [ ] สร้างไฟล์ครบ 8 ไฟล์:
  - [ ] App.js
  - [ ] components/Header.js
  - [ ] components/StudentCard.js
  - [ ] components/SummaryBox.js
  - [ ] styles/AppStyles.js
  - [ ] styles/HeaderStyles.js
  - [ ] styles/StudentCardStyles.js
  - [ ] styles/SummaryBoxStyles.js
- [ ] พิมพ์โค้ดครบทุกไฟล์
- [ ] บันทึกไฟล์ทุกไฟล์แล้ว
- [ ] ติดตั้ง Expo Go บนมือถือแล้ว

**ถ้าครบทุกข้อ พร้อมรันแล้ว!**

```bash
npm start
```

---

# 🎓 คำแนะนำเพิ่มเติม

## เคล็ดลับการพิมพ์โค้ด

1. **พิมพ์ทีละไฟล์** - อย่าเร่งรีบ
2. **ตรวจสอบทีละบรรทัด** - ก่อนพิมพ์บรรทัดถัดไป
3. **บันทึกบ่อยๆ** - กด Ctrl+S หรือ Cmd+S
4. **ทดสอบระหว่างทาง** - ลองรันดูหลังพิมพ์เสร็จ 4-5 ไฟล์

## สิ่งที่ต้องระวัง

1. **ตัวพิมพ์ใหญ่-เล็ก** - JavaScript แยกตัวพิมพ์ใหญ่เล็ก
2. **เครื่องหมาย** - วงเล็บ, เซมิโคลอน, จุลภาค
3. **Path ของไฟล์** - `./components/Header` ไม่ใช่ `./component/Header`
4. **ชื่อไฟล์** - ต้องตรงกับที่ import

---

# 🎉 สำเร็จแล้ว!

ขอแสดงความยินดี! คุณได้สร้างแอพ React Native แอพแรกสำเร็จแล้ว

## ขั้นตอนถัดไป

1. ทดลองเล่นกับแอพ - กดปุ่มต่างๆ
2. ลองแก้ไขข้อมูล - เปลี่ยนชื่อนักเรียน
3. ลองเปลี่ยนสี - แก้ไขใน styles/
4. ทำแบบฝึกหัด - พัฒนาแอพต่อ

---

# 📞 ต้องการความช่วยเหลือ?

หากพบปัญหา:
1. อ่านส่วน "การแก้ปัญหา" อีกครั้ง
2. ตรวจสอบ Checklist
3. ดู Error message ใน Terminal
4. ถามครูหรือเพื่อน

---

**ขอให้สนุกกับการเขียนโค้ด!** 💻✨

พิมพ์เอกสารนี้ไว้ใช้งานได้เลยครับ 📄🖨️
