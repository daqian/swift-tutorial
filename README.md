Swift教程第3部分：元组，协议，委托和表视图
 Ray Wenderlich on June 20, 2014
更新于2014年7月7日：更新了Xcode6-beta 3.
欢迎回到Swift教程系列！
In the first Swift tutorial, you learned the basics of the Swift language, and created your very own tip calculator class.
在Swift教程的第一部分，我们学习了Swift语言的基础，并且创建了属于我们自己的小费计算器类。
In the second Swift tutorial, you created a simple user interface for the tip calculator.
在Swift教程的第二部分，我们为小费计算器创建了一个简单的用户界面。
In this third Swift tutorial, you will learn about a new data type introduced in Swift: tuples.
在这次的Swift第三部分教程里，我们将学习Swift里引入的一个新的数据类型：
元组。
You will also learn about protocols and delegates, table views, and how to prototype your user interfaces in Playgrounds.
我们也将学习协议和委托，表视图以及如何规范游戏场地的用户界面。
This tutorial picks up where the second Swift tutorial left off. If you don’t have it already, be sure to download the sample project where you left it off last time.
Note: At the time of writing this tutorial, our understanding is we cannot post screenshots of Xcode 6 since it is still in beta. Therefore, we are suppressing screenshots in this Swift tutorial until we are sure it is OK.
Getting Started
So far, your Tip Calculator gives a proposed tip for each tip percentage. However, once you select the tip you want to pay, you have to add the tip to the bill total in your head – which kind of defeats the point!
It would be nice if your calcTipWithTipPct() method returned two values: the tip amount, and the bill total with tip applied.
In Objective-C, if you want to make a method return two values, you either have to make an Objective-C object with two properties for the return values, or you have to return a dictionary containing the two values. In Swift, there’s an alternative way: Tuples.
Let’s play around with Tuples a bit so you can get a feel for how they work. Create a new Playground in Xcode (or if you followed my advice in the first Swift tutorial, simply click on the Playground you have saved to your dock). Delete everything from your Playground so you start from a clean slate.
Unnamed Tuples
Let’s start by creating something called an Unnamed Tuple. Enter the following into your playground:
let tipAndTotal = (4.00, 25.19)
Here you group two Doubles (tip and total) into a single Tuple value. This uses inferred syntax since the compiler is able to automatically determine the type based on the initial value that you’re setting it to. Alternatively you could have written the line explicitly like this:
let tipAndTotal:(Double, Double) = (4.00, 25.19)
To access the individual elements of your tuple, you have two options: accessing by index, or decomposing by name. Add the following to the playground to try the first:
tipAndTotal.0
tipAndTotal.1
You should see 4.0 and 25.19 in the sidebar of your playground. Accessing by index works in a pinch, but isn’t as clean as decomposing by name. Add the following to your playground to try the latter:
let (theTipAmt, theTotal) = tipAndTotal
theTipAmt
theTotal
This syntax allows you to make a new constant that refers to each element in the tuple, with a particular name.
Named Tuples
Unnamed Tuples work, but as you saw takes some extra code to access each item by name.
It is often even more convenient to use Named Tuples instead, where you name your tuples at declaration time. Add the following lines to your playground to try:
let tipAndTotalNamed = (tipAmt:4.00, total:25.19)
tipAndTotalNamed.tipAmt
tipAndTotalNamed.total
As you can see this is a lot more convenient, and this is what we’ll be using for the rest of this tutorial.
Finally I’d like to point out that again you are using inferred syntax for the declaration of tipAndTotalNamed. If you wanted to use explicit syntax, it would look like this:
let tipAndTotalNamed:(tipAmt:Double, total:Double) = (4.00, 25.19)
Note that when you use explicit syntax, naming the variables on the right hand side is optional.
Returning Tuples
Now that you understand the basics of Tuples, let’s see how you can use them in your tip calculator to return two values.
Add the following code to your playground:
let total = 21.19
let taxPct = 0.06
let subtotal = total / (taxPct + 1)
func calcTipWithTipPct(tipPct:Double) -> (tipAmt:Double, total:Double) {
  let tipAmt = subtotal * tipPct
  let finalTotal = total + tipAmt
  return (tipAmt, finalTotal)
}
calcTipWithTipPct(0.20)
This is the same calcTipWithTipPct method you’ve been working with, except instead of returning a Double, it returns (tipAmt:Double, total:Double).
Here is the Playground file up to this point. Now, delete everything from your playground to start at a fresh slate.
A Full Prototype
At this point, you’re ready to take what you’ve learned and integrate it into your TipCalculatorModel class.
But before you start modifying your TipCalculator Xcode project, let’s try out the changes in this playground!
Copy your TipCalculatorModel class from your TipCalculator project into this playground. Then, modify the calcTipWithTipPct the same way you did earlier. Finally, modify returnPossibleTips to return a dictionary of Ints to Tuples instead of Ints to Doubles.
See if you can figure this out on your own; it’s good practice. But if you get stuck, check out the spoiler below!
Solution Inside: TipCalculatorModel - Modified	Show

