# CRC และ Refactoring Mercilessly ใน Extreme Programming (XP)

## CRC (Class-Responsibility-Collaboration) ใน XP

CRC เป็นเทคนิคการออกแบบซอฟต์แวร์เชิงวัตถุที่ใช้ในกระบวนการ Extreme Programming (XP) โดยใช้บัตร CRC เพื่อออกแบบและส่งเสริมการสื่อสารในทีม

### ความหมายของ CRC:
- **Class**: คลาสหรือออบเจ็กต์ในระบบ
- **Responsibility**: หน้าที่รับผิดชอบของคลาสนั้นๆ
- **Collaboration**: คลาสอื่นๆ ที่คลาสนี้ต้องทำงานร่วมด้วย

### วิธีการใช้ CRC:
1. **สร้างบัตร CRC**: บัตรกระดาษหรือดิจิทัลที่แสดงชื่อคลาส หน้าที่รับผิดชอบ และความร่วมมือกับคลาสอื่น
2. **ค้นหาคลาส**: จาก User Stories หาว่ามีคลาสหรือออบเจ็กต์อะไรบ้าง
3. **กำหนดหน้าที่**: ระบุหน้าที่รับผิดชอบของแต่ละคลาส
4. **ระบุความร่วมมือ**: ระบุว่าคลาสต้องทำงานร่วมกับคลาสใดบ้าง
5. **ปรับปรุง**: ปรับปรุงการออกแบบโดยการเคลื่อนย้าย จัดกลุ่ม หรือปรับเปลี่ยนบัตร

### ตัวอย่างบัตร CRC ในระบบจองห้องพัก:

**ชื่อคลาส: Booking (การจอง)**

**หน้าที่รับผิดชอบ:**
- เก็บข้อมูลการจอง (วันที่เช็คอิน-เช็คเอาท์, จำนวนผู้เข้าพัก)
- คำนวณค่าใช้จ่ายทั้งหมด
- จัดการสถานะการจอง (ยืนยัน, ยกเลิก, เสร็จสิ้น)

**ความร่วมมือกับ:**
- Room (ห้องพัก)
- Guest (ผู้เข้าพัก)
- Payment (การชำระเงิน)

### ประโยชน์ของ CRC:
- **เข้าใจง่าย**: ช่วยให้ทั้งนักพัฒนาและผู้ไม่ใช่นักเทคนิคเข้าใจการออกแบบได้
- **ส่งเสริมการสื่อสาร**: ทีมสามารถร่วมกันออกแบบและอภิปรายเกี่ยวกับโครงสร้างระบบ
- **ยืดหยุ่น**: ง่ายต่อการปรับเปลี่ยนเมื่อความต้องการเปลี่ยนไป
- **ลดความซับซ้อน**: ช่วยให้ออกแบบโครงสร้างที่ชัดเจนและไม่ซับซ้อนเกินไป

## Refactoring Mercilessly (การปรับปรุงโค้ดอย่างไม่ปรานี)

Refactoring Mercilessly เป็นแนวปฏิบัติใน XP ที่เน้นการปรับปรุงโค้ดอย่างต่อเนื่องและอย่างจริงจัง โดยไม่ลังเลที่จะเปลี่ยนแปลงโค้ดเพื่อทำให้มันดีขึ้น

### ความหมายและหลักการ:
- **การปรับโครงสร้าง**: เปลี่ยนแปลงโครงสร้างภายในของโค้ดโดยไม่เปลี่ยนพฤติกรรมภายนอก
- **ทำอย่างต่อเนื่อง**: ปรับปรุงโค้ดเป็นประจำ ไม่รอให้เกิดปัญหาใหญ่
- **ไม่ปรานี**: กล้าที่จะปรับเปลี่ยนแม้ว่าโค้ดนั้นจะทำงานได้อยู่แล้ว
- **มุ่งเน้นคุณภาพ**: ปรับปรุงอย่างไม่ยอมลดมาตรฐาน

