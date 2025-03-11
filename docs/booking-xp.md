# การพัฒนาระบบจองห้องพักออนไลน์ตามแนวทาง Extreme Programming (XP)

การพัฒนาระบบจองห้องพักออนไลน์ตามแนวทางของ Extreme Programming (XP) จะเน้นการพัฒนาซอฟต์แวร์ที่มีคุณภาพสูง โดยเน้นหลักการสำคัญ 5 ประการ ได้แก่ การสื่อสาร (Communication), ความเรียบง่าย (Simplicity), ข้อเสนอแนะ (Feedback), ความกล้า (Courage) และการเคารพ (Respect) โดยมีขั้นตอนการพัฒนาดังต่อไปนี้

## ขั้นตอนการดำเนินการพัฒนาตามแนวทาง Extreme Programming

### 1. การเตรียมความพร้อมองค์กร

1. **จัดตั้งทีม XP**
   - ทีมพัฒนา (แบ่งเป็นคู่ Programming Pairs)
   - ลูกค้า/ผู้แทนลูกค้า (On-site Customer)
   - ผู้เชี่ยวชาญด้านธุรกิจ (Business Expert)
   - XP Coach (ผู้ดูแลการดำเนินการตามแนวทาง XP)

2. **จัดเตรียมสภาพแวดล้อมการทำงาน**
   - จัดพื้นที่ทำงานร่วมกัน (Co-located workspace)
   - ตั้งค่าเครื่องมือพัฒนาและระบบ CI/CD
   - เตรียมอุปกรณ์สำหรับการทำ Pair Programming

3. **กำหนดกฎระเบียบและมาตรฐานในการพัฒนา**
   - กำหนดมาตรฐานการเขียนโค้ด (Coding Standards)
   - กำหนดแนวทางการเขียนและดำเนินการทดสอบ (Test-Driven Development)
   - กำหนดความถี่ในการส่งมอบงาน (Continuous Integration)

### 2. การวางแผนโปรเจค (Release Planning)

1. **จัดทำ User Stories**
   - ลูกค้า/ผู้แทนลูกค้าเขียน User Stories ที่อธิบายความต้องการระบบ
   - เขียนเป็นภาษาธรรมชาติที่เข้าใจง่าย

2. **ประเมินระยะเวลาและทรัพยากร**
   - ทีมพัฒนาประเมินแต่ละ User Story (1-3 ideal engineering weeks)
   - ลูกค้าเรียงลำดับความสำคัญของ User Stories

3. **จัดทำ Release Plan**
   - วางแผนการส่งมอบซอฟต์แวร์เป็นงวด (1-3 เดือนต่องวด)
   - กำหนดเป้าหมายแต่ละงวดตาม Business Value

### 3. วงรอบการพัฒนา (Iteration Cycle)

1. **Iteration Planning**
   - วงรอบการพัฒนา 1-2 สัปดาห์
   - ลูกค้าเลือก User Stories ที่ต้องการให้พัฒนาในวงรอบนี้
   - ทีมพัฒนาแบ่ง User Stories เป็น Engineering Tasks
   - แต่ละคู่ Programming เลือก Tasks ที่จะรับผิดชอบ

2. **การพัฒนาตาม XP Practices**
   - **Pair Programming**: พัฒนาโค้ดเป็นคู่ (หนึ่งคนเขียนโค้ด อีกคนตรวจสอบ) และมีการสลับบทบาทกัน
   - **Test-Driven Development (TDD)**: เขียนทดสอบก่อนเขียนโค้ด ตามลำดับ:
     - เขียนทดสอบที่ล้มเหลว (Red)
     - เขียนโค้ดที่ทำให้ทดสอบผ่าน (Green)
     - ปรับปรุงโค้ดให้ดีขึ้น (Refactor)
   - **Continuous Integration**: รวมโค้ดทุกวัน หรือบ่อยกว่านั้น
   - **Small Releases**: ส่งมอบเวอร์ชันเล็กๆ ที่ใช้งานได้จริงอย่างต่อเนื่อง
   - **Simple Design**: ออกแบบให้เรียบง่ายที่สุดที่สามารถทำงานได้
   - **Refactoring**: ปรับปรุงโค้ดอย่างต่อเนื่องโดยไม่เปลี่ยนฟังก์ชันการทำงาน

3. **การสื่อสารและการร่วมมือ**
   - **Stand-up Meeting**: ประชุมสั้นๆ ทุกวันเพื่อรายงานความคืบหน้า
   - **On-site Customer**: ลูกค้าอยู่กับทีมพัฒนาเพื่อตอบคำถามและให้ข้อเสนอแนะทันที
   - **Sustainable Pace**: ทำงานในอัตราที่สามารถรักษาคุณภาพได้ในระยะยาว (สัปดาห์ละประมาณ 40 ชั่วโมง)

