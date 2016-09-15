# iOS Weekly Tips - Week I
##### 15.08.16
##### Piotr Bernad


******
# Where to start learning Swift
 
 1. [The Swift Programming Language](https://itunes.apple.com/pl/book/swift-programming-language/id881256329?mt=11)
 
 2. [Swift Style Guide by Ray Wenderlich](https://github.com/raywenderlich/swift-style-guide)
 

******

 # Use optional only if needed

 
 <b><font color='red'>Don't</font> </b>
 
 
 ```
 struct User {
    let firstName: String?
    let lastName: String?
    let phoneNumber: String?
    let email: String?
    let avatarURL: NSURL?
    let address: String?
    let cards: [Card]?
    let location: Location?
    let facebookID: String?
}
 ```
 

 ```
  func fillHeader(user: User) {
    self.firstNameLabel.text = user.fistName!
    self.secondNameLabel.text = user.secondName!
  }
 ```
 
 <b><font color='green'>Do</font> </b>

 Please determine which fields are required and try to keep these fields non optional. If something has always value decalare it like that instead of making this:
 
  ```
 struct User {
    let firstName: String
    let lastName: String
    let phoneNumber: String?
    let email: String
    let avatarURL: NSURL?
    let address: String?
    let cards: [Card]?
    let location: Location?
    let facebookID: String?
}
 ```

 ```
  func fillHeader(user: User) {
    self.firstNameLabel.text = user.fistName
    self.secondNameLabel.text = user.secondName
  }
 ```
 
 ---


 # Use lazy loading
 
 <b><font color='red'>Don't</font> </b>

```
class ConversationListParentViewController : UIViewController {
    
    var myChatsViewController : ConversationListTableViewController!
    var publicChatsViewController: ConversationGroupWrapperViewController!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.myChatsViewController = Wireframe.chatsListTableViewController()
        myChatsViewController.viewModel = ConversationListViewModel()
        myChatsViewController.flowDelegate = self.flowDelegate
        
        
        self.publicChatsViewController = Wireframe.groupChatsViewController()
        publicChatsViewController.viewModel = ConversationListViewModel()
        publicChatsViewController.flowDelegate = self.flowDelegate
    }
}
```

<b><font color='green'>Do</font> </b>

```
class ConversationListParentViewController : UIViewController {
    lazy var myChatsViewController: ConversationListTableViewController = {[unowned self] in
        let controller = Wireframe.chatsListTableViewController()
        controller.viewModel = ConversationListViewModel()
        controller.flowDelegate = self.flowDelegate
        return controller
        }()

		lazy var mychatsViewController : ConversationListTableViewController = createChatViewController()

    lazy var publicChatsViewController: ConversationGroupWrapperViewController = {[unowned self] in
        let controller = Wireframe.groupChatsViewController()
        controller.flowDelegate = self.flowDelegate
        return controller
    }()
    
    func createChatViewController() -> ConversationListTableViewController {
    	let controller = Wireframe.chatsListTableViewController()
        controller.viewModel = ConversationListViewModel()
        controller.flowDelegate = self.flowDelegate
        return controller
    }
}
```
---
 
 # Activate and deactivate constraints instead of adding and removing from view
 
 <b><font color='red'>Don't</font> </b>
 ```
 func addAdViewOnBottom() {
     let constraints = [NSLayoutConstraint.constraintsWithVisualFormat("H:|[child]|", options: [], metrics: nil, views: views),
                        NSLayoutConstraint.constraintsWithVisualFormat("V:|[child]|", options: [], metrics: nil, views: views)]

    self.adView.addConstraints(constraints)
    self.adView.removeConstraint(self.heightConstraint)
 }
 ```
 
 <b><font color='green'>Do</font> </b>
 
 ```
 func addAdViewOnBottom() {
    let constraints = [NSLayoutConstraint.constraintsWithVisualFormat("H:|[child]|", options: [], metrics: nil, views: views),
                       NSLayoutConstraint.constraintsWithVisualFormat("V:|[child]|", options: [], metrics: nil, views: views)]

    NSLayoutConstraint.activateConstraints(constraints)
    NSLayoutConstraint.deactivateConstraints([self.heightConstraint])
}
 ```
 ---

 # Dependencies

Some Rules:

* Use libraries only if needed
* Estimate time before writing your own code. Keep in mind that you need to support it later.
* Always check code before including third party in to your project
* Please try to not mixing Objc with Swift, you may reach point where it cause pain in the ass

Good Libraries for Swift:
* Alamofire - Networking
* DZNEmptyDataSet - Collection Views Placeholders
* RxSwift
* Reachability.swift 
* Kingfisher - Loading and caching images from Internet
* KeychainAccess - Wrapper for Keychain

 
---

 

 # CollectionViews and TableViews
 
 <b><font color='red'>Don't</font> </b>
 
 Don't place UICollectionViewDataSource/UITableViewDataSource inside of your viewController
 
 <b><font color='green'>Do</font> </b>
 
 DataSource should be placed in another object that is responsible for preparing your data. Good practice is to keep object as small as possible.
 
---


 # Launching Application
 
 <b><font color='red'>Don't</font> </b>
 
 Don't make any long running operations like fetching user, loading database etc. at app launch. It may kill your app.
 
<b><font color='green'>Do</font> </b>
 
 Create empty ViewController that handles all logic with refreshing user/checking login state and then decide what to show user. It will help you also with handling push notifications. This controller is always on stack. 
 
 ---
 
 # Use Dynamic Frameworks for Bussines Logic Classes (models, params etc)
 
 <b><font color='green'>Do</font> </b>
 
 Create Dynamic Framework target and add all models to this target, it will help you to separate model layer from helpers etc. It forces you to think about relations between classes and simplifies structure of project. It's must have when you wan't to develop apps for Apple Watch or Apple Tv.
 
 ---
 # Use UIStackView
 
 iOS 9 introduces StackView it will help you build almost every layout without adding thousands of constraints. Simple layouts can be done without any constraint. [Must see this.](https://developer.apple.com/videos/play/wwdc2015/218/)
 
 ---
 # Learn how Autolayout works!
 
 * https://developer.apple.com/videos/play/wwdc2015/218/
 
* https://developer.apple.com/videos/play/wwdc2015/219/

---

# Force Clients and PM's to use latests SDK

It's your responsibility to explain clients and PM's why we should support only two latests iOS versions. It's very important for your personal growth to use latests apis. 

---

# Use correct function names

 <b><font color='red'>Don't</font> </b>

```
func doMove() {
    if person.isLive == false {
        return
    }
    
    person.position = CGPointMake(currentX + 1, currentY + 1)
}
```

<b><font color='green'>Do</font> </b>

```
func doMoveIfPersonIsLive() {
    if person.isLive == false {
        return
    }
    
    person.position = CGPointMake(currentX + 1, currentY + 1)
}
```

---
# Use Enums instead of Bools

 <b><font color='red'>Don't</font> </b>
```
class User {
    var isLoggedIn : Bool
    var isGuestUser : Bool
}
```

<b><font color='green'>Do</font> </b>

```
enum LoginState {
    case notAuthorized
    case logged
    case guest
}

class User {
    var loginState : LoginState
}
```

---
# Use Associated Values

 <b><font color='red'>Don't</font> </b>

```
    enum DataState {
        case loading
        case loaded
        case error
    }
    class ChatsViewModel {
        var state : DateState
        var items : [Chats]?
        var error : NSError?
    }
```

<b><font color='green'>Do</font> </b>

```
enum DataState {
        case loading
        case loaded(let items)
        case error(let error)
    }

    class ChatsViewModel {
        var state : DateState
    }
```

--- 

# Use Generics

<b><font color='green'>Do</font> </b>

```
public struct PagedResults<T: Decodable where T.DecodedType == T> {
    public let count: Int?
    public let previousPath: String?
    public let nextPath: String?
    public let results: [T]
    
    public init(count: Int?, previousPath: String?, nextPath: String?, results: [T]) {
        self.count = count
        self.previousPath = previousPath
        self.nextPath = nextPath
        self.results = results
    }
}

extension PagedResults: Decodable {
    
    public static func decode(j: JSON) -> Decoded<PagedResults<T>> {
        let a = curry(PagedResults<T>.init)
            <^> j <|? "count"
            <*> j <|? "previous"
        let b = a
            <*> j <|? "next"
            <*> j <|| "results"
        return b
    }
    
    public init(_ results: [T]) {
        self.results = results
        self.count = results.count
        self.previousPath = nil
        self.nextPath = nil
    }
}

```

--- 

# Use many storyboards

<b><font color='red'>Don't</font> </b>

Don't place all ViewControllers in one storybard! Never! Just don't!

<b><font color='green'>Do</font> </b>


Create Storyboard per User Story, even if there is one controller only.


---

# Permissions


<b><font color='red'>Don't</font> </b>

Don't ask user for all permissions at app launch.


<b><font color='green'>Do</font> </b>

Ask user for permissions only if needed. Never on app launch. Asks designers to prepare screens with explanation why permissions are needed.
Remember to handle casses where user won't allow for something, prompt him again if needed with custom alert and direct to settings. 


--- 

# Carthage

<b><font color='green'>Do</font> </b>

If you decided to use Carthage please commit compiled frameworks to your repository. Remember to be explicit about dependecy version.

---



# Files

<b><font color='green'>Do</font> </b>

Keep files as small as possible. Don't place two classes in one file.

<b><font color='green'>Do</font> </b>

Bigger extensions place in separate file. 

<b><font color='green'>Do</font> </b>

Keep files in correct place in project navigator and in project folders.

---

# Profiling

<b><font color='green'>Do</font> </b>

Profile your app before every release. Try to find memory leaks and main thread issues. Check battery usage. 

---




