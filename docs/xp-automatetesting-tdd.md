# ตัวอย่างการทำ Automated Testing ด้วย TDD สำหรับระบบจองห้องพักออนไลน์

การพัฒนาระบบจองห้องพักออนไลน์ด้วยแนวทาง Test-Driven Development (TDD) จะช่วยให้ระบบมีความถูกต้อง แข็งแกร่ง และทนทานต่อการเปลี่ยนแปลง ต่อไปนี้เป็นตัวอย่างการเขียนการทดสอบอัตโนมัติในระดับต่างๆ

## 1. Unit Tests (การทดสอบระดับหน่วย)

Unit tests เป็นการทดสอบส่วนเล็กที่สุดของโค้ด โดยทดสอบแยกจากส่วนอื่นๆ ของระบบ

### ตัวอย่าง Unit Test สำหรับคลาส Room (ห้องพัก)

```javascript
// ตัวอย่างในภาษา JavaScript ใช้ Jest เป็น Testing Framework

// กระบวนการ TDD: 1. เขียนทดสอบที่ล้มเหลว (Red)
describe('Room Class', () => {
  test('should calculate correct price for standard occupancy', () => {
    // Arrange
    const room = new Room({
      id: '101',
      type: 'Deluxe',
      basePrice: 1000,
      standardOccupancy: 2,
      maxOccupancy: 4,
      extraPersonFee: 300
    });
    
    // Act
    const price = room.calculatePrice(2, 3); // 2 คน, 3 คืน
    
    // Assert
    expect(price).toBe(3000); // 1000 บาท x 3 คืน
  });
  
  test('should add extra person fee when occupancy exceeds standard', () => {
    // Arrange
    const room = new Room({
      id: '101',
      type: 'Deluxe',
      basePrice: 1000,
      standardOccupancy: 2,
      maxOccupancy: 4,
      extraPersonFee: 300
    });
    
    // Act
    const price = room.calculatePrice(4, 2); // 4 คน, 2 คืน
    
    // Assert
    expect(price).toBe(3200); // (1000 + 300*2) บาท x 2 คืน
  });
  
  test('should throw error when occupancy exceeds maximum', () => {
    // Arrange
    const room = new Room({
      id: '101',
      type: 'Deluxe',
      basePrice: 1000,
      standardOccupancy: 2,
      maxOccupancy: 4,
      extraPersonFee: 300
    });
    
    // Act & Assert
    expect(() => {
      room.calculatePrice(5, 2);
    }).toThrow('Occupancy exceeds maximum allowed');
  });
});

// กระบวนการ TDD: 2. เขียนโค้ดให้ผ่านการทดสอบ (Green)
class Room {
  constructor({ id, type, basePrice, standardOccupancy, maxOccupancy, extraPersonFee }) {
    this.id = id;
    this.type = type;
    this.basePrice = basePrice;
    this.standardOccupancy = standardOccupancy;
    this.maxOccupancy = maxOccupancy;
    this.extraPersonFee = extraPersonFee;
  }
  
  calculatePrice(occupancy, nights) {
    if (occupancy > this.maxOccupancy) {
      throw new Error('Occupancy exceeds maximum allowed');
    }
    
    let price = this.basePrice;
    
    if (occupancy > this.standardOccupancy) {
      price += (occupancy - this.standardOccupancy) * this.extraPersonFee;
    }
    
    return price * nights;
  }
}

// กระบวนการ TDD: 3. ปรับปรุงโค้ด (Refactor)
// ปรับปรุงโค้ดให้อ่านง่ายและมีประสิทธิภาพขึ้น โดยการแยกฟังก์ชันย่อย
class Room {
  constructor({ id, type, basePrice, standardOccupancy, maxOccupancy, extraPersonFee }) {
    this.id = id;
    this.type = type;
    this.basePrice = basePrice;
    this.standardOccupancy = standardOccupancy;
    this.maxOccupancy = maxOccupancy;
    this.extraPersonFee = extraPersonFee;
  }
  
  calculatePrice(occupancy, nights) {
    this.validateOccupancy(occupancy);
    const nightlyPrice = this.calculateNightlyPrice(occupancy);
    return nightlyPrice * nights;
  }
  
  validateOccupancy(occupancy) {
    if (occupancy > this.maxOccupancy) {
      throw new Error('Occupancy exceeds maximum allowed');
    }
  }
  
  calculateNightlyPrice(occupancy) {
    let price = this.basePrice;
    if (occupancy > this.standardOccupancy) {
      price += this.calculateExtraPersonCharge(occupancy);
    }
    return price;
  }
  
  calculateExtraPersonCharge(occupancy) {
    return (occupancy - this.standardOccupancy) * this.extraPersonFee;
  }
}
```