### 4. การทดสอบ

1. **Unit Testing**
   - เขียนทดสอบหน่วยสำหรับทุกส่วนของโค้ด
   - อัตโนมัติและรันทดสอบทุกครั้งที่มีการเปลี่ยนแปลงโค้ด

2. **Acceptance Testing**
   - ลูกค้าเขียน Acceptance Tests เพื่อยืนยันว่า User Stories ตรงตามความต้องการ
   - อัตโนมัติเท่าที่เป็นไปได้

3. **Continuous Testing**
   - รันทดสอบทั้งหมดหลังการเปลี่ยนแปลงโค้ดทุกครั้ง

### 5. การส่งมอบและรับข้อเสนอแนะ

1. **การสาธิตแบบสม่ำเสมอ**
   - แสดงความคืบหน้าให้ลูกค้าเห็นสัปดาห์ละ 1-2 ครั้ง
   - รับข้อเสนอแนะและปรับปรุงทันที

2. **การปรับแผนงาน**
   - ปรับแผน Iteration และ Release ตามข้อเสนอแนะและความต้องการที่เปลี่ยนแปลง
   - เน้นความยืดหยุ่นและการตอบสนองต่อการเปลี่ยนแปลง

## ตัวอย่างการนำ XP มาใช้ในโครงการระบบจองห้องพักออนไลน์

### ตัวอย่าง User Stories

```
User Story 1: ในฐานะผู้ใช้ทั่วไป ฉันต้องการลงทะเบียนเป็นสมาชิกด้วยข้อมูลส่วนตัว เพื่อใช้งานระบบจองห้องพัก
Acceptance Tests:
- ระบบต้องสามารถรับข้อมูล: ชื่อ, อีเมล, รหัสผ่าน, เบอร์โทรศัพท์
- ระบบต้องตรวจสอบความถูกต้องของอีเมลและเบอร์โทรศัพท์
- ระบบต้องส่งอีเมลยืนยันตัวตน
- ระบบต้องไม่อนุญาตให้ลงทะเบียนด้วยอีเมลที่มีอยู่แล้ว
```

### ตัวอย่าง Iteration Plan (1 สัปดาห์)

**User Stories ที่จะพัฒนา:**
1. ลงทะเบียนสมาชิก
2. เข้าสู่ระบบ
3. ค้นหาห้องพัก (เฉพาะการค้นหาตามวันที่และจำนวนผู้เข้าพัก)

**Engineering Tasks:**
- ออกแบบและสร้างฐานข้อมูลผู้ใช้ (คู่ A)
- พัฒนา API การลงทะเบียนพร้อมทดสอบ (คู่ A)
- พัฒนา UI การลงทะเบียนพร้อมทดสอบ (คู่ B)
- พัฒนาระบบส่งอีเมลยืนยันตัวตนพร้อมทดสอบ (คู่ A)
- พัฒนา API การเข้าสู่ระบบพร้อมทดสอบ (คู่ B)
- พัฒนา UI การเข้าสู่ระบบพร้อมทดสอบ (คู่ B)
- ออกแบบและสร้างฐานข้อมูลห้องพัก (คู่ C)
- พัฒนา API ค้นหาห้องพักพร้อมทดสอบ (คู่ C)
- พัฒนา UI ค้นหาห้องพักพร้อมทดสอบ (คู่ D)

### ตัวอย่างการดำเนินการพัฒนาตาม TDD

**ขั้นตอน TDD สำหรับการพัฒนา API ลงทะเบียน:**

1. **เขียนทดสอบล้มเหลว (Red):**
```javascript
test('should register a new user with valid data', async () => {
  const userData = {
    name: 'John Doe',
    email: 'john@example.com',
    password: 'securePass123',
    phone: '0812345678'
  };
  
  const response = await api.post('/users/register', userData);
  
  expect(response.status).toBe(201);
  expect(response.body).toHaveProperty('id');
  expect(response.body.email).toBe(userData.email);
});
```

2. **เขียนโค้ดให้ผ่าน (Green):**
```javascript
app.post('/users/register', async (req, res) => {
  const { name, email, password, phone } = req.body;
  
  const user = await User.create({
    name,
    email,
    password: await bcrypt.hash(password, 10),
    phone
  });
  
  sendVerificationEmail(user.email);
  
  res.status(201).json({
    id: user.id,
    name: user.name,
    email: user.email
  });
});
```

