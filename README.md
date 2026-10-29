# NADA — Virtual Wardrobe

<p align="center">
  <img src="src/main/resources/static/logo.png" alt="NADA logo" width="200"/>
</p>

*Nothing to wear?*

NADA is a web application for managing a personal digital wardrobe. Users can upload photos of their clothes, organize them by category and other attributes, create outfits, and share their combinations with others.

## Technologies

- **Java 21 and Spring Boot 3.5.6** — application backend.
- **Spring Web / Spring MVC** — web layer following the Model–View–Controller pattern.
- **Thymeleaf** — server-rendered HTML templates.
- **Spring Data JPA and Hibernate** — object-relational mapping and data persistence.
- **MySQL 8.4** — database used in the Docker environment.
- **HTML, CSS and JavaScript** — user interface and browser interactions.
- **Maven** — dependency management and application builds.
- **Docker and Docker Compose** — application packaging and local deployment.
- **JUnit 5, Spring Boot Test and H2** — automated testing with an in-memory database.

## Architecture

The application follows the **Model–View–Controller (MVC)** pattern and uses a layered backend architecture.

- **Controllers** handle HTTP requests, coordinate application operations and return Thymeleaf views or response data.
- **Views** use Thymeleaf templates, HTML, CSS and JavaScript to render the interface.
- **Models** represent domain entities such as users, clothing items, outfits and posts.
- **Services** implement application logic, separating it from request handling.
- **Repositories** use Spring Data JPA to access the database, with Hibernate handling object-relational mapping.
- **DTOs** define request and response structures for the chat and outfit recommendation features.

The main backend flow is:

```text
HTTP Request → Controller → Service → Repository → Database
```

For server-rendered pages, controllers pass model data to Thymeleaf templates to generate the HTML response.

The application is deployed as a single Spring Boot application, with MySQL running in a separate Docker container.

## Overview

Each user has a personal wardrobe where they can store clothing items with photos and details such as category, color, brand and size.

Users can combine three items into an outfit, save it for future occasions, and add notes of up to 256 characters.

For example:

> Blue T-shirt + white trousers + black shoes → “Meeting” outfit.

The application also includes an outfit recommendation assistant and a social section where users can publish outfits and like other users' posts.

## Features

### Accounts and Personal Wardrobes

- User registration and login.
- A separate digital wardrobe for each user.
- Management of clothing items and saved outfits.
- Deleting a user also removes their associated clothing items and outfits.

### Clothing Management

- Upload clothing photos.
- Crop oversized photos within the application.
- Organize clothing by type, category, color, brand and size.
- Search by type, category, color, brand or size.
- View and edit clothing details.
- Delete clothing items.

Clothing types include tops, bottoms and footwear, with specific categories for each type.

Search results are grouped by clothing type and then by their attributes.

Deleting a clothing item also removes outfits that reference it.

### Outfit Creation

- Combine three clothing items into a personalized outfit.
- Preview selected items while creating or editing an outfit.
- Save, edit and delete outfits.
- Add notes of up to 256 characters.
- Browse previously saved outfits.

### Social Features

- Publish outfits as posts.
- Browse outfits published by other users.
- Like posts.
- Access previously liked posts.

### Outfit Assistant

An integrated assistant suggests outfit combinations using the clothing items available in the user's wardrobe.

### User Interface

- Responsive layouts for desktop and mobile devices.
- Password visibility toggles in login and registration forms.
- Image upload controls and clothing previews.

## Team

- **Francisco Montero Sánchez**
- **Ana Montero Preciado**

## Entity–Relationship Diagram

<img src="src/main/resources/static/DiagramaER.PNG" alt="NADA entity–relationship diagram" width="600"/>

## Running Locally

### Requirements

Install **Docker** and **Docker Compose**. Both are included in Docker Desktop.

### First Run

1. Clone the repository and open a terminal in its root directory:

```bash
git clone https://github.com/MDAI-UEx/nada-virtual-wardrobe.git
cd nada-virtual-wardrobe
```

2. Build and start the containers:

```bash
docker compose up -d --build
```

This command:

- Builds the application image using the Dockerfile.
- Starts the MySQL and application containers.
- Creates or updates the database schema.

Sample data must be imported separately.

3. Once the application has started and created the schema, import the sample data from `src/main/resources/data.sql`.

Run the following command from Bash or Windows Command Prompt:

```bash
docker exec -i mysql_db mysql -u nada -pN4dA_Db_P4ss_2025 -h 127.0.0.1 --protocol=tcp --default-character-set=utf8mb4 nada < src/main/resources/data.sql
```

Import the sample data **before registering additional users**, as the initial records depend on predefined identifiers.

The sample account with clothing images is:

- **Username:** `fran`
- **Password:** `fran123`

4. Open the application at **http://localhost:8080**.

### Subsequent Runs

Once the image has been built and the sample data has been imported:

```bash
docker compose up -d
```

### Stop the Application

Stop and remove the containers while keeping the database volume:

```bash
docker compose down
```

### Reset the Database

Stop the containers and remove their volumes, including the stored MySQL data:

```bash
docker compose down -v
```

After resetting the database, start the application again and repeat the sample data import:

```bash
docker compose up -d --build
```

## Project Structure

```text
.
├── src/
│   ├── main/
│   │   ├── java/com/nada/nada/
│   │   │   ├── controllers/       # Web controllers
│   │   │   ├── data/
│   │   │   │   ├── model/         # Entities and enums
│   │   │   │   ├── repository/    # Persistence repositories
│   │   │   │   └── services/      # Application logic
│   │   │   └── dto/               # Data transfer objects
│   │   └── resources/
│   │       ├── static/            # Images, CSS and JavaScript
│   │       ├── templates/         # Thymeleaf templates
│   │       ├── application.yml    # Application configuration
│   │       └── data.sql           # Sample data
│   └── test/
│       ├── java/                  # Automated tests
│       └── resources/             # Test configuration
├── Dockerfile
├── docker-compose.yml
└── pom.xml
```

## Environments and Testing

### Docker Environment

The standard execution environment uses:

- Docker Compose to run the application and database.
- MySQL for data persistence.
- The `docker` Spring profile.
- A manual import of `src/main/resources/data.sql` for sample data.

| File | Purpose |
| --- | --- |
| `Dockerfile` | Builds the application image. |
| `docker-compose.yml` | Configures the application and MySQL containers. |
| `src/main/resources/data.sql` | Provides the sample dataset. |

### Test Environment

Automated tests use:

- An in-memory H2 database.
- The `test` Spring profile.
- Configuration from `src/test/resources/application-test.yml`.
- Initial data from `src/main/resources/data.sql`.
- Hibernate schema generation with `ddl-auto=create-drop`.

The test suite includes model, repository, service and requirements tests.

To run the tests with Java 21 and Maven installed:

```bash
mvn test
```

## Future Improvements

Potential extensions include:

- Wardrobe usage statistics, such as frequently used items and favorite colors.
- Shopping suggestions based on the user's clothes.
- Shared wardrobes for friends or family.
- A refreshed list of outfit suggestions based on frequently used or popular items.
- Multiple interface languages.
- Optional links to fashion stores and promotions when creating outfits.