Here is the Playground file up to this point.
At this point, save your file and start up a new empty Playground. We’ll be returning to this Playground later.
Protocols
The next step is to prototype a table view for your app. But before you do that, you need to understand the concepts of protocols and delegates. Let’s start with protocols.
A protocol is a list of methods that specify a “contract” or “interface”. Add these lines to your Playground to see what I mean:
protocol Speaker {
  func Speak()
}
This protocol declares a single method called Speak.
Any class that conforms to this protocol must implement this method. Try this by adding two classes that conform to your protocol:
class Vicki: Speaker {
  func Speak() {
    println("Hello, I am Vicki!")
  }
}
 
class Ray: Speaker {
  func Speak() {
    println("Yo, I am Ray!")
  }
}
To mark a class as conforming to a protocol, you must put a colon after the class name, and then list the protocol (after the name of the class it inherits from, if any). These classes do not inherit from any other class, so you can just list the name of the protocol directly.
Also notice that if you do not include the Speak function, you will get a compiler error.
Now try this for a class that does inherit from another class:
class Animal {
}
class Dog : Animal, Speaker {
  func Speak() {
    println("Woof!")
  }
}
In this example, Dog inherits from Animal, so when declaring Dog you put a :, then the class it inherits from, then list any protocols. You can only inherit from 1 class in Swift, but you can conform to any number of protocols.
Optional Protocols
You can mark methods in a protocol as being optional. To try this, replace the Speaker protocol with the following:
@objc protocol Speaker {
  func Speak()
  @optional func TellJoke()
}
If you want to have a protocol with optional methods, you must prefix the protocol with the @objc tag (even if your class is not interoperating with objective-C). Then, you prefix any method that can be optional with the @optional tag.
Note there is currently no compiler error for your Person or Dog class, because the new function is optional.
In this example, Ray or Vicki can tell a joke, but sadly not a Dog. So implement this method only on those two classes:
class Vicki: Speaker {
  func Speak() {
    println("Hello, I am Vicki!")
  }
  func TellJoke() {
    println("Q: What did Sushi A say to Sushi B?")
  }
}
 
class Ray: Speaker {
  func Speak() {
    println("Yo, I am Ray!")
  }
  func TellJoke() {
    println("Q: Whats the object-oriented way to become wealthy?")
  }
  func WriteTutorial() {
    println("I'm on it!")
  }
}
Note that when you’re implementing a protocol, of course your class can have more methods than just the ones in the protocol if you want. Here the Ray class has an extra method.
Oh, and can you guess the answer to these jokes?
Solution Inside	Show