3. **ปรับปรุงโค้ด (Refactor):**
```javascript
app.post('/users/register', async (req, res) => {
  try {
    const { name, email, password, phone } = req.body;
    
    // Validation
    if (!isValidEmail(email)) {
      return res.status(400).json({ error: 'Invalid email format' });
    }
    
    if (!isValidPhone(phone)) {
      return res.status(400).json({ error: 'Invalid phone number' });
    }
    
    // Check if email already exists
    const existingUser = await User.findOne({ where: { email } });
    if (existingUser) {
      return res.status(409).json({ error: 'Email already registered' });
    }
    
    // Create user
    const user = await User.create({
      name,
      email,
      password: await bcrypt.hash(password, 10),
      phone
    });
    
    // Send verification email
    await sendVerificationEmail(user.email);
    
    // Return response
    res.status(201).json({
      id: user.id,
      name: user.name,
      email: user.email
    });
  } catch (error) {
    res.status(500).json({ error: 'Registration failed' });
  }
});
```

### ตัวอย่างการทำ Pair Programming

**คู่ A: สมชาย (Driver) และสมศรี (Navigator)**

1. **สมชาย (Driver)**: เขียนโค้ดตามการแนะนำของสมศรี
2. **สมศรี (Navigator)**: 
   - ตรวจสอบโค้ดที่สมชายเขียน
   - แนะนำวิธีการเขียนที่ดีกว่า
   - มองภาพรวมและคิดล่วงหน้า
   - ตรวจสอบข้อผิดพลาดที่อาจเกิดขึ้น

3. **การสลับบทบาท**: ทุกๆ 30 นาที สมชายและสมศรีจะสลับบทบาทกัน

### ตัวอย่างการทำ Refactoring

**ก่อน Refactoring:**
```javascript
function calculateRoomPrice(roomId, checkInDate, checkOutDate, guests) {
  const room = getRoomById(roomId);
  const basePrice = room.basePrice;
  const nights = getDaysBetween(checkInDate, checkOutDate);
  let totalPrice = basePrice * nights;
  
  // Add guest charge
  if (guests > room.standardOccupancy) {
    const extraGuests = guests - room.standardOccupancy;
    totalPrice += extraGuests * room.extraPersonFee * nights;
  }
  
  // Add seasonal rate
  if (isHighSeason(checkInDate, checkOutDate)) {
    totalPrice *= 1.2;
  }
  
  return totalPrice;
}
```

**หลัง Refactoring:**
```javascript
function calculateRoomPrice(roomId, checkInDate, checkOutDate, guests) {
  const room = getRoomById(roomId);
  const nights = getDaysBetween(checkInDate, checkOutDate);
  
  const baseAmount = calculateBaseAmount(room, nights);
  const extraGuestAmount = calculateExtraGuestAmount(room, guests, nights);
  const seasonalMultiplier = getSeasonalMultiplier(checkInDate, checkOutDate);
  
  return (baseAmount + extraGuestAmount) * seasonalMultiplier;
}

function calculateBaseAmount(room, nights) {
  return room.basePrice * nights;
}

function calculateExtraGuestAmount(room, guests, nights) {
  if (guests <= room.standardOccupancy) {
    return 0;
  }
  
  const extraGuests = guests - room.standardOccupancy;
  return extraGuests * room.extraPersonFee * nights;
}

function getSeasonalMultiplier(checkInDate, checkOutDate) {
  return isHighSeason(checkInDate, checkOutDate) ? 1.2 : 1.0;
}
```

## ข้อแตกต่างที่สำคัญระหว่าง XP กับ Scrum

1. **การพัฒนาเป็นคู่ (Pair Programming)**: XP เน้นการพัฒนาเป็นคู่ ในขณะที่ Scrum ไม่ได้กำหนดวิธีการพัฒนาโค้ด

2. **Test-Driven Development (TDD)**: XP กำหนดให้ต้องเขียนทดสอบก่อนเขียนโค้ด แต่ Scrum ไม่ได้กำหนดวิธีการทดสอบ

3. **Continuous Integration**: XP เน้นการรวมโค้ดบ่อยๆ (หลายครั้งต่อวัน) ในขณะที่ Scrum ไม่ได้กำหนดความถี่

4. **On-site Customer**: XP ต้องการให้ลูกค้าอยู่กับทีมพัฒนา ในขณะที่ Scrum มี Product Owner เป็นตัวแทนลูกค้า

5. **การปรับปรุงโค้ด (Refactoring)**: XP เน้นการปรับปรุงโค้ดอย่างต่อเนื่อง ในขณะที่ Scrum ไม่ได้กำหนดเรื่องนี้

การนำ Extreme Programming มาใช้ในการพัฒนาระบบจองห้องพักออนไลน์จะช่วยให้ได้ซอฟต์แวร์ที่มีคุณภาพสูง มีข้อบกพร่องน้อย และสามารถตอบสนองความต้องการของลูกค้าได้อย่างแท้จริง โดยเฉพาะอย่างยิ่งในกรณีที่ความต้องการอาจเปลี่ยนแปลงได้ตลอดเวลา
