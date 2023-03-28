# iOS Test-Driven Development by Tutorials: Materials

This repo contains all the downloadable materials and projects associated with the **[iOS Test-Driven Development by Tutorials](https://www.raywenderlich.com/books/ios-test-driven-development-by-tutorials)** from [raywenderlich.com](https://www.raywenderlich.com).

Each edition has its own branch, named `editions/[EDITION]`. The default branch for this repo is for the most recent edition.

## Release History

| Branch                                                                            | Edition | Release Date |
| --------------------------------------------------------------------------------- |:-------:|:------------:|
| [editions/2.0](https://github.com/raywenderlich/itdd-materials/tree/editions/2.0) | 2.0     | 2022-01-19   |
| [editions/1.0](https://github.com/raywenderlich/itdd-materials/tree/editions/1.0) | 1.0     | 2019-10-02   |

## [2. The TDD Cycle](https://www.kodeco.com/books/ios-test-driven-development-by-tutorials/v2.0/chapters/2-the-tdd-cycle)

### Getting started
```swift
import XCTest

class CashRegister {
  
}

class CashRegisterTests: XCTestCase {
    // 1
    func testInit_createsCashRegister() {
        // 2
        XCTAssertNotNil(CashRegister())
    }
}

CashRegisterTests.defaultTestSuite.run()
```


## TDDing init(availableFunds:)

```swift
import XCTest

class CashRegister {
    var availableFunds: Decimal

    init(availableFunds: Decimal) {
      self.availableFunds = availableFunds
    }
}

class CashRegisterTests: XCTestCase {

    func testInitAvailableFunds_setsAvailableFunds() {
      // given
      let availableFunds = Decimal(100)
      
      // when
      let sut = CashRegister(availableFunds: availableFunds)
      
      // then
      XCTAssertEqual(sut.availableFunds, availableFunds)
    }
}

CashRegisterTests.defaultTestSuite.run()

```

### TDDing addItem

```swift
import XCTest

class CashRegister {
    var availableFunds: Decimal
    var transactionTotal: Decimal = 0

    init(availableFunds: Decimal) {
      self.availableFunds = availableFunds
    }

    func addItem(_ cost: Decimal) {
      transactionTotal = cost
    }
}

class CashRegisterTests: XCTestCase {
    var availableFunds: Decimal!
    var sut: CashRegister!

    // 1
    override func setUp() {
      super.setUp()
      availableFunds = 100
      sut = CashRegister(availableFunds: availableFunds)
    }

    // 2
    override func tearDown() {
      availableFunds = nil
      sut = nil
      super.tearDown()
    }


    func testInitAvailableFunds_setsAvailableFunds() {
      // then
      XCTAssertEqual(sut.availableFunds, availableFunds)
    }

    func testAddItem_oneItem_addsCostToTransactionTotal() {
      // given
      let itemCost = Decimal(42)
      
      // when
      sut.addItem(itemCost)
      
      // then
      XCTAssertEqual(sut.transactionTotal, itemCost)
    }

}
```

### Adding two items

```swift
import XCTest

class CashRegister {
    var availableFunds: Decimal
    var transactionTotal: Decimal = 0
    
    init(availableFunds: Decimal) {
        self.availableFunds = availableFunds
    }
    
    func addItem(_ cost: Decimal) {
        transactionTotal += cost
    }
}

class CashRegisterTests: XCTestCase {
    var availableFunds: Decimal!
    var sut: CashRegister!
    var itemCost: Decimal!
    
    // 1
    override func setUp() {
        super.setUp()
        availableFunds = 100
        itemCost = 42
        sut = CashRegister(availableFunds: availableFunds)
    }
    
    // 2
    override func tearDown() {
        availableFunds = nil
        itemCost = nil
        sut = nil
        super.tearDown()
    }
    
    
    func testInitAvailableFunds_setsAvailableFunds() {
        // then
        XCTAssertEqual(sut.availableFunds, availableFunds)
    }
    
    func testAddItem_oneItem_addsCostToTransactionTotal() {
        
        // when
        sut.addItem(itemCost)
        
        // then
        XCTAssertEqual(sut.transactionTotal, itemCost)
    }
    
    func testAddItem_twoItems_addsCostsToTransactionTotal() {
        // given
        let itemCost2 = Decimal(20)
        let expectedTotal = itemCost + itemCost2
        
        // when
        sut.addItem(itemCost)
        sut.addItem(itemCost2)
        
        // then
        XCTAssertEqual(sut.transactionTotal, expectedTotal)
    }
    
}

CashRegisterTests.defaultTestSuite.run()

```

## [4. Test Expressions](https://www.kodeco.com/books/ios-test-driven-development-by-tutorials/v2.0/chapters/4-test-expressions)
### Assert methods
There are several assert functions in XCTest:

🖌 Equality: XCTAssertEqual, XCTAssertNotEqual  

🖌 Truthiness: XCTAssertTrue, XCTAssertFalse

🖌 Nullability: XCTAssertNil, XCTAssertNotNil

🖌 Comparison: XCTAssertLessThan, XCTAssertGreaterThan, XCTAssertLessThanOrEqual, XCTAssertGreaterThanOrEqual

🖌 Erroring: XCTAssertThrowsError, XCTAssertNoThrow