Using Protocols
Now that you’ve created a protocol and a few classes that have implemented them, let’s try using them. Add the following lines to your playground:
var speaker:Speaker
speaker = Ray()
speaker.Speak()
// speaker.WriteTutorial() // error!
(speaker as Ray).WriteTutorial()
speaker = Vicki()
speaker.Speak()
Note that rather than declaring speaker as Ray, you declare it as speaker. This means you can only call methods on speaker that exist in the Speaker protocol, so calling WriteTutorial would be an error even though speaker is actually of type Ray. You can call WriteTutorial if you cast speaker back to Ray temporarily though, as you see here.
Also note that you can set speaker to Vicki as well, since Vicki also conforms to Speaker.
Now add these lines to experiment with the optional method:
speaker.TellJoke?()
speaker = Dog()
speaker.TellJoke?()
Remember that TellJoke is an optional method, so before you call it you should check if it exists.
These lines use use a technique called optional chaining to do this. If you put a ? mark after the method name, it will check if it exists before calling it. If it does not exist, it will behave as if you’ve called a method that returns nil.
Optional chaining is a useful technique anytime you want to test if an optional value exists before using it, as an alternative to the if let (optional binding) syntax discussed earlier. You will be using this much more in the rest of this series and other Swift tutorials on this site.
Delegates
A delegate is simply a variable that conforms to a protocol, which a class typically uses to notify of events or perform various sub-tasks. Think of it like a boss giving his minion status updates, or asking him/her to do something!
To see what I mean, add a new DateSimulator class to your playground, that allows two classes that conform to Speaker to go on a date:
class DateSimulator {
 
  let a:Speaker
  let b:Speaker
 
  init(a:Speaker, b:Speaker) {
    self.a = a
    self.b = b
  }
 
  func simulate() {
    println("Off to dinner...")
    a.Speak()
    b.Speak()
    println("Walking back home...")
    a.TellJoke?()
    b.TellJoke?()
  }
}
 
