# Spring + React

Spring + React application that computes and renders the best posible path for a grid like board.

## HelloWorld app with Spring + React

### HelloWorld app with Spring
- Descargar e instalar Spring 

- Option 1: Using existing project
    - New -> Import Getting Started Content -> [Rest Service Cors](https://spring.io/guides/gs/rest-service-cors/) 
        - Build type: Maven. 
        - Code Sets: complete
    - Añadir dependencias en pom.xml

- Option 2: Using new project
    - New -> Project -> Maven -> Maven Project -> Create a simple project

- Add dependencies and parent in pom.xml

```xml
  <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.1</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	
  <dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>httpclient</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
    
    <build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```

Creating the main application:

- Create a new Java package. Ex: com.vallesilicona.pathfinding

- Create a new Java class. Ex: ValleSiliconaPathfindingApplication

```java
@SpringBootApplication
public class ValleSiliconaPathfindingApplication {

	public static void main(String[] args) {
		SpringApplication.run(ValleSiliconaPathfindingApplication.class, args);
	}

}

```

Creating the model:

- Create a new Java package. Ex: com.vallesilicona.pathfinding.models

- Create a new Java class. Ex: Greeting

```java
public class Greeting {

	private final long id;
	private final String content;

	public Greeting() {
		this.id = -1;
		this.content = "";
	}

	public Greeting(long id, String content) {
		this.id = id;
		this.content = content;
	}

	public long getId() {
		return id;
	}

	public String getContent() {
		return content;
	}
}


```

Creating the controller:

- Create a new Java package. Ex: com.vallesilicona.pathfinding.controllers

- Create a new Java class. Ex: PathfindingController

```java
@RestController
public class PathfindingController {

	@CrossOrigin()
	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(required = false, defaultValue = "World") String name) {
		System.out.println("==== get greeting ====");
		return new Greeting(1, "Hello World");
	}

}

```

### HelloWorld app with React

We are going to use NextJs for the routing handling of our React application.

- Create new [React NextJs](https://nextjs.org/learn/basics/create-nextjs-app/setup) app
    - Inside a console run: npx create-next-app@latest valle-silicona-pathfinding-ui --use-npm --example "https://github.com/vercel/next-learn/tree/master/basics/learn-starter"

- Run the development server
    - cd valle-silicona-pathfinding-ui
    - npm run dev

- Edit index.js with the REST api message

```javascript
import Head from 'next/head'
import React, { useState, useEffect } from 'react';

export default function Home() {

  const [greeting, setGreeting] = useState({});

  useEffect(() => {
    fetch("http://localhost:8080/greeting")
      .then(res => res.json())
      .then(
        (data) => {
          setGreeting(data);
        }
      )
  }, [])

  return (
    <div className="container">
      <Head>
        <title>Create Next App</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main>
        <h1 className="title">
          {greeting.content}
        </h1>
      </main>

      <style jsx>{`
        .container {
          min-height: 100vh;
          padding: 0 0.5rem;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
        }

        main {
          padding: 5rem 0;
          flex: 1;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
        }

        .title {
          margin: 0;
          line-height: 1.15;
          font-size: 4rem;
        }

        .title,
        .description {
          text-align: center;
        }
      `}</style>

      <style jsx global>{`
        html,
        body {
          padding: 0;
          margin: 0;
          font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto,
            Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue,
            sans-serif;
        }

        * {
          box-sizing: border-box;
        }
      `}</style>
    </div>
  )
}


```