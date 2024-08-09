# TheBlabberApp
This project is created as a code Challenge for Async/ Await Course - [Unit 2.2 - Meet AsyncSequence.](https://coda.io/d/iOSMentorship-io_dkx_nXWeciu/Unit-2-2-Meet-AsyncSequence_sueg6#_luc9G)


## Getting started
1. Clone this project
2. Checkout a new branch with your name
3. Finish the code challenge 
4. Submit an MR and assign that to me

## About
This Blabber app is a chat application where you send and read json response from a URL session that goes on indefinitely, receiving live updates.
```
{"activeUsers": 4}
...
{"id": "...", "message": "Mr Anderson connected", "date": "..."}
...
{"id": "...", "user": "Mr Anderson", "message": "Knock knock...", "date": "..."}
/// and so on ...
```
- First of all, navigating the book server that you have download on [code walkthrough repo](https://gitlab.com/appetiser/university/async-await-course/asyncawaitwalkthrough/-/tree/main?ref_type=heads), open that book server folder in terminal and run this command `swift run` to run the server for this chat application. 
- Wait a few minutes until you see this:
```
[ NOTICE ] Server starting on http://127.0.0.1:8080
```
- Don't move this server folder anywhere after you run this command because that would break vapor.
- Open `BladerModel.swift` as this will mainly be your app logic. Scroll down to  `readMessages(stream:_) ` function and add these linse:
```
var iterator = stream.lines.makeAsyncIterator()
    
guard let first = try await iterator.next() else {
    throw "No response from server"
}

guard
    let data = first.data(using: .utf8),
    let status = try? JSONDecoder()
    .decode(ServerStatus.self, from: data)
else {
    throw "Invalid response from server"
}

messages.append(
    Message(
    message: "\(status.activeUsers) active users"
    )
)
```
- What you did were basically read this line from the `URLSession.AsyncBytes`. 
```
{"activeUsers": 4}
```
### Challenge ###

**Your first challenge:**
You are going to Looping through the AsyncStream called `stream` and add response message to the UI. 
1. Looping through the rest of the stream using a for-loop
2. Decode each response to a `Message` type
3. Append the result to the `messages` array


**Your Second Challenge**
On the immediate right side of your message dialogue is a alternative send message button that will send these to the server:
```
3...
2...
1...
"🎉 " + [message]
```
Notice that [Message] is what users actually type.
1. Open BlabberMode and scroll down to ```countDown()``` function, Create an AsyncStream that will send those message above to server:
```
var countdown = 3
let counter = AsyncStream<String> {

}
```
2. Create a for-loop to listen to this AsyncStream and enable this stream to run as soon as it produces values. 
```
for await countdownMessage in counter {
  try await say(countdownMessage)
}
```
3. Use Task.sleep and proper logic to finish the implementation of `counter` AsyncStream. 

**Your final challenge:**
Congratulation!! If you come to this far, I want you to pat on your back and tell yourself how amazing you are to go all the way here. This challenge is gonna be a tough. 

Remember when I create an AsyncStream and picked up individual download item with a continuation object? You are going to do just that but you are going to pick up individual response from the Core Location framework to create a single output function and return a location that you can send using the button furthest to the left on your chat dialogue.

1. Open `ChatLocationDelegate` and fill out the implementation
2. Open `BlabberModel`, scroll to `shareLocation()` and fill out the implementation there too

Good Luck and happy coding!!

