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

Ultimately, any test case can be boiled down to a conditional: (does it meet an expectation or not) so any test assert can be re-composed into a XCTAssertTrue.

### [Asserting true and false](https://github.com/YamamotoDesu/itdd-materials/commit/8e5d9bdccb6fad48554e91dc88b5821d52f7b7d6)
DataModelTests.swift
```swift
@testable import FitNess
import XCTest

final class DataModelTests: XCTestCase {
  
  var sut: DataModel!
  
  override func setUpWithError() throws {
    // Put setup code here. This method is called before the invocation of each test method in the class.
    try super.setUpWithError()
    sut = DataModel()
  }
  
  override func tearDownWithError() throws {
    // Put teardown code here. This method is called after the invocation of each test method in the class.
    sut = nil
    try super.tearDownWithError()
  }

  // MARK: - Goal
  func testModel_whenStarted_goalIsNotReached() {
    XCTAssertFalse(sut.goalReached, "goalReached should be false when the model is created")
  }
  
  func testModel_whenStepsReachGoal_goalIsReached() {
    
    // given
    sut.goal = 1000
    
    // when
    sut.steps = 1000

    // then
    XCTAssertTrue(sut.goalReached)
  }
}
```

DataModel.swift
```swift
import Foundation

// Add the Data Model class here:
class DataModel {
  var goalReached: Bool {
    if let goal = goal,
       steps >= goal {
      return true
    }
    return false
  }
  
  var goal: Int?
  var steps: Int = 0

}
```
