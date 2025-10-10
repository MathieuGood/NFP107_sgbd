# Database Setup Project

This project sets up a Docker Compose environment with MariaDB and phpMyAdmin, allowing you to easily manage and interact with multiple databases defined in the provided SQL files.

## Project Structure

```
database-setup-project
├── docker-compose.yml
├── init-sql
│   ├── base_immeubles.sql
│   ├── base_voyageurs.sql
│   └── base_films.sql
└── README.md
```

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Setup Instructions

1. **Clone the repository** (or download the project files):
   ```
   git clone <repository-url>
   cd database-setup-project
   ```

2. **Navigate to the project directory**:
   ```
   cd database-setup-project
   ```

3. **Start the Docker containers**:
   ```
   docker-compose up -d
   ```

   This command will start the MariaDB and phpMyAdmin services in detached mode.

4. **Access phpMyAdmin**:
   Open your web browser and go to `http://localhost:8080`. You can log in using the following credentials:
   - **Username**: `root`
   - **Password**: `example`

5. **Databases Initialization**:
   The SQL files located in the `init-sql` directory will be automatically executed to create the following databases:
   - **Immeubles**: Contains tables for `Immeuble`, `Appart`, `Personne`, and `Propriétaire`.
   - **Voyageurs**: Contains tables for `Voyageur`, `Logement`, `Séjour`, and `Activité`.
   - **Films**: Contains tables for `Pays`, `Internaute`, `Artiste`, `Film`, `Notation`, and `Rôle`.

### Stopping the Services

To stop the Docker containers, run:
```
docker-compose down
```

### Additional Information

For more details on the database schemas, refer to the SQL files in the `init-sql` directory.