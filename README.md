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
/// Copyright (c) 2021 Razeware LLC
///
/// Permission is hereby granted, free of charge, to any person obtaining a copy
/// of this software and associated documentation files (the "Software"), to deal
/// in the Software without restriction, including without limitation the rights
/// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
/// copies of the Software, and to permit persons to whom the Software is
/// furnished to do so, subject to the following conditions:
///
/// The above copyright notice and this permission notice shall be included in
/// all copies or substantial portions of the Software.
///
/// Notwithstanding the foregoing, you may not use, copy, modify, merge, publish,
/// distribute, sublicense, create a derivative work, and/or sell copies of the
/// Software in any work that is designed, intended, or marketed for pedagogical or
/// instructional purposes related to programming, coding, application development,
/// or information technology.  Permission for such use, copying, modification,
/// merger, publication, distribution, sublicensing, creation of derivative works,
/// or sale is expressly withheld.
///
/// This project and source code may use libraries or frameworks that are
/// released under various Open-Source licenses. Use of those libraries and
/// frameworks are governed by their own individual licenses.
///
/// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
/// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
/// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
/// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
/// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
/// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
/// THE SOFTWARE.

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
