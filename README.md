# iOS Test-Driven Development by Tutorials: Materials

This repo contains all the downloadable materials and projects associated with the **[iOS Test-Driven Development by Tutorials](https://www.raywenderlich.com/books/ios-test-driven-development-by-tutorials)** from [raywenderlich.com](https://www.raywenderlich.com).

Each edition has its own branch, named `editions/[EDITION]`. The default branch for this repo is for the most recent edition.

## Release History

| Branch                                                                            | Edition | Release Date |
| --------------------------------------------------------------------------------- |:-------:|:------------:|
| [editions/2.0](https://github.com/raywenderlich/itdd-materials/tree/editions/2.0) | 2.0     | 2022-01-19   |
| [editions/1.0](https://github.com/raywenderlich/itdd-materials/tree/editions/1.0) | 1.0     | 2019-10-02   |

# [2. The TDD Cycle](https://www.kodeco.com/books/ios-test-driven-development-by-tutorials/v2.0/chapters/2-the-tdd-cycle)

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

# [4. Test Expressions](https://www.kodeco.com/books/ios-test-driven-development-by-tutorials/v2.0/chapters/4-test-expressions)
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

### [Testing Errors]()
AppModelTests
```swift
import XCTest
@testable import FitNess

class AppModelTests: XCTestCase {
  //swiftlint:disable implicitly_unwrapped_optional
  var sut: AppModel!

  override func setUpWithError() throws {
    try super.setUpWithError()
    sut = AppModel()
  }

  override func tearDownWithError() throws {
    sut = nil
    try super.tearDownWithError()
  }

  // MARK: - Given
  func givenGoalSet() {
    sut.dataModel.goal = 1000
  }

  // MARK: - Lifecycle
  func testAppModel_whenInitialized_isInNotStartedState() {
    let initialState = sut.appState
    XCTAssertEqual(initialState, AppState.notStarted)
  }

  // MARK: - Start
  func testModelWithNoGoal_whenStarted_throwsError() {
    XCTAssertThrowsError(try sut.start())
  }
  
  func testAppModel_whenStarted_isInInProgressState() {
    // given
    givenGoalSet()
    
    // when started
    try? sut.start()

    // then it is in inProgress
    let observedState = sut.appState
    XCTAssertEqual(observedState, .inProgress)
  }
  

  
  func testStart_withGoalSet_throwsError() {
    // given
    givenGoalSet()
    
    // then
    XCTAssertNoThrow(try sut.start())
  }
}
```

AppModel.swift
```swift
import Foundation

class AppModel {
  static let instance = AppModel()
  let dataModel = DataModel()

  var appState: AppState = .notStarted

  func start() throws {
    
    guard dataModel.goal != nil else {
      throw AppError.goalNotSet
    }
    
    appState = .inProgress
  }
}
```

StepCountController.swift
```swift
  // MARK: - UI Actions

  @IBAction func startStopPause(_ sender: Any?) {
    do {
      try AppModel.instance.start()
    } catch {
      showNeedGoalAlert()
    }

    updateUI()
  }
```

<img width="300" alt="スクリーンショット 2023-03-28 10 58 18" src="https://user-images.githubusercontent.com/47273077/228107158-53e84337-f42f-4651-83b2-1b7848622656.png">


## View controller testing
### [Functional view controller testing](https://github.com/YamamotoDesu/itdd-materials/commit/5ddbcb4f9fdec6631ed4f3bf3f6a9707c7d0e64a)
The important thing when testing view controllers is to not test the views and controls directly. This is better done using UI automation tests. Here, the goal is to check the logic and state of the view controller.
```swift
  func testDataModel_whenGoalUpdate_updatesToNewGoal() {
    // when
    sut.updateGoal(newGoal: 50)
    
    // then
    XCTAssertEqual(AppModel.instance.dataModel.goal, 50)
  }
}
```

StepCountController.swift
```swift
  func updateGoal(newGoal: Int) {
    // update this function
    AppModel.instance.dataModel.goal = newGoal
  }
 ```

### [Using the host app](https://github.com/YamamotoDesu/itdd-materials/commit/3f9c92bc73a4c356e87162d15e930f32c5aff56b)

StepCountControllerTests.swift
```swift
func testChaseView_whenLoaded_isNotStarted() {
  // when loaded, then
  let chaseView = sut.chaseView
  XCTAssertEqual(chaseView?.state, .notStarted)
}
```

Test Classes/ViewControllers.swift(under FitNessTests target)
```swift
import UIKit
@testable import FitNess

func getRootViewController() -> RootViewController {
  guard let controller =
    (UIApplication.shared.connectedScenes.first as? UIWindowScene)?
    .windows
    .first?
    .rootViewController as? RootViewController else {
    assert(false, "Did not a get RootViewController")
  }
  return controller
}
```

Test Extensions/RootViewController+Tests.swift(under FitNessTests target)
```swift
import UIKit
@testable import FitNess

extension RootViewController {
  var stepController: StepCountController {
    return children.first { $0 is StepCountController }
      as! StepCountController
  }
}
```

Note: One alternate way of retrieving and testing a view controller can be done as follows: First, get a reference to the storyboard:

let storyboard = UIStoryboard(name: "Main", bundle: nil)

Second, get a reference to the view controller:

let stepController = storyboard.instantiateViewcontroller(withIdentifier: "stepController") as! StepCountController

Finally, if needed, you may load the view as follows:

stepController.loadViewIfNeeded()

Following this pattern allows you to instantiate a fresh view controller for each test, and it affords the option to set up and tear down the view controller for each test.

### [Fixing the tests](https://github.com/YamamotoDesu/itdd-materials/commit/a4fcf53ccca5144eb1acbddb0e626f17af9fd386)
StepCountControllerTests.swift, and replace setUpWithError() with the following:
```swift
override func setUpWithError() throws {
  try super.setUpWithError()
  let rootController = getRootViewController()
  sut = rootController.stepController
}
```

AppModelTests.swift
```swift
func givenInProgress() {
  givenGoalSet()
  sut.startStopPause(nil)
}
```

StepCountControllerTests.swift
```swift
func testChaseView_whenInProgress_viewIsInProgress() {
  // given
  givenInProgress()

  // then
  let chaseView = sut.chaseView
  XCTAssertEqual(chaseView?.state, .inProgress)
}
```

StepCountController.swift
```swift
private func updateChaseView() {
  chaseView.state = AppModel.instance.appState
}
```

### [Test ordering matters](https://github.com/YamamotoDesu/itdd-materials/commit/472456b72146f24a58f84831fce2c6fe7189d26b)

AppModelTests.swift
```swift
// MARK: - Restart
func testAppModel_whenReset_isInNotStartedState() {
  // given
  givenInProgress()

  // when
  sut.restart()

  // then
  XCTAssertEqual(sut.appState, .notStarted)
}
```

AppModel.swift
```swift
func restart() {
  appState = .notStarted
}
```

StepCountControllerTests.swift
```swift
override func tearDownWithError() throws {
  AppModel.instance.dataModel.goal = nil
  AppModel.instance.restart()
  sut.updateUI()
  try super.tearDownWithError()
}
```

### Randomized order
There is also an option in the Test action of the scheme to randomize the test order. Edit the FitNess scheme. Select the Test action. In the center pane, next to FitNessTests is an Options… button. Click that and, in the pop-up, check Randomize execution order. This will cause the tests to run in a random order each time.
![image](https://user-images.githubusercontent.com/47273077/228414846-98053d76-4dc7-4ca7-8c97-632756e9c36a.png)

### Code coverage
![image](https://user-images.githubusercontent.com/47273077/228415061-6c48360c-9dd6-4270-9623-1ddb2f8c5d9b.png)

### Using test breakpoints
A good next step is to try out the debugger. In the Breakpoint navigator, click the + all the way at the bottom. Select Test Failure Breakpoint.
![image](https://user-images.githubusercontent.com/47273077/228424942-a2117682-b047-4f5d-a8b3-5882317b0f8c.png)

## [5. Test Expectations](https://www.kodeco.com/books/ios-test-driven-development-by-tutorials/v2.0/chapters/5-test-expectations)
### Writing an asynchronous test

AppModelTests.swift
```swift
  // MARK: - State Changes
  func testAppModel_whenStateChanges_executesCallback() {
    //given
    givenInProgress()
    var observedState = AppState.notStarted
    
    let expected = expectation(description: "callback happened")
    sut.stateChangedCallback = { model in
      observedState = model.appState
      
      expected.fulfill()
    }
    
    // when
    sut.pause()
    
    // then
    wait(for: [expected], timeout: 1)
    XCTAssertEqual(observedState, .paused)
  }
  ```
  
AppModel
```swift
    var appState: AppState = .notStarted {
    didSet {
      stateChangedCallback?(self)
    }
  }
  var stateChangedCallback: ((AppModel) -> Void)?
```

expectation(description:) is an XCTestCase method that creates an XCTestExpectation object. 
The description helps identify a failure in the test logs. 

fulfill() is called on the expectation to indicate it has been fulfilled - specifically, the callback has occurred. Here stateChangedCallback will trigger on sut when a state change occurs.

wait(for:timeout:) causes the test runner to pause until all expectations are fulfilled or the timeout time (in seconds) passes.
The assertion will not be called until the wait completes.

### Testing for true asynchronicity
StepCountControllerTests.swift
```swift
func testController_whenCaught_buttonLabelIsTryAgain() {
  // given
  givenInProgress()
  let exp = expectation(description: "button title change")
  let observer = ButtonObserver()
  observer.observe(sut.startButton, expectation: exp)

  // when
  whenCaught()

  // then
  waitForExpectations(timeout: 1)
  let text = sut.startButton.title(for: .normal)
  XCTAssertEqual(text, AppState.caught.nextStateButtonLabel)
}

func testController_whenComplete_buttonLabelIsStartOver() {
  // given
  givenInProgress()
  let exp = expectation(description: "button title change")
  let observer = ButtonObserver()
  observer.observe(sut.startButton, expectation: exp)

  // when
  whenCompleted()

  // then
  waitForExpectations(timeout: 1)
  let text = sut.startButton.title(for: .normal)
  XCTAssertEqual(text, AppState.completed.nextStateButtonLabel)
}

func whenCaught() {
  AppModel.instance.setToCaught()
}

func whenCompleted() {
  AppModel.instance.setToComplete()
}

```

ButtonObserver.swift
```swift
import XCTest

class ButtonObserver {
  var token: NSKeyValueObservation?

  func observe(_ button: UIButton, expectation: XCTestExpectation) {
    token = button
      .observe(\.titleLabel?.text, options: [.new]) { _, _ in
        expectation.fulfill()
      }
  }

  deinit {
    token?.invalidate()
  }
}
```

StepCountController.swift:
```swift
  override func viewDidLoad() {
    super.viewDidLoad()

    AppModel.instance.stateChangedCallback = { model in
      DispatchQueue.main.async {
        self.updateUI()
      }
    }
  }
```