### ตัวอย่าง Unit Test สำหรับ BookingService (บริการจองห้องพัก)

```javascript
describe('BookingService', () => {
  // Mock Dependencies
  const mockRoomRepository = {
    findAvailableRooms: jest.fn(),
    findById: jest.fn()
  };
  
  const mockBookingRepository = {
    save: jest.fn()
  };
  
  beforeEach(() => {
    // Clear mock calls between tests
    jest.clearAllMocks();
  });
  
  test('should create a booking successfully', async () => {
    // Arrange
    const bookingService = new BookingService(mockRoomRepository, mockBookingRepository);
    const bookingData = {
      roomId: '101',
      checkInDate: '2025-04-15',
      checkOutDate: '2025-04-18',
      guestName: 'John Doe',
      guestEmail: 'john@example.com',
      occupancy: 2
    };
    
    const mockRoom = new Room({
      id: '101',
      type: 'Deluxe',
      basePrice: 1000,
      standardOccupancy: 2,
      maxOccupancy: 4,
      extraPersonFee: 300
    });
    
    mockRoomRepository.findById.mockResolvedValue(mockRoom);
    mockRoomRepository.findAvailableRooms.mockResolvedValue([mockRoom]);
    mockBookingRepository.save.mockResolvedValue({ 
      id: 'booking123', 
      ...bookingData, 
      totalPrice: 3000 
    });
    
    // Act
    const result = await bookingService.createBooking(bookingData);
    
    // Assert
    expect(result).toHaveProperty('id', 'booking123');
    expect(result.totalPrice).toBe(3000);
    expect(mockRoomRepository.findAvailableRooms).toHaveBeenCalledWith(
      '2025-04-15', 
      '2025-04-18'
    );
    expect(mockBookingRepository.save).toHaveBeenCalled();
  });
  
  test('should throw error when room is not available', async () => {
    // Arrange
    const bookingService = new BookingService(mockRoomRepository, mockBookingRepository);
    const bookingData = {
      roomId: '101',
      checkInDate: '2025-04-15',
      checkOutDate: '2025-04-18',
      guestName: 'John Doe',
      guestEmail: 'john@example.com',
      occupancy: 2
    };
    
    mockRoomRepository.findById.mockResolvedValue({ id: '101' });
    mockRoomRepository.findAvailableRooms.mockResolvedValue([]);
    
    // Act & Assert
    await expect(bookingService.createBooking(bookingData))
      .rejects
      .toThrow('Room is not available for the selected dates');
    
    expect(mockBookingRepository.save).not.toHaveBeenCalled();
  });
});
```

## 2. Integration Tests (การทดสอบการทำงานร่วมกัน)

Integration tests ทดสอบการทำงานร่วมกันของหลาย module หรือ component

### ตัวอย่าง Integration Test สำหรับการจองห้องพัก