let sim = DateSimulator(a:Vicki(), b:Ray())
sim.simulate()
Imagine you want to be able to notify another class when the date begins or ends. This could be useful if you wanted to make a status indicator in your app appear or disappear when this occurs, for example.
To do this, first create a protocol with the events you want to notify, like so (add this before DateSimulator):
protocol DateSimulatorDelegate {
  func dateSimulatorDidStart(sim:DateSimulator, a:Speaker, b:Speaker)
  func dateSimulatorDidEnd(sim:DateSimulator, a: Speaker, b:Speaker)
}
Then create a class that conforms to this protocol (add this right after the DateSimulatorDelegate):
class LoggingDateSimulator:DateSimulatorDelegate {
  func dateSimulatorDidStart(sim:DateSimulator, a:Speaker, b:Speaker) {
    println("Date started!")
  }
  func dateSimulatorDidEnd(sim:DateSimulator, a: Speaker, b: Speaker)  {
    println("Date ended!")
  }
}
For simplicity, you’ll simply log out these events.
Then, add a new property to DateSimulator that takes a class that conforms to this protocol:
var delegate:DateSimulatorDelegate?
This is an example of a delegate. Again, a delegate is just some class that implements a protocol, that you might want to notify about events, or have it do certain tasks on your behalf.
Note that you are making it optional here, as the DateSimulator should be able to work fine, whether or not the delegate is set.
Right before the line sim.simulate(), set this variable to your LoggingDateSimulator:
sim.delegate = LoggingDateSimulator()
Finally, modify your simulate() function to call the delegate appropriately at the beginning and end of the method.
Try to do this part yourself, as it’s good practice! Note that you should check if delegate is set before using it – I recommend you try out optional chaining to solve this.
Solution Inside: Solution: Complete DateSimulator Class	Show
Here is the playground file up to this point.
Now, save your file and return back to the playground file you saved earlier in this tutorial with your new and improved TipCalculatorModel class in it. It’s time to put this into a table view!
Table Views, Delegates, and Data Sources
Now that you understand the concepts of protocols and delegates, you are ready to use table views in your app.
It turns out table views have a property called delegate – you can set it to a class that conforms to UITableViewDelegate. This is a protocol with a bunch of optional methods to let you know about events like when a row is selected, or when the table view enters edit mode.
Table views also have another property called dataSource – you can set it to a class that conforms to UITableViewDataSource. The difference is instead of notifying this class about events, the table view asks it for data – like how many rows to display, and what should be displayed in each row.
The delegate is optional, but the dataSource is required. So let’s try this out by creating a data source for your tip calculator.
One of the cool things about playgrounds is you can actually prototype and visualize views (like UITableView there as well! This is a great way to make sure it’s working before integrating it into your main project.
Again, make sure you’re in the playground with your new and improved TipCalculatorModel class. Then add this code to the bottom of the file:
// 1
import UIKit
// 2
class TestDataSource : NSObject {
 
  // 3
  let tipCalc = TipCalculatorModel(total: 33.25, taxPct: 0.06)
  var possibleTips = Dictionary<Int, (tipAmt:Double, total:Double)>()
  var sortedKeys:Int[] = []
 
  // 4
  init() {
    possibleTips = tipCalc.returnPossibleTips()
    sortedKeys = sorted(Array(possibleTips.keys))
    super.init()
  }
 
}
Let’s go over this section by section.
In order to use UIKit classes like UITableView, you first need to import the UIKit framework. If you get an error on this line, bring up the File Inspector (View\Utilities\Show File Inspector) and set the Platform to iOS.
One of the requirements of implementing UITableViewDataSource is that your class extends NSObject (either directly or through intermediate classes).
Here you initialize the tip calculator, and make an empty array for the possible tips and sorted keys. Note that you are keeping possibleTips and sortedKeys as variables (not constants) because in the actual app you’ll want these to be able to change over time.
In init, you set up the two variables with initial values.
Now that you have a base, let’s make the class conform to UITableViewDataSource. To do this, first add the data source to the end of the class declaration:
class TestDataSource: NSObject, UITableViewDataSource {
Then add this two new method:
func tableView(tableView: UITableView!, numberOfRowsInSection section: Int) -> Int {
  return sortedKeys.count
}
This is one of the two required methods you must implement to conform to UITableViewDataSource. It asks you how many rows are in each section of the table view. This table view will only have 1 section, so you return the number of values in sortedKeys (i.e. the the number of possible tip percentages).
Next, add the other required method:
// 1
func tableView(tableView: UITableView!, cellForRowAtIndexPath indexPath: NSIndexPath!) -> UITableViewCell! {
  // 2
  let cell = UITableViewCell(style: UITableViewCellStyle.Value2, reuseIdentifier: nil)
 
  // 3
  let tipPct = sortedKeys[indexPath.row]
  // 4
  let tipAmt = possibleTips[tipPct]!.tipAmt
  let total = possibleTips[tipPct]!.total
 
  // 5
  cell.textLabel.text = "\(tipPct)%:"
  cell.detailTextLabel.text = String(format:"Tip: $%0.2f, Total: $%0.2f", tipAmt, total)
  return cell
}
Let’s go over this section by section:
This method is called for each row in your table view. You need to return the view that represents this row, which is a subclass of UITableViewCell.
You can create table view cells with a built-in style, or create your own custom subclasses for your own style. Here you are creating a table view cell with a default style – UITableViewCellStyle.Value2 in particular. Note that inferred typing allows you to shorten this to just .Value2 if you want, but I’m keeping the long form here for clarity.
One of the parameters to this method is indexPath, which is a simple collection of the section and row for this table view cell. Since there’s just one section, you use the row to pull out the appropriate tip percentage to display from sortedKeys.
Next you want to make a variable for each element in the Tuple for that tip percentage. Remember from the previous tutorial that when you access an item in a dictionary you get an optional value, since it is possible that there is nothing in the dictionary for that particular key. However, you are sure there is something for that key in this case so use the ! for forced unwrapping.
Finally, the built-in UITableViewCell has two properties for its sublabels: textLable and detailTextLabel. Here you set these and return the cell.
Finally, test your table view by adding the following code to the bottom of your playground:
let testDataSource = TestDataSource()
let tableView = UITableView(frame:CGRect(x: 0, y: 0, width: 320, height: 320), style:.Plain)
tableView.dataSource = testDataSource
tableView.reloadData()
This creates a table view of a hardcoded size, and sets its data source to your new class. It then calls reloadData() to refresh the table view.
Move your mouse over to the sidebar of the playground, to the final line, and click on the eyeball next to UITableView. You should see a neat preview showing your table view with your tip calculator results!
Here is the playground file up to this point.
You’re done prototyping the new and improved TipCalculatorModel and your new table view. It’s time to integrate this code back into your main project!
Finishing Touches
Open your TipCalculator project, and copy the new and improved TipCalculatorModel overtop your existing implementation.
Next, open Main.storyboard, select your Text View (the results area) and delete it. From the Object Library, drag a Table View (not a Table View Controller) into your view controller. Set X=0, Y=187, Width=580, Height=293.
Re-set up Auto Layout by clicking the fourth button in the bottom right of Interface Builder (that looks like a Tie Fighter) and click Clear Constraints and then again with Add Missing Constraints to View Controller.
Finally, select your table view, and select the sixth Inspector (the Connections Inspector). You’ll see two entries for dataSource and delegate – drag the buttons to the right of those over to your view controller in the Document Outline.
Now your view controller is set as both the data source and the delegate of the view controller – similar to how you set the data source of the table view to your test class programmatically earlier.
Finally, make sure your Assistant Editor is open and showing ViewController.swift. Control-drag from the table view down below your final outlet. A popup will appear – enter tableView for the name, and click Connect. Now you can refer to your table view in code.
Now for the code modifications. Open ViewController.swift, and mark your class as conforming to UITableViewDataSource:
class ViewController: UIKit.UIViewController, UITableViewDataSource {
Then, add two new variables below tipCalc:
var possibleTips = Dictionary<Int, (tipAmt:Double, total:Double)>()
var sortedKeys:[Int] = []
Replace calculateTapped() with the following:
@IBAction func calculateTapped(sender : AnyObject) {
  tipCalc.total = Double(totalTextField.text.bridgeToObjectiveC().doubleValue)
  possibleTips = tipCalc.returnPossibleTips()
  sortedKeys = sorted(Array(possibleTips.keys))
  tableView.reloadData()
}
This sets up possibleTips and sortedKeys and triggers a reload of the table view.
Delete the line that sets the resultsTextView in refreshUI().
Copy your two table view methods from your playground into your class, and finally delete all comments from the class.
I’ve attached a spoiler below with the completed class, in case you want to check your work.
Solution Inside: Completed ViewController.swift	Show
Build and run, and enjoy the new look of your tip calculator!
Where To Go From Here?
Here is the finished Xcode project from this tutorial series so far.
Congratulations, you have learned a lot in this tutorial! You have learned about Tuples, Protocols and Delegates, and Table Views, and have upgraded the look and functionality of your Tip Calculator class.
Stay tuned for a bunch more Swift tutorials. We’ll be showing you how you can work with table views, Sprite Kit, some iOS 8 APIs, and much more!
In the meantime, if you want to learn more here are some great resources to check out:
Apple’s Swift Programming book
You can also read the book as an interactive playground in Xcode (Help\Documentation and API Reference\New Features in Xcode 6 Beta\Swift Language\The Swift Programming Language\A Swift Tour\Open Playground
Swift videos from WWDC 2014
Swift Language Cheat Sheet and Quick Reference
Swift Language FAQ
Swift Language Highlights: An Objective-C Developer’s Perspective
Video Tutorial: Introduction to Swift Part 0: Introduction
Check out our three new Swift books, covering Swift, iOS 8, and more
Thanks for reading this tutorial, and if you have any comments or questions please join in the forum discussion below!
