# Tutorial 3 - StatesAndObjects

Tutorial 3 is going to talk about states and objects, and why they're important.

## States

Swift loves structs. As such, a lot of SwiftUI is built using structs. However, in Swift, structs are immutable. This means that it's very hard to have the state of your application change given that it can't have variables change. Luckily enough, Apple has provided some nifty tools to get around this, starting with the `@State` decorator.

Basically, a `State` is a variable not technically managed by the struct. Instead, SwiftUI controls the state, and when it changes, refreshes the view using the new `State`. What's really cool is that this all happens automatically: all you need to do is put the `@State` decorator.

For our example, create a new Xcode single view application (as you did in tutorial 1). What we're going to do is add a button (like in tutorial 2), but this time we're going to make it do something the user can see. To start, your ContentView should look like this:

```
struct ContentView: View {
    var body: some View {
        VStack {
            Text("Hello, World!")
            Button(action: {
                print("Button pressed...")
            }) {
                Text("Press me")
                    .padding()
            }
        }
    }
}
```

Let's add a `@State` variable named "isDaytime" and make it true by default (ignore why the private's there, just know that it's best practice to have it there).

```
struct ContentView: View {
    
    @State private var isDaytime: Bool = true
    
    var body: some View {
        VStack {
            Text("Hello, World!")
            Button(action: {
                print("Button pressed...")
            }) {
                Text("Press me")
                    .padding()
            }
        }
    }
}
```

Next, let's use the variable. We're going to use what's called a *turnary operator*. It's basically a compact if statement. The way it works is like this: `ifThisVariableOrConditionIsTrue ? returnThisValue : andReturnThisIfItsFalse`.

```
struct ContentView: View {
    
    @State private var isDaytime: Bool = true
    
    var body: some View {
        VStack {
            Text(isDaytime ? "Good morning" : "Good evening")
            Button(action: {
                print("Button pressed...")
            }) {
                Text("Press me")
                    .padding()
            }
        }
    }
}
```

Finally, let's make the button change the state:

```
struct ContentView: View {
    
    @State private var isDaytime: Bool = true
    
    var body: some View {
        VStack {
            Text(isDaytime ? "Good morning" : "Good evening")
            Button(action: {
                self.isDaytime.toggle()
            }) {
                Text("Press me")
                    .padding()
            }
        }
    }
}
```

Try running that, and you'll see that pushing the button changes the text.

In summary, `@State` is used for small variables to control what a singular view looks like. What about complex sets of data that we want to pass between views? Well, that's were `@ObservedObject` comes into play...

## ObservedObject

An `@ObservedObject` is similar to `@State`, but since `@State` only works for structs (which cannot have to references to the same one), we can't share data models between views. As such, we can use classes that are `@ObservedObject`s to do exactly that.

Let's say the app we started in the previous section tracks sleep cycles, and we want to pass userdata we retrieve on the main menu to a deeper menu. First, we're going to make the user model (feel free to put this in a new file, or create a new empty Swift file and import SwiftUI:

```
class User: ObservableObject {
    @Published public var username: String
    @Published public var days: Int = 0
    
    init(_ username: String) {
        self.username = username
    }
}
```

You'll notice that we use the `@Published` tag for the variables within here and we've put `: ObservableObject` to indicate this class is an `ObservableObject`. `@Published` is *exactly* like `@State` except it's used for variables within `ObservableObject`. Ignore the rest of the class terminology for now (as we'll be going into it in more detail in a later tutorial), all you need to know is it stores a username and the amount of days the user has "used" the app.

Next, let's add a new view with the following code:

```
struct UserDataView: View {
    
    @ObservedObject var user: User
    
    var body: some View {
        VStack {
            Text("Username: \(user.username)")
            Text("Days on app: \(user.days)")
        }.navigationBarTitle("Profile", displayMode: .inline)
    }
}
```

You may notice I've already included the `@ObervedObject` protocol

# NEEDS WORK