```javascript
// Integration test ระหว่าง BookingService, RoomRepository และ BookingRepository
describe('Booking Integration', () => {
  // ใช้ฐานข้อมูลจำลองหรือฐานข้อมูลทดสอบ
  let roomRepository;
  let bookingRepository;
  let bookingService;
  
  beforeAll(async () => {
    // เชื่อมต่อกับฐานข้อมูลทดสอบ
    await db.connect('mongodb://localhost:27017/hotel-test');
    
    // สร้าง repositories และ service จริง (ไม่ใช่ mock)
    roomRepository = new RoomRepository(db);
    bookingRepository = new BookingRepository(db);
    bookingService = new BookingService(roomRepository, bookingRepository);
    
    // เตรียมข้อมูลทดสอบ
    await roomRepository.saveMany([
      new Room({
        id: '101',
        type: 'Deluxe',
        basePrice: 1000,
        standardOccupancy: 2,
        maxOccupancy: 4,
        extraPersonFee: 300
      }),
      new Room({
        id: '102',
        type: 'Suite',
        basePrice: 2000,
        standardOccupancy: 2,
        maxOccupancy: 3,
        extraPersonFee: 500
      })
    ]);
  });
  
  afterAll(async () => {
    // ล้างข้อมูลทดสอบและตัดการเชื่อมต่อ
    await db.dropDatabase();
    await db.disconnect();
  });
  
  test('should book a room and update availability', async () => {
    // Arrange
    const bookingData = {
      roomId: '101',
      checkInDate: '2025-04-15',
      checkOutDate: '2025-04-18',
      guestName: 'John Doe',
      guestEmail: 'john@example.com',
      occupancy: 2
    };
    
    // Act
    const booking = await bookingService.createBooking(bookingData);
    
    // Assert
    expect(booking).toHaveProperty('id');
    expect(booking.totalPrice).toBe(3000);
    
    // ตรวจสอบว่าห้องไม่ว่างในช่วงเวลาที่จอง
    const availableRooms = await roomRepository.findAvailableRooms(
      '2025-04-16', 
      '2025-04-17'
    );
    expect(availableRooms.map(room => room.id)).not.toContain('101');
    
    // ตรวจสอบว่าสามารถค้นหาการจองได้
    const savedBooking = await bookingRepository.findById(booking.id);
    expect(savedBooking).toMatchObject(bookingData);
  });
  
  test('should not allow double booking', async () => {
    // Arrange
    const bookingData1 = {
      roomId: '102',
      checkInDate: '2025-05-01',
      checkOutDate: '2025-05-05',
      guestName: 'Alice Smith',
      guestEmail: 'alice@example.com',
      occupancy: 2
    };
    
    const bookingData2 = {
      roomId: '102',
      checkInDate: '2025-05-03',
      checkOutDate: '2025-05-06',
      guestName: 'Bob Brown',
      guestEmail: 'bob@example.com',
      occupancy: 2
    };
    
    // Act
    await bookingService.createBooking(bookingData1);
    
    // Assert
    await expect(bookingService.createBooking(bookingData2))
      .rejects
      .toThrow('Room is not available for the selected dates');
  });
});
```

## 3. Acceptance Tests (การทดสอบการยอมรับ)

Acceptance tests ทดสอบระบบจากมุมมองของผู้ใช้ ว่าตรงตามความต้องการหรือไม่

### ตัวอย่าง Acceptance Test สำหรับกระบวนการจองห้องพัก

```javascript
// ใช้ Cucumber.js สำหรับเขียน BDD-style acceptance tests
// features/booking.feature

Feature: Hotel Room Booking
  As a customer
  I want to book a hotel room
  So that I can have a place to stay during my trip

  Scenario: Customer successfully books an available room
    Given there is a "Deluxe" room available from "2025-06-10" to "2025-06-15"
    When I book this room for 2 guests
    And I provide my details
      | Name       | Email             | Phone        |
      | John Smith | john@example.com  | 081-234-5678 |
    And I confirm the booking
    Then I should receive a booking confirmation
    And the total amount should be correct
    And the room should no longer be available for those dates

  Scenario: Customer cannot book a room that is already booked
    Given a "Suite" room has been booked from "2025-07-01" to "2025-07-05"
    When I try to book the same room from "2025-07-03" to "2025-07-08"
    Then I should see an error message "Room is not available for the selected dates"
    And I should be offered alternative available rooms

  Scenario: Customer cancels a booking
    Given I have a confirmed booking for a "Deluxe" room
    When I request to cancel my booking
    Then I should receive a cancellation confirmation
    And the room should become available again
```

