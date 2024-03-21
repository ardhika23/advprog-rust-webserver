# 📝Tutorial & Exercise📝

**Student Details :**

|  `Attribute`  | `Information`              |
|---------------|----------------------------|
| Name          | Ardhika Satria Narendra    |
| Student ID    | 2206821866                 |
| Class         | Advanced Programming KKI   |

---
<details>
<summary>Module 01: Coding Standards</summary>

## Questions and Answers

### -> Commit 1 Reflection 

We bring std::io::prelude and std::io::BufReader into scope to get access to traits and types that let us read from and write to the stream. In the for loop in the main function, instead of printing a message that says we made a connection, we now call the new handle_connection function and pass the stream to it.

In the handle_connection function, we create a new BufReader instance that wraps a mutable reference to the stream. BufReader adds buffering by managing calls to the std::io::Read trait methods for us.

We create a variable named http_request to collect the lines of the request the browser sends to our server. We indicate that we want to collect these lines in a vector by adding the Vec<_> type annotation.

BufReader implements the std::io::BufRead trait, which provides the lines method. The lines method returns an iterator of Result<String, std::io::Error> by splitting the stream of data whenever it sees a newline byte. To get each String, we map and unwrap each Result. The Result might be an error if the data isn’t valid UTF-8 or if there was a problem reading from the stream. Again, a production program should handle these errors more gracefully, but we’re choosing to stop the program in the error case for simplicity.

The browser signals the end of an HTTP request by sending two newline characters in a row, so to get one request from the stream, we take lines until we get a line that is the empty string. Once we’ve collected the lines into the vector, we’re printing them out using pretty debug formatting so we can take a look at the instructions the web browser is sending to our server.

### -> Commit 2 Reflection

![Commit 2 screen capture](/assets/images/commit2.png)

We’ve added fs to the use statement to bring the standard library’s filesystem module into scope. The code for reading the contents of a file to a string should look familiar; we used it in Chapter 12 when we read the contents of a file for our I/O project in Listing 12-4.

Next, we use format! to add the file’s contents as the body of the success response. To ensure a valid HTTP response, we add the Content-Length header which is set to the size of our response body, in this case the size of hello.html.

Run this code with cargo run and load 127.0.0.1:7878 in our browser; we should see our HTML rendered!

Currently, we’re ignoring the request data in http_request and just sending back the contents of the HTML file unconditionally. That means if we try requesting 127.0.0.1:7878/something-else in our browser, we will still get back this same HTML response. At the moment, our server is very limited and does not do what most web servers do. We want to customize our responses depending on the request and only send back the HTML file for a well-formed request to /.

### -> Commit 3 Reflection

![Commit 3 screen capture](/assets/images/commit3.png)

We’re only going to be looking at the first line of the HTTP request, so rather than reading the entire request into a vector, we’re calling next to get the first item from the iterator. The first unwrap takes care of the Option and stops the program if the iterator has no items. The second unwrap handles the Result and has the same effect as the unwrap that was in the map added in Listing 20-2.

Next, we check the request_line to see if it equals the request line of a GET request to the / path. If it does, the if block returns the contents of our HTML file.

If the request_line does not equal the GET request to the / path, it means we’ve received some other request. We’ll add code to the else block in a moment to respond to all other requests.

We’ll also return some HTML for a page to render in the browser indicating the response to the end user.

### -> Commit 4 Reflection

We switched from if to match now that we have three cases. We need to explicitly match on a slice of request_line to pattern match against the string literal values; match doesn’t do automatic referencing and dereferencing like the equality method does.

The first arm is the same as the if block from Listing 20-9. The second arm matches a request to /sleep. When that request is received, the server will sleep for 5 seconds before rendering the successful HTML page. The third arm is the same as the else block from Listing 20-9.

Start the server using cargo run. Then open two browser windows: one for http://127.0.0.1:7878/ and the other for http://127.0.0.1:7878/sleep. If we enter the / URI a few times, as before, we’ll see it respond quickly. But if we enter /sleep and then load /, we’ll see that / waits until sleep has slept for its full 5 seconds before loading.

### -> Commit 5 Reflection 

We use ThreadPool::new to create a new thread pool with a configurable number of threads, in this case four. Then, in the for loop, pool.execute has a similar interface as thread::spawn in that it takes a closure the pool should run for each stream. We need to implement pool.execute so it takes the closure and gives it to a thread in the pool to run. 

We chose usize as the type of the size parameter, because we know that a negative number of threads doesn’t make any sense. 

The F type parameter is the one we’re concerned with here; the T type parameter is related to the return value, and we’re not concerned with that. We can see that spawn uses FnOnce as the trait bound on F. This is probably what we want as well, because we’ll eventually pass the argument we get in execute to spawn.git 

We still use the () after FnOnce because this FnOnce represents a closure that takes no parameters and returns the unit type (). Just like function definitions, the return type can be omitted from the signature, but even if we have no parameters, we still need the parentheses.

### -> Commit 6 Bonus

The build function will have the same functionality as new. The difference between new and build is primarily in error handling: new typically instantiates an object directly, often panicking on invalid input like a size of 0. Build, however, returns a Result, allowing for graceful error handling by returning an Err on invalid input, aligning with Rust's error handling conventions.

</details>

---

