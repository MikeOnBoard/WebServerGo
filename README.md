# Go Web Server Project

## Project Overview
This Go web server project demonstrates a simple HTTP server that serves both static HTML files and handles basic form submissions. It includes:

Serving static files (e.g., HTML files) from a ``static`` directory.

-Handling GET and POST requests using predefined routes.

-Displaying user input from an HTML form on the web server.

-The project consists of the following files:

``main.go``: The main Go file that defines the server logic.

``form.html``: An HTML form for collecting user input.

``index.html``: A static HTML page served by the server.

### Project Structure

    .
    ├── main.go          # Go source code for the web server
    └── static           # Directory for static HTML files
        ├── form.html    # HTML form for user input
        └── index.html   # Static HTML page

## Requirements
To run this project, you'll need:

Go installed on your machine. You can download and install it from https://golang.org/dl/.

A web browser for testing the server's endpoints.

## Running the Project

### Option 1: Running from Source
Clone the project files into a directory.

Navigate to the project directory in your terminal.

Compile and run the server: You can run the Go server directly by compiling it:
```bash
go run main.go
```
If you want to build the server binary, you can use:
```bash
go build -o go_server
./go_server
```
#### Access the server: The server will start at port ``8080``. 
You can open your web browser and navigate to ``http://localhost:8080`` to see the homepage.

To view the static homepage, visit: http://localhost:8080.

To access the form page, go to: http://localhost:8080/form.html.

To view the "Hello" page, visit: http://localhost:8080/hello.

### Option 2: Running the Compiled Binary

If you already have the ``go_server`` binary, you can run it directly:

Open a terminal and navigate to the directory containing g``o_server``.
Run the binary:
```bash
./go_server
```

This will start the server at ``http://localhost:8080``.

## Project Details

### 1. ``main.go``
This is the Go source code that defines the web server.

#### Static file server: The server serves all static files (like ``index.html`` and ``form.html``) from the static directory using ``http.FileServer``.
```Go
    fileserver := http.FileServer(http.Dir("./static"))
    http.Handle("/", fileserver)
```
#### Form handler: The server handles form submissions via the ``/form`` route. The ``formHandler`` function processes the form data submitted via the POST request, extracts the ``name`` and ``address`` fields, and returns them as a response.
```Go
func formHandler(w http.ResponseWriter, r *http.Request) {
    if err := r.ParseForm(); err != nil {
        fmt.Fprintf(w, "ParseForm() err: %v", err)
        return
    }
    fmt.Fprintf(w, "POST request successful")
    name := r.FormValue("name")
    address := r.FormValue("address")
    fmt.Fprintf(w, "Name = %s\n", name)
    fmt.Fprintf(w, "Address = %s\n", address)
}
```
#### Hello handler: This handler responds to GET requests at ``/hello``. It returns a simple "Hello!" message if the request is valid.
```go
func helloHandler(w http.ResponseWriter, r *http.Request) {
    if r.URL.Path != "/hello" {
        http.Error(w, "404 not found", http.StatusNotFound)
        return
    }
    if r.Method != "GET" {
        http.Error(w, "Method is not supported", http.StatusNotFound)
        return
    }
    fmt.Fprintf(w, "Hello!")
}
```
#### Starting the server: The server listens on port ``8080``, and if it encounters an error, it logs the error and exits.
```go
fmt.Print("Starting server at port 8080\n")
if err := http.ListenAndServe(":8080", nil); err != nil {
    log.Fatal(err)
}
```
### 2. ``static/form.html``
This is a basic HTML form that collects a user's name and address and submits it to the ``/form`` route.
```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
        </head>
        <body>
            <div>
                <form method="POST" action="/form">
                    <label>Name</label><input name="name" type="text" value=""/>
                    <label>Address</label><input name="address" type="text" value="" />
                    <input type="submit" value="submit" />
                </form>
            </div>
        </body>
    </html>
```

### 3. ``static/index.html``
A simple static HTML file that acts as the homepage for the server. It is served whenever a user accesses the root path (``/``).
```html
    <html>
        <head>
            <title>Static website</title>
        </head>
        <body>
            <h2>Static website</h2>
        </body>
    </html>
```

## How the Project Works

Serving Static Files: When the server starts, it serves the ``index.html`` and ``form.html`` pages from the ``static`` directory. Any static resources (CSS, JavaScript, images) could also be placed in this folder and accessed directly from the browser.

Handling the Form Submission: The form in ``form.html`` sends a POST request to the server at ``/form``. The Go server processes the request, extracts the form data (name and address), and responds with the submitted values.

Dynamic Routing: The server also provides a dynamic route (``/hello``) that responds with a simple message when accessed via a GET request.

## Additional Features

Extend the project: You can add more routes, templates, and handlers to expand the functionality of the server. For example, adding a new route that serves JSON responses, or implementing more complex form validation.

Error Handling: The project already demonstrates basic error handling when parsing forms and handling unsupported routes.

## Conclusion

This project provides a basic but functional web server built in Go. It handles static file serving, form processing, and basic GET requests. It can be used as a starting point for building more complex web applications in Go.