### ตัวอย่างการเขียน Step Definitions สำหรับ Acceptance Tests

```javascript
// features/step_definitions/booking_steps.js

const { Given, When, Then } = require('cucumber');
const assert = require('assert');
const axios = require('axios');

// API endpoint
const API_URL = 'http://localhost:8080/api';
let apiResponse;
let roomData;
let bookingData;

Given('there is a {string} room available from {string} to {string}', async function (roomType, checkIn, checkOut) {
  // ตรวจสอบว่ามีห้องว่างในช่วงเวลาที่ต้องการ
  const response = await axios.get(`${API_URL}/rooms/available`, {
    params: { checkIn, checkOut, roomType }
  });
  
  assert.strictEqual(response.status, 200);
  assert.ok(response.data.rooms.length > 0, 'No available rooms found');
  
  // เก็บข้อมูลห้องเพื่อใช้ในขั้นตอนต่อไป
  roomData = response.data.rooms[0];
});

When('I book this room for {int} guests', function (guestCount) {
  // เตรียมข้อมูลการจอง
  bookingData = {
    roomId: roomData.id,
    occupancy: guestCount
  };
});

When('I provide my details', function (dataTable) {
  // รับข้อมูลจาก table
  const guestInfo = dataTable.hashes()[0];
  
  // เพิ่มข้อมูลผู้เข้าพักในการจอง
  bookingData.guestName = guestInfo.Name;
  bookingData.guestEmail = guestInfo.Email;
  bookingData.guestPhone = guestInfo.Phone;
});

When('I confirm the booking', async function () {
  // ส่งคำขอจองห้องพัก
  try {
    apiResponse = await axios.post(`${API_URL}/bookings`, bookingData);
  } catch (error) {
    apiResponse = error.response;
  }
});

Then('I should receive a booking confirmation', function () {
  assert.strictEqual(apiResponse.status, 201);
  assert.ok(apiResponse.data.id, 'No booking ID received');
  assert.strictEqual(apiResponse.data.status, 'confirmed');
});

Then('the total amount should be correct', function () {
  const nights = calculateNights(bookingData.checkInDate, bookingData.checkOutDate);
  const expectedPrice = roomData.basePrice * nights;
  
  assert.strictEqual(apiResponse.data.totalPrice, expectedPrice);
});

Then('the room should no longer be available for those dates', async function () {
  const { checkInDate, checkOutDate } = bookingData;
  
  const response = await axios.get(`${API_URL}/rooms/available`, {
    params: { checkIn: checkInDate, checkOut: checkOutDate }
  });
  
  const roomIds = response.data.rooms.map(room => room.id);
  assert.ok(!roomIds.includes(roomData.id), 'Room is still showing as available');
});

// ฟังก์ชันช่วย
function calculateNights(checkIn, checkOut) {
  const startDate = new Date(checkIn);
  const endDate = new Date(checkOut);
  const diffTime = Math.abs(endDate - startDate);
  return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
}
```

## 4. การสร้าง End-to-End Tests (การทดสอบตั้งแต่ต้นจนจบ)

E2E tests ทดสอบระบบทั้งหมดรวมทั้ง UI ในสภาพแวดล้อมที่ใกล้เคียงกับการใช้งานจริง

