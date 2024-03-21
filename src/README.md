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

</details>

---