### สถานการณ์ที่ต้อง Refactor:
1. **โค้ดซ้ำซ้อน**: พบส่วนโค้ดที่ทำงานคล้ายกัน
2. **คลาสหรือเมธอดใหญ่เกินไป**: มีความรับผิดชอบมากเกินไป
3. **ชื่อไม่สื่อความหมาย**: ตัวแปร เมธอด หรือคลาสมีชื่อที่สับสน
4. **ความซับซ้อนที่ไม่จำเป็น**: โครงสร้างซับซ้อนเกินความจำเป็น
5. **Feature Envy**: เมธอดสนใจข้อมูลของคลาสอื่นมากกว่าข้อมูลของตัวเอง

### ตัวอย่างการ Refactor ในระบบจองห้องพัก:

**ก่อน Refactor:**
```java
public class Booking {
    // ... fields ...
    
    public double calculateTotalPrice() {
        double total = 0;
        // คำนวณราคาห้องพัก
        total += room.getPricePerNight() * numberOfNights;
        
        // คำนวณค่าบริการเสริม
        if (hasBreakfast) {
            total += breakfastPrice * numberOfNights * numberOfGuests;
        }
        if (hasAirportTransfer) {
            total += airportTransferPrice;
        }
        
        // คำนวณส่วนลด
        if (numberOfNights > 7) {
            total = total * 0.9; // ส่วนลด 10% สำหรับการพักมากกว่า 7 คืน
        }
        if (isLowSeason) {
            total = total * 0.85; // ส่วนลด 15% ในช่วง Low Season
        }
        
        return total;
    }
}
```

**หลัง Refactor:**
```java
public class Booking {
    // ... fields ...
    
    public double calculateTotalPrice() {
        double basePrice = calculateBasePrice();
        double additionalServicesPrice = calculateAdditionalServicesPrice();
        double totalBeforeDiscount = basePrice + additionalServicesPrice;
        double discount = calculateDiscount(totalBeforeDiscount);
        
        return totalBeforeDiscount - discount;
    }
    
    private double calculateBasePrice() {
        return room.getPricePerNight() * numberOfNights;
    }
    
    private double calculateAdditionalServicesPrice() {
        double price = 0;
        if (hasBreakfast) {
            price += breakfastPrice * numberOfNights * numberOfGuests;
        }
        if (hasAirportTransfer) {
            price += airportTransferPrice;
        }
        return price;
    }
    
    private double calculateDiscount(double total) {
        double discount = 0;
        
        if (numberOfNights > 7) {
            discount += total * 0.1; // ส่วนลด 10% สำหรับการพักมากกว่า 7 คืน
        }
        if (isLowSeason) {
            discount += total * 0.15; // ส่วนลด 15% ในช่วง Low Season
        }
        
        return discount;
    }
}
```

### ประโยชน์ของ Refactoring Mercilessly:
- **โค้ดอ่านง่ายขึ้น**: ทำให้โค้ดอ่านและเข้าใจง่ายขึ้น
- **ลดความซับซ้อน**: แยกความรับผิดชอบให้ชัดเจน
- **ง่ายต่อการบำรุงรักษา**: เปลี่ยนแปลงได้ง่ายในอนาคต
- **ลดข้อผิดพลาด**: ลดโอกาสเกิดข้อผิดพลาดจากโค้ดที่ซับซ้อน
- **ปรับปรุงประสิทธิภาพ**: บางครั้งการ Refactor ทำให้โค้ดทำงานได้เร็วขึ้น

CRC และ Refactoring Mercilessly เป็นแนวปฏิบัติที่สำคัญใน XP ที่ช่วยให้ทีมสามารถพัฒนาซอฟต์แวร์ที่มีคุณภาพสูง อ่านง่าย และบำรุงรักษาได้ง่าย การผสมผสานทั้งสองแนวคิดช่วยให้การพัฒนาเป็นไปอย่างมีประสิทธิภาพและยั่งยืน