```javascript
// End-to-End Test ด้วย Cypress
// cypress/integration/booking_flow.spec.js

describe('Room Booking Flow', () => {
  before(() => {
    // เตรียมข้อมูลทดสอบผ่าน API หรือฐานข้อมูลโดยตรง
    cy.request('POST', '/api/test/reset-data');
    cy.request('POST', '/api/test/seed-data');
  });

  it('should allow a user to search, select, and book a room', () => {
    // 1. เข้าหน้าค้นหาห้องพัก
    cy.visit('/');
    
    // 2. กรอกข้อมูลค้นหา
    cy.get('#check-in-date').type('2025-08-15');
    cy.get('#check-out-date').type('2025-08-20');
    cy.get('#occupancy').select('2');
    cy.get('#search-button').click();
    
    // 3. ตรวจสอบผลการค้นหา
    cy.get('.room-card').should('have.length.at.least', 1);
    
    // 4. เลือกห้องพักห้องแรก
    cy.get('.room-card').first().within(() => {
      cy.get('.view-details-button').click();
    });
    
    // 5. ดูรายละเอียดห้องพักและจอง
    cy.url().should('include', '/room/');
    cy.get('.room-details').should('be.visible');
    cy.get('#book-now-button').click();
    
    // 6. กรอกข้อมูลการจอง
    cy.url().should('include', '/booking');
    cy.get('#guest-name').type('Jane Doe');
    cy.get('#guest-email').type('jane@example.com');
    cy.get('#guest-phone').type('089-876-5432');
    cy.get('#payment-method').select('Credit Card');
    cy.get('#card-number').type('4111111111111111');
    cy.get('#card-expiry').type('12/28');
    cy.get('#card-cvv').type('123');
    
    // 7. ยืนยันการจอง
    cy.get('#confirm-booking-button').click();
    
    // 8. ตรวจสอบการยืนยันการจอง
    cy.url().should('include', '/booking-confirmation');
    cy.get('.booking-reference').should('be.visible');
    cy.get('.booking-status').should('contain', 'Confirmed');
    
    // 9. ตรวจสอบอีเมลยืนยัน (จำลอง)
    cy.request('GET', '/api/test/latest-email?email=jane@example.com')
      .its('body')
      .should('contain', 'Booking Confirmation');
  });
  
  it('should show error when trying to book an unavailable room', () => {
    // สร้างการจองทดสอบก่อน
    cy.request('POST', '/api/test/create-test-booking', {
      roomId: '101',
      checkInDate: '2025-09-10',
      checkOutDate: '2025-09-15'
    });
    
    // พยายามจองห้องเดียวกันในช่วงเวลาที่ซ้อนกัน
    cy.visit('/');
    cy.get('#check-in-date').type('2025-09-12');
    cy.get('#check-out-date').type('2025-09-18');
    cy.get('#occupancy').select('2');
    cy.get('#search-button').click();
    
    // ตรวจสอบว่าห้องที่จองไปแล้วไม่แสดงในผลการค้นหา
    cy.get('.room-card').each(($el) => {
      cy.wrap($el).find('.room-number').should('not.contain', '101');
    });
  });
});
```

## สรุปการทำ Automated Testing ด้วย TDD

การพัฒนาระบบจองห้องพักออนไลน์ด้วยแนวทาง TDD และ Automated Testing ที่ครอบคลุมทั้ง Unit Tests, Integration Tests, Acceptance Tests และ E2E Tests จะช่วยให้:

1. **มีคุณภาพสูง**: ตรวจจับข้อผิดพลาดตั้งแต่เริ่มต้นการพัฒนา
2. **ง่ายต่อการบำรุงรักษา**: สามารถปรับปรุงหรือเพิ่มฟีเจอร์ใหม่โดยมั่นใจได้ว่าจะไม่กระทบกับฟีเจอร์เดิม
3. **มีเอกสารที่มีชีวิต**: การทดสอบเป็นเหมือนเอกสารที่อธิบายการทำงานของระบบ
4. **การพัฒนาเร็วขึ้นในระยะยาว**: แม้จะใช้เวลามากกว่าในช่วงแรก แต่ช่วยลดเวลาในการแก้บัก

วงจร TDD (Red-Green-Refactor) ช่วยให้ทีมพัฒนาพัฒนาซอฟต์แวร์อย่างมีโครงสร้างและเป็นระบบ ทำให้ได้ซอฟต์แวร์ที่มีคุณภาพสูงและพร้อมรับการเปลี่ยนแปลงในอนาคต
