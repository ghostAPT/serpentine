# serpentine
serpentine is a Windows RAT (Remote Administration Tool) that lets you interact with the clients using a multiplatform RESTful C2 server.

Functionalities:

- Startup

- Get files

- Put files

- Keylogger (Just get `%APPDATA%/svchost/log` file)

- Reverse shell (Using `netcat` as a client, listen on a port `nc -l -p 5555` and request a reverse shell connection to that port)

- Reverse proxy (Using [`revp`](https://github.com/jafarlihi/revp), upload `revp` to the client and execute it with a reverse shell)

- Take screenshots

### Dissection
`client` directory holds the Windows portion of the RAT made with C++ and Boost.

`server` holds the RESTful server portion made with Java and Spring.

`frontend` can hold any number of frontends that consume the RESTful API but currently holds only a terminal client made with Go.

#### Build
Build `server` with `mvn package` and then run the JAR with 2 arguments, first being the client-listening port and second the port RESTful API will be served at. Example: `java -jar target/serpentine-0.1.0.jar 2222 8080`

Build `client` with Visual Studio and also include Boost libraries: https://www.boost.org/doc/libs/1_73_0/more/getting_started/windows.html

Build `goserpentine` `frontend` with `go build` and run the resulting executable to see the list of options.

#### API
`/client` GET --> Returns list of currently connected clients (clients that pinged in the last 10 seconds)

`/client` POST --> Changes name associated with a client

`/file/{client}` POST --> Fetches a file from a client

`/file/{client}` PUT --> Uploads a file to a client

`/shell/{client}` POST --> Makes a client initiate a reverse shell connection

`/desktop/{client}` GET --> Takes and fetches a screenshot
